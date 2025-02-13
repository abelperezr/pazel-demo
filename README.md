# Pazel

Este repositorio contiene la documentación detallada de una API para la gestión de suscriptores, dispositivos y autenticación JWT

This repository contains detailed documentation for an API that manages subscribers, devices, and JWT authentication

https://github.com/users/abelperezr/packages/container/package/pazel

## Prerequisites
- Docker 20.10+
- MongoDB instance (local or Atlas)

## How to deploy?

local deployment:

docker run -d `
  --network bng `
  --ip 10.64.1.31 `
  --name pazel `
  -p 8013:8000 `
  -e MONGODB_URL="mongodb://IP-DE-TU-MONGO:27017" `
  -e MONGO_DB_NAME="TU_BD" `
  pazel:0.0.1


  integration with MongodB Atlas:

  docker run -d `
  --network bng `
  --ip 10.64.1.30 `
  --name pazel `
  -p 8012:8000 `
    -e MONGODB_URL="mongodb+srv://TU-USUARIO:TU-PASSWORD@clusterxx......mongodb.net/?..........." `
  -e MONGO_DB_NAME="TU_BD" `
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

## Endpoints

### Autenticación JWT

#### Registrar un usuario

**POST** `/auth/register`

- **Cuerpo de la solicitud:**
```json
{
  "username": "string",
  "email": "string",
  "password": "string"
}
```

- **Respuesta:**
  - `200 OK`: Usuario registrado correctamente.
  - `404 Not Found`: No encontrado.
  - `422 Validation Error`: Error en la validación de la solicitud.

#### Obtener token de acceso

**POST** `/auth/token`

- **Cuerpo de la solicitud:**
  - `grant_type`: `password`
  - `username` *(string, requerido)*
  - `password` *(string, requerido)*

- **Respuesta:**
  - `200 OK`: Token de acceso.
  - `404 Not Found`: No encontrado.
  - `422 Validation Error`: Error en la validación de la solicitud.

---


### Dispositivos

#### Obtener dispositivos

**GET** `/devices/`

- **Parámetros:**
  - `id` *(string, query, opcional)*

- **Respuesta:**
  - `200 OK`: Lista de dispositivos.
  - `422 Validation Error`: Error en la validación de la solicitud.

#### Crear un dispositivo

**POST** `/devices/`

- **Cuerpo de la solicitud:**
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

- **Respuesta:**
  - `201 Created`: Dispositivo creado correctamente.
  - `422 Validation Error`: Error en la validación de la solicitud.


#### Obtener un dispositivo específico

**GET** `/devices/{id}`

- **Parámetros:**
  - `id` *(string, path)*

- **Respuesta:**
  - `200 OK`: Información del dispositivo.
  - `422 Validation Error`: Error en la validación de la solicitud.

#### Eliminar un dispositivo

**DELETE** `/devices/{id}`

- **Parámetros:**
  - `id` *(string, path)*

- **Respuesta:**
  - `204 No Content`: Eliminación exitosa.
  - `422 Validation Error`: Error en la validación de la solicitud.

### Configuración de Hostname

#### Establecer hostname

**POST** `/hostname/{device_id}`

- **Parámetros:**
  - `device_id` *(string, path)*

- **Cuerpo de la solicitud:**
```json
{
  "hostname": "string"
}
```

- **Respuesta:**
  - `201 Created`: Hostname establecido correctamente.
  - `422 Validation Error`: Error en la validación de la solicitud.



### Subscriptores

#### Obtener subscriptores de un dispositivo

**GET** `/subs/{device_id}`

- **Parámetros:**
  - `device_id` *(string, path)*: Identificador del dispositivo.
  - `limit` *(integer, query, opcional)*: Número máximo de subscriptores a retornar (valor por defecto: 20, mínimo: 1).

- **Respuesta:**
  - `200 OK`: Lista de subscriptores en formato JSON.
  - `422 Validation Error`: Error en la validación de parámetros.

#### Crear un subscriptor

**POST** `/subs/{device_id}`

- **Parámetros:**
  - `device_id` *(string, path)*: Identificador del dispositivo.
  
- **Cuerpo de la solicitud:**
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

#### Obtener un subscriptor específico

**GET** `/subs/{device_id}/{name}`

- **Parámetros:**
  - `device_id` *(string, path)*
  - `name` *(string, path)*

- **Respuesta:**
  - `200 OK`: Datos del subscriptor.
  - `422 Validation Error`: Error en la validación de la solicitud.

#### Eliminar un subscriptor

**DELETE** `/subs/{device_id}/{name}`

- **Parámetros:**
  - `device_id` *(string, path)*
  - `name` *(string, path)*

- **Respuesta:**
  - `204 No Content`: Eliminación exitosa.
  - `422 Validation Error`: Error en la validación de la solicitud.

#### Actualizar un subscriptor

**PATCH** `/subs/{device_id}/{name}`

- **Parámetros:**
  - `device_id` *(string, path)*
  - `name` *(string, path)*

- **Cuerpo de la solicitud:**
```json
{
  "state": "string",
  "sla": "string"
}
```

- **Respuesta:**
  - `200 OK`: Subscriptor actualizado correctamente.
  - `422 Validation Error`: Error en la validación de la solicitud.




