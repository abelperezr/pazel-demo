# Pazel

Este repositorio contiene la documentación detallada de una API para la gestión de suscriptores, dispositivos y autenticación JWT

This repository contains detailed documentation for an API that manages subscribers, devices, and JWT authentication

https://github.com/users/abelperezr/packages/container/package/pazel

Para más información de la API : "TU-IP:PUERTO/docs" o  "TU-IP:PUERTO/redoc"
For more information about the API: "YOUR-IP:PORT/docs" or "YOUR-IP:PORT/redoc"

## Prerequisitos / Prerequisites
- Docker 20.10+
- MongoDB instance (local or Atlas)

## Como desplegar/  How to deploy?

### despliegue local / local deployment:

docker run -d `
  --network NOMBRE-DE-LA-RED-DOCKER/DOCKER-NETWORK `
  --ip IP-DEL-CONTENEDOR/CONTAINER-IP `
  --name pazel `
  -p 8013:8000 `
  -e MONGODB_URL="mongodb://IP-DE-TU-MONGO/MONGO-IP:27017" `
  -e MONGO_DB_NAME="TU_BD/YOUR-DB" `
  pazel:0.0.1


 ### Integración con MongodB Atlas / integration with MongodB Atlas:

  docker run -d `
  --network NOMBRE-DE-LA-RED-DOCKER/DOCKER-NETWORK `
  --ip IP-DEL-CONTENEDOR/CONTAINER-IP `
  --name pazel `
  -p 8012:8000 `
  -e MONGODB_URL="mongodb+srv://ATLAS_USER:ATLAS_PASS@cluster.mongodb.net/pazel_db?retryWrites=true&w=majority" \
  -e MONGO_DB_NAME="TU_BD/YOUR-DB" `
  pazel:0.0.1 `

## WorkFlow

Crea un usuario,  **POST** `/auth/register`
Obten un token,  **POST** `/auth/token`
Agrega un dispositivo , **POST** `/devices/`
Al dispositivo asocias los suscriptores **POST** `/subs/{device_id}`


Create a user, **POST** `/auth/register`
Get a token, **POST** `/auth/token`
Add a device, **POST** `/devices/`
Associate subscribers with the device **POST** `/subs/{device_id}`


---
## Endpoints

### Autenticación JWT / JWT Authentication

#### Registrar un usuario / Register an User

**POST** `/auth/register`

- **Cuerpo de la solicitud / Body:**
```json
{
  "username": "string",
  "email": "string",
  "password": "string"
}
```

- **Respuesta / Response:**
  - `200 OK`: Usuario registrado correctamente.
  - `404 Not Found`: No encontrado.
  - `422 Validation Error`: Error en la validación de la solicitud.

#### Obtener token de acceso / Get a Token

**POST** `/auth/token`

- **Cuerpo de la solicitud / Body:** 
  - `grant_type`: `password`
  - `username` *(string, requerido)*
  - `password` *(string, requerido)*

- **Respuesta / Response:**
  - `200 OK`: Token de acceso.
  - `404 Not Found`: No encontrado.
  - `422 Validation Error`: Error en la validación de la solicitud.

---


### Dispositivos / Devices

#### Obtener dispositivos / Get Devices

**GET** `/devices/`

- **Parámetros / Parameters:**
  - `id` *(string, query, opcional)*

- **Respuesta  / Response:**
  - `200 OK`: Lista de dispositivos.
  - `422 Validation Error`: Error en la validación de la solicitud.

#### Crear un dispositivo / Create a Device

**POST** `/devices/`

- **Cuerpo de la solicitud / Body:**
```json
{
  "id": "string",
  "dev_name": "string",
  "host": "string",
  "auth_username": "string",
  "auth_password": "string",
  "platform": "string"
}
```

- **Respuesta / Response:**
  - `201 Created`: Dispositivo creado correctamente.
  - `422 Validation Error`: Error en la validación de la solicitud.


#### Obtener un dispositivo específico / Get a specific device

**GET** `/devices/{id}`

- **Parámetros / Parameters:**
  - `id` *(string, path)*

- **Respuesta / Response:**
  - `200 OK`: Información del dispositivo.
  - `422 Validation Error`: Error en la validación de la solicitud.

#### Eliminar un dispositivo / Delete a Device

**DELETE** `/devices/{id}`

- **Parámetros / Parameters:**
  - `id` *(string, path)*

- **Respuesta / Response:**
  - `204 No Content`: Eliminación exitosa.
  - `422 Validation Error`: Error en la validación de la solicitud.

### Configuración de Hostname / Hostname Configuration

#### Establecer hostname

**POST** `/hostname/{device_id}`

- **Parámetros / Parameters:**
  - `device_id` *(string, path)*

- **Cuerpo de la solicitud / Body:**
```json
{
  "hostname": "string"
}
```

- **Respuesta / Response:**
  - `201 Created`: Hostname establecido correctamente.
  - `422 Validation Error`: Error en la validación de la solicitud.



### Subscriptores / Subscribers

#### Obtener subscriptores de un dispositivo / Get subscribers of a device

**GET** `/subs/{device_id}`

- **Parámetros / Parameters:**
  - `device_id` *(string, path)*: Identificador del dispositivo.
  - `limit` *(integer, query, opcional)*: Número máximo de subscriptores a retornar (valor por defecto: 20, mínimo: 1).

- **Respuesta / Response:**
  - `200 OK`: Lista de subscriptores en formato JSON.
  - `422 Validation Error`: Error en la validación de parámetros.

#### Crear un subscriptor / Create a subscribers

**POST** `/subs/{device_id}`

- **Parámetros / Parameters:**
  - `device_id` *(string, path)*: Identificador del dispositivo.
  
- **Cuerpo de la solicitud / Body:**
```json
{
  "id": "string",
  "name": "string",
  "state": "string",
  "mac": "string",
  "subprofile": "string",
  "sla": "string",
  "ipv4_pool": "string",
  "ipv6_pool": "string",
  "ipv6_pd": "string",
  "ipv4": "string",
  "ipv4_mask": "string",
  "default_router": "string",
  "ipv4_dns_1": "string",
  "ipv4_dns_2": "string",
  "ipv6_dns": "string",
  "ipv6": "string",
  "ipv6_prefix": "string",
  "ipv6_len": 0
}
```

- **Respuesta:**
  - `201 Created`: Subscriptor creado correctamente.
  - `422 Validation Error`: Error en la validación de la solicitud.

#### Obtener un subscriptor específico / Get a specific subscribers

**GET** `/subs/{device_id}/{name}`

- **Parámetros / Parameters:**
  - `device_id` *(string, path)*
  - `name` *(string, path)*

- **Respuesta / Response:**
  - `200 OK`: Datos del subscriptor.
  - `422 Validation Error`: Error en la validación de la solicitud.

#### Eliminar un subscriptor / Delete a subscriber

**DELETE** `/subs/{device_id}/{name}`

- **Parámetros / Parameters:**
  - `device_id` *(string, path)*
  - `name` *(string, path)*

- **Respuesta / Response:**
  - `204 No Content`: Eliminación exitosa.
  - `422 Validation Error`: Error en la validación de la solicitud.

#### Actualizar un subscriptor / Update a subscriber

**PATCH** `/subs/{device_id}/{name}`

- **Parámetros / Parameters:**
  - `device_id` *(string, path)*
  - `name` *(string, path)*

- **Cuerpo de la solicitud / Body:**
```json
{
  "state": "string",
  "sla": "string"
}
```

- **Respuesta / Response:**
  - `200 OK`: Subscriptor actualizado correctamente.
  - `422 Validation Error`: Error en la validación de la solicitud.




