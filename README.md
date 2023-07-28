# Reporte con Procedimiento Almacenado con Parametros
Implementación de reporte mensual con procedimientos almacenados con parámetros con SSIS.

<b>1.</b> Creamos un nuevo paquete SSIS Packages el cuál llamaremos <b>Reporte con procedimiento almacenado con parametros</b>

<b>2.</b> En la base de datos NORTHWND crearemos un Procedimiento Almacenado para ejecutar una consulta con dos variables (Fecha Inicial y Fecha Final). Estos parámetros serán solo el año YYYY

```Ruby
CREATE PROCEDURE sp_ReporteEnvioClientes
@FechaIni int,
@FechaFin int
AS
SELECT	Customers.Country, 
		Customers.CompanyName, 
		Customers.CustomerID, 
		Orders.OrderID, 
		CONVERT(varchar(10), Orders.OrderDate, 111) AS OrderDate,
		DATEPART(YYYY, Orders.OrderDate) as año,
		DATEPART(MM, Orders.OrderDate) as mes,
		DATEPART(DD, Orders.OrderDate) as dia,
		Products.ProductName, 
		[Order Details].UnitPrice, 
		[Order Details].Quantity		

FROM     Customers INNER JOIN
                  Orders ON Customers.CustomerID = Orders.CustomerID INNER JOIN
                  [Order Details] ON Orders.OrderID = [Order Details].OrderID INNER JOIN
                  Products ON [Order Details].ProductID = Products.ProductID
WHERE DATEPART(YYYY, Orders.OrderDate)>= @FechaIni AND DATEPART(YYYY, Orders.OrderDate)<= @Fechafin
GO
```
<b>3.</b> Inicialmente crearemos las variables que vamos a utilizar. En este caso las de FechaIni, FechaFin, y la variable que va a guardar la expresión de la ejecución del SP.
```"EXECUTE sp_ReporteEnvioClientes "+ (DT_WSTR, 4) @[User::FechaIni] + ", " + (DT_WSTR, 4) @[User::FechaFin]```

<p align="center">
<img src="https://github.com/csantamaria89/Reporte_con_procedimiento_Almacenado_con_Parametro/blob/main/assets/Imagen1.png"  height=450>
</p>

<b>4.</b> Ahora en el ControlFlow arrastramos un Data Flow Task el cual llamaremos Reporte con parametro. Hacemos doble clic y agregamos un OLE DB Source. Configuramos la conexión con la DB NORTHWND, seleccionamos SQL commandfrom variable y agregamos la variable que tiene guardad la ejecución del SP:

<p align="center">
<img src="https://github.com/csantamaria89/Reporte_con_procedimiento_Almacenado_con_Parametro/blob/main/assets/Imagen2.png"  height=450>
</p>

<b>5.</b> Creamos un Flat File Destination, que es el archivo de texto donde vamos a alojar el reporte.

<p align="center">
<img src="https://github.com/csantamaria89/Reporte_con_procedimiento_Almacenado_con_Parametro/blob/main/assets/Imagen3.png"  height=450>
</p>

<b>6.</b> En las siguientes imagenes se muestra el paso a paso para deployar nuestro proyecto:

<p align="center">
<img src="https://github.com/csantamaria89/Reporte_con_procedimiento_Almacenado_con_Parametro/blob/main/assets/Imagen4.png"  height=450>
<img src="https://github.com/csantamaria89/Reporte_con_procedimiento_Almacenado_con_Parametro/blob/main/assets/Imagen5.png"  height=450>
<img src="https://github.com/csantamaria89/Reporte_con_procedimiento_Almacenado_con_Parametro/blob/main/assets/Imagen6.png"  height=450>
<img src="https://github.com/csantamaria89/Reporte_con_procedimiento_Almacenado_con_Parametro/blob/main/assets/Imagen7.png"  height=450>
<img src="https://github.com/csantamaria89/Reporte_con_procedimiento_Almacenado_con_Parametro/blob/main/assets/Imagen8.png"  height=450>
<img src="https://github.com/csantamaria89/Reporte_con_procedimiento_Almacenado_con_Parametro/blob/main/assets/Imagen9.png"  height=450>
<img src="https://github.com/csantamaria89/Reporte_con_procedimiento_Almacenado_con_Parametro/blob/main/assets/Imagen10.png"  height=450>
<img src="https://github.com/csantamaria89/Reporte_con_procedimiento_Almacenado_con_Parametro/blob/main/assets/Imagen11.png"  height=450>
</p>

<b>6.</b> Podemos ejecutar el reporte directamente del SQL Service Manager y ver que efectivamente esta generando el archivo de texto con el reporte.
<img src="https://github.com/csantamaria89/Reporte_con_procedimiento_Almacenado_con_Parametro/blob/main/assets/Imagen12.png"  height=450>

<b>7.</b> Ahora se crearan dos variables tipo parametro en el proyecto "FechaIni y FechaFin" esto con el fin de poder ingresar los años por demanda

<img src="https://github.com/csantamaria89/Reporte_con_procedimiento_Almacenado_con_Parametro/blob/main/assets/Imagen13.png"  height=250>
