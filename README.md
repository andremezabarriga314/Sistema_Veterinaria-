<div align="center">

# 🐾 Sistema Veterinaria Profesional

**Gestión clínica veterinaria con Django + MariaDB**

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![Django](https://img.shields.io/badge/Django-WSGI-092E20?logo=django&logoColor=white)
![MariaDB](https://img.shields.io/badge/MariaDB-10.5+-003545?logo=mariadb&logoColor=white)
![Estado](https://img.shields.io/badge/uso-educativo-blue)

</div>

---

## 📋 Descripción

Proyecto educativo completo, basado en la estructura de **Agenda Simple** (`config/`, `apps/`, `templates/control/`, `static/control/`).

### 🗂️ Tablas de negocio

| Tabla | Descripción |
|---|---|
| `propietarios` | Dueños de las mascotas |
| `mascotas` | Pacientes registrados |
| `veterinarios` | Personal médico |
| `citas` | Agenda de atención |
| `diagnosticos` | Resultados de consulta |
| `medicamentos` | Catálogo de fármacos |
| `recetas` | Prescripciones (un medicamento por registro) |

> Django crea además tablas internas para usuarios, permisos y sesiones.

---

## 💻 Instalación en Windows (PowerShell)

1. Descomprime el proyecto en `C:\xampp\htdocs\veterinaria_profesional` (o cualquier carpeta). XAMPP solo sirve para MariaDB; Django usa su propio servidor.
2. Crea el entorno virtual:
   ```powershell
   py -m venv .venv
   ```
3. Instala las dependencias sin activar el entorno:
   ```powershell
   .\.venv\Scripts\python.exe -m pip install -r requirements.txt
   ```
4. Copia `.env.example` como `.env` y completa los datos reales de MariaDB, en especial `DB_PORT` (normalmente `3306`; en Agenda Simple era `3307`).
5. En phpMyAdmin, ejecuta el SQL de `database/crear_bd.sql`.
6. Migra la base de datos:
   ```powershell
   .\.venv\Scripts\python.exe manage.py migrate
   ```
7. Crea el administrador:
   ```powershell
   .\.venv\Scripts\python.exe manage.py createsuperuser
   ```
8. Inicia el servidor:
   ```powershell
   .\.venv\Scripts\python.exe manage.py runserver
   ```
9. Abre 👉 http://127.0.0.1:8000/login/

> ⚠️ **Nunca** reutilices ni copies la carpeta `.venv` entre equipos. Crea un entorno nuevo en cada PC.

---

## 🚀 Despliegue en hosting (Linux con Python y MariaDB)

1. Contrata un hosting **compatible con aplicaciones WSGI/ASGI Python**, con MariaDB 10.5+ y HTTPS. Un hosting PHP/XAMPP estándar no es suficiente.
2. Instala `requirements.txt` y configura el `.env`:

   | Variable | Valor |
   |---|---|
   | `DJANGO_DEBUG` | `0` |
   | `DJANGO_SECRET_KEY` | Cadena aleatoria y secreta |
   | `DJANGO_ALLOWED_HOSTS` | `tudominio.com` |
   | `DJANGO_CSRF_TRUSTED_ORIGINS` | `https://tudominio.com` |
   | `DB_*` | Datos reales de la base |
   | `REGISTRATION_CODE` | Código secreto de invitación |

3. Ejecuta:
   ```bash
   python manage.py migrate
   python manage.py collectstatic --noinput
   python manage.py check --deploy
   ```
4. Configura la aplicación WSGI (`config.wsgi:application`) con Gunicorn/uWSGI o el panel del proveedor, y un proxy inverso con HTTPS. Si estás tras un proxy, configura `SECURE_PROXY_SSL_HEADER` **solo cuando** el proveedor elimine y establezca correctamente ese encabezado.
5. Antes de almacenar historias clínicas reales, implementa:
   - Respaldos cifrados de la base de datos
   - Control de acceso por puesto
   - Retención de datos y consentimiento
   - Registro de accesos
   - Plan de recuperación
6. **Registro de usuarios:** en producción solo se permite el alta con el código `REGISTRATION_CODE`. Si está vacío, el registro se deshabilita. Para cerrar altas, borra la variable y reinicia la aplicación.

### ⚠️ Limitación deliberada de esta versión

Todos los usuarios autenticados pueden ver y modificar registros. Para una clínica con varios roles o sucursales se requiere autorización granular, auditoría, políticas de privacidad y revisión de seguridad antes de usarla con datos clínicos reales.

Las recetas contienen **un medicamento por registro**; para varios medicamentos se crean varias recetas vinculadas al mismo diagnóstico.

---

## 🖨️ Reportes

- `/reportes/citas/`: citas filtradas por rango de fechas y estado, listas para imprimir.
- En las listas de citas, diagnósticos y recetas: acción **Imprimir**.
- En la ventana de impresión elige **Guardar como PDF** en el navegador.

## 🧪 Pruebas

```bash
python manage.py test apps
```

## 🔄 Orden de registros recomendado

```
Propietarios → Mascotas → Veterinarios → Medicamentos → Citas → Diagnósticos → Recetas
```

## 📚 Documentación

Consulta el archivo [`MANUAL_DEL_PROFESOR.md`](MANUAL_DEL_PROFESOR.md).

---

## 🧱 Organización de modelos y tipografía

Las clases están separadas en `apps/models/`:

`Propietario.py` · `Mascota.py` · `Veterinario.py` · `Cita.py` · `Diagnostico.py` · `Medicamento.py` · `Receta.py`

`apps/models/__init__.py` las registra mediante importaciones. El texto estándar y los controles se muestran con letra de **16 px**.

> ✅ **No necesitas borrar ni recrear la base de datos:** la migración inicial conserva la misma estructura.

## 🎨 Diseño y paleta de colores

La interfaz usa una paleta veterinaria: **verde azulado** (salud y confianza), **coral** (acciones y alertas) y fondos suaves que descansan la vista.

| Uso | Color | Hex |
|---|---|---|
| Primario | ![](https://img.shields.io/badge/-%20%20%20%20%20-0F8B8D) | `#0F8B8D` |
| Primario oscuro (navbar, hover) | ![](https://img.shields.io/badge/-%20%20%20%20%20-0A5F61) | `#0A5F61` |
| Primario suave (resaltados) | ![](https://img.shields.io/badge/-%20%20%20%20%20-E3F3F3) | `#E3F3F3` |
| Acento (acciones destacadas) | ![](https://img.shields.io/badge/-%20%20%20%20%20-F26B5B) | `#F26B5B` |
| Fondo | ![](https://img.shields.io/badge/-%20%20%20%20%20-F4F8F8) | `#F4F8F8` |
| Texto | ![](https://img.shields.io/badge/-%20%20%20%20%20-1F2D3D) | `#1F2D3D` |
| Éxito | ![](https://img.shields.io/badge/-%20%20%20%20%20-2E9E6B) | `#2E9E6B` |
| Aviso | ![](https://img.shields.io/badge/-%20%20%20%20%20-F2A93B) | `#F2A93B` |
| Error | ![](https://img.shields.io/badge/-%20%20%20%20%20-D64545) | `#D64545` |

### Activar los estilos

1. Crear el archivo `static/control/css/estilo.css` con el CSS de abajo.
2. En `templates/control/base.html`, dentro de `<head>`:

   ```html
   {% load static %}
   <link rel="stylesheet" href="{% static 'control/css/estilo.css' %}">
   ```

3. Recargar con **Ctrl + F5**. En producción, ejecutar `python manage.py collectstatic --noinput`.

### Clases disponibles

| Clase | Función |
|---|---|
| `.navbar`, `.marca` | Barra superior con degradado |
| `.contenedor`, `.tarjeta` | Contenedor central y tarjetas con sombra |
| `.btn`, `.btn-acento`, `.btn-peligro`, `.btn-claro` | Botones |
| `.badge-pendiente`, `-confirmada`, `-atendida`, `-cancelada` | Estados de las citas |
| `.mensaje.success / .warning / .error` | Mensajes de Django |
| `.no-imprimir` | Oculta elementos al imprimir o guardar como PDF |

Ejemplo de estado de una cita:

```html
<span class="badge badge-{{ cita.estado|lower }}">{{ cita.estado }}</span>
```

<details>
<summary><b>Ver CSS completo (<code>estilo.css</code>)</b></summary>

```css
:root {
  --primario: #0F8B8D;
  --primario-oscuro: #0A5F61;
  --primario-suave: #E3F3F3;
  --acento: #F26B5B;
  --fondo: #F4F8F8;
  --tarjeta: #FFFFFF;
  --texto: #1F2D3D;
  --borde: #D9E4E6;
  --exito: #2E9E6B;
  --aviso: #F2A93B;
  --error: #D64545;
  --radio: 12px;
  --sombra: 0 2px 10px rgba(15, 60, 70, .08);
}

* { box-sizing: border-box; }

body {
  margin: 0;
  font-family: "Segoe UI", system-ui, -apple-system, Roboto, sans-serif;
  font-size: 16px;
  line-height: 1.55;
  color: var(--texto);
  background: var(--fondo);
}

a { color: var(--primario); text-decoration: none; }
a:hover { color: var(--primario-oscuro); text-decoration: underline; }

h1, h2, h3 { color: var(--primario-oscuro); margin: 0 0 .6em; line-height: 1.25; }
h1 { font-size: 1.8rem; }
h2 { font-size: 1.4rem; }

/* Navegación */
.navbar {
  display: flex; align-items: center; gap: 1.5rem;
  padding: .9rem 1.5rem;
  background: linear-gradient(90deg, var(--primario-oscuro), var(--primario));
  box-shadow: var(--sombra);
}
.navbar .marca { color: #fff; font-weight: 700; font-size: 1.2rem; }
.navbar .marca::before { content: "🐾 "; }
.navbar a { color: #E9F7F7; padding: .4rem .7rem; border-radius: 8px; }
.navbar a:hover, .navbar a.activo {
  background: rgba(255, 255, 255, .18); color: #fff; text-decoration: none;
}

/* Contenedor y tarjetas */
.contenedor { max-width: 1100px; margin: 2rem auto; padding: 0 1rem; }
.tarjeta {
  background: var(--tarjeta);
  border: 1px solid var(--borde);
  border-radius: var(--radio);
  box-shadow: var(--sombra);
  padding: 1.5rem;
  margin-bottom: 1.5rem;
}

/* Botones */
.btn {
  display: inline-block; cursor: pointer;
  padding: .55rem 1.1rem; font-size: 1rem; font-weight: 600;
  color: #fff; background: var(--primario);
  border: 0; border-radius: 8px;
  transition: background .15s, transform .05s;
}
.btn:hover { background: var(--primario-oscuro); color: #fff; text-decoration: none; }
.btn:active { transform: translateY(1px); }
.btn-acento { background: var(--acento); }
.btn-acento:hover { background: #D9503F; }
.btn-peligro { background: var(--error); }
.btn-peligro:hover { background: #B73535; }
.btn-claro { background: var(--primario-suave); color: var(--primario-oscuro); }
.btn-claro:hover { background: #CDE9E9; color: var(--primario-oscuro); }

/* Formularios */
label { display: block; font-weight: 600; margin: .9rem 0 .3rem; }
input, select, textarea {
  width: 100%; font: inherit; color: var(--texto);
  padding: .55rem .75rem;
  background: #fff; border: 1px solid var(--borde); border-radius: 8px;
}
input:focus, select:focus, textarea:focus {
  outline: none; border-color: var(--primario);
  box-shadow: 0 0 0 3px rgba(15, 139, 141, .2);
}
.errorlist { list-style: none; padding: 0; margin: .3rem 0; color: var(--error); font-size: .9rem; }

/* Tablas */
.tabla-wrap { overflow-x: auto; }
table { width: 100%; border-collapse: collapse; background: #fff; }
th { text-align: left; padding: .75rem; color: #fff; background: var(--primario); }
th:first-child { border-top-left-radius: 10px; }
th:last-child { border-top-right-radius: 10px; }
td { padding: .7rem .75rem; border-bottom: 1px solid var(--borde); }
tbody tr:nth-child(even) { background: #F8FCFC; }
tbody tr:hover { background: var(--primario-suave); }

/* Estados de citas */
.badge { display: inline-block; padding: .15rem .65rem; font-size: .85rem; font-weight: 600; border-radius: 999px; }
.badge-pendiente  { background: #FFF1D6; color: #8A5A00; }
.badge-confirmada { background: #DCEFFB; color: #11598A; }
.badge-atendida   { background: #DDF3E8; color: #1D6B47; }
.badge-cancelada  { background: #FBE0E0; color: #9B2C2C; }

/* Mensajes de Django */
.mensaje { padding: .8rem 1rem; border-radius: 8px; margin-bottom: 1rem;
           border-left: 5px solid var(--primario); background: var(--primario-suave); }
.mensaje.success { border-color: var(--exito); background: #E3F5EC; }
.mensaje.warning { border-color: var(--aviso); background: #FFF4DE; }
.mensaje.error   { border-color: var(--error); background: #FCE6E6; }

/* Login */
.login-caja { max-width: 400px; margin: 6vh auto; }
.login-caja h1 { text-align: center; }

/* Impresión */
@media print {
  body { background: #fff; font-size: 12pt; }
  .navbar, .btn, .no-imprimir { display: none !important; }
  .tarjeta { box-shadow: none; border: 0; padding: 0; }
  th { background: #eee !important; color: #000 !important; }
}

/* Móvil */
@media (max-width: 640px) {
  .navbar { flex-wrap: wrap; gap: .5rem; }
  .contenedor { margin: 1rem auto; }
}
```

</details>