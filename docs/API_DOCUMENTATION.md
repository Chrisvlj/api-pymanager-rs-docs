# API Documentation

## Table of Contents
1. [Authentication](#authentication)
2. [User Endpoints](#user-endpoints)
3. [Product Endpoints](#product-endpoints)
4. [Activity Endpoints](#activity-endpoints)
5. [Invoice Endpoints](#invoice-endpoints)
6. [Data Models](#data-models)
7. [Error Responses](#error-responses)

## Authentication

All endpoints (except user registration and login) require authentication using a Bearer token in the Authorization header:

```
Authorization: Bearer <token>
```

To obtain a token, use the login endpoint.

## User Endpoints

### Register User
Creates a new user for a specific company.

- **URL**: `/users/registro/:ruc`
- **Method**: `POST`
- **URL Parameters**: 
  - `ruc` (string): The RUC (company identification) of the company
- **Data Parameters**: 
  - `nombre` (string, optional): User's name
  - `email` (string, required): User's email
  - `password_hash` (string, required): User's password (hashed)
  - `rol` (string, optional): User's role
  - `usuario` (string, required): Username
  - `cedula` (string, required): User's identification number
- **Success Response**:
  - Code: 201 CREATED
  - Content: `{"mensaje": "Usuario creado correctamente"}`
- **Error Response**:
  - Code: 400 BAD REQUEST
  - Content: `{"error": "Empresa no encontrada"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "Error creating user"}`

### Login User
Authenticates a user and returns a token.

- **URL**: `/users/login`
- **Method**: `POST`
- **Data Parameters**: 
  - `username` (string): Username
  - `password` (string): Password
- **Success Response**:
  - Code: 200 OK
  - Content: `{"token": "<token>"}`
- **Error Response**:
  - Code: 401 UNAUTHORIZED
  - Content: `{"error": "Credenciales inválidas"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "Error interno"}`

### Get User by Name
Retrieves a user by their name.

- **URL**: `/users/nombre/:name`
- **Method**: `GET`
- **URL Parameters**: 
  - `name` (string): The name of the user to retrieve
- **Headers**: 
  - `Authorization: Bearer <token>`
- **Success Response**:
  - Code: 200 OK
  - Content: User object
- **Error Response**:
  - Code: 404 NOT FOUND
  - Content: `{"error": "Usuario no encontrado"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "No se pudo obtener conexión a la base de datos"}`

### Get Users from Company
Retrieves all users from the authenticated user's company.

- **URL**: `/users/mi-empresa`
- **Method**: `GET`
- **Headers**: 
  - `Authorization: Bearer <token>`
- **Success Response**:
  - Code: 200 OK
  - Content: Array of user objects
- **Error Response**:
  - Code: 400 BAD REQUEST
  - Content: `{"error": "Token no encontrado"}`
  - Code: 404 NOT FOUND
  - Content: `{"error": "Usuario no encontrado"}` or `{"error": "No se encontraron usuarios para esta empresa"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "No se pudo obtener conexión a la base de datos"}`

### Update User
Updates the authenticated user's information.

- **URL**: `/users/:id`
- **Method**: `PUT`
- **Headers**: 
  - `Authorization: Bearer <token>`
- **Data Parameters**: 
  - `nombre` (string, optional): User's name
  - `password_hash` (string, required): User's password (hashed)
  - `rol` (string, optional): User's role
  - `usuario` (string, required): Username
  - `cedula` (string, required): User's identification number
- **Success Response**:
  - Code: 201 CREATED
  - Content: `{"mensaje": "Usuario actualizado correctamente"}`
- **Error Response**:
  - Code: 400 BAD REQUEST
  - Content: `{"error": "Token no encontrado"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "No se pudo obtener conexión a la base de datos"}` or `{"error": "Error actualizando usuario"}`

### Delete User
Deletes a user by ID.

- **URL**: `/users/:id`
- **Method**: `DELETE`
- **URL Parameters**: 
  - `id` (integer): The ID of the user to delete
- **Headers**: 
  - `Authorization: Bearer <token>`
- **Success Response**:
  - Code: 200 OK
  - Content: `{"mensaje": "Usuario eliminado correctamente"}`
- **Error Response**:
  - Code: 400 BAD REQUEST
  - Content: `{"error": "token no encontrado"}` or `{"error": "Usuario no encontrado"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "No se pudo obtener conexión a la base de datos"}` or `{"error": "Error al eliminar usuario"}`

## Product Endpoints

### Add Product
Adds a new product to the authenticated user's company.

- **URL**: `/productos/agregar-producto/`
- **Method**: `POST`
- **Headers**: 
  - `Authorization: Bearer <token>`
- **Data Parameters**: 
  - `nombre` (string, optional): Product name
  - `descripcion` (string, optional): Product description
  - `precio_venta` (number, optional): Selling price
  - `stock_actual` (integer, optional): Current stock
  - `stock_minimo` (integer, optional): Minimum stock
  - `categoria` (string, optional): Product category
- **Success Response**:
  - Code: 201 CREATED
  - Content: `{"mensaje": "Producto creado correctamente"}`
- **Error Response**:
  - Code: 400 BAD REQUEST
  - Content: `{"error": "token no encontrado"}` or `{"error": "Usuario no encontrado"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "No se pudo obtener conexión a la base de datos"}` or `{"error": "Error creando producto"}`

### List Products
Retrieves all products from the authenticated user's company.

- **URL**: `/productos`
- **Method**: `GET`
- **Headers**: 
  - `Authorization: Bearer <token>`
- **Success Response**:
  - Code: 200 OK
  - Content: Array of product objects
- **Error Response**:
  - Code: 400 BAD REQUEST
  - Content: `{"error": "Token no encontrado"}`
  - Code: 404 NOT FOUND
  - Content: `{"error": "Usuario no encontrado"}` or `{"error": "No se encontraron productos para esta empresa"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "No se pudo obtener conexión a la base de datos"}`

### Get Product by ID
Retrieves a specific product by its ID.

- **URL**: `/productos/:id`
- **Method**: `GET`
- **URL Parameters**: 
  - `id` (integer): The ID of the product to retrieve
- **Headers**: 
  - `Authorization: Bearer <token>`
- **Success Response**:
  - Code: 200 OK
  - Content: Product object
- **Error Response**:
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "No se pudo obtener conexión a la base de datos"}` or `{"error": "Error mostrando producto"}`

### Update Product
Updates a specific product by its ID.

- **URL**: `/productos/:id`
- **Method**: `PUT`
- **URL Parameters**: 
  - `id` (integer): The ID of the product to update
- **Headers**: 
  - `Authorization: Bearer <token>`
- **Data Parameters**: 
  - `nombre` (string, optional): Product name
  - `descripcion` (string, optional): Product description
  - `precio_venta` (number, optional): Selling price
  - `stock_actual` (integer, optional): Current stock
  - `stock_minimo` (integer, optional): Minimum stock
  - `categoria` (string, optional): Product category
- **Success Response**:
  - Code: 201 CREATED
  - Content: `{"mensaje": "Producto actualizado correctamente"}`
- **Error Response**:
  - Code: 400 BAD REQUEST
  - Content: `{"error": "token no encontrado"}` or `{"error": "Usuario no encontrado"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "No se pudo obtener conexión a la base de datos"}` or `{"error": "Error actualizando producto"}`

### Delete Product
Deletes a specific product by its ID.

- **URL**: `/productos/:id`
- **Method**: `DELETE`
- **URL Parameters**: 
  - `id` (integer): The ID of the product to delete
- **Headers**: 
  - `Authorization: Bearer <token>`
- **Success Response**:
  - Code: 201 CREATED
  - Content: `{"mensaje": "Producto eliminado correctamente"}`
- **Error Response**:
  - Code: 400 BAD REQUEST
  - Content: `{"error": "token no encontrado"}` or `{"error": "Usuario no encontrado"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "No se pudo obtener conexión a la base de datos"}` or `{"error": "Error eliminando producto"}`

## Activity Endpoints

### Get Recent Activity
Retrieves recent activity for the authenticated user's company.

- **URL**: `/actividades/reciente`
- **Method**: `GET`
- **Headers**: 
  - `Authorization: Bearer <token>`
- **Success Response**:
  - Code: 200 OK
  - Content: Array of activity objects
- **Error Response**:
  - Code: 400 BAD REQUEST
  - Content: `{"error": "Token no encontrado"}`
  - Code: 404 NOT FOUND
  - Content: `{"error": "Usuario no encontrado"}` or `{"error": "No se encontraron usuarios para esta empresa"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "No se pudo obtener conexión a la base de datos"}`

## Invoice Endpoints

### Create Invoice
Creates a new invoice for the authenticated user's company.

- **URL**: `/facturas`
- **Method**: `POST`
- **Headers**: 
  - `Authorization: Bearer <token>`
- **Data Parameters**: 
  - `empresa_id` (integer, optional): Company ID
  - `usuario_id` (integer, optional): User ID
  - `cliente_nombre` (string, optional): Client name
  - `fecha` (datetime, optional): Invoice date
  - `total` (number, optional): Invoice total
  - `cliente_telefono` (string, optional): Client phone
  - `cliente_correo` (string, optional): Client email
  - `empresa_nombre` (string, optional): Company name
  - `nombre` (string, optional): Invoice name
- **Success Response**:
  - Code: 201 CREATED
  - Content: `{"mensaje": "Factura creada correctamente"}`
- **Error Response**:
  - Code: 400 BAD REQUEST
  - Content: `{"error": "token no encontrado"}` or `{"error": "Empresa no encontrada"}` or `{"error": "Usuario no encontrado"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "No se pudo obtener conexión a la base de datos"}` or `{"error": "Error creating factura"}`

### Get Invoices
Retrieves all invoices for the authenticated user's company.

- **URL**: `/facturas`
- **Method**: `GET`
- **Headers**: 
  - `Authorization: Bearer <token>`
- **Success Response**:
  - Code: 200 OK
  - Content: Array of invoice objects
- **Error Response**:
  - Code: 400 BAD REQUEST
  - Content: `{"error": "Token no encontrado"}`
  - Code: 404 NOT FOUND
  - Content: `{"error": "Usuario no encontrado"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "No se pudo obtener conexión a la base de datos"}` or `{"error": "Error al obtener las facturas"}`

### Get Invoice Detail
Retrieves details of a specific invoice by its ID.

- **URL**: `/facturas/detalle/:id`
- **Method**: `GET`
- **URL Parameters**: 
  - `id` (integer): The ID of the invoice to retrieve
- **Headers**: 
  - `Authorization: Bearer <token>`
- **Success Response**:
  - Code: 200 OK
  - Content: Invoice object
- **Error Response**:
  - Code: 400 BAD REQUEST
  - Content: `{"error": "Token no encontrado"}`
  - Code: 404 NOT FOUND
  - Content: `{"error": "Usuario no encontrado"}` or `{"error": "Factura no encontrada"}`
  - Code: 500 INTERNAL SERVER ERROR
  - Content: `{"error": "No se pudo obtener conexión a la base de datos"}`

## Data Models

### User
```rust
pub struct User {
    pub id: i32,
    pub nombre: Option<String>,
    pub email: String,
    pub password_hash: String,
    pub rol: Option<String>,
    pub usuario: String,
    pub cedula: String,
    pub empresa_id: i32,
}
```

### Product
```rust
pub struct Producto {
    pub id: i32,
    pub empresa_id: i32,
    pub nombre: Option<String>,
    pub descripcion: Option<String>,
    pub precio_venta: Option<bigdecimal::BigDecimal>,
    pub stock_actual: Option<i32>,
    pub stock_minimo: Option<i32>,
    pub categoria: Option<String>,
}
```

### Invoice
```rust
pub struct Factura {
    pub id: i32,
    pub empresa_id: i32,
    pub usuario_id: Option<i32>,
    pub cliente_nombre: Option<String>,
    pub cliente_identificacion: Option<String>,
    pub fecha: Option<chrono::NaiveDateTime>,
    pub total: Option<bigdecimal::BigDecimal>,
    pub cliente_telefono: Option<String>,
    pub cliente_correo: Option<String>,
    pub empresa_nombre: Option<String>,
    pub nombre: Option<String>,
}
```

### Activity
```rust
pub struct Auditoria {
    pub id: i32,
    pub usuario_id: i32,
    pub empresa_id: i32,
    pub accion: String,
    pub entidad: String,
    pub entidad_id: Option<i32>,
    pub descripcion: Option<String>,
    pub fecha: Option<chrono::NaiveDateTime>,
}
```

## Error Responses

The API uses standard HTTP status codes to indicate the success or failure of requests:

- `200 OK`: The request was successful
- `201 CREATED`: The resource was successfully created
- `400 BAD REQUEST`: The request was invalid or cannot be served
- `401 UNAUTHORIZED`: Authentication is required or has failed
- `404 NOT FOUND`: The requested resource could not be found
- `500 INTERNAL SERVER ERROR`: An error occurred on the server

All error responses follow this format:
```json
{
  "error": "Error message"
}