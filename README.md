A continuación, te presento los scripts SQL que proporcionaste, sin el formato Markdown.

1️⃣ Crear la base de datos y usarla

CREATE DATABASE IF NOT EXISTS tiendadb;
USE tiendadb;

2️⃣ Crear tablas

CREATE TABLE clientes (
id INT AUTO_INCREMENT PRIMARY KEY,
nombre VARCHAR(100) NOT NULL,
ciudad VARCHAR(50)
);

CREATE TABLE pedidos (
id INT AUTO_INCREMENT PRIMARY KEY,
cliente_id INT,
fecha DATE,
total DECIMAL(10,2),
FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);

CREATE TABLE productos (
id INT AUTO_INCREMENT PRIMARY KEY,
nombre VARCHAR(100),
categoria VARCHAR(50),
precio DECIMAL(10,2)
);

CREATE TABLE detalle_pedidos (
id INT AUTO_INCREMENT PRIMARY KEY,
pedido_id INT,
producto_id INT,
cantidad INT,
FOREIGN KEY (pedido_id) REFERENCES pedidos(id),
FOREIGN KEY (producto_id) REFERENCES productos(id)
);

3️⃣ Insertar datos de ejemplo

INSERT INTO clientes (nombre, ciudad) VALUES
('Ana Torres', 'Bogotá'),
('Luis Gómez', 'Medellín'),
('Marta López', 'Bogotá'),
('Juan Rodríguez', 'Cali');

INSERT INTO productos (nombre, categoria, precio) VALUES
('Laptop Lenovo', 'Electrónica', 3500000),
('Mouse Logitech', 'Electrónica', 80000),
('Silla Gamer', 'Muebles', 950000),
('Escritorio Oficina', 'Muebles', 650000),
('Auriculares Sony', 'Electrónica', 220000);

INSERT INTO pedidos (cliente_id, fecha, total) VALUES
(1, '2025-08-01', 3580000),
(1, '2025-08-10', 950000),
(2, '2025-08-05', 2280000),
(3, '2025-08-03', 650000),
(4, '2025-08-07', 80000);

INSERT INTO detalle_pedidos (pedido_id, producto_id, cantidad) VALUES
(1, 1, 1),
(1, 2, 1),
(2, 3, 1),
(3, 5, 1),
(3, 2, 1),
(4, 4, 1),
(5, 2, 1);

4️⃣ Consultas básicas

4.1 Mostrar todos los clientes y sus pedidos

SELECT c.nombre AS cliente, p.fecha, p.total
FROM clientes c
INNER JOIN pedidos p ON c.id = p.cliente_id;

4.2 Total gastado por cada cliente

SELECT c.nombre, SUM(p.total) AS total_gastado
FROM clientes c
INNER JOIN pedidos p ON c.id = p.cliente_id
GROUP BY c.nombre;

4.3 Top 3 clientes que más gastaron

SELECT c.nombre, SUM(p.total) AS total_gastado
FROM clientes c
INNER JOIN pedidos p ON c.id = p.cliente_id
GROUP BY c.nombre
ORDER BY total_gastado DESC
LIMIT 3;

4.4 Productos más vendidos

SELECT pr.nombre, SUM(dp.cantidad) AS total_vendidos
FROM productos pr
INNER JOIN detalle_pedidos dp ON pr.id = dp.producto_id
GROUP BY pr.nombre
ORDER BY total_vendidos DESC;

4.5 Ingresos por categoría de producto

SELECT pr.categoria, SUM(pr.precio * dp.cantidad) AS ingresos
FROM productos pr
INNER JOIN detalle_pedidos dp ON pr.id = dp.producto_id
GROUP BY pr.categoria
ORDER BY ingresos DESC;

4.6 Clientes sin pedidos

SELECT c.nombre
FROM clientes c
LEFT JOIN pedidos p ON c.id = p.cliente_id
WHERE p.id IS NULL;

4.7 Ventas por ciudad

SELECT c.ciudad, SUM(p.total) AS ventas_totales
FROM clientes c
INNER JOIN pedidos p ON c.id = p.cliente_id
GROUP BY c.ciudad;

4.8 Ventas mensuales

SELECT DATE_FORMAT(p.fecha, '%Y-%m') AS mes, SUM(p.total) AS total_mes
FROM pedidos p
GROUP BY mes
ORDER BY mes;
