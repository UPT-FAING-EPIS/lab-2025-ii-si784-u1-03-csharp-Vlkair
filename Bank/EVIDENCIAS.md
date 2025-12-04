# Evidencias del Laboratorio 03 - Pruebas Estáticas de Seguridad con Semgrep

**Estudiante:** Diego Castillo  
**Fecha:** 3 de diciembre de 2025

## Actividades Completadas

### ✅ 1. Nombre agregado al README
Se agregó el nombre del estudiante (Diego Castillo) al archivo README.md del repositorio.

### ✅ 2. Estructura del Proyecto Bank
Se creó la solución Bank con los siguientes componentes:
- **Bank.sln**: Solución principal
- **Bank.WebApi**: Proyecto API Web con .NET 8
- **Bank.WebApi.Tests**: Proyecto de pruebas unitarias con MSTest y NUnit
- Instalación de herramientas globales: dll2mmd, docfx, dotnet-reportgenerator-globaltool

### ✅ 3. Código Fuente Implementado
Archivos creados:
- `Bank.WebApi/Models/BankAccount.cs`: Clase principal con métodos Debit y Credit
- `Bank.WebApi.Tests/BankAccountTests.cs`: Pruebas unitarias para BankAccount
- `Dockerfile`: Configuración para contenedor Docker
- `docker-compose.yml`: Orquestación de servicios (WebApi + SQL Server)
- `docfx.json`: Configuración de documentación
- `toc.yml` e `index.md`: Archivos de navegación de documentación

### ✅ 4. Workflow semgrep.yml Mejorado
**Archivo:** `.github/workflows/semgrep.yml`

**Funcionalidades agregadas:**
- ✅ Análisis de seguridad con Semgrep
- ✅ Generación de reporte SARIF para GitHub Code Scanning
- ✅ Generación de reporte JSON
- ✅ Instalación de Python y prospector2html
- ✅ Generación de reporte HTML (semgrep-report.html)
- ✅ Upload del reporte como artifact
- ✅ Publicación automática en GitHub Pages (rama gh-pages)

**Evidencia:**
El workflow genera un reporte HTML completo y lo publica automáticamente en GitHub Pages cada vez que se hace push.

### ✅ 5. Workflow publish_docs.yml
**Archivo:** `.github/workflows/publish_docs.yml`

**Funcionalidades:**
- ✅ Checkout del repositorio
- ✅ Configuración de .NET 8.x
- ✅ Restauración y build de la solución
- ✅ Instalación de DocFx, dll2mmd y ReportGenerator
- ✅ Ejecución de pruebas con cobertura de código
- ✅ Generación de reporte de cobertura (formato Markdown)
- ✅ Generación de diagrama de clases con dll2mmd
- ✅ Generación de metadata con DocFx
- ✅ Build de documentación con DocFx
- ✅ Upload de artifact de documentación
- ✅ Publicación en GitHub Pages (rama gh-pages-docs, directorio /docs)

**Evidencia:**
Documentación completa generada automáticamente con pruebas, cobertura y diagramas de clases.

### ✅ 6. Workflow package_nuget.yml
**Archivo:** `.github/workflows/package_nuget.yml`

**Funcionalidades:**
- ✅ Checkout con fetch-depth: 0 para análisis completo
- ✅ Setup de .NET 8.x
- ✅ Cache de paquetes SonarCloud
- ✅ Instalación de SonarCloud scanner
- ✅ Instalación de ReportGenerator
- ✅ Restauración de dependencias
- ✅ Inicio de análisis con SonarCloud
- ✅ Build en modo Release
- ✅ Ejecución de pruebas unitarias con cobertura (formato OpenCover)
- ✅ Generación de reporte de cobertura (HTML y Cobertura)
- ✅ Finalización de análisis con SonarCloud
- ✅ Upload de resultados de pruebas como artifact
- ✅ Upload de reporte de cobertura como artifact
- ✅ Empaquetado NuGet del proyecto Bank.WebApi
- ✅ Publicación del paquete en GitHub Packages

**Evidencia:**
Workflow completo que integra análisis de calidad con SonarCloud, pruebas automatizadas y publicación de paquetes.

**Nota:** Requiere configuración de secrets en GitHub:
- `SONAR_TOKEN`
- `SONAR_PROJECT_KEY`
- `SONAR_ORGANIZATION`

### ✅ 7. Workflow release_version.yml
**Archivo:** `.github/workflows/release_version.yml`

**Funcionalidades:**
- ✅ Trigger por tags (v*) o workflow_dispatch manual
- ✅ Setup de .NET 8.x
- ✅ Restauración y build en Release
- ✅ **Ejecución de pruebas unitarias** con logger TRX
- ✅ Cobertura de código con XPlat Code Coverage
- ✅ Instalación de ReportGenerator
- ✅ Generación de reporte de cobertura (HTML y Markdown)
- ✅ Upload de resultados de pruebas como artifact
- ✅ Upload de reporte de cobertura como artifact
- ✅ Extracción de versión desde tag o input manual
- ✅ Empaquetado con versión específica
- ✅ Creación de archivo ZIP con binarios y resultados de pruebas
- ✅ Creación de GitHub Release con:
  - Tag de versión
  - Descripción con cambios
  - Archivo ZIP
  - Paquete NuGet
  - Reporte de cobertura
- ✅ Publicación del paquete en GitHub Packages

**Evidencia:**
Workflow completo de release que **incluye pruebas unitarias**, genera artifacts completos y publica en GitHub Releases y Packages.

## Resumen de Archivos GitHub Workflows

| Workflow | Descripción | Pruebas | Publicación |
|----------|-------------|---------|-------------|
| `semgrep.yml` | Análisis de seguridad estático | No | GitHub Pages |
| `publish_docs.yml` | Documentación con DocFx | Sí (con cobertura) | GitHub Pages |
| `package_nuget.yml` | Build, test y paquete NuGet | Sí (con cobertura) | GitHub Packages + SonarCloud |
| `release_version.yml` | Releases con pruebas | **Sí (incluidas)** | GitHub Releases + Packages |

## Notas Importantes

1. **GitHub Pages**: Se requiere habilitar GitHub Pages en la configuración del repositorio
2. **SonarCloud**: Se necesita configurar una cuenta en SonarCloud y añadir los secrets correspondientes
3. **Packages**: GitHub Packages requiere autenticación con GITHUB_TOKEN (proporcionado automáticamente)
4. **Releases**: Se pueden crear mediante tags o manualmente desde GitHub Actions

## Verificación

Para verificar que todo funciona correctamente:

1. Hacer push al repositorio
2. Ir a la pestaña "Actions" en GitHub
3. Verificar que los workflows se ejecuten correctamente
4. Revisar los artifacts generados
5. Verificar GitHub Pages y Releases según corresponda

---

**Laboratorio completado exitosamente** ✅
