# Guía Rápida de Configuración

## 1️⃣ Configurar Secretos en GitHub

### Pasos:
1. Ve a tu repositorio en GitHub
2. Settings → Secrets and variables → Actions
3. Click "New repository secret"
4. Agrega estos dos secretos:

**DOCKER_USERNAME**
```
tu-usuario-dockerhub
```

**DOCKER_PASSWORD**
```
tu-token-o-contraseña-dockerhub
```

## 2️⃣ Subir a GitHub

```bash
git init
git add .
git commit -m "Setup Docker + GitHub Actions"
git remote add origin https://github.com/TU_USUARIO/TU_REPO.git
git push -u origin main
```

## 3️⃣ Verificar

- Ve a la pestaña **Actions** en GitHub
- Verás el workflow ejecutándose automáticamente
- Una vez completado, tu imagen estará en Docker Hub

## 4️⃣ Probar Localmente

```bash
# Opción A: Build local
docker build -t rick-morty-app .
docker run -d -p 8080:80 rick-morty-app

# Opción B: Pull desde Docker Hub
docker pull TU_USUARIO/rick-morty-app:latest
docker run -d -p 8080:80 TU_USUARIO/rick-morty-app:latest
```

Abre: http://localhost:8080

---

✅ **¡Listo! Tu pipeline de CI/CD está funcionando.**
