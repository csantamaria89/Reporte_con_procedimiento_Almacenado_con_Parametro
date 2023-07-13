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
<img src="https://github.com/csantamaria89/CargaIncremental-SSIS/blob/main/assets/Imagen1.png"  height=450>
</p>
