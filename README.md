
CONSULTAS MULTITABLAS (composición interna)

![Alt text](drawSQL-jardineria-export-2023-11-09.png)

1. Obten un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

	SELECT 
	c.nombre_cliente AS Nombre_Cliente,
	CONCAT(e.nombre, ''. e.apellido1, '', e.apellido2) AS Nombre_Representante_Ventas
	FROM cliente c
	JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado

2. Muestra el nombre de los clientes que hayan realizado los pagos junto con el nnombre de sus representates de ventas

	SELECT DISTINCT c.nombre_cliente AS Nombre_cliente, CONCAT(e.nombre, '' , e.apellido1,'',e.apellido2) AS Representante_Ventas
	FROM cliente c
	INNER JOIN empleado e IN c.codigo_empleado_rep_ventas = e.codigo_empleado
	INNER JOIN pago p ON c.codigo_cliente = p.codigo_cliente
	
	
	
	
3. muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representante de ventas

	SELECT
	c.nombres_cliente AS Nombre_Cliente,
	CONCAT(e.nombre. '',e.apellido1) AS Nombre_Representate_Ventas
	FROM cliente c
	LEFT JOIN empleado e ON 
	c.codigo_empleado_rep_ventas = e.codigo_empleado
	LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente
	WHERE c.codigo_transaccion IS NULL;

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representates junto con la ciudad de la oficina a la que pertenece el representante.

	SELECT DISTINCT c.nombre_cliente, CONCAT (e.nombre, '', e.apellido) AS Nombre_Representate, o.ciudad  AS oficina
	FROM cliente c
	JOIN pago p ON c.codigo_cliente = p.codigo_cliente
	JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
	JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;
	ORDEN BY c.nombre_cliente ASC, Nombre_Representate ASC, oficina ASC;

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representates junto con la ciudad de oficina a la que pertence el representate.

	SELECT c.nombre_cliente, CONCAT(e.nombre, '', e.apellido1) AS Nombre_Representante, o.ciudad AS oficina 
	FROM cliente c
	LEFT JOIN pago p ON c.codigo_cliente = p.codigo_cliente
	JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
	JOIN oficina o ON e.codigo_oficina = o.codigo_oficina
	WHERE p.id_transaccion IS NULL ORDER BY c.nombre_cliente ASC, Nombre_Representante, oficina ASC;
	

6. Lista la direccion de las oficinas que tengan clientes en Fuenlabrada

	SELECT DISTINCT o.linea_direccion1, o.linea_direccion2, o.ciudad, o.region, o.pais
	FROM oficina o JOIN empleado e ON o.codigo_oficina = e.codigo_oficina JOIN cliente c
	ON e.codigo_empleado  = c.codigo_empleado_rep_ventas
	WHERE c.ciudad = 'Fuenlabrada';

7. Devuelve el noombre d elos clientes y el nombre de sus representates junto con la ciudad de la oficina a la que pertenece el representante

	SELECT
	c.nombre_cliente AS Nombre_Cliente,
	e.nombre AS Nombre_Representanre,
	o.ciudad AS Ciudad_Oficina_Representante
	FROM cliente c
	JOIN empleado e ON c.codigo_empleado_rep_ventas = e.codigo_empleado
	JOIN oficina o ON e.codigo_oficina = o.codigo_oficina;

8. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.

	SELECT
	CONCAT(e1.nombre, ' ', e1.apellido1) AS NombreEmpleado,
	CONCAT(e2.nombre, ' ', e2.apellido1) AS NombreJefe
	FROM empleado e1
	LEFT JOIN empleado e2 ON e1.codigo_jefe = e2.codigo_empleado;

9. Devuelve un listado que muestre el nombre de cada empleado, el nombre de su jefe y el nombre del jefe de sus jefes.

	SELECT
	e1.codigo_empleado AS CodigoEmpleado,
	CONCAT(e1.nombre, ' ', e1.apellido1) AS NombreEmpleado,
	CONCAT(e2.nombre, ' ', e2.apellido1) AS NombreJefe,
  	CONCAT(e3.nombre, ' ', e3.apellido1) AS NombreJefeDelJefe
	FROM empleado e1
	LEFT JOIN empleado e2 ON e1.codigo_jefe = e2.codigo_empleado
	LEFT JOIN empleado e3 ON e2.codigo_jefe = e3.codigo_empleado;

10. devuelve el nombre de los clientes a los que no se les ha entregado a tiempo un pedido

	SELECT DISTINCT c.nombre_cliente AS NombreCliente
	FROM cliente c
	INNER JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
	WHERE p.fecha_entrega > p.fecha_esperada;

11. devuelve un listado de las diferentes gamas de producto que ha comprado cada cliente.

	SELECT DISTINCT
    c.nombre_cliente AS NombreCliente,
    g.gama AS GamaProducto
    FROM cliente c
    JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
    JOIN detalle_pedido dp ON p.codigo_pedido = dp.codigo_pedido
    JOIN producto pr ON dp.codigo_producto = pr.codigo_producto
    JOIN gama_producto g ON pr.gama = g.gama
    GROUP BY c.nombre_cliente, g.gama;



-------------------------------------------------------------------


sub consultas con 
1. devuelve el nombre del cliente con mayor limite de credito.

SELECT * FROM cliente ORDER BY nombre_cliente ASC; 



SELECT nombre_cliente  AS cliente, limite_credito AS credito FROM cliente WHERE  limite_credito = (
 SELECT MAX(limite_credito) FROM cliente
);

2. Devuelve un listado con la ciudad y el telefono de las oficinas de España.

	SELECT ciudad, telefono 
	FROM oficina
	WHERE ciudad IN (
	SELECT ciudad FROM oficina WHERE ciudad LIKE '%Madrid%');

