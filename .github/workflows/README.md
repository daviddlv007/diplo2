# GitHub Actions Workflows - Documentación DevOps

Este directorio contiene los workflows de GitHub Actions que implementan el pipeline DevOps con Docker descrito en `tarea.md`.

## 📋 Workflows Disponibles

### 1. **docker-build.yml** - Docker Build & Test
**Propósito**: Construcción y validación rápida de la imagen Docker.

**Qué hace**:
- ✓ Construye la imagen Docker (`docker build -t miapp-devops .`)
- ✓ Verifica que la imagen se creó correctamente
- ✓ Ejecuta un contenedor para validar que funciona
- ✓ Valida que la aplicación está corriendo

**Se activa con**:
- Push a `main` o `develop`
- Cambios en: `Dockerfile`, archivos `.cs`, `.csproj`
- Pull requests

**Resultado**: Imagen Docker verificada y funcional

---

### 2. **docker-devops-pipeline.yml** - DevOps Pipeline with Docker
**Propósito**: Pipeline completo con tres etapas: Build & Test, Docker Build, Deploy.

**Etapas**:

#### Etapa 1: Build and Test
- Checkout del código
- Setup de .NET 8
- Restaurar dependencias (`dotnet restore`)
- Build de la aplicación (`dotnet build`)
- Ejecución de tests (`dotnet test`)

#### Etapa 2: Build Docker Image (necesita pasar Etapa 1)
- Construye la imagen: `docker build -t miapp-devops:latest -t miapp-devops:$SHA .`
- Verifica la imagen con `docker inspect`
- Ejecuta un contenedor de prueba
- Valida que el contenedor está corriendo

#### Etapa 3: Push Docker Image (solo en pushes a main/develop)
- Reconstruye la imagen con tags
- Muestra información del build

**Se activa con**: Push a `main`/`develop` o Pull requests

---

### 3. **cicd-pipeline.yml** - Full CI/CD Pipeline
**Propósito**: Pipeline completo con modelo CI/Testing/Delivery como se describe en `tarea.md`.

**Etapas (modelo DevOps tradicional)**:

```
CI → Testing → Delivery → Summary
```

#### CI (Continuous Integration)
- Build y unit tests
- Verificación de código

#### Testing
- Integration tests
- Validaciones funcionales

#### Delivery
- Build de imagen Docker: `docker build -t miapp-devops:latest -t miapp-devops:$SHA .`
- Validación de imagen
- Ejecución y prueba del contenedor
- Health checks

#### Summary
- Reporte final de ejecución

**Se activa con**:
- Push a cualquier rama
- Workflow dispatch manual
- Pull requests

---

## 🚀 Cómo Usar

### Verificar los workflows
1. Ve a la pestaña **Actions** en tu repositorio de GitHub
2. Verás los tres workflows listados:
   - `Docker Build & Test`
   - `DevOps Pipeline with Docker`
   - `Full CI/CD Pipeline`

### Ejecutar un workflow manualmente
```bash
# El pipeline Full CI/CD puede ejecutarse manualmente en GitHub
# Actions > Full CI/CD Pipeline > Run workflow
```

### Ver logs de ejecución
1. Ve a **Actions**
2. Selecciona el workflow que deseas revisar
3. Haz clic en la ejecución específica
4. Expande cada step para ver logs detallados

---

## 📦 Comandos Docker Ejecutados

Los workflows ejecutan estos comandos equivalentes a lo especificado en `tarea.md`:

```bash
# Parte 3: Construir Imagen Docker
docker build -t miapp-devops .

# Parte 4: Ejecutar Contenedor
docker run -d -p 8080:80 miapp-devops
docker ps

# Verificación
docker images
docker inspect miapp-devops
```

---

## ✅ Checklist de Cumplimiento de tarea.md

- [x] **Parte 1**: Verificación de Docker (en runners de GitHub)
- [x] **Parte 2**: Dockerfile existente en repositorio
- [x] **Parte 3**: Construcción de imagen (`docker build -t miapp-devops .`)
- [x] **Parte 4**: Ejecución de contenedor (`docker run -d -p 8080:...`)
- [x] **Parte 5**: Integración en Pipeline (Steps en workflow)
- [x] **Parte 6**: Workflows automatizados como DevOps

---

## 🔧 Configuración

### Variables de Entorno Disponibles
```yaml
DOCKER_IMAGE_NAME: miapp-devops
REGISTRY: ghcr.io  # Para futuras integraciones
```

### Triggers Configurados
- `push` a main/develop
- `pull_request` a main/develop  
- `workflow_dispatch` (ejecución manual)

---

## 📊 Monitoreo

### Métricas por Workflow

| Workflow | Tiempo | Complejidad | Recomendación |
|----------|--------|-------------|---------------|
| docker-build.yml | ~2-3 min | Baja | Para cambios en Dockerfile |
| docker-devops-pipeline.yml | ~5-8 min | Media | Pipeline estándar |
| cicd-pipeline.yml | ~8-12 min | Alta | Pipeline completo con tests |

---

## 🐳 Próximos Pasos (Opcional)

Para completar la integración de DevOps:

1. **Registry Docker**: Integrar con Docker Hub o GitHub Container Registry
   ```yaml
   - uses: docker/login-action@v2
   ```

2. **Azure DevOps**: Replicar estos workflows en Azure Pipelines si es necesario

3. **Notificaciones**: Agregar notificaciones en Slack/Teams en caso de fallo

4. **Security Scanning**: Agregar escaneo de vulnerabilidades en imágenes

---

## 📝 Ejemplo de Ejecución

Cuando hagas un `git push` a la rama `develop`:

1. GitHub Actions se dispara automáticamente
2. El workflow `docker-devops-pipeline.yml` se ejecuta
3. Se ejecutan los 3 jobs en paralelo/secuencia:
   - ✓ Build and test (.NET)
   - ✓ Build Docker image
   - ✓ Push Docker image (si es main/develop)
4. Recibes notificación del resultado

---

Para más información, consulta [GitHub Actions Documentation](https://docs.github.com/en/actions)
