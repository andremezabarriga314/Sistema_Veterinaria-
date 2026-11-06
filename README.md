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