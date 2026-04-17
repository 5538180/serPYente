# Preparacion-Examen-Laravel-Php

Repositorio de estudio para preparar un examen práctico de **Laravel + PHP** desde cero, tomando como referencia el tipo de trabajo y organización vista en proyectos como `backend-EAC`, `eportfolio` y `marcapersonal`, pero usando un dominio más sencillo para practicar: una **academia**.

Este material está pensado para cubrir el recorrido completo:

- instalación del entorno
- Git y GitHub
- Composer
- PHP y Laravel
- Laravel Installer
- creación de proyecto
- Breeze
- Sanctum
- controladores
- modelos y migraciones
- relaciones Eloquent
- factories y seeders
- rutas web y API
- requests, resources y services
- Swagger
- tests
- flujo de trabajo de examen

---

## Objetivo

Aprender a desarrollar casi todo lo necesario en una aplicación Laravel real, centrada en backend, incluyendo:

- PHP orientado a objetos aplicado a Laravel
- Eloquent ORM
- relaciones (`hasOne`, `belongsTo`, `hasMany`, `belongsToMany`, `hasManyThrough`, polimórficas y autorrelaciones)
- migraciones y claves foráneas
- tablas pivote
- factories y seeders
- controladores web y API
- `FormRequest`
- `Resource`
- servicios
- versionado de API (`/api/v1`)
- autenticación con Sanctum
- documentación con Swagger / L5 Swagger
- tests `Feature` y `Unit`
- flujo real con Git y GitHub

---

## Proyecto guía del repositorio

El dominio de práctica será una **academia** con estas entidades principales:

- Centro
- Usuario
- PerfilUsuario
- Aula
- HorarioAula
- Profesor
- Alumno
- Pago
- Curso
- Leccion
- Tarea
- Taller
- Foto
- Comentario
- Etiqueta
- Aviso
- Categoria

Con este dominio se pueden practicar:

- 2 relaciones 1 a 1
- varias 1 a muchos
- 2 muchos a muchos
- 2 `hasManyThrough`
- 2 polimórficas 1 a 1
- 2 polimórficas 1 a muchos
- 2 polimórficas muchos a muchos
- 2 autorrelaciones

---

## Estructura del material

### 00. Visión general
- [docs/00-ruta-general.md](docs/00-ruta-general.md)

### 01. Fundamentos previos
- [docs/01-instalacion-entorno.md](docs/01-instalacion-entorno.md)
- [docs/02-git-y-github-desde-cero.md](docs/02-git-y-github-desde-cero.md)
- [docs/03-composer-php-y-laravel-installer.md](docs/03-composer-php-y-laravel-installer.md)

### 02. Dominio y contexto
- [docs/04-contexto-entidades.md](docs/04-contexto-entidades.md)
- [docs/05-mermaid-y-relaciones.md](docs/05-mermaid-y-relaciones.md)

### 03. Proyecto Laravel
- [docs/06-crear-primer-proyecto-laravel.md](docs/06-crear-primer-proyecto-laravel.md)
- [docs/07-estructura-proyecto.md](docs/07-estructura-proyecto.md)
- [docs/08-comandos-php-artisan.md](docs/08-comandos-php-artisan.md)

### 04. Desarrollo por fases
- [docs/09-hoja-de-ruta-por-fases.md](docs/09-hoja-de-ruta-por-fases.md)
- [docs/10-modelos-migraciones-y-relaciones.md](docs/10-modelos-migraciones-y-relaciones.md)

### 05. Backend HTTP/API
- [docs/11-rutas-controladores-requests-resources-services.md](docs/11-rutas-controladores-requests-resources-services.md)
- [docs/12-breeze-sanctum-y-autenticacion.md](docs/12-breeze-sanctum-y-autenticacion.md)
- [docs/13-swagger-y-documentacion-api.md](docs/13-swagger-y-documentacion-api.md)

### 06. Datos y pruebas
- [docs/14-factories-seeders-y-datos.md](docs/14-factories-seeders-y-datos.md)
- [docs/15-tests-feature-y-unit.md](docs/15-tests-feature-y-unit.md)

### 07. Flujo de examen
- [docs/16-guion-de-desarrollo-en-examen.md](docs/16-guion-de-desarrollo-en-examen.md)
- [docs/17-checklist-final.md](docs/17-checklist-final.md)

### 08. Soluciones y ejemplos
- [soluciones/01-comandos-base.md](soluciones/01-comandos-base.md)
- [soluciones/02-snippets-relaciones.md](soluciones/02-snippets-relaciones.md)
- [soluciones/03-snippets-api.md](soluciones/03-snippets-api.md)
- [soluciones/04-snippets-git-github.md](soluciones/04-snippets-git-github.md)

---

## Cómo usar este repositorio

Orden recomendado:

1. Instalar entorno
2. Repasar Git y GitHub
3. Entender Composer, PHP y Laravel Installer
4. Crear el primer proyecto Laravel
5. Entender el dominio y las relaciones
6. Implementar por fases
7. Añadir autenticación, API y documentación
8. Poblar datos y hacer tests
9. Cerrar con el guion de examen

---

## Nombre futuro

Este repositorio se está preparando en `serPYente`, pero está organizado para que puedas renombrarlo después como:

**Preparacion-Examen-Laravel-Php**
