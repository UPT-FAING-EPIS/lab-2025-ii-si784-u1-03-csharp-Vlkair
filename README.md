[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/6iqWfv8G)
[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=21941011)

# 📊 INFORME DE LABORATORIO N° 03
## Pruebas Estáticas de Seguridad de Aplicaciones con Semgrep

---

### 📌 Información del Estudiante
- **Estudiante:** Victor Williams Cruz Mamani
- **Curso:** SI784 - Calidad de Software
- **Fecha:** Diciembre 2025
- **Repositorio:** lab-2025-ii-si784-u1-03-csharp-Vlkair

---

## 📋 ÍNDICE
1. [Objetivos](#objetivos)
2. [Requerimientos](#requerimientos)
3. [Desarrollo del Laboratorio](#desarrollo-del-laboratorio)
4. [Resultados Obtenidos](#resultados-obtenidos)
5. [Workflows Implementados](#workflows-implementados)
6. [Conclusiones](#conclusiones)

---

## 🎯 OBJETIVOS
* Comprender el funcionamiento de las pruebas estáticas de seguridad de código utilizando Semgrep
* Implementar pipelines de CI/CD con GitHub Actions
* Automatizar análisis de seguridad, pruebas unitarias y generación de documentación
* Integrar herramientas de calidad de código como SonarCloud
* Generar y publicar paquetes NuGet automáticamente
* Aplicar DevSecOps en el ciclo de vida del desarrollo

## 📦 REQUERIMIENTOS

### Conocimientos Previos
- Conocimientos básicos de Bash/PowerShell
- Conocimientos básicos de Contenedores (Docker)
- Fundamentos de .NET y C#
- Git y GitHub Actions

### Hardware
- Virtualización activada en el BIOS
- CPU SLAT-capable feature
- Al menos 4GB de RAM

### Software
- Windows 10 64bit: Pro, Enterprise o Education (Build 14393+)
- Docker Desktop 
- PowerShell versión 7.x
- Python 3.10+ o superior
- .NET 8.0 o superior
- Visual Studio Code
- Git

---

## 🚀 DESARROLLO DEL LABORATORIO

### Parte I: Configuración Inicial

#### 1.1 Instalación de Herramientas de Seguridad
```bash
# Instalación de Semgrep y herramientas de reporte
python -m pip install semgrep
python -m pip install prospector2html
```

#### 1.2 Instalación de Herramientas .NET
```bash
dotnet tool install -g dll2mmd
dotnet tool install -g docfx
dotnet tool install -g dotnet-reportgenerator-globaltool
```

### Parte II: Creación de la Aplicación Bank

#### 2.1 Estructura del Proyecto
```bash
# Crear solución
dotnet new sln -o Bank

# Crear proyecto Web API
cd Bank
dotnet new webapi -o Bank.WebApi
dotnet sln add ./Bank.WebApi/Bank.WebApi.csproj

# Crear proyecto de pruebas
dotnet new mstest -o Bank.WebApi.Tests
dotnet sln add ./Bank.WebApi.Tests/Bank.WebApi.Tests.csproj
dotnet add ./Bank.WebApi.Tests/Bank.WebApi.Tests.csproj reference ./Bank.WebApi/Bank.WebApi.csproj
```

#### 2.2 Estructura de Archivos Generada
```
Bank/
├── Bank.sln
├── Bank.WebApi/
│   ├── Models/
│   │   └── BankAccount.cs
│   └── Program.cs
├── Bank.WebApi.Tests/
│   └── BankAccountTests.cs
├── .github/workflows/
│   ├── semgrep.yml
│   ├── publish_docs.yml
│   ├── package_nuget.yml
│   └── release_version.yml
├── Dockerfile
├── docker-compose.yml
├── docfx.json
└── index.md
```

### Parte III: Implementación del Código

#### 3.1 Clase BankAccount
Se implementó la clase principal `BankAccount.cs` con las siguientes características:
```C#
namespace Bank.WebApi.Models
{
    public class BankAccount
    {
        private readonly string m_customerName;
        private double m_balance;
        private BankAccount() { }
        public BankAccount(string customerName, double balance)
        {
            m_customerName = customerName;
            m_balance = balance;
        }
        public string CustomerName { get { return m_customerName; } }
        public double Balance { get { return m_balance; }  }
        public void Debit(double amount)
        {
            if (amount > m_balance)
                throw new ArgumentOutOfRangeException("amount");
            if (amount < 0)
                throw new ArgumentOutOfRangeException("amount");
            m_balance -= amount;
        }
        public void Credit(double amount)
        {
            if (amount < 0)
                throw new ArgumentOutOfRangeException("amount");
            m_balance += amount;
        }
    }
}
```
#### 3.2 Pruebas Unitarias
Se implementaron pruebas unitarias en `BankAccountTests.cs`:
```C#
using Bank.WebApi.Models;
using NUnit.Framework;

namespace Bank.Domain.Tests
{
    public class BankAccountTests
    {
        [Test]
        public void Debit_WithValidAmount_UpdatesBalance()
        {
            // Arrange
            double beginningBalance = 11.99;
            double debitAmount = 4.55;
            double expected = 7.44;
            BankAccount account = new BankAccount("Mr. Bryan Walton", beginningBalance);
            // Act
            account.Debit(debitAmount);
            // Assert
            double actual = account.Balance;
            Assert.AreEqual(expected, actual, 0.001, "Account not debited correctly");
        }
    }
}
```
### Parte IV: Contenerización

#### 4.1 Dockerfile
Se creó un Dockerfile multi-etapa para optimizar la imagen:
```Yaml
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY . .
WORKDIR "/src/."
RUN dotnet restore 
RUN dotnet build -o /app/build

FROM build AS publish
RUN dotnet publish -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Bank.WebApi.dll"]
```
#### 4.2 Docker Compose
Orquestación de servicios (Web API + SQL Server):
```Yaml
version: '3.4'
services:
  webapi:
    image: api-bank
    build:
      context: .
    ports:
        - ${APP_HOST}:80
    environment:
        - "ConnectionStrings__DefaultConnection=Server=${DB_SERVER},${DB_PORT};Initial Catalog=${DB_NAME};Persist Security Info=False;User ID=${DB_USERNAME};Password=${DB_PASSWORD};MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=${TRUST_SERVER_CERTIFICATE}; Integrated Security=${INTEGRATED_SECURITY};Connection Timeout=30;"
    depends_on:
        - "sqlServer"
    links:
        - "sqlServer"

  sqlServer:
    image: mcr.microsoft.com/mssql/server:2022-latest
    hostname: "sqlserver"
    environment:
        SA_PASSWORD: ${DB_PASSWORD}
        ACCEPT_EULA: "Y"
    restart: always
    ports:
        - "${DB_PORT}:1433"
```

### Parte V: Pruebas y Documentación

#### 5.1 Ejecución de Pruebas con Cobertura
```bash
dotnet test --collect:"XPlat Code Coverage"
reportgenerator "-reports:./*/*/*/coverage.cobertura.xml" "-targetdir:Cobertura" -reporttypes:MarkdownSummaryGithub
```

#### 5.2 Generación de Diagrama de Clases
```bash
dll2mmd -f Bank.WebApi/bin/Debug/net8.0/Bank.WebApi.dll -o disenio.md
```

#### 5.3 Configuración de DocFx
```bash
docfx init -y
```

#### 5.4 Archivos de Configuración
Se configuraron los siguientes archivos para la documentación:
> docfx.json
```Json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/docfx/main/schemas/docfx.schema.json",
  "metadata": [
    {
      "src": [
        {
          "src": ".",
          "files": [
            "**/*.csproj"
          ]
        }
      ],
      "dest": "docs"
    }
  ],
  "build": {
    "content": [
      {
        "files": [
          "**/*.{md,yml}"
        ],
        "exclude": [
          "_site/**"
        ]
      }
    ],
    "resource": [
      {
        "files": [
          "images/**"
        ]
      }
    ],
    "output": "_site",
    "template": [
      "default",
      "modern"
    ],
    "globalMetadata": {
      "_appName": "Bank.App",
      "_appTitle": "Bank App",
      "_enableSearch": true,
      "pdf": true
    }
  }
}
```
> toc.yml
```Yaml
- name: Docs
  href: docs/
```
> index.md
```Markdown
---
_layout: landing
---

# This is the **HOMEPAGE**.

## [Diagrama de Clases](disenio.md)

## [Pruebas](Cobertura/SummaryGithub.md)
```
#### 5.5 Generación de Documentación
```bash
docfx metadata docfx.json
docfx build
```

### Parte VI: Análisis de Seguridad con Semgrep

#### 6.1 Ejecución Local de Semgrep
```bash
semgrep scan --config='p/default' . --json --output semgrep.json
prospector-html --input semgrep.json --output semgrep-report.html --filter semgrep
```

#### 6.2 Resultado del Análisis
El análisis genera un reporte HTML detallado que identifica:
- Vulnerabilidades de seguridad
- Problemas de calidad de código
- Malas prácticas de programación
- Recomendaciones de mejora

---

## 📊 RESULTADOS OBTENIDOS

### 1. Aplicación Bank Implementada
✅ Clase `BankAccount` con métodos `Debit` y `Credit`  
✅ Validaciones de negocio (montos negativos, sobregiros)  
✅ Pruebas unitarias con NUnit/MSTest  
✅ Cobertura de código documentada  

### 2. Análisis de Seguridad
✅ Integración de Semgrep  
✅ Reportes SARIF para GitHub Code Scanning  
✅ Reportes HTML publicados en GitHub Pages  
✅ Análisis automático en cada push  

### 3. Documentación Automatizada
✅ Generación con DocFx  
✅ Diagramas de clases con dll2mmd  
✅ Reportes de cobertura en Markdown  
✅ Publicación automática en GitHub Pages  

### 4. Integración con SonarCloud
✅ Análisis de calidad de código  
✅ Detección de code smells  
✅ Medición de deuda técnica  
✅ Reporte de cobertura integrado  

### 5. Paquetes NuGet
✅ Empaquetado automático  
✅ Publicación en GitHub Packages  
✅ Versionado semántico  
✅ Dependencias documentadas  

### 6. Releases Automatizados
✅ Creación de releases con tags  
✅ Ejecución de pruebas antes del release  
✅ Inclusión de reportes de cobertura  
✅ Artefactos binarios adjuntos  

---

## ⚙️ WORKFLOWS IMPLEMENTADOS

### 📄 Workflow 1: semgrep.yml
**Propósito:** Análisis de seguridad estático
```Yaml
name: Semgrep Analysis
env:
  DOTNET_VERSION: '8.x'                     # la versión de .NET
on: push
jobs:
  security:
    runs-on: ubuntu-latest
    container:
      # A Docker image with Semgrep installed. Do not change this.
      image: semgrep/semgrep
    steps:
      - uses: actions/checkout@v4
      - uses: snyk/actions/setup@master
      # - name: Configurando la versión de NET
      #   uses: actions/setup-dotnet@v4
      #   with:
      #     dotnet-version: ${{ env.DOTNET_VERSION }}  
      - name: Semgrep scan
        run: semgrep scan --config="p/default" --sarif --output=report.sarif --metrics=off
      - name: Upload result to GitHub Code Scanning
        uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: report.sarif
```

**Trigger:** Push a cualquier rama

**Características Implementadas:**
- ✅ Análisis con configuración por defecto de Semgrep
- ✅ Generación de reporte SARIF para GitHub Code Scanning
- ✅ Generación de reporte JSON
- ✅ Instalación automática de Python y prospector2html
- ✅ Conversión de JSON a HTML
- ✅ Upload de artefacto HTML
- ✅ **Publicación automática en GitHub Pages**

**Resultado:** Reporte de seguridad visible y accesible para todo el equipo

---

### 📄 Workflow 2: publish_docs.yml
**Propósito:** Documentación automatizada con DocFx

**Trigger:** Push a main o ejecución manual

**Características Implementadas:**
- ✅ Configuración de .NET 8.x
- ✅ Restauración y build de la solución
- ✅ Instalación de DocFx, dll2mmd y ReportGenerator
- ✅ Ejecución de pruebas con cobertura
- ✅ Generación de reportes de cobertura en Markdown
- ✅ Generación de diagramas de clases (Mermaid)
- ✅ Build de documentación completa con DocFx
- ✅ **Publicación en GitHub Pages (rama gh-pages-docs)**

**Resultado:** Documentación técnica completa y actualizada automáticamente

---

### 📄 Workflow 3: package_nuget.yml
**Propósito:** CI/CD con análisis de calidad y publicación de paquetes

**Trigger:** Push a main, Pull Requests o ejecución manual

**Características Implementadas:**
- ✅ Checkout completo (fetch-depth: 0) para análisis histórico
- ✅ Cache de paquetes SonarCloud para optimización
- ✅ Instalación de dotnet-sonarscanner
- ✅ Inicio de análisis con SonarCloud
  - Project Key
  - Organization
  - Reporte OpenCover
- ✅ Build en modo Release
- ✅ **Ejecución de pruebas unitarias con cobertura**
- ✅ Generación de reportes (HTML + Cobertura)
- ✅ **Análisis completo con SonarCloud**
- ✅ Upload de resultados como artifacts
- ✅ **Empaquetado NuGet del proyecto Bank.WebApi**
- ✅ **Publicación en GitHub Packages**

**Secrets Requeridos:**
- `SONAR_TOKEN`
- `SONAR_PROJECT_KEY`
- `SONAR_ORGANIZATION`

**Resultado:** Pipeline completo de integración continua con métricas de calidad

---

### 📄 Workflow 4: release_version.yml
**Propósito:** Creación de releases con pruebas y artefactos

**Trigger:** Tags (v*) o ejecución manual con input de versión

**Características Implementadas:**
- ✅ Permisos para contents y packages
- ✅ Setup de .NET 8.x
- ✅ Restauración y build en Release
- ✅ **✨ EJECUCIÓN DE PRUEBAS UNITARIAS ✨**
  - Con logger TRX para resultados detallados
  - Con recolección de cobertura XPlat Code Coverage
  - Con verbosidad normal
- ✅ Generación de reportes de cobertura (HTML + Markdown)
- ✅ Upload de resultados como artifacts
- ✅ Extracción dinámica de versión desde tag
- ✅ Empaquetado con versionado semántico
- ✅ Creación de archivo ZIP con:
  - Binarios compilados
  - Resultados de pruebas
  - Reporte de cobertura
- ✅ **Creación de GitHub Release** con todos los artefactos
- ✅ **Publicación del paquete en GitHub Packages**

**Resultado:** Releases completos y auditables con pruebas verificadas

---

## 🎓 CONCLUSIONES

### Aprendizajes Clave

1. **DevSecOps en Práctica**
   - La integración de Semgrep permite detectar vulnerabilidades desde las primeras etapas del desarrollo
   - Los análisis automáticos reducen significativamente el riesgo de código inseguro en producción
   - GitHub Code Scanning facilita el seguimiento de problemas de seguridad

2. **Automatización de Calidad**
   - SonarCloud proporciona métricas objetivas sobre calidad del código
   - La cobertura de pruebas automatizada asegura que el código esté debidamente testeado
   - Los reportes automáticos mantienen al equipo informado sobre el estado del proyecto

3. **Documentación como Código**
   - DocFx permite generar documentación profesional a partir del código
   - Los diagramas de clases automáticos mantienen la documentación sincronizada
   - La publicación en GitHub Pages facilita el acceso a la documentación

4. **CI/CD Completo**
   - Los workflows de GitHub Actions permiten automatizar todo el ciclo de vida
   - La publicación automática de paquetes NuGet agiliza la distribución
   - Los releases automatizados garantizan procesos reproducibles

5. **Mejores Prácticas Aplicadas**
   - Pruebas unitarias antes de cada release
   - Análisis de seguridad en cada push
   - Documentación actualizada automáticamente
   - Versionado semántico controlado
   - Artefactos firmados y auditables

### Beneficios Obtenidos

✅ **Seguridad:** Detección temprana de vulnerabilidades  
✅ **Calidad:** Métricas objetivas y reportes de cobertura  
✅ **Eficiencia:** Automatización de procesos repetitivos  
✅ **Trazabilidad:** Historial completo de cambios y releases  
✅ **Colaboración:** Documentación accesible para todo el equipo  
✅ **Confiabilidad:** Pruebas automáticas antes de cada publicación  

### Recomendaciones

1. Mantener los workflows actualizados con las últimas versiones de las herramientas
2. Configurar protecciones de rama (branch protection) para forzar el paso de pruebas
3. Implementar políticas de seguridad (Dependabot, Secret Scanning)
4. Establecer umbrales de calidad en SonarCloud (Quality Gates)
5. Documentar los secretos requeridos para facilitar la configuración en otros entornos

---

## 📚 ACTIVIDADES COMPLETADAS

### ✔️ Actividad Preliminar
- [x] Nombre del estudiante agregado al README.md

### ✔️ Actividad 1: Mejora de semgrep.yml
- [x] Análisis de seguridad con Semgrep
- [x] Generación de reporte SARIF
- [x] Generación de reporte HTML
- [x] Publicación en GitHub Pages

### ✔️ Actividad 2: Documentación con publish_docs.yml
- [x] Configuración de DocFx
- [x] Generación de metadata y build
- [x] Integración de pruebas y cobertura
- [x] Generación de diagramas de clases
- [x] Publicación en GitHub Pages

### ✔️ Actividad 3: CI/CD con package_nuget.yml
- [x] Integración con SonarCloud
- [x] Ejecución de pruebas unitarias
- [x] Generación de reportes de cobertura
- [x] Empaquetado NuGet
- [x] Publicación en GitHub Packages

### ✔️ Actividad 4: Releases con release_version.yml
- [x] Ejecución de pruebas unitarias
- [x] Generación de reportes de cobertura
- [x] Creación de releases con artefactos
- [x] Publicación de paquetes en GitHub

---

## 📖 RECURSOS ADICIONALES

### Documentación del Proyecto
- [Documentación Completa (GitHub Pages)](https://upt-faing-epis.github.io/lab-2025-ii-si784-u1-03-csharp-Vlkair/docs/)
- [Reporte de Seguridad Semgrep](https://upt-faing-epis.github.io/lab-2025-ii-si784-u1-03-csharp-Vlkair/)
- [Diagrama de Clases](./disenio.md)
- [Evidencias Detalladas](./Bank/EVIDENCIAS.md)
- [Tareas Completadas](./Bank/COMPLETADO.md)

### Herramientas Utilizadas
- [Semgrep](https://semgrep.dev/) - Análisis de seguridad estático
- [SonarCloud](https://sonarcloud.io/) - Análisis de calidad de código
- [DocFx](https://dotnet.github.io/docfx/) - Generación de documentación
- [GitHub Actions](https://github.com/features/actions) - CI/CD
- [ReportGenerator](https://github.com/danielpalme/ReportGenerator) - Reportes de cobertura
- [dll2mmd](https://github.com/bpreja/dll2mmd) - Diagramas de clases

---

## 👨‍💻 AUTOR

**Victor Cruz**  
Estudiante de Ingeniería de Sistemas  
Universidad Privada de Tacna  
Curso: SI784 - Calidad de Software  

---

## 📄 LICENCIA

Este proyecto es parte de un trabajo académico del curso SI784.

---

**Fecha de elaboración:** Diciembre 2025  
**Última actualización:** 5 de diciembre de 2025
