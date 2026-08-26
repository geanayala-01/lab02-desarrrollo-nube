# Desarrollo de una Estrategia para la Nube — Documentación de Pasos

**Curso:** Desarrollo de Soluciones en la Nube — 5 C24 Sección AB
**Objetivo:** Publicar una aplicación HTML de perfil y un CRUD de la tabla `usuarios` en Render.

---

## 1. Estructura del proyecto

```
project/
├── perfil-html/          -> Página HTML estática de perfil
│   └── index.html
├── crud-usuarios/         -> Aplicación CRUD (Node.js + Express + PostgreSQL)
│   ├── db/
│   │   ├── pool.js
│   │   └── schema.sql
│   ├── public/
│   │   └── index.html
│   ├── server.js
│   ├── package.json
│   ├── .env.example
│   └── .gitignore
└── DOCUMENTACION.md
```

---

## 2. Requisitos previos

- Cuenta en [Render](https://render.com) (ya la tienes).
- Repositorio en GitHub (ya lo tienes) — vas a subir esta carpeta ahí.
- Node.js instalado localmente si quieres probar antes de desplegar (opcional).

---

## 3. Subir el código a GitHub

Estos son los comandos que **tú** debes ejecutar en la terminal, dentro de la carpeta `project`:

```bash
git init
git add .
git commit -m "Perfil HTML y CRUD de usuarios listos para Render"
git branch -M main
git remote add origin <URL-DE-TU-REPOSITORIO>.git
git push -u origin main
```

> Reemplaza `<URL-DE-TU-REPOSITORIO>` por la URL de tu repo en GitHub.

---

## 4. Desplegar la página HTML de perfil en Render

1. Entra a tu panel de Render → **New +** → **Static Site**.
2. Conecta tu repositorio de GitHub.
3. Configura:
   - **Root Directory:** `perfil-html`
   - **Build Command:** (dejar vacío)
   - **Publish Directory:** `.`
4. Click en **Create Static Site**.
5. Render te dará una URL pública tipo `https://tu-perfil.onrender.com`.

---

## 5. Crear la base de datos PostgreSQL en Render

1. En el panel de Render → **New +** → **PostgreSQL**.
2. Ponle un nombre (ej. `db-usuarios`) y elige el plan **Free**.
3. Click en **Create Database**.
4. Espera a que el estado sea **Available**.
5. Copia el valor de **Internal Database URL** (lo usarás en el paso 6).

---

## 6. Desplegar el CRUD (Web Service) en Render

1. En el panel de Render → **New +** → **Web Service**.
2. Conecta el mismo repositorio de GitHub.
3. Configura:
   - **Root Directory:** `crud-usuarios`
   - **Runtime:** Node
   - **Build Command:** `npm install`
   - **Start Command:** `npm start`
4. En la sección **Environment Variables**, agrega:
   - `DATABASE_URL` = (pega el Internal Database URL copiado en el paso 5)
5. Click en **Create Web Service**.
6. Render instalará dependencias, ejecutará `npm start`, y la app creará automáticamente la tabla `usuarios` (ver `db/schema.sql`, se ejecuta al iniciar el servidor en `server.js`).
7. Cuando el estado sea **Live**, abre la URL pública (ej. `https://crud-usuarios.onrender.com`).

---

## 7. Probar el CRUD

Desde la URL pública del Web Service:

1. **Crear usuario:** Llena el formulario (nombre, correo, edad) y click en "Agregar Usuario".
2. **Leer usuarios:** La tabla se carga automáticamente al abrir la página (`GET /api/usuarios`).
3. **Actualizar usuario:** Click en "Editar" en una fila, modifica los campos y click en "Actualizar Usuario".
4. **Eliminar usuario:** Click en "Eliminar" en una fila.

También puedes probar la API directamente con `curl` o Postman:

```bash
# Listar usuarios
curl https://crud-usuarios.onrender.com/api/usuarios

# Crear usuario
curl -X POST https://crud-usuarios.onrender.com/api/usuarios \
  -H "Content-Type: application/json" \
  -d '{"nombre":"Juan Perez","correo":"juan@mail.com","edad":25}'
```

---

## 8. Evidencias a incluir en el informe

- Captura del repositorio en GitHub con la estructura de carpetas.
- Captura del Static Site desplegado (perfil-html) funcionando.
- Captura de la base de datos PostgreSQL en estado **Available**.
- Captura del Web Service (crud-usuarios) en estado **Live**.
- Captura del CRUD funcionando: crear, listar, editar y eliminar un usuario.
- URLs públicas finales de ambos despliegues.

---

## 9. Notas importantes

- El plan **Free** de Render "duerme" los servicios tras 15 minutos de inactividad; la primera carga tras inactividad puede tardar ~30-60 segundos.
- La tabla `usuarios` se crea automáticamente al iniciar el servidor (no necesitas ejecutar `schema.sql` manualmente), pero también puedes ejecutarlo a mano desde el **Shell** de la base de datos en Render si lo prefieres.
- Nunca subas tu archivo `.env` real a GitHub (ya está excluido en `.gitignore`); usa `.env.example` como referencia.
