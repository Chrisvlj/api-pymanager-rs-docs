# Documentación de la API

## Tabla de Contenidos
1. [Autenticación](#autenticación)
2. [Endpoints de Usuario](#endpoints-de-usuario)
3. [Endpoints de Producto](#endpoints-de-producto)
4. [Endpoints de Actividad](#endpoints-de-actividad)
5. [Endpoints de Factura](#endpoints-de-factura)
6. [Modelos de Datos](#modelos-de-datos)
7. [Respuestas de Error](#respuestas-de-error)

## Autenticación

Todos los endpoints (excepto el registro y el inicio de sesión de usuarios) requieren autenticación usando un token Bearer en el encabezado de Autorización:

```
Authorization: Bearer <token>
```

Para obtener un token, use el endpoint de inicio de sesión.

## Endpoints de Usuario

### Registrar Usuario
Crea un nuevo usuario para una empresa específica.

- **URL**: `/users/registro/:ruc`
- **Método**: `POST`
- **Parámetros de URL**: 
  - `ruc` (string): El RUC (identificación de empresa) de la empresa
- **Parámetros de Datos**: 
  - `nombre` (string, opcional): Nombre del usuario
  - `email` (string, requerido): Correo electrónico del usuario
  - `password_hash` (string, requerido): Contraseña del usuario (hash)
  - `rol` (string, opcional): Rol del usuario
  - `usuario` (string, requerido): Nombre de usuario
  - `cedula` (string, requerido): Número de identificación del usuario
- **Respuesta Exitosa**:
  - Código: 201 CREADO
  - Contenido: `{"mensaje": "Usuario creado correctamente"}`
- **Respuesta de Error**:
  - Código: 400 SOLICITUD INCORRECTA
  - Contenido: `{"error": "Empresa no encontrada"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "Error creating user"}`

### Iniciar Sesión de Usuario
Autentica un usuario y devuelve un token.

- **URL**: `/users/login`
- **Método**: `POST`
- **Parámetros de Datos**: 
  - `username` (string): Nombre de usuario
  - `password` (string): Contraseña
- **Respuesta Exitosa**:
  - Código: 200 OK
  - Contenido: `{"token": "<token>"}`
- **Respuesta de Error**:
  - Código: 401 NO AUTORIZADO
  - Contenido: `{"error": "Credenciales inválidas"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "Error interno"}`

### Obtener Usuario por Nombre
Recupera un usuario por su nombre.

- **URL**: `/users/nombre/:name`
- **Método**: `GET`
- **Parámetros de URL**: 
  - `name` (string): El nombre del usuario a recuperar
- **Encabezados**: 
  - `Authorization: Bearer <token>`
- **Respuesta Exitosa**:
  - Código: 200 OK
  - Contenido: Objeto de usuario
- **Respuesta de Error**:
  - Código: 404 NO ENCONTRADO
  - Contenido: `{"error": "Usuario no encontrado"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "No se pudo obtener conexión a la base de datos"}`

### Obtener Usuarios de la Empresa
Recupera todos los usuarios de la empresa del usuario autenticado.

- **URL**: `/users/mi-empresa`
- **Método**: `GET`
- **Encabezados**: 
  - `Authorization: Bearer <token>`
- **Respuesta Exitosa**:
  - Código: 200 OK
  - Contenido: Array de objetos de usuario
- **Respuesta de Error**:
  - Código: 400 SOLICITUD INCORRECTA
  - Contenido: `{"error": "Token no encontrado"}`
  - Código: 404 NO ENCONTRADO
  - Contenido: `{"error": "Usuario no encontrado"}` o `{"error": "No se encontraron usuarios para esta empresa"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "No se pudo obtener conexión a la base de datos"}`

### Actualizar Usuario
Actualiza la información del usuario autenticado.

- **URL**: `/users/:id`
- **Método**: `PUT`
- **Encabezados**: 
  - `Authorization: Bearer <token>`
- **Parámetros de Datos**: 
  - `nombre` (string, opcional): Nombre del usuario
  - `password_hash` (string, requerido): Contraseña del usuario (hash)
  - `rol` (string, opcional): Rol del usuario
  - `usuario` (string, requerido): Nombre de usuario
  - `cedula` (string, requerido): Número de identificación del usuario
- **Respuesta Exitosa**:
  - Código: 201 CREADO
  - Contenido: `{"mensaje": "Usuario actualizado correctamente"}`
- **Respuesta de Error**:
  - Código: 400 SOLICITUD INCORRECTA
  - Contenido: `{"error": "Token no encontrado"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "No se pudo obtener conexión a la base de datos"}` o `{"error": "Error actualizando usuario"}`

### Eliminar Usuario
Elimina un usuario por ID.

- **URL**: `/users/:id`
- **Método**: `DELETE`
- **Parámetros de URL**: 
  - `id` (integer): El ID del usuario a eliminar
- **Encabezados**: 
  - `Authorization: Bearer <token>`
- **Respuesta Exitosa**:
  - Código: 200 OK
  - Contenido: `{"mensaje": "Usuario eliminado correctamente"}`
- **Respuesta de Error**:
  - Código: 400 SOLICITUD INCORRECTA
  - Contenido: `{"error": "token no encontrado"}` o `{"error": "Usuario no encontrado"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "No se pudo obtener conexión a la base de datos"}` o `{"error": "Error al eliminar usuario"}`

## Endpoints de Producto

### Agregar Producto
Agrega un nuevo producto a la empresa del usuario autenticado.

- **URL**: `/productos/agregar-producto/`
- **Método**: `POST`
- **Encabezados**: 
  - `Authorization: Bearer <token>`
- **Parámetros de Datos**: 
  - `nombre` (string, opcional): Nombre del producto
  - `descripcion` (string, opcional): Descripción del producto
  - `precio_venta` (number, opcional): Precio de venta
  - `stock_actual` (integer, opcional): Stock actual
  - `stock_minimo` (integer, opcional): Stock mínimo
  - `categoria` (string, opcional): Categoría del producto
- **Respuesta Exitosa**:
  - Código: 201 CREADO
  - Contenido: `{"mensaje": "Producto creado correctamente"}`
- **Respuesta de Error**:
  - Código: 400 SOLICITUD INCORRECTA
  - Contenido: `{"error": "token no encontrado"}` o `{"error": "Usuario no encontrado"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "No se pudo obtener conexión a la base de datos"}` o `{"error": "Error creando producto"}`

### Listar Productos
Recupera todos los productos de la empresa del usuario autenticado.

- **URL**: `/productos`
- **Método**: `GET`
- **Encabezados**: 
  - `Authorization: Bearer <token>`
- **Respuesta Exitosa**:
  - Código: 200 OK
  - Contenido: Array de objetos de producto
- **Respuesta de Error**:
  - Código: 400 SOLICITUD INCORRECTA
  - Contenido: `{"error": "Token no encontrado"}`
  - Código: 404 NO ENCONTRADO
  - Contenido: `{"error": "Usuario no encontrado"}` o `{"error": "No se encontraron productos para esta empresa"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "No se pudo obtener conexión a la base de datos"}`

### Obtener Producto por ID
Recupera un producto específico por su ID.

- **URL**: `/productos/:id`
- **Método**: `GET`
- **Parámetros de URL**: 
  - `id` (integer): El ID del producto a recuperar
- **Encabezados**: 
  - `Authorization: Bearer <token>`
- **Respuesta Exitosa**:
  - Código: 200 OK
  - Contenido: Objeto de producto
- **Respuesta de Error**:
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "No se pudo obtener conexión a la base de datos"}` o `{"error": "Error mostrando producto"}`

### Actualizar Producto
Actualiza un producto específico por su ID.

- **URL**: `/productos/:id`
- **Método**: `PUT`
- **Parámetros de URL**: 
  - `id` (integer): El ID del producto a actualizar
- **Encabezados**: 
  - `Authorization: Bearer <token>`
- **Parámetros de Datos**: 
  - `nombre` (string, opcional): Nombre del producto
  - `descripcion` (string, opcional): Descripción del producto
  - `precio_venta` (number, opcional): Precio de venta
  - `stock_actual` (integer, opcional): Stock actual
  - `stock_minimo` (integer, opcional): Stock mínimo
  - `categoria` (string, opcional): Categoría del producto
- **Respuesta Exitosa**:
  - Código: 201 CREADO
  - Contenido: `{"mensaje": "Producto actualizado correctamente"}`
- **Respuesta de Error**:
  - Código: 400 SOLICITUD INCORRECTA
  - Contenido: `{"error": "token no encontrado"}` o `{"error": "Usuario no encontrado"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "No se pudo obtener conexión a la base de datos"}` o `{"error": "Error actualizando producto"}`

### Eliminar Producto
Elimina un producto específico por su ID.

- **URL**: `/productos/:id`
- **Método**: `DELETE`
- **Parámetros de URL**: 
  - `id` (integer): El ID del producto a eliminar
- **Encabezados**: 
  - `Authorization: Bearer <token>`
- **Respuesta Exitosa**:
  - Código: 201 CREADO
  - Contenido: `{"mensaje": "Producto eliminado correctamente"}`
- **Respuesta de Error**:
  - Código: 400 SOLICITUD INCORRECTA
  - Contenido: `{"error": "token no encontrado"}` o `{"error": "Usuario no encontrado"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "No se pudo obtener conexión a la base de datos"}` o `{"error": "Error eliminando producto"}`

## Endpoints de Actividad

### Obtener Actividad Reciente
Recupera la actividad reciente para la empresa del usuario autenticado.

- **URL**: `/actividades/reciente`
- **Método**: `GET`
- **Encabezados**: 
  - `Authorization: Bearer <token>`
- **Respuesta Exitosa**:
  - Código: 200 OK
  - Contenido: Array de objetos de actividad
- **Respuesta de Error**:
  - Código: 400 SOLICITUD INCORRECTA
  - Contenido: `{"error": "Token no encontrado"}`
  - Código: 404 NO ENCONTRADO
  - Contenido: `{"error": "Usuario no encontrado"}` o `{"error": "No se encontraron usuarios para esta empresa"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "No se pudo obtener conexión a la base de datos"}`

## Endpoints de Factura

### Crear Factura
Crea una nueva factura para la empresa del usuario autenticado.

- **URL**: `/facturas`
- **Método**: `POST`
- **Encabezados**: 
  - `Authorization: Bearer <token>`
- **Parámetros de Datos**: 
  - `empresa_id` (integer, opcional): ID de empresa
  - `usuario_id` (integer, opcional): ID de usuario
  - `cliente_nombre` (string, opcional): Nombre del cliente
  - `fecha` (datetime, opcional): Fecha de la factura
  - `total` (number, opcional): Total de la factura
  - `cliente_telefono` (string, opcional): Teléfono del cliente
  - `cliente_correo` (string, opcional): Correo electrónico del cliente
  - `empresa_nombre` (string, opcional): Nombre de la empresa
  - `nombre` (string, opcional): Nombre de la factura
- **Respuesta Exitosa**:
  - Código: 201 CREADO
  - Contenido: `{"mensaje": "Factura creada correctamente"}`
- **Respuesta de Error**:
  - Código: 400 SOLICITUD INCORRECTA
  - Contenido: `{"error": "token no encontrado"}` o `{"error": "Empresa no encontrada"}` o `{"error": "Usuario no encontrado"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "No se pudo obtener conexión a la base de datos"}` o `{"error": "Error creating factura"}`

### Obtener Facturas
Recupera todas las facturas para la empresa del usuario autenticado.

- **URL**: `/facturas`
- **Método**: `GET`
- **Encabezados**: 
  - `Authorization: Bearer <token>`
- **Respuesta Exitosa**:
  - Código: 200 OK
  - Contenido: Array de objetos de factura
- **Respuesta de Error**:
  - Código: 400 SOLICITUD INCORRECTA
  - Contenido: `{"error": "Token no encontrado"}`
  - Código: 404 NO ENCONTRADO
  - Contenido: `{"error": "Usuario no encontrado"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "No se pudo obtener conexión a la base de datos"}` o `{"error": "Error al obtener las facturas"}`

### Obtener Detalle de Factura
Recupera los detalles de una factura específica por su ID.

- **URL**: `/facturas/detalle/:id`
- **Método**: `GET`
- **Parámetros de URL**: 
  - `id` (integer): El ID de la factura a recuperar
- **Encabezados**: 
  - `Authorization: Bearer <token>`
- **Respuesta Exitosa**:
  - Código: 200 OK
  - Contenido: Objeto de factura
- **Respuesta de Error**:
  - Código: 400 SOLICITUD INCORRECTA
  - Contenido: `{"error": "Token no encontrado"}`
  - Código: 404 NO ENCONTRADO
  - Contenido: `{"error": "Usuario no encontrado"}` o `{"error": "Factura no encontrada"}`
  - Código: 500 ERROR INTERNO DEL SERVIDOR
  - Contenido: `{"error": "No se pudo obtener conexión a la base de datos"}`

## Modelos de Datos

### Usuario
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

### Producto
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

### Factura
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

### Actividad
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

## Respuestas de Error

La API usa códigos de estado HTTP estándar para indicar el éxito o fracaso de las solicitudes:

- `200 OK`: La solicitud fue exitosa
- `201 CREADO`: El recurso fue creado exitosamente
- `400 SOLICITUD INCORRECTA`: La solicitud fue inválida o no puede ser servida
- `401 NO AUTORIZADO`: Se requiere autenticación o ha fallado
- `404 NO ENCONTRADO`: El recurso solicitado no pudo ser encontrado
- `500 ERROR INTERNO DEL SERVIDOR`: Ocurrió un error en el servidor

Todas las respuestas de error siguen este formato:
```json
{
  "error": "Mensaje de error"
}