3. Devuelve un listado del nombre, apellidos y email de los empleados cuyo jefe tiene un codigo de jefe igual a 7.

	SELECT nombre, apellido1, apellido2, email
	FROM empleado
	WHERE codigo_jefe IN (
	SELECT codigo_jefe FROM empleado WHERE codigo_jefe LIKE 7 ); 


4. Devuelve el nombre del puesto, nombre, apellidos, email del jefe de la empresa.

	SELECT puesto, nombre, apellido1, apellido2, email 
	FROM empleado
	WHERE puesto IN (
		SELECT puesto FROM empleado WHERE puesto LIKE '%Director general%'
	);

5. Devuelve un listado con el nombre, apellidos y puesto de aquellos empleados que no sean representantes de ventas.

SELECT nombre, apellido1, apellido2, puesto
FROM empleado
WHERE puesto <> 'Representante Ventas';


6. Devuelve  un listado con el nombre de los todos los clientes españoles.

	SELECT nombre_cliente AS Nombre
	FROM cliente
	WHERE pais IN (
		SELECT pais FROM cliente WHERE pais LIKE '%Spain%'
	);

7. Devuelve un listado con los distintos estados por los que  puede pasar un pedido.

	SELECT DISTINCT estado 
	FROM pedido;

8. Devuelve un listado con el codigo de cliente  de aquellos clientes que realizaron algun pago en 2008. Tenga en cuenta que debera eliminar aquellos codigos de cliente que aparezcan repetidos. Resuelva la consulta:

- Utilizando la funcion YEAR de MYSQL 
- Utilizando la función DATE_FORMAT de MySQL
- Sin utiliar ninguna de las funiones anteriores


SELECT DISTINCT codigo_cliente, fecha_pago 
FROM pago
WHERE YEAR(fecha_pago) = 2008;


9. Devuelve un listado con el codigo de pedido, codigo de cliente, fecha esperada y fecha de entrega de los pedidos que no han sido entregados a tiempo.

	SELECT codigo_pedido, codigo_cliente, fecha_esperada, estado
	FROM pedido
	WHERE fecha_entrega (
		SELECT fecha_entrega
	);

	SELECT codigo_pedido, codigo_cliente, fecha_esperada, fecha_entrega
	FROM pedido
	WHERE  fecha_entrega IS NULL;



10. Devuelve un listado con el codigo de pedido, codigo de cliente, fecha esperada y fecha de entrega de los pedidos cuya entrega ha sudo al menos dos dias antes de la fecha esperada

- Utilizando la funcion ADDDATE de MySQL
- ultilizando la funcion DATE DIFF de MySQL ¿Sería posible resolver esta consulta utilizando el operador de suma+ o resta -?

11. Devuleve un listado de todos los pedidos que fuerin rechazados en 2000.

12. devuelve un listado de todos los pedidos que han sido entregados en el mes de enero de cualquier año.



## ------------------------------------------------------------------------
# Consultas referentes a los tips MySQL

## Video 1 (5 tips MySQL GROUP BY)


### 1. cuenta el número total de empleados en cada oficina, mostrando el código de la oficina, el país al que pertenece y la cantidad total de empleados en esa oficina. <BR>
	
	
	SELECT o.codigo_oficina, o.pais, COUNT(*) AS TotalEmpleados 
	FROM empleado e 
	INNER JOIN oficina o ON e.codigo_oficina = o.codigo_oficina 
	GROUP BY o.codigo_oficina, o.pais; 
	


### 2. busca la cantidad mínima de crédito límite entre los clientes cuyos nombres de contacto comienzan con la letra 'A'. Además, muestra solo aquellos clientes cuyos nombres de contacto comienzan con 'A' y tienen más de 2 pedidos realizados. <br>


	SELECT c.nombre_cliente, MIN(c.limite_credito) AS MinLimiteCredito
	FROM cliente c
	INNER JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
	WHERE SUBSTRING(c.nombre_contacto, 1, 1) = 'A'
	GROUP BY c.nombre_cliente
	HAVING COUNT(*) > 2;



### 3. Busca contar el número total de productos en cada gama, mostrando la primera letra de cada gama y la cantidad total de productos en esa gama.<br>


	SELECT SUBSTRING(p.gama, 1, 1) AS gamaLetter, COUNT(*) AS totalProductos
	FROM producto p
	INNER JOIN gama_producto g ON p.gama = g.gama
	GROUP BY SUBSTRING(p.gama, 1, 1);


### 4. muestra el nombre del producto y la cantidad total de productos por gama.


	SELECT SUBSTRING(p.gama, 1, 1) AS primeraLetraGama, COUNT(*) AS totalProductos
	FROM producto p
	INNER JOIN gama_producto g ON p.gama = g.gama
	GROUP BY SUBSTRING(p.gama, 1, 1);


### 5. muestra el nombre de cada cliente y la cantidad total de pedidos que ha realizado. 

	SELECT c.nombre_cliente, COUNT(p.codigo_pedido) AS total_pedidos
	FROM cliente c
	LEFT JOIN pedido p ON c.codigo_cliente = p.codigo_cliente
	GROUP BY c.nombre_cliente;


# Video 2 (5 tips MySQL WHERE)

### 1.



### 2.



### 3.



### 4.


### 5.


# Video 3(5 tips MySQL UPDATE)

### 1.



### 2.



### 3.



### 4.


### 5.

# Video 4 ( 5 tips MySQL SELECT)

### 1.



### 2.



### 3.



### 4.


### 5.



 













	

 
