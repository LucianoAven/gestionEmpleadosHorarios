# 📋 Sistema de Gestión de Empleados y Horarios

Sistema completo de gestión empresarial que permite administrar usuarios, empleados, horarios y solicitudes de cambio de horario con autenticación JWT y recuperación de contraseñas por email.

## 🚀 Características

- **👥 Gestión de Usuarios**: Registro, login, perfiles y autenticación JWT
- **👨‍💼 Gestión de Empleados**: CRUD completo con paginación y búsqueda
- **🕐 Gestión de Horarios**: Asignación y administración de horarios de trabajo
- **📝 Solicitudes de Cambio**: Sistema de solicitudes de modificación de horarios
- **📧 Recuperación de Contraseña**: Envío de emails con SendGrid
- **🔐 Control de Acceso**: Roles de administrador y empleado
- **📱 Interfaz Moderna**: Frontend con React + PrimeReact
- **📄 Exportación PDF**: Generación de reportes en PDF

## 🛠️ Tecnologías Utilizadas

### Backend
- **Node.js** + **Express.js**
- **Sequelize ORM** + **MySQL**
- **JWT** para autenticación
- **bcrypt** para encriptación de contraseñas
- **SendGrid** para envío de emails
- **Nodemailer** para gestión de correos

### Frontend
- **React** + **Vite**
- **PrimeReact** para componentes UI
- **Axios** para peticiones HTTP
- **Formik** + **Yup** para formularios y validación
- **React Router** para navegación
- **jsPDF** para exportación PDF

## 📋 Prerrequisitos

Antes de comenzar, asegúrate de tener instalado:

