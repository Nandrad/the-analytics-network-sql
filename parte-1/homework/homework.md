# Homework

Bienvenidos a la seccion de tarea para el hogar de la parte 1. Es recomendable realizar estas preguntas durante el transcurso 
de la semana para poder seguir el ritmo a las clases.


Cada pregunta sera respondida con una query que devuelva los datos necesarios para reponder la misma. Muchas de las preguntas pueden tener 
mas de una manera de ser respondidas y en otras es posible que debas aplicar tu criteria para decidir cual es la mejor opcion. 

Ejemplo: 

Pregunta: Cuantas tiendas estan ubicadas en Argentina? 

Respuesta: ``` select count(distinct codigo_tienda) from tiendas where pais = 'Argentina'``` 

Como la tabla tiendas tiene valores unicos por tienda otra respuesta puede ser:

Respuesta 2: ``` select count(codigo_tienda) from tiendas where pais = 'Argentina'``` 

Ambas respuestas van a ser consideradas validas.


## Clase 1

1. Mostrar todos los productos dentro de la categoria electro junto con todos los detalles. 

		SELECT* 
		FROM 	
			stg.product_master
		WHERE
			categoria='Electro'


2. Cuales son los producto producidos en China? 

		SELECT* 
		FROM 
			stg.product_master
		WHERE 
			origen='China'

3. Mostrar todos los productos de Electro ordenados por nombre. 

		SELECT 
			codigo_producto,nombre
		FROM 
			stg.product_master
		WHERE 
			categoria='Electro'
		ORDER BY 
			nombre ASC

4. Cuales son las TV que se encuentran activas para la venta?

		SELECT* 
		FROM 
			stg.product_master
		WHERE  
			subcategoria='TV' 
		AND 	
			is_active=true

5. Mostrar todas las tiendas de Argentina ordenadas por fecha de apertura de las mas antigua a la mas nueva. 


		SELECT* 
		FROM
			stg.store_master
		WHERE 
			pais='Argentina'
		ORDER BY 
			fecha_apertura ASC

6. Cuales fueron las ultimas 5 ordenes de ventas? 

		SELECT* 
		FROM 
			stg.order_line_sale
		ORDER BY 
			fecha DESC
		LIMIT 5

7. Mostrar los primeros 10 registros de el conteo de trafico por Super store ordenados por fecha. 

		SELECT* 
		FROM 
			stg.super_store_count
		ORDER BY 
			fecha
		LIMIT 10

8. Cuales son los producto de electro que no son Soporte de TV ni control remoto. 

		SELECT* 
		FROM 
			stg.product_master
		WHERE 
			categoria = 'Electro' 
		AND 
			subsubcategoria != 'Soporte' AND subsubcategoria != 'Control remoto'

9. Mostrar todas las lineas de venta donde el monto sea mayor a $100.000 solo para transacciones en pesos.

		SELECT* 
		FROM 
			stg.order_line_sale
		WHERE 
			venta > 100000 
		AND 
			moneda = 'ARS'

10. Mostrar todas las lineas de ventas de Octubre 2022.

		SELECT* 
		FROM 
			stg.order_line_sale
		WHERE 
			fecha BETWEEN '2022-10-01' AND '2022-10-31'

11. Mostrar todos los productos que tengan EAN.

		SELECT* 
		FROM
			stg.product_master
		WHERE 
			ean IS NOT NULL


12. Mostrar todas las lineas de venta que que hayan sido vendidas entre 1 de Octubre de 2022 y 10 de Noviembre de 2022.

		SELECT* 
		FROM 
			stg.order_line_sale
		WHERE 
			fecha BETWEEN '2022-10-01' AND '2022-11-10'


## Clase 2

1. Cuales son los paises donde la empresa tiene tiendas? 


		SELECT 
			pais
		FROM 
			stg.store_master
		group by 
			pais


2. Cuantos productos por subcategoria tiene disponible para la venta? 


		SELECT 
			count(codigo_producto),
			subcategoria
		FROM 
			stg.product_master
		GROUP BY 
			subcategoria

