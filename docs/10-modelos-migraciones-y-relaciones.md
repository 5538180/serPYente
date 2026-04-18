# 10. Modelos, migraciones y relaciones

## 1. Qué es este bloque

Este bloque convierte el dominio de la academia y el Mermaid en Laravel real.

Aquí aprendes a transformar:

- entidades en tablas
- relaciones en claves foráneas o tablas pivote
- estructura lógica en métodos de Eloquent

No basta con saber el nombre de una relación. Tienes que saber:

- en qué tabla va la clave foránea
- qué tabla intermedia necesitas si la relación es muchos a muchos
- qué método Eloquent representa la relación
- cómo consultar esa relación desde el código

---

## 2. Para qué sirve

Sirve para construir correctamente la base de datos y la capa de modelos.

Si esta parte está bien hecha, luego es mucho más fácil:

- crear CRUD
- poblar la base de datos con seeders
- montar una API
- escribir tests

Si esta parte está mal, el resto del proyecto se vuelve más difícil.

---

## 3. Cómo se utiliza este bloque dentro del curso

El orden correcto de trabajo es este:

1. relaciones 1 a 1
2. relaciones 1 a muchos
3. relaciones muchos a muchos
4. `hasManyThrough`
5. relaciones polimórficas
6. autorrelaciones

Ese orden va de lo más simple a lo más complejo y reduce errores.

---

## 4. Relación 1 a 1

### Caso 1: Usuario -> Perfil

#### Qué hace
Cada usuario tiene un único perfil ampliado.

#### Para qué sirve
Separa los datos principales del usuario de otros secundarios como teléfono, dirección o bio.

#### Cómo se utiliza
Se crea una tabla `perfiles` con una clave foránea `user_id` que apunta a `users`.

#### Cómo se crea
```bash
# Crea el modelo Perfil y además una migración asociada
php artisan make:model Perfil -m
```

#### Ejemplo de migración `perfiles`
```php
Schema::create('perfiles', function (Blueprint $table) {
    // Crea la columna id autoincremental y la marca como clave primaria
    $table->id();

    // Crea user_id como clave foránea y la enlaza con la tabla users
    $table->foreignId('user_id')->constrained()->cascadeOnDelete();

    // Campo de texto corto opcional para teléfono
    $table->string('telefono')->nullable();

    // Campo de texto corto opcional para dirección
    $table->string('direccion')->nullable();

    // Campo de texto largo opcional para una biografía
    $table->text('bio')->nullable();

    // Crea created_at y updated_at automáticamente
    $table->timestamps();
});
```

#### Explicación del ejemplo
- `Schema::create(...)` crea una tabla nueva.
- `Blueprint $table` representa la estructura de esa tabla.
- `id()` crea la clave primaria.
- `foreignId('user_id')` crea la columna que conectará con el usuario.
- `constrained()` enlaza por convención con `users.id`.
- `cascadeOnDelete()` borra el perfil si se borra el usuario.
- `nullable()` permite guardar `null` en ese campo.
- `timestamps()` crea las columnas de control temporal.

#### Ejemplo de modelo `User`
```php
public function perfil()
{
    // hasOne indica que un usuario tiene un único perfil asociado
    return $this->hasOne(Perfil::class);
}
```

#### Ejemplo de modelo `Perfil`
```php
public function user()
{
    // belongsTo indica que este perfil pertenece a un único usuario
    return $this->belongsTo(User::class);
}
```

#### Explicación del ejemplo
- `public function perfil()` define un método de relación.
- `return` devuelve el objeto relación de Eloquent.
- `hasOne(...)` representa la relación 1 a 1 desde el lado propietario.
- `belongsTo(...)` representa la relación desde el lado que tiene la clave foránea.

#### Error típico
Poner la clave foránea en la tabla equivocada.

#### Relación con el examen
Es una relación muy habitual para demostrar que entiendes modelos básicos.

---

## 5. Relación 1 a muchos

### Caso 1: Curso -> Lección

#### Qué hace
Un curso puede tener muchas lecciones.

#### Para qué sirve
Permite dividir el contenido de un curso en partes más pequeñas.

#### Cómo se utiliza
La tabla `lecciones` tendrá una clave foránea `curso_id`.

#### Cómo se crea
```bash
# Crea el modelo Curso y su migración
php artisan make:model Curso -m

# Crea el modelo Leccion y su migración
php artisan make:model Leccion -m
```

#### Ejemplo de migración `cursos`
```php
Schema::create('cursos', function (Blueprint $table) {
    // Clave primaria de la tabla cursos
    $table->id();

    // Título del curso
    $table->string('titulo');

    // Fechas de creación y actualización
    $table->timestamps();
});
```

