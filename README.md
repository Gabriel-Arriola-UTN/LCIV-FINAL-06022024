# LCIV-FINAL-06022024 -- API REST de Sorteo de Loteria

## Contexto del Negocio:
Es el encargado de diseñar un sistema para la carga de apuestas y control de premios a entregar a los ganadores según un determinado criterio de valoración.


## Enpoint a diseñar

### Registrar apuesta **(10 pts)**

- Endpoint: ```loteria/apuesta```
- Método: ```POST```
- Request:
  ```
  {
    "fecha_sorteo": "2024-12-25",
    "id_cliente": "Lorem Ipsum",
    "numero": "56789",
    "montoApostado": 100
  }  ```
- Respuesta:
  ```
  {
    "id_sorteo": 123,
    "fecha_sorteo": "2024-12-25",
    "id_cliente": "Lorem Ipsum",
    "numero": "9876",
    "resultado": "GANADOR"
  }
  ```

### Recuperar información del sorteo **(10 pts)**

- Endpoint: ```loteria/sorteo/{id_sorteo}```
- Método: ```GET```
- Respuesta:
  ```
  {
    "id_sorteo": 123,
    "fecha_sorteo": "2024-12-25",
    "apuestas": [
                "id_cliente": "Lorem Ipsum",
                "numero": "56789",
                "resultado": "GANADOR",
                "montoApostado": 100,
                "premio": 350000
               ],
    "totalEnReserva": 650100
  }
  ```

### Recuperar información general de apuestas ganadas **(10 pts)**

- Endpoint: ```loteria/total/{id_sorteo}```
- Método: ```GET```
- Si dado un determinado número de sorteo existen jugadores con apuestas ganadoras, 
mostrar los totales y persistir toda la informacion de la respuesta en la base de datos
- Respuesta:
  ```
  {
    "id_sorteo": 123,
    "fecha_sorteo": "2024-12-25",
    "totalDeApuestas": 100,
    "totalPagado": 350000,
    "totalEnReserva": 650100
  }
  ```

## Reglas del Negocio:

### Información de la API Externa:

La api externa debe ser levantada via docker con la siguiente imagen:

``` docker pull gabrielarriola/api-loteria ```

Y una vez levantada se puede acceder a su documentación en la url:

``` http://localhost:8080/docs#/ ```

Para llevar a cabo el registro y control de premios de manera efectiva,
su aplicación interactuará con una API externa que proporcionará información clave.
A continuación, se detalla la información que se consultará desde la API externa:

### Números sorteados por fecha:

Se deberá consultar los numeros ganadores del sorteo correspondiente a la fecha ingresada

- Endpoint: ```/sorteo/?fecha=2024-01-16```
- Método: ```GET```
- Respuesta:
 ```
{
  "id_sorteo": 123,
  "fecha_sorteo": "2023-01-16",
  "totalEnReserva": 1000000,
  "numerosSorteados":[
				    {1, 56789},
				    {2, 10130},
				    {3, 15081},
				    {4, 20442},
				    {5, 25973}
  ]
}
 ```
---
### Factor de calculo de premios

### Monto máximo de apuesta **(5 pts)**
- Las apuestas no podrán ser mayores al 1% del total en reserva del día del sorteo

### Premios a pagar segun cantidad de asiertos en terminación del numero **(10 pts)**
- 2 cifras: 700 %
- 3 cifras: 7000 %
- 4 cifras: 60000 %
- 5 cifras: 350000 %

### Estimar pozo base de sorteo (10 pts)
- El monto total del premio a entregar no podrá superar la reserva presente, si sucede este escenario 
aplicar un ajuste del 25% sobre el premio por cada 5 jugadores presentes en dicho sorteo.

### Total acumulado en reserva (5 pts)
- El monto acumulado por día es la resultante de la sumatoria de las apuestas más la reserva existente 
y la posterior quita de los premios otorgados

---
### Docker (10 pts)
- Crear un archivo Dockerfile para dockerizar el proyecto
- Crear un archivo docker-compose que permita levantar tanto este servicio como el servicio externo
---
### Testing **(30 pts)**
- Es obligatorio la creación de testing unitario sobre la lógica de negocio de todo el proyecto