3. Cuales son las ordenes de venta de Argentina de mayor a $100.000? 

		SELECT
			producto,
			sum(cantidad) as Cantidad
		FROM 
			stg.order_line_sale

4. Obtener los decuentos otorgados durante Noviembre de 2022 en cada una de las monedas?

		SELECT 
			sum(descuento),
			moneda
		FROM 
			stg.order_line_sale
		WHERE  
			fecha  BETWEEN '2022-11-01' AND '2022-11-30'
		GROUP BY
			moneda

5. Obtener los impuestos pagados en Europa durante el 2022. 

		SELECT 
			sum(descuento),
			moneda
		FROM 
			stg.order_line_sale
		WHERE  
			fecha  BETWEEN '2022-11-01' AND '2022-11-30'
		GROUP BY
			moneda

6. En cuantas ordenes se utilizaron creditos? 

		SELECT 
			count(creditos)
		FROM
			stg.order_line_sale
		WHERE
			creditos is not null
	
7. Cual es el % de descuentos otorgados (sobre las ventas) por tienda?

		SELECT 
			tienda,
			(descuento/venta) as porcentaje_descuento
		FROM
			stg.order_line_sale
		GROUP BY 
			tienda,
			porcentaje_descuento

8. Cual es el inventario promedio por dia que tiene cada tienda?


		SELECT 
			tienda,
			count(sku)/count(fecha) as inventario_promedio_dia
		FROM
			stg.inventory
		GROUP BY 
			tienda

9. Obtener las ventas netas y el porcentaje de descuento otorgado por producto en Argentina. 

		SELECT
			producto,
			sum(venta) as ventas,
			sum(descuento)/sum(venta) as porcentaje_descuento
		FROM 
			stg.order_line_sale
		WHERE 
			moneda = 'ARS'
		GROUP BY 
			producto

10. Las tablas "market_count" y "super_store_count" representan dos sistemas distintos que usa la empresa para contar la cantidad de gente que ingresa a tienda, uno para las tiendas de Latinoamerica y otro para Europa. Obtener en una unica tabla, las entradas a tienda de ambos sistemas. 

		SELECT 
			tienda,cast(fecha as char),conteo
		FROM
			stg.market_count
		UNION ALL
		SELECT 
			tienda,fecha,conteo
		FROM
			stg.super_store_count
	
	
11. Cuales son los productos disponibles para la venta (activos) de la marca Phillips? 

		SELECT *
		FROM
			stg.product_master
		WHERE
			nombre LIKE '%PHILIPS%' 	
		AND 
			is_active is true

12. Obtener el monto vendido por tienda y moneda y ordenarlo de mayor a menor por valor nominal.
		SELECT 
			tienda,
			sum(venta) as total_ventas
		FROM
			stg.order_line_sale
		GROUP BY
			tienda,
			moneda
		ORDER BY 
			total_ventas DESC 

13. Cual es el precio promedio de venta de cada producto en las distintas monedas? Recorda que los valores de venta, impuesto, descuentos y creditos es por el total de la linea.
	
		SELECT 
			moneda,
			producto,
			sum(venta)/count(producto)
		FROM
			stg.order_line_sale
		GROUP BY
			moneda,
			producto

14. Cual es la tasa de impuestos que se pago por cada orden de venta?
	
		
		SELECT 
			orden,
			sum(impuestos)/sum(venta) as tasa_impuesto
		FROM
			stg.order_line_sale
		GROUP BY
			orden

## Clase 3

1. Mostrar nombre y codigo de producto, categoria y color para todos los productos de la marca Philips y Samsung, mostrando la leyenda "Unknown" cuando no hay un color disponible

		select 
			nombre,
			codigo_producto,
			categoria,
			case when color is null then 'UNKNOWN'
			else color
			end as color
		from 
			stg.product_master
		where
			nombre like '%PHILIPS%'
	
2. Calcular las ventas brutas y los impuestos pagados por pais y provincia en la moneda correspondiente.
		
		select
			sm.pais,
			sm.provincia,
			ls.moneda,
			sum(venta),
			sum(impuestos)
		from 
			stg.order_line_sale ls
		left join 
			stg.store_master sm on sm.codigo_tienda=ls.tienda
		group by
			sm.pais,
			sm.provincia,
			ls.moneda