#### Ejemplo de migración `lecciones`
```php
Schema::create('lecciones', function (Blueprint $table) {
    // Clave primaria de la tabla lecciones
    $table->id();

    // Clave foránea que apunta al curso propietario
    $table->foreignId('curso_id')->constrained()->cascadeOnDelete();

    // Título de la lección
    $table->string('titulo');

    // Contenido largo opcional
    $table->text('contenido')->nullable();

    // Fechas automáticas
    $table->timestamps();
});
```

#### Ejemplo de modelo `Curso`
```php
public function lecciones()
{
    // hasMany indica que un curso puede tener muchas lecciones
    return $this->hasMany(Leccion::class);
}
```

#### Ejemplo de modelo `Leccion`
```php
public function curso()
{
    // belongsTo indica que la lección pertenece a un curso concreto
    return $this->belongsTo(Curso::class);
}
```

#### Explicación del ejemplo
- La clave foránea va en la tabla del lado "muchos".
- `$curso->lecciones` devolverá la colección de lecciones del curso.

---

### Caso 2: Lección -> Tarea

#### Qué hace
Una lección puede tener muchas tareas.

#### Para qué sirve
Permite colgar ejercicios o actividades dentro de cada lección.

#### Cómo se utiliza
La tabla `tareas` tendrá `leccion_id`.

#### Cómo se crea
```bash
# Crea el modelo Tarea y su migración
php artisan make:model Tarea -m
```

#### Ejemplo de migración `tareas`
```php
Schema::create('tareas', function (Blueprint $table) {
    // Clave primaria de la tabla tareas
    $table->id();

    // Clave foránea que enlaza la tarea con la lección
    $table->foreignId('leccion_id')->constrained()->cascadeOnDelete();

    // Título de la tarea
    $table->string('titulo');

    // Descripción larga opcional
    $table->text('descripcion')->nullable();

    // Fechas automáticas
    $table->timestamps();
});
```

#### Ejemplo de modelo `Leccion`
```php
public function tareas()
{
    // hasMany indica que una lección puede tener muchas tareas
    return $this->hasMany(Tarea::class);
}
```

#### Ejemplo de modelo `Tarea`
```php
public function leccion()
{
    // belongsTo indica que la tarea pertenece a una lección concreta
    return $this->belongsTo(Leccion::class);
}
```

---

## 6. Relación muchos a muchos

### Caso 1: Curso <-> Alumno

#### Qué hace
Un curso tiene muchos alumnos y un alumno puede estar en muchos cursos.

#### Para qué sirve
Modela las matrículas.

#### Cómo se utiliza
Necesitas una tabla intermedia `alumno_curso`.

#### Cómo se crea
```bash
# Crea el modelo Alumno y su migración
php artisan make:model Alumno -m
```

#### Ejemplo de migración `alumnos`
```php
Schema::create('alumnos', function (Blueprint $table) {
    // Clave primaria de la tabla alumnos
    $table->id();

    // Nombre del alumno
    $table->string('nombre');

    // Email único del alumno
    $table->string('email')->unique();

    // Fechas automáticas
    $table->timestamps();
});
```

#### Ejemplo de tabla pivote `alumno_curso`
```php
Schema::create('alumno_curso', function (Blueprint $table) {
    // Clave primaria de la tabla pivote
    $table->id();

    // Clave foránea hacia alumnos
    $table->foreignId('alumno_id')->constrained()->cascadeOnDelete();

    // Clave foránea hacia cursos
    $table->foreignId('curso_id')->constrained()->cascadeOnDelete();

    // Fechas automáticas
    $table->timestamps();
});
```

#### Ejemplo de modelo `Curso`
```php
public function alumnos()
{
    // belongsToMany indica relación N:M con la tabla pivote por convención
    return $this->belongsToMany(Alumno::class);
}
```

#### Ejemplo de modelo `Alumno`
```php
public function cursos()
{
    // belongsToMany indica que un alumno puede estar en muchos cursos
    return $this->belongsToMany(Curso::class);
}
```

#### Explicación del ejemplo
- Una relación N:M nunca se representa con una sola clave foránea.
- Necesitas una tabla intermedia.
- Laravel busca la pivote por convención si usas nombres correctos.

---

### Caso 2: Taller <-> Alumno

#### Qué hace
Un taller tiene muchos alumnos y un alumno puede participar en muchos talleres.

#### Para qué sirve
Practicar una segunda relación N:M con otro contexto.

#### Cómo se utiliza
Se crea la tabla `alumno_taller`.

#### Cómo se crea
```bash
# Crea el modelo Taller y su migración
php artisan make:model Taller -m
```

#### Ejemplo de migración `talleres`
```php
Schema::create('talleres', function (Blueprint $table) {
    // Clave primaria de la tabla talleres
    $table->id();

    // Nombre del taller
    $table->string('nombre');

    // Fechas automáticas
    $table->timestamps();
});
```

