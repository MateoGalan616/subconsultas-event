### Subconsultas event
## Generar dos sentencias con subsconsultas en la base de datos EVENT.
## Identificar claramente las sentencias que extraiga informacion valiosa para el contexto de la base de datos.
## Desarrollar los ejercicios en fomato markdow (descripción de la sentencia, código, captura).
```
CREATE TABLE member(
id SERIAL,
fullname VARCHAR (100) NOT NULL,
email VARCHAR (200) NOT NULL,
age INT NOT NULL,
PRIMARY KEY (id)
);


CREATE TABLE event(
id SERIAL,
start_date DATE NOT NULL,
end_date DATE NOT NULL,
city VARCHAR (50) NOT NULL,
PRIMARY KEY (id)
);

CREATE TABLE conference (
    id INT PRIMARY KEY,
    title VARCHAR(100),
    speaker VARCHAR(100),
    hour TIME,
    day DATE,
    total_attendees INT,
    event_id INT,
    FOREIGN KEY (event_id) REFERENCES event(id)
);

CREATE TABLE register (
    id INT PRIMARY KEY,
    member_id INT,
    conference_id INT,
    registered_at DATE,
    assisted BOOLEAN,
    FOREIGN KEY (member_id) REFERENCES member(id),
    FOREIGN KEY (conference_id) REFERENCES conference(id)
)

```

## Sentencias:
# Subconsulta en la cláusula WHERE para encontrar conferencias que tienen más asistentes que el promedio de asistentes de todas las conferencias
# realiza una consulta para seleccionar todas las conferencias que tienen un número de asistentes total (total_attendees) mayor que el promedio de asistentes de todas las conferencias.

# SELECT * FROM conference: Esta parte de la consulta selecciona todas las columnas de la tabla conference.

# WHERE total_attendees > (...): Aquí se especifica una condición para filtrar los resultados de la tabla conference. Se seleccionarán solo aquellas filas donde el valor de la columna total_attendees sea mayor que el valor devuelto por la subconsulta.

# (SELECT AVG(total_attendees) FROM conference): Esta es una subconsulta que calcula el promedio (AVG) de los valores de la columna total_attendees de todas las filas en la tabla conference. La subconsulta se ejecuta primero, y su resultado se utiliza en la cláusula WHERE de la consulta principal.
```
SELECT *
FROM conference
WHERE total_attendees > (
    SELECT AVG(total_attendees)
    FROM conference
);

```
## Captura: 

<img src="./Capturas/Conference.png"


# Subconsulta en la cláusula FROM para listar los eventos que tienen conferencias con más de 50 asistentes:
# realiza una consulta para seleccionar todos los eventos que tienen al menos una conferencia asociada con más de 50 asistentes.
# SELECT * FROM event e: Esta parte de la consulta selecciona todas las columnas de la tabla event y le asigna el alias e.

# WHERE EXISTS (...): La cláusula WHERE EXISTS se utiliza para comprobar la existencia de filas en la subconsulta. La consulta principal solo devolverá las filas de la tabla event para las que la subconsulta devuelva alguna fila.

# (SELECT 1 FROM conference c WHERE c.event_id = e.id AND c.total_attendees > 50): Esta es una subconsulta correlacionada que se ejecuta para cada fila de la tabla event. Aquí se selecciona un valor arbitrario (1 en este caso) de la tabla conference (con el alias c), donde se cumplen dos condiciones
```
SELECT *
FROM event e
WHERE EXISTS (
    SELECT 1
    FROM conference c
    WHERE c.event_id = e.id
      AND c.total_attendees > 50
);

```

## Captura: 

<img src="./Capturas/event.png" 


