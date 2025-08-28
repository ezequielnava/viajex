# MotoTaxi App - Aplicación de Viajes en Moto-Taxi

Una aplicación completa de viajes en moto-taxi similar a Uber, desarrollada con React Native para el frontend y Django para el backend.

## Características Principales

### Para Usuarios
- ✅ Registro e inicio de sesión (Email, Google, Facebook)
- ✅ Solicitud de viajes con estimación de costo
- ✅ Seguimiento en tiempo real del conductor
- ✅ Pagos en efectivo y Pago Móvil
- ✅ Historial de viajes
- ✅ Sistema de calificaciones y reseñas
- ✅ Chat en tiempo real con el conductor
- ✅ Notificaciones push para actualizaciones de viaje

### Para Conductores
- ✅ Registro con verificación de documentos
- ✅ Control de disponibilidad
- ✅ Aceptación de viajes con notificaciones push
- ✅ Navegación integrada con Google Maps
- ✅ Panel de control de ganancias
- ✅ Chat con pasajeros
- ✅ Seguimiento de ubicación en tiempo real

## Arquitectura Técnica

### Backend (Django)
- **Framework**: Django 4.2 con Django REST Framework
- **Base de datos**: MySQL
- **Autenticación**: JWT + Django Allauth (Google, Facebook)
- **Tiempo real**: Django Channels + WebSockets + Socket.IO
- **APIs**: Google Maps para geolocalización y geocodificación
- **Notificaciones**: Sistema de notificaciones push integrado
- **Tareas asíncronas**: Celery con Redis

### Frontend (React Native)
- **Navegación**: React Navigation v6
- **Estado**: React Context API con hooks personalizados
- **Mapas**: React Native Maps con Google Maps
- **Notificaciones**: React Native Push Notification
- **Tiempo real**: Socket.IO client para WebSockets
- **Geolocalización**: React Native Geolocation Service

## Configuración del Proyecto

### Prerrequisitos
- Python 3.8+
- Node.js 16+
- MySQL 8.0+
- Redis (para WebSockets y Celery)
- Google Maps API Key
- Credenciales de Google/Facebook OAuth

### Configuración del Backend

