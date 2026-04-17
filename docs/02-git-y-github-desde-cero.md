# 02. Git y GitHub desde cero

## 1. Qué es Git
Git es un sistema de control de versiones. Sirve para:

- guardar el historial del proyecto
- volver atrás si algo se rompe
- trabajar por cambios pequeños y ordenados
- sincronizar tu trabajo con GitHub

## 2. Qué es GitHub
GitHub es el repositorio remoto. Tu flujo real será:

1. trabajas en local
2. haces `git add`
3. haces `git commit`
4. haces `git push`

## 3. Configuración inicial en Debian
```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
git config --global init.defaultBranch main
```

Comprobar:
```bash
git config --list
```

## 4. Crear un repositorio local
```bash
mkdir mi-proyecto
cd mi-proyecto
git init
```

Explicación:
- `git init` crea la carpeta `.git`
- ahí Git guarda el historial del proyecto

## 5. Ciclo básico de trabajo
```bash
git status
git add .
git commit -m "feat: inicio del proyecto"
```

### Qué significa cada comando
- `git status`: qué ha cambiado
- `git add .`: pasar cambios a staging
- `git commit`: crear una fotografía del proyecto

## 6. Conectar con GitHub
### Si ya existe el repo remoto
```bash
git remote add origin https://github.com/USUARIO/REPO.git
git branch -M main
git push -u origin main
```

Explicación:
- `remote add origin`: conecta tu repo local con GitHub
- `branch -M main`: deja la rama principal con nombre `main`
- `push -u`: sube y deja configurada la rama remota

## 7. Flujo de trabajo diario
```bash
git add .
git commit -m "feat: añadir modelo Curso"
git push
```

## 8. Traer cambios remotos
```bash
git pull
```

## 9. Ver historial
```bash
git log --oneline
```

## 10. Crear ramas
```bash
git checkout -b feature/cursos
```

Volver a main:
```bash
git checkout main
```

Fusionar rama:
```bash
git merge feature/cursos
```

## 11. `.gitignore`
Ejemplo típico Laravel:
```txt
/vendor
/node_modules
/.env
/public/build
/storage/*.key
```

## 12. Errores típicos
### `nothing to commit`
No hay cambios preparados o ya están guardados.

### `rejected push`
El remoto tiene cambios que tú no tienes.
```bash
git pull --rebase
git push
```

### `Author identity unknown`
Falta configurar nombre o email.

## 13. Flujo recomendado para examen
1. crear proyecto
2. primer commit
3. cada bloque importante = un commit
4. no acumular demasiados cambios sin guardar

## 14. Ejemplo real de flujo Laravel + GitHub
```bash
laravel new academia
cd academia
git init
git add .
git commit -m "chore: proyecto base laravel"
git remote add origin https://github.com/USUARIO/academia.git
git branch -M main
git push -u origin main
```

## 15. Qué debes dominar
- inicializar repo
- hacer commit
- subir a GitHub
- traer cambios
- crear rama
- fusionar rama
