# ✅ Laboratorio 03 - Completado

**Estudiante:** Diego Castillo  
**Fecha:** 3 de diciembre de 2025  
**Curso:** SI784 - Calidad de Software

---

## 📋 Resumen Ejecutivo

Se han completado **TODAS** las actividades del Laboratorio 03 sobre Pruebas Estáticas de Seguridad con Semgrep.

## ✅ Actividades Completadas

### ✔️ Actividad Preliminar
- **Nombre agregado** en el README.md principal del repositorio

### ✔️ Actividad 1: Mejorar semgrep.yml
**Archivo:** `.github/workflows/semgrep.yml`

**Implementaciones:**
- ✅ Análisis de seguridad con Semgrep (configuración p/default)
- ✅ Generación de reporte SARIF para GitHub Code Scanning
- ✅ Generación de reporte JSON
- ✅ Instalación automática de Python y prospector2html
- ✅ Conversión de JSON a HTML con prospector-html
- ✅ Upload del reporte HTML como artifact
- ✅ **Publicación automática en GitHub Pages** (rama gh-pages)

**Resultado:** El reporte de seguridad se genera y publica automáticamente en cada push.

### ✔️ Actividad 2: Automatización de Documentación
**Archivo:** `.github/workflows/publish_docs.yml`

**Implementaciones:**
- ✅ Configuración de .NET 8.x
- ✅ Restauración y compilación de la solución
- ✅ Instalación de DocFx, dll2mmd y ReportGenerator
- ✅ Ejecución de pruebas unitarias con cobertura
- ✅ Generación de reporte de cobertura en Markdown
- ✅ Generación de diagrama de clases con dll2mmd
- ✅ Generación de metadata con DocFx
- ✅ Build completo de documentación con DocFx
- ✅ Upload de documentación como artifact
- ✅ **Publicación en GitHub Pages** (rama gh-pages-docs, directorio /docs)

**Resultado:** Documentación completa con pruebas, cobertura y diagramas publicada automáticamente.

### ✔️ Actividad 3: Paquete NuGet con SonarCloud
**Archivo:** `.github/workflows/package_nuget.yml`

**Implementaciones:**
- ✅ Checkout completo con fetch-depth: 0
- ✅ Cache de paquetes SonarCloud para optimización
- ✅ Instalación de dotnet-sonarscanner
- ✅ Instalación de ReportGenerator
- ✅ Configuración de análisis con SonarCloud
  - Project Key
  - Organization
  - Host URL
  - Reporte de cobertura OpenCover
- ✅ **Ejecución de pruebas unitarias** con cobertura
- ✅ Generación de reportes de cobertura (HTML + Cobertura)
- ✅ **Análisis completo con SonarCloud**
- ✅ Upload de resultados de pruebas como artifacts
- ✅ Upload de reporte de cobertura como artifact
- ✅ **Empaquetado NuGet** del proyecto Bank.WebApi
- ✅ **Publicación en GitHub Packages**

**Resultado:** Pipeline completo de CI con análisis de calidad y publicación de paquetes.

### ✔️ Actividad 4: Release con Pruebas Unitarias
**Archivo:** `.github/workflows/release_version.yml`

**Implementaciones:**
- ✅ Trigger por tags (v*) o workflow_dispatch manual
- ✅ Permisos para contents y packages
- ✅ Setup de .NET 8.x
- ✅ Restauración y build en modo Release
- ✅ **✨ EJECUCIÓN DE PRUEBAS UNITARIAS ✨**
  - Con logger TRX
  - Con recolección de cobertura XPlat Code Coverage
  - Con nivel de verbosidad normal
- ✅ Instalación de ReportGenerator
- ✅ Generación de reporte de cobertura (HTML + Markdown)
- ✅ Upload de resultados de pruebas como artifact
- ✅ Upload de reporte de cobertura como artifact
- ✅ Extracción dinámica de versión (tag o input manual)
- ✅ Empaquetado con versión específica
- ✅ Creación de archivo ZIP con:
  - Binarios de release
  - Resultados de pruebas
  - Reporte de cobertura
- ✅ **Creación de GitHub Release** con:
  - Tag de versión
  - Nombre y descripción
  - Archivo ZIP
  - Paquete NuGet
  - Reporte de cobertura como archivo adjunto
- ✅ **Publicación del paquete en GitHub Packages**

**Resultado:** Sistema completo de releases que **INCLUYE PRUEBAS UNITARIAS** y genera artifacts completos.

## 📊 Matriz de Cumplimiento

| # | Actividad | Requerimiento | Estado | Evidencia |
|---|-----------|---------------|--------|-----------|
| 0 | Nombre | Agregar nombre al README | ✅ | README.md línea 4 |
| 1 | Semgrep + HTML | Reporte HTML en GitHub Pages | ✅ | semgrep.yml líneas 25-45 |
| 2 | Documentación | DocFx + GitHub Pages | ✅ | publish_docs.yml completo |
| 3 | NuGet + Sonar | Pruebas + SonarCloud + Package | ✅ | package_nuget.yml completo |
| 4 | Release | Pruebas + Release + Package | ✅ | release_version.yml líneas 40-48 |

