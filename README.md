--176412 Antonio de Jesus Morales Quiroz.
--Libros:
CREATE TABLE libros (
    id_libro SERIAL PRIMARY KEY,
    titulo VARCHAR(50),
   autor VARCHAR(50),
    anio_publicacion INT
);
--Prestamos:

CREATE TABLE prestamos (
    id_prestamo SERIAL PRIMARY KEY,
    id_libro INT,
   FOREIGN KEY (id_libro)  REFERENCES libros(id_libro),
   id_usuario INT,
    fecha_prestamo DATE,
    fecha_devolucion DATE
);

--Usuarios:

CREATE TABLE Usuarios (
    id_usuario SERIAL PRIMARY KEY,
   nombre VARCHAR(50),
    email VARCHAR(50)
);


-- Insertar datos de prueba
INSERT INTO libros (titulo, autor, anio_publicacion) VALUES
( 'Madame Bovary', 'Gustave Flaubert', 1857),
( 'La educación sentimental', 'Gustave Flaubert', 1869),
( 'Romancero gitano', 'Federico García Lorca', 	1928),
('Cien años de soledad', '	Gabriel García Márquez', 1968);

INSERT INTO prestamos ( id_libro, id_usuario, fecha_prestamo, fecha_devolucion) VALUES
( 1, 1, '2025-02-01', NULL),
( 2, 2, '2025-01-20', '2025-01-30'),
( 4, 2, '2025-01-20',   NULL),
( 3, 1, '2025-01-20', NULL);

-- Insertar datos de usuarios
INSERT INTO usuarios (nombre, email) VALUES
('Antonio', 'Antonio@example.com'),
('Jesus', 'Jesus@example.com');

-- Consulta para obtener los libros prestados actualmente
SELECT libros.titulo, usuarios.nombre, prestamos.fecha_prestamo
FROM prestamos
JOIN libros ON prestamos.id_libro = libros.id_libro
JOIN usuarios ON prestamos.id_usuario = usuarios.id_usuario
WHERE prestamos.fecha_devolucion IS NULL;




-- Consulta para listar los usuarios que han prestado libros en el último mes
SELECT DISTINCT usuarios.nombre, usuarios.email
FROM prestamos
JOIN usuarios ON prestamos.id_usuario = usuarios.id_usuario
WHERE prestamos.fecha_prestamo >= CURRENT_DATE - INTERVAL '1 month';

-- Consulta para encontrar los libros que no han sido prestados nunca
SELECT libros.titulo, libros.autor
FROM libros
LEFT JOIN prestamos ON libros.id_libro = prestamos.id_libro
WHERE prestamos.id_libro IS NULL;

-- Consulta para contar el número de préstamos por usuario
SELECT usuarios.nombre, COUNT(prestamos.id_prestamo) AS total_prestamos
FROM usuarios
LEFT JOIN prestamos ON usuarios.id_usuario = prestamos.id_usuario
GROUP BY usuarios.id_usuario, usuarios.nombre
ORDER BY total_prestamos DESC;

-- Join (⨝):
SELECT *
FROM libros
JOIN prestamos ON id_libro = id_libro;
