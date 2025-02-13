# 🚀 Pazel API
### We see the problems as Pazel/Puzzle to be solved
Este repositorio contiene la documentación detallada de una API para la gestión de suscriptores, dispositivos y autenticación JWT.

This repository contains detailed documentation for an API that manages subscribers, devices, and JWT authentication.

Docker Pull:
📦 [Docker Package](https://github.com/users/abelperezr/packages/container/package/pazel)

Descarga / Download:
https://github.com/abelperezr/pazel-demo/blob/main/Pazel.postman_collection

---

## ⚠️ Requisitos de Compatibilidad / Compatibility Requirements

📌 **Solo funciona con dispositivos SROS de Nokia y autenticación con IPoE.**

📌 **Only works with Nokia SROS devices and IPoE authentication.**

---


## 📖 Documentación Adicional / Additional Documentation

📌 Para más información de la API, visita:
- `http://TU-IP:PUERTO/docs` (Swagger UI)
- `http://TU-IP:PUERTO/redoc` (ReDoc UI)

- `http://YOUR-IP:PORT/docs` (Swagger UI)
- `http://YOUR-IP:PORT/redoc` (ReDoc UI)

---

## 🔧 Prerequisitos / Prerequisites

Asegúrate de tener instalado / Make sure you have installed:
- **Docker** `20.10+`
- **MongoDB** (instancia local o en MongoDB Atlas)
- **Enable Netconf on SROS/Habilitar Netconf en SROS** (instancia local o en MongoDB Atlas)

---

## 🚀 Despliegue / Deployment

### 📍 Despliegue Local / Local Deployment
```sh
docker run -d \
  --network NOMBRE-DE-LA-RED-DOCKER \
  --ip IP-DEL-CONTENEDOR \
  --name pazel \
  -p 8013:8000 \
  -e MONGODB_URL="mongodb://IP-DE-TU-MONGO:27017" \
  -e MONGO_DB_NAME="TU_BD" \
  pazel:0.0.1
```

### ☁️ Integración con MongoDB Atlas / MongoDB Atlas Integration
```sh
docker run -d \
  --network NOMBRE-DE-LA-RED-DOCKER \
  --ip IP-DEL-CONTENEDOR \
  --name pazel \
  -p 8012:8000 \
  -e MONGODB_URL="mongodb+srv://ATLAS_USER:ATLAS_PASS@cluster.mongodb.net/pazel_db?retryWrites=true&w=majority" \
  -e MONGO_DB_NAME="TU_BD" \
  pazel:0.0.1
```

---

## ⚙️ WorkFlow

1️⃣ **Crear un usuario:** `POST /auth/register`
2️⃣ **Obtener un token:** `POST /auth/token`
3️⃣ **Agregar un dispositivo:** `POST /devices/`
4️⃣ **Asociar suscriptores al dispositivo:** `POST /subs/{device_id}`


1️⃣ **Create a user:** `POST /auth/register`
2️⃣ **Get a Token:** `POST /auth/token`
3️⃣ **Add a device:** `POST /devices/`
4️⃣ **Associate subscribers with the device:** `POST /subs/{device_id}`

---

## 📡 Endpoints

### 🔑 Autenticación JWT / JWT Authentication

#### 🔹 Registrar un usuario / Register a User
```http
POST /auth/register
```
**Cuerpo de la solicitud / Request Body:**
```json
{
  "username": "string",
  "email": "string",
  "password": "string"
}
```
**Respuestas / Responses:**
- `200 OK`: Usuario registrado correctamente.
- `422 Validation Error`: Error en la validación de la solicitud.

#### 🔹 Obtener Token de acceso / Get a Token
```http
POST /auth/token
```
**Cuerpo de la solicitud / Request Body:**
```json
{
  "grant_type": "password",
  "username": "string",
  "password": "string"
}
```
**Respuestas / Responses:**
- `200 OK`: Token generado.
- `422 Validation Error`: Error en la validación de la solicitud.

---

### 📟 Dispositivos / Devices

#### 🔹 Obtener dispositivos / Get Devices
```http
GET /devices/
```
**Parámetros / Parameters:**
- `id` *(string, query, opcional)*

**Respuestas / Responses:**
- `200 OK`: Lista de dispositivos.
- `422 Validation Error`: Error en la validación.

#### 🔹 Crear un dispositivo / Create a Device
```http
POST /devices/
```
**Cuerpo de la solicitud / Request Body:**
```json
{
  "id": "string",
  "dev_name": "string",
  "host": "string",
  "auth_username": "string",
  "auth_password": "string"
}
```
**Respuestas / Responses:**
- `201 Created`: Dispositivo creado correctamente.
- `422 Validation Error`: Error en la validación.

#### 🔹 Obtener un dispositivo específico / Get a specific device
```http
GET /devices/{id}
```
**Respuestas / Responses:**
- `200 OK`: Información del dispositivo.
- `422 Validation Error`: Error en la validación.

#### 🔹 Eliminar un dispositivo / Delete a Device
```http
DELETE /devices/{id}
```
**Respuestas / Responses:**
- `204 No Content`: Eliminación exitosa.
- `422 Validation Error`: Error en la validación.

---

### 🏠 Configuración de Hostname / Hostname Configuration

#### 🔹 Establecer hostname
```http
POST /hostname/{device_id}
```
**Cuerpo de la solicitud / Request Body:**
```json
{
  "hostname": "string"
}
```
**Respuestas / Responses:**
- `201 Created`: Hostname establecido correctamente.
- `422 Validation Error`: Error en la validación.

---

### 👥 Subscriptores / Subscribers

#### 🔹 Obtener subscriptores de un dispositivo / Get subscribers of a device
```http
GET /subs/{device_id}
```
**Respuestas / Responses:**
- `200 OK`: Lista de subscriptores.
- `422 Validation Error`: Error en la validación.

#### 🔹 Crear un subscriptor / Create a Subscriber
```http
POST /subs/{device_id}
```
**Cuerpo de la solicitud / Request Body:**
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
  "ipv6_pd": "string"
}
```
**Respuestas / Responses:**
- `201 Created`: Subscriptor creado correctamente.
- `422 Validation Error`: Error en la validación.

#### 🔹 Eliminar un subscriptor / Delete a Subscriber
```http
DELETE /subs/{device_id}/{name}
```
**Respuestas / Responses:**
- `204 No Content`: Eliminación exitosa.
- `422 Validation Error`: Error en la validación.

#### 🔹 Actualizar un subscriptor / Update a Subscriber
```http
PATCH /subs/{device_id}/{name}
```
**Cuerpo de la solicitud / Request Body:**
```json
{
  "state": "string", //optional
  "sla": "string"  //optional
}
```
**Respuestas / Responses:**
- `200 OK`: Subscriptor actualizado correctamente.
- `422 Validation Error`: Error en la validación.

---

📌 **Contribuciones:** Si deseas contribuir a este proyecto, ¡siéntete libre de abrir un PR o issue! 🚀
