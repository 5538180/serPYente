# 09. Hoja de ruta por fases

## Fase 1. Preparar entorno
Objetivo: tener Debian listo para Laravel.

Debes instalar:
- PHP
- Composer
- Git
- Node.js y npm
- SQLite o MariaDB
- Laravel Installer

## Fase 2. Git y GitHub
Objetivo: controlar versiones desde el primer minuto.

Debes ser capaz de:
- crear repo local
- conectar con GitHub
- hacer commits útiles
- subir cambios

## Fase 3. Crear proyecto Laravel
Objetivo: levantar una app funcional.

Pasos:
1. `laravel new academia`
2. configurar `.env`
3. crear base de datos
4. `php artisan migrate`
5. `php artisan serve`

## Fase 4. Breeze y parte web base
Objetivo: disponer de login, layout y estructura visual inicial.

Aprendes:
- autenticación web
- vistas Blade
- layouts
- componentes Blade

## Fase 5. Diseñar el dominio
Objetivo: entender el Mermaid y traducirlo a tablas y modelos.

Aquí debes decidir:
- qué tablas existen
- qué claves foráneas tienen
- qué tablas pivote necesitas
- qué relaciones serán polimórficas

## Fase 6. Modelos y migraciones
Objetivo: construir la estructura de base de datos.

Haz primero relaciones simples:
- Usuario -> Perfil
- Curso -> Leccion
- Leccion -> Tarea

Luego relaciones más avanzadas:
- Curso <-> Alumno
- Taller <-> Alumno
- Foto polimórfica
- Comentario polimórfico
- Etiqueta polimórfica N:M

## Fase 7. Seeders y factories
Objetivo: poblar la base de datos sin meter datos a mano.

Aprendes a:
- crear datos mínimos reales
- probar relaciones
- usar `migrate:fresh --seed`

## Fase 8. CRUD web
Objetivo: construir controladores y rutas web.

Haz al menos:
- index
- show
- create
- store
- edit
- update

## Fase 9. API versionada
Objetivo: trabajar como en `backend-EAC`.

Debes usar:
- `routes/api.php`
- prefijo `/api/v1`
- controladores API
- respuestas JSON

## Fase 10. Requests, Resources y Services
Objetivo: limpiar y profesionalizar la app.

- `FormRequest`: validación
- `Resource`: salida JSON consistente
- `Service`: lógica de negocio más compleja

## Fase 11. Sanctum
Objetivo: proteger rutas API.

Aprendes:
- instalar Sanctum
- emitir tokens
- proteger rutas con `auth:sanctum`

## Fase 12. Swagger
Objetivo: documentar tu API.

Aprendes:
- instalar L5 Swagger
- generar documentación
- describir endpoints

## Fase 13. Tests
Objetivo: comprobar que la aplicación funciona de forma repetible.

Haz:
- tests Feature para endpoints
- tests Unit para lógica concreta

## Fase 14. Modo examen
Objetivo: saber por dónde empezar bajo presión.

Orden recomendado en examen:
1. levantar proyecto
2. configurar DB
3. migraciones
4. modelos y relaciones
5. seeders
6. controladores y rutas
7. validar
8. probar