#### Ejemplo de tabla pivote `alumno_taller`
```php
Schema::create('alumno_taller', function (Blueprint $table) {
    // Clave primaria de la tabla pivote
    $table->id();

    // Clave foránea hacia alumnos
    $table->foreignId('alumno_id')->constrained()->cascadeOnDelete();

    // Clave foránea hacia talleres
    $table->foreignId('taller_id')->constrained()->cascadeOnDelete();

    // Fechas automáticas
    $table->timestamps();
});
```

#### Ejemplo de modelo `Taller`
```php
public function alumnos()
{
    // belongsToMany define la relación muchos a muchos con alumnos
    return $this->belongsToMany(Alumno::class);
}
```

#### Ejemplo de modelo `Alumno`
```php
public function talleres()
{
    // belongsToMany define la relación muchos a muchos con talleres
    return $this->belongsToMany(Taller::class);
}
```

---

## 7. Relación hasManyThrough

### Caso: Curso -> Tarea a través de Lección

#### Qué hace
Permite acceder a las tareas de un curso pasando por sus lecciones.

#### Para qué sirve
Evita recorrer manualmente cada lección para obtener sus tareas.

#### Cómo se utiliza
Se define en el modelo `Curso`.

#### Ejemplo
```php
public function tareas()
{
    // hasManyThrough indica que Curso llega a Tarea a través de Leccion
    return $this->hasManyThrough(Tarea::class, Leccion::class);
}
```

#### Explicación del ejemplo
- Primer argumento: modelo final al que quieres llegar.
- Segundo argumento: modelo intermedio.
- Luego podrás hacer `$curso->tareas` directamente.

---

## 8. Polimórfica 1 a 1

### Caso: Foto de usuario o portada de curso

#### Qué hace
Una misma tabla guarda imágenes de distintos modelos.

#### Para qué sirve
Evita crear una tabla de fotos para cada tipo de entidad.

#### Cómo se utiliza
Se crea una tabla `fotos` con columnas polimórficas.

#### Cómo se crea
```bash
# Crea el modelo Foto y su migración
php artisan make:model Foto -m
```

#### Ejemplo de migración `fotos`
```php
Schema::create('fotos', function (Blueprint $table) {
    // Clave primaria de la tabla fotos
    $table->id();

    // Ruta o nombre del archivo de imagen
    $table->string('ruta');

    // Crea imageable_id e imageable_type para la relación polimórfica
    $table->morphs('imageable');

    // Fechas automáticas
    $table->timestamps();
});
```

#### Ejemplo de modelo `Foto`
```php
public function imageable()
{
    // morphTo permite que Foto pertenezca a distintos modelos
    return $this->morphTo();
}
```

#### Ejemplo de modelo `User`
```php
public function foto()
{
    // morphOne indica que el usuario tiene una única foto polimórfica
    return $this->morphOne(Foto::class, 'imageable');
}
```

#### Ejemplo de modelo `Curso`
```php
public function foto()
{
    // morphOne indica que el curso tiene una única foto polimórfica
    return $this->morphOne(Foto::class, 'imageable');
}
```

#### Explicación del ejemplo
- `imageable_id` guarda el id del registro relacionado.
- `imageable_type` guarda el tipo de modelo relacionado.
- La misma tabla sirve para varios modelos.

---

## 9. Polimórfica 1 a muchos

### Caso: Comentario sobre curso o tarea

#### Qué hace
Una misma tabla de comentarios sirve para comentar distintos tipos de contenido.

#### Para qué sirve
Evita crear tablas separadas como `comentarios_curso` y `comentarios_tarea`.

#### Cómo se utiliza
Se crea una tabla `comentarios` con columnas polimórficas.

#### Cómo se crea
```bash
# Crea el modelo Comentario y su migración
php artisan make:model Comentario -m
```

#### Ejemplo de migración `comentarios`
```php
Schema::create('comentarios', function (Blueprint $table) {
    // Clave primaria de la tabla comentarios
    $table->id();

    // Usuario que escribe el comentario
    $table->foreignId('user_id')->constrained()->cascadeOnDelete();

    // Texto del comentario
    $table->text('texto');

    // Crea commentable_id y commentable_type para la relación polimórfica
    $table->morphs('commentable');

    // Fechas automáticas
    $table->timestamps();
});
```

#### Ejemplo de modelo `Comentario`
```php
public function commentable()
{
    // morphTo permite que un comentario pertenezca a distintos modelos
    return $this->morphTo();
}
```

#### Ejemplo de modelo `Curso`
```php
public function comentarios()
{
    // morphMany indica que un curso puede tener muchos comentarios polimórficos
    return $this->morphMany(Comentario::class, 'commentable');
}
```

