# Frontend DevOps

Interfaz web para gestión de Inventario y Tickets desarrollada con React + Vite.

## 🚀 Descripción
Aplicación frontend para la solución Innovatech Chile. Proporciona una interfaz moderna y responsiva para la gestión de inventario y sistema de tickets.

## 📋 Requisitos
- Node.js v18+ (para desarrollo)
- Docker y Docker Compose
- npm o yarn

## 🛠️ Stack Tecnológico
- **React** ^18.3.1
- **Vite** ^5.4.10
- **CSS** personalizado

## 📦 Instalación

### Modo Desarrollo (sin Docker)
```bash
npm install
npm run dev
```

### Con Docker Compose
```bash
docker-compose up --build
```

## 🔨 Comandos Disponibles
```bash
npm run dev      # Inicia servidor de desarrollo
npm run build    # Compila para producción
npm run preview  # Preview del build
```

## 🌍 Puerto
- **80** (HTTP) - Acceso a la aplicación
- **5173** (Dev) - Servidor de desarrollo Vite

## 🔐 Variables de Entorno
```
VITE_API_URL=http://10.0.2.210:3001
```

## 📡 Rutas de API Consumidas
- `GET /api/dashboard` - Dashboard principal
- `GET /api/items` - Obtener inventario
- `POST /api/items` - Crear nuevo item
- `GET /api/tickets` - Obtener tickets
- `POST /api/tickets` - Crear nuevo ticket

## 📂 Estructura del Proyecto
```
src/
├── api.js          # Configuración de API
├── App.jsx         # Componente principal
├── main.jsx        # Punto de entrada
└── styles.css      # Estilos globales
```

## 🐳 Docker
La aplicación está configurada con Nginx para servir archivos estáticos en producción.

## 🚀 Deploy
Push a rama `deploy` dispara GitHub Actions que:
1. Compila imagen React
2. Push a ECR
3. Despliega en EC2 via SSM

## 📝 Notas
- El archivo `nginx.conf` contiene la configuración de Nginx
- El archivo `vite.config.js` contiene la configuración de Vite
- Accesible en: http://98.88.43.196
- Puerto: 80

## Estructura

- `src/` - Código React
- `Dockerfile` - Multi-stage build (Node + Nginx)
- `.github/workflows/frontend-deploy.yml` - CI/CD

## Deployment

Automatizado vía GitHub Actions en push a rama `deploy`.

### Flujo de Deployment

1. GitHub Actions obtiene el código
2. Compilar imagen Docker
3. Hacer push a AWS ECR
4. Enviar comando SSM a instancia EC2
5. EC2 descarga la imagen más reciente e inicia el contenedor

### Deployment Manual

```bash
# Push a rama deploy
git checkout deploy
git commit --allow-empty -m "Desplegar frontend"
git push origin deploy

# Monitorear en GitHub Actions
```

## Integración API

El frontend se comunica con la API del backend en `VITE_API_URL`.

Endpoints principales:
- GET /api/products - Obtener todos los productos
- POST /api/products - Crear producto
- GET /api/tickets - Obtener todos los tickets
- POST /api/tickets - Crear ticket

## Configuración Nginx

Configurado para aplicación de una sola página (SPA) con React Router:

- Todas las solicitudes enrutan a index.html
- Assets caché con hashes de versión
- Compresión gzip habilitada
- Headers CORS configurados

## Solución de Problemas

### Frontend carga pero llamadas API fallan

Verificar:
- Contenedor Backend está corriendo: `docker ps`
- Logs del Backend: `docker logs innovatech_backend`
- Conectividad de red entre Frontend y Backend
- Variable de entorno VITE_API_URL es correcta

### Imágenes se muestran rotas

- Verificar que los assets estén en la salida compilada
- Revisar configuración de Nginx para rutas de assets correctas
- Revisar consola del navegador para errores 404

### Deployment SSM falla

- Verificar que labrole tenga política AmazonSSMManagedInstanceCore
- Verificar SSM Agent está corriendo: `sudo systemctl status amazon-ssm-agent`
- Revisar logs de GitHub Actions para error detallado

## Pipeline CI/CD

GitHub Actions workflow automáticamente:
- Compila imagen Docker en push a deploy
- Ejecuta docker build con Node 18
- Hace push a repositorio AWS ECR
- Envía comando a EC2 vía AWS Systems Manager
- Descarga imagen e inicia contenedor

## Seguridad

- Secrets almacenados en GitHub Secrets
- Credenciales AWS usan roles IAM
- SSM Agent maneja comunicación segura a EC2
- Sin claves SSH expuestas
- Variables de entorno inyectadas en tiempo de ejecución

## Rendimiento

- Compilación multi-etapa reduce tamaño de imagen
- Nginx sirve archivos estáticos eficientemente
- Compresión gzip habilitada
- Caché del navegador configurado
- Vite optimiza compilación React

## Monitoreo

Verificar estado de la aplicación:

```bash
# Vía sesión SSM
docker logs innovatech_frontend

# Verificar contenedores en ejecución
docker ps

# Verificar vinculación de puerto
docker port innovatech_frontend
```

=======
Interfaz web para gestión de Inventario y Tickets desarrollada con React + Vite.

## 🚀 Descripción
Aplicación frontend para la solución Innovatech Chile. Proporciona una interfaz moderna y responsiva para la gestión de inventario y sistema de tickets.

## 📋 Requisitos
- Node.js v18+ (para desarrollo)
- Docker y Docker Compose
- npm o yarn

## 🛠️ Stack Tecnológico
- **React** ^18.3.1
- **Vite** ^5.4.10
- **CSS** personalizado

## 📦 Instalación

### Modo Desarrollo (sin Docker)
```bash
npm install
npm run dev
```

### Con Docker Compose
```bash
docker-compose up --build
```

## 🔨 Comandos Disponibles
```bash
npm run dev      # Inicia servidor de desarrollo
npm run build    # Compila para producción
npm run preview  # Preview del build
```

## 🌍 Puerto
- **80** (HTTP) - Acceso a la aplicación
- **5173** (Dev) - Servidor de desarrollo Vite

## 🔐 Variables de Entorno
```
VITE_API_URL=http://localhost:3001
```

## 📡 Rutas de API Consumidas
- `GET /api/dashboard` - Dashboard principal
- `GET /api/items` - Obtener inventario
- `POST /api/items` - Crear nuevo item
- `GET /api/tickets` - Obtener tickets
- `POST /api/tickets` - Crear nuevo ticket

## 📂 Estructura del Proyecto
```
src/
├── api.js          # Configuración de API
├── App.jsx         # Componente principal
├── main.jsx        # Punto de entrada
└── styles.css      # Estilos globales
```

## 🐳 Docker
La aplicación está configurada con Nginx para servir archivos estáticos en producción.

## 📝 Notas
- El archivo `nginx.conf` contiene la configuración de Nginx
- El archivo `vite.config.js` contiene la configuración de Vite
>>>>>>> Stashed changes
