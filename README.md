# Laboratorio: Docker + GitHub Actions CI/CD

Este laboratorio demuestra cómo crear un pipeline de CI/CD utilizando GitHub Actions para construir y publicar automáticamente una imagen Docker de una aplicación React.

## 📋 Descripción del Proyecto

Aplicación web React (Rick & Morty) containerizada con Docker y desplegada automáticamente a Docker Hub mediante GitHub Actions.

## 🏗️ Arquitectura del Laboratorio

### Componentes:

1. **Dockerfile**: Imagen multi-stage que construye y sirve la aplicación React
2. **GitHub Actions**: Pipeline automatizado de CI/CD
3. **Docker Hub**: Registro de imágenes Docker

## 📦 Estructura del Proyecto

```
docker-test-main/
├── .github/
│   └── workflows/
│       └── docker-build-push.yml   # GitHub Action workflow
├── public/                          # Archivos públicos de React
├── src/                             # Código fuente de React
├── Dockerfile                       # Definición de la imagen Docker
├── .dockerignore                    # Archivos excluidos del build
├── package.json                     # Dependencias del proyecto
└── README.md                        # Este archivo
```

## 🐳 Dockerfile Explicado

El `Dockerfile` utiliza un **build multi-stage** para optimizar el tamaño de la imagen:

### Etapa 1: Build
- Usa `node:18-alpine` como base
- Instala dependencias con `npm ci`
- Construye la aplicación React (`npm run build`)

### Etapa 2: Producción
- Usa `nginx:alpine` (imagen ligera)
- Copia los archivos estáticos desde la etapa de build
- Expone el puerto 80
- Sirve la aplicación con nginx

## ⚙️ GitHub Actions Workflow

El archivo `.github/workflows/docker-build-push.yml` define el pipeline:

### Pasos del Workflow:

1. **Checkout**: Descarga el código del repositorio
2. **Docker Buildx**: Configura el builder de Docker con capacidades avanzadas
3. **Docker Login**: Autenticación en Docker Hub usando secretos
4. **Build and Push**: Construye la imagen y la sube a Docker Hub

### Triggers:
- Se ejecuta automáticamente en cada `push` a la rama `main`
- También puede ejecutarse manualmente desde GitHub Actions

## 🚀 Instrucciones para Probar el Laboratorio

### Prerequisitos

- Cuenta en [Docker Hub](https://hub.docker.com/)
- Repositorio en GitHub
- Git instalado localmente

### Paso 1: Configurar Secretos en GitHub

1. Ve a tu repositorio en GitHub
2. Click en **Settings** → **Secrets and variables** → **Actions**
3. Click en **New repository secret**
4. Crea dos secretos:
   - `DOCKER_USERNAME`: Tu usuario de Docker Hub
   - `DOCKER_PASSWORD`: Tu token de acceso de Docker Hub (recomendado) o contraseña

> **Nota**: Para crear un token de acceso en Docker Hub:
> - Ve a Account Settings → Security → New Access Token

### Paso 2: Subir el Código a GitHub

```bash
# Inicializar repositorio (si no existe)
git init

# Agregar archivos
git add .

# Hacer commit
git commit -m "Initial commit: Docker + GitHub Actions setup"

# Conectar con tu repositorio remoto
git remote add origin https://github.com/TU_USUARIO/TU_REPOSITORIO.git

# Subir a GitHub
git push -u origin main
```

### Paso 3: Verificar la Ejecución del Workflow

1. Ve a tu repositorio en GitHub
2. Click en la pestaña **Actions**
3. Verás el workflow "Docker Build and Push" ejecutándose
4. Click en el workflow para ver los detalles de cada paso

### Paso 4: Verificar la Imagen en Docker Hub

1. Ve a [Docker Hub](https://hub.docker.com/)
2. Busca tu repositorio `rick-morty-app`
3. Deberías ver las tags `latest` y el SHA del commit

## 🧪 Probar la Imagen Localmente

### Opción 1: Build Local

```bash
# Construir la imagen
docker build -t rick-morty-app:local .

# Ejecutar el contenedor
docker run -d -p 8080:80 rick-morty-app:local

# Abrir en el navegador
# http://localhost:8080
```

### Opción 2: Pull desde Docker Hub

```bash
# Descargar la imagen desde Docker Hub
docker pull TU_USUARIO/rick-morty-app:latest

# Ejecutar el contenedor
docker run -d -p 8080:80 TU_USUARIO/rick-morty-app:latest

# Abrir en el navegador
# http://localhost:8080
```

### Comandos Útiles de Docker

```bash
# Ver contenedores en ejecución
docker ps

# Ver logs del contenedor
docker logs <container_id>

# Detener el contenedor
docker stop <container_id>

# Eliminar el contenedor
docker rm <container_id>

# Ver imágenes locales
docker images

# Eliminar imagen
docker rmi rick-morty-app:local
```

## 📊 Flujo de CI/CD Completo

```
1. Developer hace push a main
         ↓
2. GitHub Actions se activa automáticamente
         ↓
3. Checkout del código
         ↓
4. Configuración de Docker Buildx
         ↓
5. Login a Docker Hub
         ↓
6. Build de la imagen Docker
         ↓
7. Push de la imagen a Docker Hub
         ↓
8. Imagen disponible para deployment
```

## 🔧 Personalización

### Cambiar el nombre de la imagen:

Edita `.github/workflows/docker-build-push.yml` línea 32-33:

```yaml
tags: |
  ${{ secrets.DOCKER_USERNAME }}/TU_NOMBRE_APP:latest
  ${{ secrets.DOCKER_USERNAME }}/TU_NOMBRE_APP:${{ github.sha }}
```

### Agregar más tags:

```yaml
tags: |
  ${{ secrets.DOCKER_USERNAME }}/rick-morty-app:latest
  ${{ secrets.DOCKER_USERNAME }}/rick-morty-app:v1.0.0
  ${{ secrets.DOCKER_USERNAME }}/rick-morty-app:${{ github.sha }}
```

## 📝 Notas Importantes

- **Seguridad**: Nunca subas credenciales en el código. Usa siempre GitHub Secrets
- **Optimización**: El Dockerfile usa multi-stage builds para reducir el tamaño final
- **Cache**: El workflow usa cache de Docker para acelerar builds subsecuentes
- **Tags**: Se crean dos tags automáticamente: `latest` y el SHA del commit

## 🐛 Troubleshooting

### Error: "Invalid username or password"
- Verifica que los secretos `DOCKER_USERNAME` y `DOCKER_PASSWORD` estén correctamente configurados
- Si usas 2FA en Docker Hub, debes usar un Access Token, no tu contraseña

### Error: "denied: requested access to the resource is denied"
- Verifica que el nombre de usuario en los secretos sea correcto
- Asegúrate de que el repositorio en Docker Hub exista o que tengas permisos para crearlo

### El workflow no se ejecuta
- Verifica que el archivo esté en `.github/workflows/`
- Verifica que el push sea a la rama `main`
- Revisa la pestaña Actions en GitHub para ver errores

## 📚 Recursos Adicionales

- [Docker Documentation](https://docs.docker.com/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Docker Build Push Action](https://github.com/docker/build-push-action)
- [Create React App Documentation](https://create-react-app.dev/)

## 👨‍💻 Desarrollo Local

### Instalar dependencias:
```bash
npm install
```

### Ejecutar en modo desarrollo:
```bash
npm run dev
```

### Construir para producción:
```bash
npm run build
```

---

**Laboratorio creado para práctica de Docker + GitHub Actions CI/CD** 🚀
