# Wiki Terraria

Aplicación de escritorio hecha en Java que funciona como una wiki del juego Terraria. Puedes consultar ítems, ver cómo se craftean o quién los dropea, guardar favoritos e iniciar sesión con tu cuenta.

---

## ¿De qué va el proyecto?

Es una wiki interactiva donde puedes buscar objetos de Terraria organizados por categorías (Armas, Materiales, Mesas, Objetos). Cada ítem tiene su ficha con descripción y método de obtención.

Cosas que puedes hacer:
- Iniciar sesión o registrarte
- Ver ítems por categoría y buscar por nombre
- Ver la receta de crafteo de un ítem o quién lo dropea
- Guardar ítems en favoritos
- Si eres admin, añadir ítems nuevos

Tecnologías usadas: Java 17, JavaFX (WebView), HTML/CSS y MySQL.

---

## Estructura del código

```
src/main/java/
├── app/
│   ├── MainApp.java        → Abre la ventana JavaFX
│   └── Controlador.java    → Conecta el HTML con Java
├── model/
│   ├── Item.java           → Clase base de los ítems
│   ├── Arma.java           → Extiende Item
│   ├── Ingrediente.java    → Material + cantidad
│   ├── Jefe.java           → Implementa FuenteDrop
│   ├── Enemigo.java        → Implementa FuenteDrop
│   ├── FuenteDrop.java     → Interfaz para Jefe y Enemigo
│   └── Usuario.java        → Datos del usuario
├── mysql/
│   ├── ItemDAO.java
│   ├── ArmaDAO.java
│   ├── UsuarioDAO.java
│   ├── JefeDAO.java
│   ├── EnemigoDAO.java
│   ├── IngredientesDAO.java
│   └── FavoritosDAO.java
└── util/
    └── DatabaseConnection.java  → Conexión a MySQL (Singleton)

src/main/resources/html/
├── login.html
├── menu.html
├── lista.html
├── ficha.html
└── favoritos.html
```

---

## Diagrama de clases

```mermaid
classDiagram
    class FuenteDrop {
        <<interface>>
        +getId() int
        +getNombre() String
        +getRutaImagen() String
    }
    class Jefe {
        -int idJefe
        -String nombre
        -String rutaImagen
    }
    class Enemigo {
        -int idEnemigo
        -String nombre
        -String rutaImagen
    }
    class Item {
        -int idItem
        -String nombre
        -String tipo
        -boolean esCrafteable
        -List ingredientes
        -List fuentes
    }
    class Arma {
        -String clase
        -int dano
    }
    class Usuario {
        -String email
        -boolean esAdmin
        -List favoritos
    }
    class Ingrediente {
        -Item item
        -int cantidad
    }

    FuenteDrop <|.. Jefe
    FuenteDrop <|.. Enemigo
    Item <|-- Arma
    Item o-- Ingrediente
    Item o-- FuenteDrop
    Usuario o-- Item
```

---

## Diagrama Entidad-Relación

```mermaid
erDiagram
    usuarios {
        varchar email PK
        varchar nombre
        varchar contrasena
        int es_admin
    }
    items_terraria {
        int id_item PK
        varchar nombre
        varchar tipo
        int es_crafteable
        int id_estacion FK
    }
    armas {
        int id_item PK
        varchar clase
        int dano
    }
    ingredientes_receta {
        int id_item FK
        int id_item_material FK
        int cantidad
    }
    jefes {
        int id_jefe PK
        varchar nombre
    }
    enemigos {
        int id_enemigo PK
        varchar nombre
    }
    favoritos {
        varchar email_usuario FK
        int id_item FK
    }

    usuarios ||--o{ favoritos : "guarda"
    items_terraria ||--o{ favoritos : "guardado en"
    items_terraria ||--o| armas : "es"
    items_terraria ||--o{ ingredientes_receta : "necesita"
    jefes ||--o{ items_terraria : "dropea"
    enemigos ||--o{ items_terraria : "dropea"
```

---

## Manual de uso

**Login**
Abre la app, escribe tu email y contraseña y pulsa Entrar.


**Menú principal**
Elige una categoría para ver los ítems de ese tipo.


**Ficha de ítem**
Pulsa un ítem para ver su descripción. Si se craftea verás los ingredientes y la mesa necesaria. Si se dropea verás quién lo suelta.

**Favoritos**
Pulsa "Guardar objeto" en cualquier ficha para guardarlo. Accede a tu lista desde el menú.


