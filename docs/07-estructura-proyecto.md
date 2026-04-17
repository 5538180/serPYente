# 07. Estructura del proyecto Laravel

## 1. Carpeta `app/`
Aquí vive el código principal de la aplicación.

### Partes clave
- `Models/`: modelos Eloquent
- `Http/Controllers/`: controladores
- `Http/Requests/`: validaciones
- `Http/Resources/`: recursos JSON
- `Services/`: lógica de negocio extraída

## 2. Carpeta `database/`
Contiene todo lo relacionado con la base de datos.

- `migrations/`: estructura de tablas
- `seeders/`: datos de prueba
- `factories/`: generación automática de datos falsos

## 3. Carpeta `routes/`
Define las rutas de la aplicación.

- `web.php`: rutas web con Blade
- `api.php`: rutas API
- `auth.php`: rutas de autenticación si usas Breeze

## 4. Carpeta `resources/`
Contiene la parte visual.

- `views/`: vistas Blade
- `css/`
- `js/`

## 5. Carpeta `tests/`
Contiene pruebas.

- `Feature/`: pruebas de rutas, controladores y flujos
- `Unit/`: pruebas de lógica concreta

## 6. Archivos importantes
### `.env`
Configuración local del proyecto.

### `composer.json`
Dependencias PHP.

### `package.json`
Dependencias JavaScript.

### `artisan`
Entrada de comandos de Laravel.

## 7. Qué debes recordar para examen
- modelo: `app/Models`
- controlador: `app/Http/Controllers`
- rutas: `routes/`
- migraciones: `database/migrations`
- seeders: `database/seeders`
- vistas: `resources/views`