3. Calcular las ventas totales por subcategoria de producto para cada moneda ordenados por subcategoria y moneda.

		select
			pm.subcategoria,
			ls.moneda,
			sum(ls.venta)
		from
			stg.product_master pm
		left join
			stg.order_line_sale ls on pm.codigo_producto=ls.producto
		group by pm.subcategoria,
		 	ls.moneda
			
4. Calcular las unidades vendidas por subcategoria de producto y la concatenacion de pais, provincia; usar guion como separador y usarla para ordernar el resultado.
		
		select 
			pm.subcategoria,
			SUM(ls.cantidad) as cantidad,
		concat(sm.pais,'-',sm.provincia)as ubicacion
		from 
			stg.order_line_sale ls 
		left join
			stg.product_master pm  on ls.producto=pm.codigo_producto
		left join 
			stg.store_master sm on ls.tienda=sm.codigo_tienda
		group by pm.subcategoria,cantidad,ubicacion
		order by ubicacion 

5. Mostrar una vista donde sea vea el nombre de tienda y la cantidad de entradas de personas que hubo desde la fecha de apertura para el sistema "super_store". 

es con create view??

		select 
			tienda,
			sum(conteo)
		from 
			stg.super_store_count
		group by tienda

6. Cual es el nivel de inventario promedio en cada mes a nivel de codigo de producto y tienda; mostrar el resultado con el nombre de la tienda.



7. Calcular la cantidad de unidades vendidas por material. Para los productos que no tengan material usar 'Unknown', homogeneizar los textos si es necesario.

8. Mostrar la tabla order_line_sales agregando una columna que represente el valor de venta bruta en cada linea convertido a dolares usando la tabla de tipo de cambio.

9. Calcular cantidad de ventas totales de la empresa en dolares.

10. Mostrar en la tabla de ventas el margen de venta por cada linea. Siendo margen = (venta - promociones) - costo expresado en dolares.

11. Calcular la cantidad de items distintos de cada subsubcategoria que se llevan por numero de orden.



## Clase 4 
1. Crear un backup de la tabla product_master. Utilizar un esquema llamada "bkp" y agregar un prefijo al nombre de la tabla con la fecha del backup en forma de numero entero.
2. Hacer un update a la nueva tabla (creada en el punto anterior) de product_master agregando la leyendo "N/A" para los valores null de material y color. Pueden utilizarse dos sentencias.
3. Hacer un update a la tabla del punto anterior, actualizando la columa "is_active", desactivando todos los productos en la subsubcategoria "Control Remoto". 
4. Agregar una nueva columna a la tabla anterior llamada "is_local" indicando los productos producidos en Argentina y fuera de Argentina.
5. Agregar una nueva columna a la tabla de ventas llamada "line_key" que resulte ser la concatenacion de el numero de orden y el codigo de producto.  
6. Eliminar todos los valores de la tabla "order_line_sale" para el POS 1.
7. Crear una tabla llamada "employees" (por el momento vacia) que tenga un id (creado de forma incremental), nombre, apellido, fecha de entrada, fecha salida, telefono, pais, provincia, codigo_tienda, posicion. Decidir cual es el tipo de dato mas acorde.
8. Insertar nuevos valores a la tabla "employees" para los siguientes 4 empleados: 
	- Juan Perez, 2022-01-01, telefono +541113869867, Argentina, Santa Fe, tienda 2, Vendedor. 
	- Catalina Garcia, 2022-03-01, Argentina, Buenos Aires, tienda 2, Representante Comercial
	- Ana Valdez, desde 2020-02-21 hasta 2022-03-01, España, Madrid, tienda 8, Jefe Logistica
	- Fernando Moralez, 2022-04-04, España, Valencia, tienda 9, Vendedor.
9. Crear un backup de la tabla "cost" agregandole una columna que se llame "last_updated_ts" que sea el momento exacto en el cual estemos realizando el backup en formato datetime.
10. El cambio en la tabla "order_line_sale" en el punto 6 fue un error y debemos volver la tabla a su estado original, como lo harias?
