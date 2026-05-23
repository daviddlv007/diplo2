Sección Complementaria: Integración con Docker
Objetivo Complementario
Introducir el uso de contenedores Docker como parte del flujo DevOps para empaquetar y
ejecutar aplicaciones de manera consistente en diferentes entornos.
Escenario
Luego de completar el pipeline:
CI
↓
Testing
↓
Delivery
el equipo deberá contenerizar la aplicación utilizando Docker.
El objetivo es preparar la aplicación para despliegues modernos y portables.
Herramientas
• Docker Desktop
• Visual Studio / VS Code
• .NET 8 o .NET 9
• Azure DevOps
Actividades Complementarias
Parte 1: Instalar Docker
Cada grupo deberá:
• instalar Docker Desktop,
• verificar funcionamiento.
Validación:
docker --version
Parte 2: Crear Dockerfile
Crear un archivo:
Dockerfile
Ejemplo:
FROM mcr.microsoft.com/dotnet/aspnet:9.0 AS base
WORKDIR /app
EXPOSE 80
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS build
WORKDIR /src
COPY . .
RUN dotnet publish -c Release -o /app/publish
FROM base AS final
WORKDIR /app
COPY --from=build /app/publish .
ENTRYPOINT ["dotnet", "MiAplicacion.dll"]
Objetivo:
Empaquetar la aplicación en un contenedor.
Parte 3: Construir Imagen Docker
Ejecutar:
docker build -t miapp-devops .
Verificar imagen:
docker images
Parte 4: Ejecutar Contenedor
Ejecutar:
docker run -d -p 8080:80 miapp-devops
Verificar ejecución:
docker ps
Acceder desde navegador:
http://localhost:8080
Parte 5: Integrar Docker al Pipeline
Agregar un step adicional en el stage Delivery:
- script: |
 docker build -t miapp-devops .
 displayName: 'Build Docker Image'
Objetivo:
Automatizar creación de contenedores.
Parte 6: Reflexión DevOps
Discutir:
1. ¿Qué ventajas ofrece Docker?
2. ¿Cómo ayuda a DevOps?
3. ¿Qué problemas evita la contenerización?
4. ¿Qué diferencia existe entre VM y contenedores
Resultados Esperados
Al finalizar esta sección, los estudiantes podrán:
Crear contenedores Docker
Ejecutar aplicaciones contenerizadas
Integrar Docker en pipelines DevOps
Comprender portabilidad y consistencia de entornos
Concepto Clave
Docker permite empaquetar aplicaciones y dependencias en contenedores ligeros,
facilitando despliegues consistentes y automatizados dentro de DevOps.