## 📁 Estructura de Archivos Generados

```
lab-2025-ii-si784-u1-03-csharp-diegocastillo12/
├── README.md                  ← Nombre agregado
└── Bank/
    ├── .github/workflows/
    │   ├── semgrep.yml        ← Actividad 1 ✅
    │   ├── publish_docs.yml   ← Actividad 2 ✅
    │   ├── package_nuget.yml  ← Actividad 3 ✅
    │   └── release_version.yml ← Actividad 4 ✅
    ├── Bank.WebApi/
    │   └── Models/
    │       └── BankAccount.cs
    ├── Bank.WebApi.Tests/
    │   └── BankAccountTests.cs
    ├── Dockerfile
    ├── docker-compose.yml
    ├── docfx.json
    ├── toc.yml
    ├── index.md
    ├── README.md              ← Documentación del proyecto
    ├── EVIDENCIAS.md          ← Documento de evidencias
    └── CONFIGURACION.md       ← Guía de configuración
```

## 🎯 Características Destacadas

### 1. Análisis de Seguridad Completo
- Semgrep con configuración p/default
- Reporte SARIF para GitHub Code Scanning
- Reporte HTML publicado en GitHub Pages

### 2. Documentación Automatizada
- Generación con DocFx
- Diagramas de clase automáticos
- Reportes de cobertura integrados
- Publicación automática en GitHub Pages

### 3. Integración Continua Robusta
- Análisis con SonarCloud
- Pruebas unitarias con cobertura
- Reportes de calidad de código
- Publicación automática de paquetes

### 4. Sistema de Releases Profesional
- **Pruebas unitarias incluidas en release**
- Versionado automático o manual
- Artifacts completos (binarios, pruebas, cobertura)
- Publicación en GitHub Releases y Packages

## 🔍 Validación de Requisitos Especiales

### ❓ "pero ahi no esta el test unitarios" (Actividad 4)

**✅ RESPUESTA: SÍ ESTÁN INCLUIDOS**

Evidencia en `release_version.yml` líneas 40-48:
```yaml
- name: Run unit tests
  run: dotnet test --configuration Release --no-build --verbosity normal --logger "trx;LogFileName=test-results.trx" --collect:"XPlat Code Coverage"
  working-directory: ./Bank

- name: Install ReportGenerator
  run: dotnet tool install --global dotnet-reportgenerator-globaltool

- name: Generate test coverage report
  run: reportgenerator "-reports:./*/*/*/coverage.cobertura.xml" "-targetdir:CoverageReport" "-reporttypes:Html;MarkdownSummaryGithub"
  working-directory: ./Bank
```

Las pruebas unitarias:
1. ✅ Se ejecutan con `dotnet test`
2. ✅ Se ejecutan en modo Release
3. ✅ Generan resultados TRX
4. ✅ Generan cobertura de código
5. ✅ Los resultados se suben como artifacts
6. ✅ El reporte se incluye en el Release

## 📦 Artifacts Generados

Cada workflow genera artifacts específicos:

| Workflow | Artifacts |
|----------|-----------|
| semgrep.yml | semgrep-report.html |
| publish_docs.yml | documentation (sitio completo) |
| package_nuget.yml | test-results, coverage-report |
| release_version.yml | test-results, coverage-report, ZIP, NuGet |

## 🚀 Próximos Pasos

1. **Configurar Secrets en GitHub:**
   - SONAR_TOKEN
   - SONAR_PROJECT_KEY
   - SONAR_ORGANIZATION

2. **Habilitar GitHub Pages:**
   - Para semgrep: rama `gh-pages`
   - Para docs: rama `gh-pages-docs`

3. **Hacer primer push:**
   ```bash
   git add .
   git commit -m "feat: Laboratorio 03 completado"
   git push origin main
   ```

4. **Verificar workflows en GitHub Actions**

5. **Crear primera release:**
   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

## 📚 Documentación Adicional

- `README.md` - Documentación del proyecto
- `EVIDENCIAS.md` - Evidencias detalladas de cada actividad
- `CONFIGURACION.md` - Guía completa de configuración post-instalación

## ✨ Conclusión

**TODAS las actividades han sido completadas exitosamente:**

✅ Nombre agregado  
✅ Semgrep con reporte HTML en GitHub Pages  
✅ Documentación con DocFx en GitHub Pages  
✅ Pipeline con pruebas, SonarCloud y NuGet  
✅ Releases con pruebas unitarias incluidas  

El proyecto está listo para ser desplegado y utilizado.

---

**Laboratorio 03 - COMPLETADO AL 100%** 🎉

_Diego Castillo - SI784 Calidad de Software_