- [Node.js](https://nodejs.org/) (versión 16 o superior)
- [MySQL](https://dev.mysql.com/downloads/) (versión 8.0 o superior)
- [Git](https://git-scm.com/)
- Cuenta en [SendGrid](https://sendgrid.com/) para envío de emails

## 🔧 Instalación

### 1. Clonar el Repositorio

```bash
git clone https://github.com/tu-usuario/efiReact.git
cd efiReact
```

### 2. Configurar la Base de Datos

**Crear la base de datos en MySQL:**

```sql
CREATE DATABASE gestion_empleados;
USE gestion_empleados;
```

### 3. Configurar el Backend

```bash
cd API-REST-DB
npm install
```

**Configurar variables de entorno:**

Crear archivo `.env` en `API-REST-DB/`:

```env
# URL del Frontend
FRONT_URL=http://localhost:5173

# Configuración de SendGrid
SMTP_HOST=smtp.sendgrid.net
SMTP_PORT=587
SMTP_USER=apikey
SMTP_PASS=TU_SENDGRID_API_KEY_AQUI
MAIL_FROM=tu-email@ejemplo.com

# JWT Secret
JWT_SECRET=tu_clave_secreta_jwt_aqui

# Base de Datos (si usas variables de entorno)
DB_HOST=localhost
DB_NAME=gestion_empleados
DB_USER=tu_usuario_mysql
DB_PASS=tu_contraseña_mysql
```

**Configurar base de datos:**

Editar `API-REST-DB/config/config.json`:

```json
{
  "development": {
    "username": "tu_usuario_mysql",
    "password": "tu_contraseña_mysql",
    "database": "gestion_empleados",
    "host": "127.0.0.1",
    "dialect": "mysql"
  }
}
```

**Ejecutar migraciones:**

```bash
# Si no tienes sequelize-cli instalado globalmente
npm install -g sequelize-cli

# Ejecutar migraciones
npx sequelize-cli db:migrate
```

### 4. Configurar el Frontend

```bash
cd ../FRONT-REACT
npm install
```

## 🚀 Ejecución

### Opción 1: Ejecutar por Separado

**Terminal 1 - Backend:**
```bash
cd API-REST-DB
npm run dev
```
El backend estará disponible en: `http://localhost:3000`

**Terminal 2 - Frontend:**
```bash
cd FRONT-REACT
npm run dev
```
El frontend estará disponible en: `http://localhost:5173`

### Opción 2: Script de Desarrollo (Recomendado)

Crear un script para ejecutar ambos servicios:

**`start-dev.sh` (Linux/Mac):**
```bash
#!/bin/bash
echo "Iniciando Backend..."
cd API-REST-DB && npm run dev &
echo "Iniciando Frontend..."
cd ../FRONT-REACT && npm run dev &
wait
```

**`start-dev.bat` (Windows):**
```bat
@echo off
echo Iniciando Backend...
cd API-REST-DB && start cmd /k npm run dev
echo Iniciando Frontend...
cd ../FRONT-REACT && start cmd /k npm run dev
```

## 📧 Configuración de SendGrid

1. **Crear cuenta en SendGrid**
2. **Verificar dominio/email** en SendGrid
3. **Crear API Key:**
   - Ve a Settings → API Keys
   - Create API Key
   - Selecciona "Restricted Access"
   - Otorga permisos de "Mail Send"
   - Copia la API Key generada
4. **Actualizar `.env`** con la API Key

## 👥 Usuarios por Defecto

El sistema incluye roles de usuario:

- **admin**: Acceso completo al sistema
- **empleado**: Acceso a su propio perfil y horarios

**Para crear el primer administrador:**
1. Registra un usuario desde `/registro`
2. Actualiza manualmente en la BD: `UPDATE Users SET rol = 'admin' WHERE email = 'tu-email@ejemplo.com'`

## 🗂️ Estructura del Proyecto

```
efiReact/
├── API-REST-DB/                 # Backend Node.js + Express
│   ├── controllers/             # Lógica de controladores
│   ├── models/                  # Modelos de Sequelize
│   ├── routes/                  # Definición de rutas
│   ├── migrations/              # Migraciones de BD
│   ├── middlewares/             # Middlewares personalizados
│   ├── utils/                   # Utilidades (mailer, etc.)
│   ├── config/                  # Configuración de BD
│   ├── .env                     # Variables de entorno
│   ├── index.js                 # Punto de entrada
│   └── package.json
│
└── FRONT-REACT/                 # Frontend React
    ├── src/
    │   ├── components/          # Componentes reutilizables
    │   ├── context/             # Context API para estado global
    │   ├── layouts/             # Páginas principales
    │   ├── services/            # Servicios de API
    │   ├── utils/               # Utilidades (PDF, rutas, etc.)
    │   ├── App.jsx              # Componente principal
    │   └── main.jsx             # Punto de entrada
    ├── public/                  # Archivos estáticos
    ├── package.json
    └── vite.config.js
```

## 🔗 Endpoints Principales

### Autenticación
- `POST /auth/register` - Registro de usuario
- `POST /auth/login` - Inicio de sesión
- `POST /auth/forgotPassword` - Recuperar contraseña

### Usuarios
- `GET /usuarios/profile` - Obtener perfil del usuario logueado
- `GET /usuarios` - Listar usuarios (solo admin)
- `PUT /usuarios/:id` - Actualizar usuario

### Empleados
- `GET /empleados` - Listar empleados paginados (admin)
- `GET /empleados/:id` - Obtener empleado por ID
- `POST /empleados` - Crear empleado (admin)
- `PUT /empleados/:id` - Actualizar empleado (admin)
- `DELETE /empleados/:id` - Eliminar empleado (admin)

### Horarios
- `GET /horarios` - Listar horarios (admin)
- `GET /horarios/:id` - Obtener horario por ID
- `POST /horarios` - Crear horario (admin)
- `PUT /horarios/:id` - Actualizar horario (admin)
- `DELETE /horarios/:id` - Eliminar horario (admin)

## 🐛 Solución de Problemas

### Error de Conexión a BD
```bash
# Verificar que MySQL esté corriendo
sudo systemctl start mysql    # Linux
brew services start mysql     # Mac
net start mysql              # Windows
```

### Error de SendGrid
- Verificar que la API Key sea válida
- Confirmar que el dominio/email esté verificado
- Revisar límites de envío en SendGrid

### Error de CORS
- Verificar que el frontend esté en `http://localhost:5173`
- Confirmar configuración de CORS en el backend

### Problemas de Paginación
- Asegurarse de que `totalRecords` se actualice correctamente
- Verificar que las funciones CRUD recarguen los datos

## 🤝 Contribuir

1. Fork el proyecto
2. Crear una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abrir un Pull Request

## 📝 Licencia

Este proyecto está bajo la Licencia ISC. Ver `LICENSE` para más detalles.

## 👨‍💻 Autor

**Luciano Avendaño**
- Email: l.avendano@itecriocuarto.org.ar
- Institución: ITEC Río Cuarto

---