1. **Clonar el repositorio y navegar al backend**
\`\`\`bash
cd backend
\`\`\`

2. **Crear entorno virtual**
\`\`\`bash
python -m venv venv
source venv/bin/activate  # En Windows: venv\Scripts\activate
\`\`\`

3. **Instalar dependencias**
\`\`\`bash
pip install -r requirements.txt
\`\`\`

4. **Configurar variables de entorno**
\`\`\`bash
cp .env.example .env
# Editar .env con tus credenciales:
# - DATABASE_URL
# - GOOGLE_MAPS_API_KEY
# - GOOGLE_OAUTH2_CLIENT_ID
# - FACEBOOK_APP_ID
# - REDIS_URL
\`\`\`

5. **Configurar base de datos MySQL**
\`\`\`bash
# Ejecutar el script SQL para crear la base de datos
mysql -u root -p < scripts/create_database.sql
\`\`\`

6. **Ejecutar migraciones**
\`\`\`bash
python scripts/run_migrations.py
# O manualmente:
# python manage.py makemigrations
# python manage.py migrate
\`\`\`

7. **Crear superusuario (opcional)**
\`\`\`bash
python manage.py createsuperuser
# Usuario por defecto: admin@mototaxi.com / admin123
\`\`\`

8. **Ejecutar servidor de desarrollo**
\`\`\`bash
python manage.py runserver
\`\`\`

9. **Ejecutar Celery (en terminal separado)**
\`\`\`bash
celery -A ride_app worker -l info
\`\`\`

### Configuración del Frontend (React Native)

1. **Navegar al directorio frontend**
\`\`\`bash
cd frontend
\`\`\`

2. **Instalar dependencias**
\`\`\`bash
npm install
# o
yarn install
\`\`\`

3. **Configurar variables de entorno**
\`\`\`bash
cp .env.example .env
# Configurar:
# - API_BASE_URL=http://localhost:8000/api
# - GOOGLE_MAPS_API_KEY
# - GOOGLE_OAUTH_CLIENT_ID
# - FACEBOOK_APP_ID
\`\`\`

4. **Configurar permisos (Android)**
\`\`\`xml
<!-- android/app/src/main/AndroidManifest.xml -->
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.INTERNET" />
\`\`\`

5. **Ejecutar en iOS**
\`\`\`bash
cd ios && pod install && cd ..
npx react-native run-ios
\`\`\`

6. **Ejecutar en Android**
\`\`\`bash
npx react-native run-android
\`\`\`

## Estructura del Proyecto

\`\`\`
mototaxi-app/
├── backend/
│   ├── ride_app/          # Configuración principal Django
│   ├── users/             # App de usuarios y autenticación
│   ├── drivers/           # App de conductores
│   ├── trips/             # App de viajes y WebSockets
│   ├── notifications/     # Servicio de notificaciones
│   ├── utils/             # Utilidades (geocoding, helpers)
│   ├── scripts/           # Scripts de configuración
│   ├── requirements.txt
│   └── manage.py
├── frontend/
│   ├── src/
│   │   ├── components/    # Componentes reutilizables
│   │   ├── context/       # Contextos globales (Auth, Trip)
│   │   ├── hooks/         # Hooks personalizados (WebSocket)
│   │   ├── navigation/    # Configuración de navegación
│   │   ├── screens/       # Pantallas principales
│   │   ├── services/      # Servicios API y WebSocket
│   │   ├── styles/        # Sistema de diseño
│   │   └── types/         # Tipos TypeScript
│   ├── android/
│   ├── ios/
│   └── package.json
└── README.md
\`\`\`

## APIs Principales

### Autenticación
- `POST /api/auth/register/` - Registro de usuario
- `POST /api/auth/login/` - Inicio de sesión
- `POST /api/auth/refresh/` - Renovar token
- `POST /api/auth/google/` - Login con Google
- `POST /api/auth/facebook/` - Login con Facebook

### Usuarios
- `GET /api/users/profile/` - Perfil del usuario
- `PUT /api/users/profile/` - Actualizar perfil
- `POST /api/users/rating/` - Calificar conductor

### Conductores
- `GET /api/drivers/available/` - Conductores disponibles
- `PUT /api/drivers/status/` - Actualizar disponibilidad
- `POST /api/drivers/register/` - Registro como conductor
- `GET /api/drivers/earnings/` - Ganancias del conductor

### Viajes
- `POST /api/trips/request/` - Solicitar viaje
- `GET /api/trips/current/` - Viaje actual
- `PUT /api/trips/<id>/accept/` - Aceptar viaje (conductor)
- `PUT /api/trips/<id>/start/` - Iniciar viaje
- `PUT /api/trips/<id>/complete/` - Completar viaje
- `PUT /api/trips/<id>/cancel/` - Cancelar viaje
- `GET /api/trips/history/` - Historial de viajes

### WebSocket Events
- `trip_update` - Actualizaciones de estado del viaje
- `driver_location_update` - Ubicación del conductor en tiempo real
- `new_message` - Mensajes de chat
- `trip_request` - Nueva solicitud de viaje (para conductores)

## Funcionalidades Implementadas

### Sistema de Autenticación
- Registro y login con email/password
- Autenticación social (Google, Facebook)
- JWT tokens con refresh automático
- Middleware de autenticación personalizado

### Gestión de Viajes
- Algoritmo de matching conductor-pasajero por proximidad
- Cálculo dinámico de tarifas basado en distancia y tiempo
- Estados de viaje: solicitado → aceptado → conductor llegó → en progreso → completado
- Sistema de cancelación con políticas diferenciadas

### Comunicación en Tiempo Real
- WebSockets con Django Channels y Socket.IO
- Chat bidireccional conductor-pasajero
- Seguimiento de ubicación en tiempo real
- Notificaciones push automáticas

### Sistema de Pagos
- Pago en efectivo
- Integración preparada para Pago Móvil
- Cálculo automático de comisiones
- Historial de transacciones

### Geolocalización
- Integración completa con Google Maps API
- Geocodificación de direcciones
- Cálculo de rutas y distancias
- Seguimiento GPS en tiempo real

## Tecnologías Utilizadas

### Backend
- Django 4.2 + Django REST Framework
- Django Channels (WebSockets)
- MySQL 8.0
- Redis (cache y message broker)
- Celery (tareas asíncronas)
- Django Allauth (OAuth social)
- JWT Authentication
- Google Maps API

### Frontend
- React Native 0.72+
- TypeScript
- React Navigation v6
- React Context API
- Socket.IO Client
- React Native Maps
- React Native Push Notification
- React Native Geolocation Service
- React Native Vector Icons

## Scripts Útiles

### Backend
\`\`\`bash
# Ejecutar migraciones
python scripts/run_migrations.py

# Crear datos de prueba
python manage.py shell < scripts/create_test_data.py

# Ejecutar tests
python manage.py test

# Ejecutar servidor con SSL (producción)
python manage.py runserver_plus --cert-file cert.pem
\`\`\`

### Frontend
\`\`\`bash
# Limpiar cache
npx react-native start --reset-cache

# Generar APK de release
cd android && ./gradlew assembleRelease

# Ejecutar tests
npm test

# Linting
npm run lint
\`\`\`

## Despliegue

### Backend (Producción)
1. Configurar servidor con Ubuntu/CentOS
2. Instalar Nginx, Gunicorn, Redis, MySQL
3. Configurar variables de entorno de producción
4. Ejecutar migraciones y collectstatic
5. Configurar SSL con Let's Encrypt

### Frontend (Distribución)
1. **Android**: Generar APK firmado o subir a Google Play Store
2. **iOS**: Compilar para App Store con Xcode

## Contribución

1. Fork el proyecto
2. Crear una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abrir un Pull Request

## Licencia

Este proyecto está bajo la Licencia MIT. Ver el archivo `LICENSE` para más detalles.

## Soporte

Para soporte técnico o consultas sobre el proyecto:
- Email: soporte@mototaxi.com
- Issues: GitHub Issues
- Documentación: Wiki del proyecto

---

**Desarrollado con ❤️ para revolucionar el transporte urbano**
