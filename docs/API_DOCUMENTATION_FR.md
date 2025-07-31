# Documentation de l'API

## Table des Matières
1. [Authentification](#authentification)
2. [Points de terminaison utilisateur](#points-de-terminaison-utilisateur)
3. [Points de terminaison produit](#points-de-terminaison-produit)
4. [Points de terminaison activité](#points-de-terminaison-activité)
5. [Points de terminaison facture](#points-de-terminaison-facture)
6. [Modèles de données](#modèles-de-données)
7. [Réponses d'erreur](#réponses-d-erreur)

## Authentification

Tous les points de terminaison (à l'exception de l'enregistrement et de la connexion des utilisateurs) nécessitent une authentification à l'aide d'un jeton Bearer dans l'en-tête Authorization :

```
Authorization: Bearer <token>
```

Pour obtenir un jeton, utilisez le point de terminaison de connexion.

## Points de terminaison utilisateur

### Enregistrer un utilisateur
Crée un nouvel utilisateur pour une entreprise spécifique.

- **URL** : `/users/registro/:ruc`
- **Méthode** : `POST`
- **Paramètres d'URL** : 
  - `ruc` (string) : Le RUC (identification de l'entreprise) de l'entreprise
- **Paramètres de données** : 
  - `nombre` (string, optionnel) : Nom de l'utilisateur
  - `email` (string, requis) : Email de l'utilisateur
  - `password_hash` (string, requis) : Mot de passe de l'utilisateur (haché)
  - `rol` (string, optionnel) : Rôle de l'utilisateur
  - `usuario` (string, requis) : Nom d'utilisateur
  - `cedula` (string, requis) : Numéro d'identification de l'utilisateur
- **Réponse réussie** :
  - Code : 201 CRÉÉ
  - Contenu : `{"mensaje": "Usuario creado correctamente"}`
- **Réponse d'erreur** :
  - Code : 400 REQUÊTE INCORRECTE
  - Contenu : `{"error": "Empresa no encontrada"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "Error creating user"}`

### Connexion de l'utilisateur
Authentifie un utilisateur et renvoie un jeton.

- **URL** : `/users/login`
- **Méthode** : `POST`
- **Paramètres de données** : 
  - `username` (string) : Nom d'utilisateur
  - `password` (string) : Mot de passe
- **Réponse réussie** :
  - Code : 200 OK
  - Contenu : `{"token": "<token>"}`
- **Réponse d'erreur** :
  - Code : 401 NON AUTORISÉ
  - Contenu : `{"error": "Credenciales inválidas"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "Error interno"}`

### Obtenir un utilisateur par nom
Récupère un utilisateur par son nom.

- **URL** : `/users/nombre/:name`
- **Méthode** : `GET`
- **Paramètres d'URL** : 
  - `name` (string) : Le nom de l'utilisateur à récupérer
- **En-têtes** : 
  - `Authorization: Bearer <token>`
- **Réponse réussie** :
  - Code : 200 OK
  - Contenu : Objet utilisateur
- **Réponse d'erreur** :
  - Code : 404 NON TROUVÉ
  - Contenu : `{"error": "Usuario no encontrado"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "No se pudo obtener conexión a la base de datos"}`

### Obtenir les utilisateurs de l'entreprise
Récupère tous les utilisateurs de l'entreprise de l'utilisateur authentifié.

- **URL** : `/users/mi-empresa`
- **Méthode** : `GET`
- **En-têtes** : 
  - `Authorization: Bearer <token>`
- **Réponse réussie** :
  - Code : 200 OK
  - Contenu : Tableau d'objets utilisateur
- **Réponse d'erreur** :
  - Code : 400 REQUÊTE INCORRECTE
  - Contenu : `{"error": "Token no encontrado"}`
  - Code : 404 NON TROUVÉ
  - Contenu : `{"error": "Usuario no encontrado"}` ou `{"error": "No se encontraron usuarios para esta empresa"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "No se pudo obtener conexión a la base de datos"}`

### Mettre à jour l'utilisateur
Met à jour les informations de l'utilisateur authentifié.

- **URL** : `/users/:id`
- **Méthode** : `PUT`
- **En-têtes** : 
  - `Authorization: Bearer <token>`
- **Paramètres de données** : 
  - `nombre` (string, optionnel) : Nom de l'utilisateur
  - `password_hash` (string, requis) : Mot de passe de l'utilisateur (haché)
  - `rol` (string, optionnel) : Rôle de l'utilisateur
  - `usuario` (string, requis) : Nom d'utilisateur
  - `cedula` (string, requis) : Numéro d'identification de l'utilisateur
- **Réponse réussie** :
  - Code : 201 CRÉÉ
  - Contenu : `{"mensaje": "Usuario actualizado correctamente"}`
- **Réponse d'erreur** :
  - Code : 400 REQUÊTE INCORRECTE
  - Contenu : `{"error": "Token no encontrado"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "No se pudo obtener conexión a la base de datos"}` ou `{"error": "Error actualizando usuario"}`

### Supprimer l'utilisateur
Supprime un utilisateur par ID.

- **URL** : `/users/:id`
- **Méthode** : `DELETE`
- **Paramètres d'URL** : 
  - `id` (integer) : L'ID de l'utilisateur à supprimer
- **En-têtes** : 
  - `Authorization: Bearer <token>`
- **Réponse réussie** :
  - Code : 200 OK
  - Contenu : `{"mensaje": "Usuario eliminado correctamente"}`
- **Réponse d'erreur** :
  - Code : 400 REQUÊTE INCORRECTE
  - Contenu : `{"error": "token no encontrado"}` ou `{"error": "Usuario no encontrado"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "No se pudo obtener conexión a la base de datos"}` ou `{"error": "Error al eliminar usuario"}`

## Points de terminaison produit

### Ajouter un produit
Ajoute un nouveau produit à l'entreprise de l'utilisateur authentifié.

- **URL** : `/productos/agregar-producto/`
- **Méthode** : `POST`
- **En-têtes** : 
  - `Authorization: Bearer <token>`
- **Paramètres de données** : 
  - `nombre` (string, optionnel) : Nom du produit
  - `descripcion` (string, optionnel) : Description du produit
  - `precio_venta` (number, optionnel) : Prix de vente
  - `stock_actual` (integer, optionnel) : Stock actuel
  - `stock_minimo` (integer, optionnel) : Stock minimum
  - `categoria` (string, optionnel) : Catégorie du produit
- **Réponse réussie** :
  - Code : 201 CRÉÉ
  - Contenu : `{"mensaje": "Producto creado correctamente"}`
- **Réponse d'erreur** :
  - Code : 400 REQUÊTE INCORRECTE
  - Contenu : `{"error": "token no encontrado"}` ou `{"error": "Usuario no encontrado"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "No se pudo obtener conexión a la base de datos"}` ou `{"error": "Error creando producto"}`

### Lister les produits
Récupère tous les produits de l'entreprise de l'utilisateur authentifié.

- **URL** : `/productos`
- **Méthode** : `GET`
- **En-têtes** : 
  - `Authorization: Bearer <token>`
- **Réponse réussie** :
  - Code : 200 OK
  - Contenu : Tableau d'objets produit
- **Réponse d'erreur** :
  - Code : 400 REQUÊTE INCORRECTE
  - Contenu : `{"error": "Token no encontrado"}`
  - Code : 404 NON TROUVÉ
  - Contenu : `{"error": "Usuario no encontrado"}` ou `{"error": "No se encontraron productos para esta empresa"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "No se pudo obtener conexión a la base de datos"}`

### Obtenir un produit par ID
Récupère un produit spécifique par son ID.

- **URL** : `/productos/:id`
- **Méthode** : `GET`
- **Paramètres d'URL** : 
  - `id` (integer) : L'ID du produit à récupérer
- **En-têtes** : 
  - `Authorization: Bearer <token>`
- **Réponse réussie** :
  - Code : 200 OK
  - Contenu : Objet produit
- **Réponse d'erreur** :
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "No se pudo obtener conexión a la base de datos"}` ou `{"error": "Error mostrando producto"}`

### Mettre à jour le produit
Met à jour un produit spécifique par son ID.

- **URL** : `/productos/:id`
- **Méthode** : `PUT`
- **Paramètres d'URL** : 
  - `id` (integer) : L'ID du produit à mettre à jour
- **En-têtes** : 
  - `Authorization: Bearer <token>`
- **Paramètres de données** : 
  - `nombre` (string, optionnel) : Nom du produit
  - `descripcion` (string, optionnel) : Description du produit
  - `precio_venta` (number, optionnel) : Prix de vente
  - `stock_actual` (integer, optionnel) : Stock actuel
  - `stock_minimo` (integer, optionnel) : Stock minimum
  - `categoria` (string, optionnel) : Catégorie du produit
- **Réponse réussie** :
  - Code : 201 CRÉÉ
  - Contenu : `{"mensaje": "Producto actualizado correctamente"}`
- **Réponse d'erreur** :
  - Code : 400 REQUÊTE INCORRECTE
  - Contenu : `{"error": "token no encontrado"}` ou `{"error": "Usuario no encontrado"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "No se pudo obtener conexión a la base de datos"}` ou `{"error": "Error actualizando producto"}`

### Supprimer le produit
Supprime un produit spécifique par son ID.

- **URL** : `/productos/:id`
- **Méthode** : `DELETE`
- **Paramètres d'URL** : 
  - `id` (integer) : L'ID du produit à supprimer
- **En-têtes** : 
  - `Authorization: Bearer <token>`
- **Réponse réussie** :
  - Code : 201 CRÉÉ
  - Contenu : `{"mensaje": "Producto eliminado correctamente"}`
- **Réponse d'erreur** :
  - Code : 400 REQUÊTE INCORRECTE
  - Contenu : `{"error": "token no encontrado"}` ou `{"error": "Usuario no encontrado"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "No se pudo obtener conexión a la base de datos"}` ou `{"error": "Error eliminando producto"}`

## Points de terminaison activité

### Obtenir l'activité récente
Récupère l'activité récente pour l'entreprise de l'utilisateur authentifié.

- **URL** : `/actividades/reciente`
- **Méthode** : `GET`
- **En-têtes** : 
  - `Authorization: Bearer <token>`
- **Réponse réussie** :
  - Code : 200 OK
  - Contenu : Tableau d'objets activité
- **Réponse d'erreur** :
  - Code : 400 REQUÊTE INCORRECTE
  - Contenu : `{"error": "Token no encontrado"}`
  - Code : 404 NON TROUVÉ
  - Contenu : `{"error": "Usuario no encontrado"}` ou `{"error": "No se encontraron usuarios para esta empresa"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "No se pudo obtener conexión a la base de datos"}`

## Points de terminaison facture

### Créer une facture
Crée une nouvelle facture pour l'entreprise de l'utilisateur authentifié.

- **URL** : `/facturas`
- **Méthode** : `POST`
- **En-têtes** : 
  - `Authorization: Bearer <token>`
- **Paramètres de données** : 
  - `empresa_id` (integer, optionnel) : ID de l'entreprise
  - `usuario_id` (integer, optionnel) : ID de l'utilisateur
  - `cliente_nombre` (string, optionnel) : Nom du client
  - `fecha` (datetime, optionnel) : Date de la facture
  - `total` (number, optionnel) : Total de la facture
  - `cliente_telefono` (string, optionnel) : Téléphone du client
  - `cliente_correo` (string, optionnel) : Email du client
  - `empresa_nombre` (string, optionnel) : Nom de l'entreprise
  - `nombre` (string, optionnel) : Nom de la facture
- **Réponse réussie** :
  - Code : 201 CRÉÉ
  - Contenu : `{"mensaje": "Factura creada correctamente"}`
- **Réponse d'erreur** :
  - Code : 400 REQUÊTE INCORRECTE
  - Contenu : `{"error": "token no encontrado"}` ou `{"error": "Empresa no encontrada"}` ou `{"error": "Usuario no encontrado"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "No se pudo obtener conexión a la base de datos"}` ou `{"error": "Error creating factura"}`

### Obtenir les factures
Récupère toutes les factures pour l'entreprise de l'utilisateur authentifié.

- **URL** : `/facturas`
- **Méthode** : `GET`
- **En-têtes** : 
  - `Authorization: Bearer <token>`
- **Réponse réussie** :
  - Code : 200 OK
  - Contenu : Tableau d'objets facture
- **Réponse d'erreur** :
  - Code : 400 REQUÊTE INCORRECTE
  - Contenu : `{"error": "Token no encontrado"}`
  - Code : 404 NON TROUVÉ
  - Contenu : `{"error": "Usuario no encontrado"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "No se pudo obtener conexión a la base de datos"}` ou `{"error": "Error al obtener las facturas"}`

### Obtenir le détail de la facture
Récupère les détails d'une facture spécifique par son ID.

- **URL** : `/facturas/detalle/:id`
- **Méthode** : `GET`
- **Paramètres d'URL** : 
  - `id` (integer) : L'ID de la facture à récupérer
- **En-têtes** : 
  - `Authorization: Bearer <token>`
- **Réponse réussie** :
  - Code : 200 OK
  - Contenu : Objet facture
- **Réponse d'erreur** :
  - Code : 400 REQUÊTE INCORRECTE
  - Contenu : `{"error": "Token no encontrado"}`
  - Code : 404 NON TROUVÉ
  - Contenu : `{"error": "Usuario no encontrado"}` ou `{"error": "Factura no encontrada"}`
  - Code : 500 ERREUR INTERNE DU SERVEUR
  - Contenu : `{"error": "No se pudo obtener conexión a la base de datos"}`

## Modèles de données

### Utilisateur
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

### Produit
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

### Facture
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

### Activité
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

## Réponses d'erreur

L'API utilise des codes d'état HTTP standard pour indiquer le succès ou l'échec des requêtes :

- `200 OK` : La requête a réussi
- `201 CRÉÉ` : La ressource a été créée avec succès
- `400 REQUÊTE INCORRECTE` : La requête était invalide ou ne peut pas être servie
- `401 NON AUTORISÉ` : L'authentification est requise ou a échoué
- `404 NON TROUVÉ` : La ressource demandée n'a pas pu être trouvée
- `500 ERREUR INTERNE DU SERVEUR` : Une erreur s'est produite sur le serveur

Toutes les réponses d'erreur suivent ce format :
```json
{
  "error": "Message d'erreur"
}