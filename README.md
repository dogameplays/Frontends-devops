# Frontends-devops

Frontend React + Vite para Innovatech Inventory System

## Setup Local

```bash
npm install
npm run dev          # Desarrollo
npm run build        # Producción
```

## Docker

```bash
docker build -t innovatech-frontend .
docker run -p 80:80 -e VITE_API_URL=http://10.0.2.210:3001 innovatech-frontend
```

## Deploy

Push a rama `deploy` dispara GitHub Actions que:
1. Compila imagen React
2. Push a ECR
3. Despliega en EC2 via SSM

## Variables Importantes

- `VITE_API_URL=http://10.0.2.210:3001` (Backend API)
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