#### Ejemplo de modelo `Tarea`
```php
public function comentarios()
{
    // morphMany indica que una tarea puede tener muchos comentarios polimórficos
    return $this->morphMany(Comentario::class, 'commentable');
}
```

---

## 10. Polimórfica muchos a muchos

### Caso: Etiquetas para cursos y tareas

#### Qué hace
Permite que una misma etiqueta se aplique a varios tipos de entidad.

#### Para qué sirve
Clasificar cursos y tareas usando una tabla de etiquetas compartida.

#### Cómo se utiliza
Se crea una tabla `etiquetas` y una pivote polimórfica `taggables`.

#### Cómo se crea
```bash
# Crea el modelo Etiqueta y su migración
php artisan make:model Etiqueta -m
```

#### Ejemplo de migración `etiquetas`
```php
Schema::create('etiquetas', function (Blueprint $table) {
    // Clave primaria de la tabla etiquetas
    $table->id();

    // Nombre de la etiqueta
    $table->string('nombre');

    // Fechas automáticas
    $table->timestamps();
});
```

#### Ejemplo de tabla pivote polimórfica `taggables`
```php
Schema::create('taggables', function (Blueprint $table) {
    // Clave primaria de la tabla pivote polimórfica
    $table->id();

    // Clave foránea hacia etiquetas
    $table->foreignId('etiqueta_id')->constrained('etiquetas')->cascadeOnDelete();

    // Id del modelo relacionado
    $table->unsignedBigInteger('taggable_id');

    // Tipo de modelo relacionado
    $table->string('taggable_type');

    // Fechas automáticas
    $table->timestamps();
});
```

#### Ejemplo de modelo `Etiqueta`
```php
public function cursos()
{
    // morphedByMany recupera todos los cursos asociados a esta etiqueta
    return $this->morphedByMany(Curso::class, 'taggable');
}

public function tareas()
{
    // morphedByMany recupera todas las tareas asociadas a esta etiqueta
    return $this->morphedByMany(Tarea::class, 'taggable');
}
```

#### Ejemplo de modelo `Curso`
```php
public function etiquetas()
{
    // morphToMany asocia un curso con varias etiquetas compartidas
    return $this->morphToMany(Etiqueta::class, 'taggable');
}
```

#### Ejemplo de modelo `Tarea`
```php
public function etiquetas()
{
    // morphToMany asocia una tarea con varias etiquetas compartidas
    return $this->morphToMany(Etiqueta::class, 'taggable');
}
```

---

## 11. Autorrelación

### Caso 1: Categoría jerárquica

#### Qué hace
Permite que una categoría tenga otra categoría como padre.

#### Para qué sirve
Construir jerarquías como:
- Programación
  - PHP
  - Laravel

#### Cómo se utiliza
Se crea una clave foránea `parent_id` en la propia tabla.

#### Cómo se crea
```bash
# Crea el modelo Categoria y su migración
php artisan make:model Categoria -m
```

#### Ejemplo de migración
```php
Schema::create('categorias', function (Blueprint $table) {
    // Clave primaria
    $table->id();

    // Nombre de la categoría
    $table->string('nombre');

    // Clave foránea opcional que apunta a la misma tabla categorias
    $table->foreignId('parent_id')->nullable()->constrained('categorias')->nullOnDelete();

    // Fechas automáticas
    $table->timestamps();
});
```

#### Ejemplo de modelo
```php
public function padre()
{
    // belongsTo porque esta categoría puede pertenecer a otra categoría padre
    return $this->belongsTo(Categoria::class, 'parent_id');
}

public function hijas()
{
    // hasMany porque una categoría puede tener muchas subcategorías
    return $this->hasMany(Categoria::class, 'parent_id');
}
```

### Caso 2: Usuario mentor

#### Qué hace
Un usuario puede tener otro usuario como mentor.

#### Para qué sirve
Practicar una segunda autorrelación en un contexto distinto.

#### Cómo se utiliza
Se añade `mentor_id` a `users`.

#### Ejemplo de migración en `users`
```php
Schema::table('users', function (Blueprint $table) {
    // mentor_id apunta a otro usuario de la misma tabla users
    $table->foreignId('mentor_id')->nullable()->constrained('users')->nullOnDelete();
});
```

#### Ejemplo de modelo `User`
```php
public function mentor()
{
    // belongsTo porque este usuario pertenece a un mentor concreto
    return $this->belongsTo(User::class, 'mentor_id');
}

public function mentorizados()
{
    // hasMany porque un mentor puede tener muchos usuarios a su cargo
    return $this->hasMany(User::class, 'mentor_id');
}
```

---

## 12. Qué debes dominar al terminar este bloque

Debes saber:

- dónde va cada clave foránea
- cuándo hace falta tabla pivote
- cuándo usar `morph*`
- cuándo usar `hasManyThrough`
- cómo traducir el Mermaid a modelos reales
