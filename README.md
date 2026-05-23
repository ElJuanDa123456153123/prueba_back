# 🏢 365Soft - Sistema de Gestión de Alquileres (Backend)

Sistema multi-tenant completo para la gestión de propiedades y alquileres, desarrollado con NestJS, PostgreSQL y Docker.

---

## 🎯 Características Principales

- **Multi-tenant**: Arquitectura con schemas PostgreSQL para aislamiento completo de datos
- **Gestión de Propiedades**: Catálogo público, unidades, reservas y bloqueos de fechas
- **Contratos**: Generación de contratos, renovaciones, historial y PDFs
- **Solicitudes de Alquiler**: Workflow completo con screening, aprobación y documentación
- **Pagos**: Múltiples métodos (Stripe, PayPal, PayU, QR Bolivianos), webhooks y reembolsos
- **Mantenimiento**: Solicitudes, técnicos, asignación a proveedores, seguimiento de etapas
- **Inspecciones**: Checklists detallados con fotos y PDFs
- **Gastos**: Registro, categorización y reportes
- **Notificaciones**: Sistema en tiempo real con WebSockets
- **Portal de Propietarios**: Acceso a estados de cuenta, contratos y reportes
- **Portal Público**: Sitio web por subdominio para cada tenant
- **Blacklist**: Lista negra de inquilinos problemáticos
- **Auditoría**: Log completo de acciones

---

## 🛠 Stack Tecnológico

- **Framework**: NestJS 11.0.1
- **Lenguaje**: TypeScript 5.7.3
- **Base de Datos**: PostgreSQL 18 con TypeORM 0.3.28
- **Autenticación**: JWT + Passport
- **Contenedorización**: Docker + Docker Compose
- **Documentación**: Swagger/OpenAPI
- **Arquitectura**: Monolito modular con aislación multi-tenant por schema

---

## 📋 Requisitos Previos

- Node.js 20+
- Docker Desktop
- npm

---

## 🚀 Instalación Rápida

```bash
# 1. Clonar el repositorio
git clone https://github.com/ElJuanDa123456153123/GestionAlquileres_Backend.git
cd GestionAlquileres_Backend

# 2. Instalar dependencias
npm install

# 3. Iniciar PostgreSQL con Docker
docker-compose up -d postgres

# 4. Configurar variables de entorno (opcional, ya viene con .env)
# El .env usa puerto 5433 para PostgreSQL

# 5. Iniciar el servidor
npm run start:dev
```

El backend estará disponible en:
- **API**: http://localhost:3000
- **Health Check**: http://localhost:3000/health
- **Documentación Swagger**: http://localhost:3000/api
- **pgAdmin**: http://localhost:5050

---

## 📚 Estructura del Proyecto

```
src/
├── auth/              # Autenticación JWT y registro
├── tenants/           # Gestión de tenants multi-tenant
├── users/             # Usuarios y permisos
├── properties/        # Propiedades, catálogo y unidades
├── contracts/         # Contratos y renovaciones
├── applications/      # Solicitudes de alquiler
├── payments/          # Pagos y procesadores
├── maintenance/       # Solicitudes de mantenimiento
├── inspections/       # Inspecciones de propiedades
├── expenses/          # Gastos operacionales
├── notifications/     # Sistema de notificaciones
├── employees/         # Gestión de empleados
├── blacklist/         # Lista negra de inquilinos
├── common/            # Configuración, guards, decoradores
│   ├── config/        # Variables de entorno
│   ├── guards/        # Guards de autenticación/autorización
│   ├── decorators/    # Decoradores personalizados
│   ├── tenant/        # Lógica multi-tenant
│   └── storage/       # Sistema de archivos (local/S3)
└── main.ts            # Punto de entrada
```

---

## 🔧 Comandos Disponibles

```bash
# Desarrollo
npm run start:dev       # Iniciar con hot-reload

# Producción
npm run build           # Compilar TypeScript
npm run start:prod      # Iniciar producción

# Calidad
npm run lint:check      # Verificar linting
npm run lint:fix        # Corregir automáticamente
npm test                # Ejecutar tests unitarios
npm run test:e2e        # Tests end-to-end
```

---

## 🗄 Base de Datos

### PostgreSQL con Docker

El proyecto incluye `docker-compose.yml` para levantar PostgreSQL:

```bash
docker-compose up -d postgres
```

**Configuración:**
- Puerto: 5433 (para no conflictuar con PostgreSQL local)
- Usuario: postgres
- Password: postgres
- Base de datos: gestion_alquileres
- pgAdmin: http://localhost:5050

### Arquitectura Multi-tenant

El sistema usa **schemas PostgreSQL** para el aislamiento multi-tenant:
- Cada tenant tiene su propio schema en la misma base de datos
- Las tablas del schema `public` almacenan metadata de tenants
- Los datos de cada tenant están aislados en su schema
- Connection pooling dinámico por tenant

---

## 🔐 Seguridad

- **Autenticación**: JWT con access tokens y refresh tokens
- **Autorización**: RBAC (Role-Based Access Control) con permisos granulares
- **Guards**: Protección de rutas por roles y permisos
- **Rate Limiting**: Protección contra fuerza bruta y DoS
- **Validación**: DTOs con class-validator
- **Sanitización**: Protección contra SQL injection y XSS

---

## 📖 Documentación Completa

La documentación detallada está en la carpeta `docs/`:

- [Guía de desarrollo](docs/getting-started.md)
- [Arquitectura](docs/architecture.md)
- [Configuración](docs/configuration.md)
- [Testing](docs/testing.md)
- [Seguridad](docs/security.md)
- [Producción](docs/ops/production.md)
- [Documentación de módulos](docs/modules/)

---

## 🌐 API Endpoints

### Autenticación
- `POST /auth/register-admin` - Registrar tenant + admin
- `POST /auth/login-admin` - Login administrador
- `POST /auth/:slug/login` - Login usuario
- `POST /auth/:slug/register` - Registro usuario
- `GET /auth/me` - Obtener usuario actual

### Propiedades (Admin)
- `GET /:slug/admin/properties` - Listar propiedades
- `POST /:slug/admin/properties` - Crear propiedad
- `GET /:slug/admin/properties/:id` - Ver detalle
- `PATCH /:slug/admin/properties/:id` - Actualizar
- `DELETE /:slug/admin/properties/:id` - Eliminar

### Contratos
- `GET /:slug/admin/contracts` - Listar contratos
- `POST /:slug/admin/contracts` - Crear contrato
- `GET /:slug/admin/contracts/:id/pdf` - Generar PDF
- `POST /:slug/admin/contracts/:id/renew` - Renovar

### Pagos
- `GET /:slug/admin/payments` - Listar pagos
- `POST /:slug/admin/payments` - Crear pago manual
- `PATCH /:slug/admin/payments/:id/approve` - Aprobar pago
- `POST /:slug/tenant/payments` - Crear pago (inquilino)
- `POST /:slug/tenant/qr-payments` - Generar QR

Y muchos más... ver documentación completa en Swagger: http://localhost:3000/api

---

## 🐳 Docker

### Desarrollo
```bash
docker-compose up -d postgres
```

### Producción
```bash
docker build -t gestion-backend:prod .
docker run -p 3000:3000 gestion-backend:prod
```

---

## 📄 Licencia

Propiedad de 365Soft - Todos los derechos reservados

---

## 👨‍💻 Equipo de Desarrollo

365Soft Bolivia - 2026
