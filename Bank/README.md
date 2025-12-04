# Bank Application

## Descripción
Aplicación bancaria desarrollada en .NET 8 con pruebas unitarias, análisis de seguridad y documentación automatizada.

## Estructura del Proyecto

```
Bank/
├── .github/workflows/          # GitHub Actions workflows
│   ├── semgrep.yml            # Análisis de seguridad + GitHub Pages
│   ├── publish_docs.yml       # Documentación con DocFx
│   ├── package_nuget.yml      # Build, test, SonarCloud y NuGet
│   └── release_version.yml    # Releases con pruebas unitarias
├── Bank.WebApi/               # Proyecto API Web
│   └── Models/
│       └── BankAccount.cs     # Clase principal
├── Bank.WebApi.Tests/         # Proyecto de pruebas
│   └── BankAccountTests.cs    # Pruebas unitarias
├── Dockerfile                  # Configuración Docker
├── docker-compose.yml          # Orquestación de servicios
├── docfx.json                  # Configuración DocFx
├── toc.yml                     # Tabla de contenidos
├── index.md                    # Página principal de docs
└── EVIDENCIAS.md               # Documento de evidencias
```

## Prerrequisitos

- .NET 8.0 SDK o superior
- Docker Desktop (opcional)
- Python 3.x con pip (para Semgrep)
- Cuenta en GitHub (para workflows)
- Cuenta en SonarCloud (opcional, para análisis de calidad)

## Instalación Local

### 1. Restaurar dependencias
```bash
cd Bank
dotnet restore
```

### 2. Compilar la solución
```bash
dotnet build --configuration Release
```

### 3. Ejecutar pruebas
```bash
dotnet test
```

### 4. Ejecutar pruebas con cobertura
```bash
dotnet test --collect:"XPlat Code Coverage"
reportgenerator "-reports:./*/*/*/coverage.cobertura.xml" "-targetdir:Cobertura" -reporttypes:MarkdownSummaryGithub
```

## Ejecución con Docker

### Compilar y ejecutar
```bash
docker-compose up --build
```

La API estará disponible en `http://localhost:${APP_HOST}`

## Workflows de GitHub Actions

### 1. Semgrep Analysis (semgrep.yml)
- **Trigger:** Push a cualquier rama
- **Funcionalidades:**
  - Análisis de seguridad con Semgrep
  - Generación de reporte SARIF para Code Scanning
  - Generación de reporte HTML
  - Publicación en GitHub Pages

### 2. Publish Documentation (publish_docs.yml)
- **Trigger:** Push a main o manual
- **Funcionalidades:**
  - Ejecución de pruebas con cobertura
  - Generación de diagrama de clases
  - Generación de documentación con DocFx
  - Publicación en GitHub Pages

### 3. Build and Publish NuGet (package_nuget.yml)
- **Trigger:** Push a main, PR o manual
- **Funcionalidades:**
  - Análisis con SonarCloud
  - Ejecución de pruebas unitarias
  - Reporte de cobertura
  - Empaquetado NuGet
  - Publicación en GitHub Packages

**Secrets requeridos:**
- `SONAR_TOKEN`
- `SONAR_PROJECT_KEY`
- `SONAR_ORGANIZATION`

### 4. Create Release Version (release_version.yml)
- **Trigger:** Tag v* o manual
- **Funcionalidades:**
  - Ejecución de pruebas unitarias
  - Generación de cobertura
  - Empaquetado de release
  - Creación de GitHub Release
  - Publicación en GitHub Packages

**Uso:**
```bash
# Crear un tag y hacer push
git tag v1.0.0
git push origin v1.0.0

# O ejecutar manualmente desde GitHub Actions
```

## Documentación

La documentación se genera automáticamente con DocFx e incluye:
- Diagrama de clases
- Reporte de pruebas
- Cobertura de código
- Documentación API

Acceder en: `https://<usuario>.github.io/<repo>/docs/`

## Análisis de Seguridad

El reporte de Semgrep se publica automáticamente en:
`https://<usuario>.github.io/<repo>/`

## Clase BankAccount

### Propiedades
- `CustomerName`: Nombre del cliente (solo lectura)
- `Balance`: Balance actual de la cuenta (solo lectura)

### Métodos
- `Debit(double amount)`: Retira fondos de la cuenta
- `Credit(double amount)`: Deposita fondos en la cuenta

### Ejemplo de uso
```csharp
BankAccount account = new BankAccount("John Doe", 1000.00);
account.Credit(500.00);  // Balance: 1500.00
account.Debit(200.00);   // Balance: 1300.00
```

## Configuración de GitHub

### 1. Habilitar GitHub Pages
1. Ir a Settings > Pages
2. Source: Deploy from a branch
3. Branch: gh-pages (o gh-pages-docs para documentación)
4. Guardar

### 2. Configurar SonarCloud
1. Crear cuenta en https://sonarcloud.io
2. Crear nuevo proyecto
3. Obtener: Organization Key, Project Key y Token
4. Agregar secrets en GitHub:
   - `SONAR_TOKEN`
   - `SONAR_PROJECT_KEY`
   - `SONAR_ORGANIZATION`

### 3. Habilitar GitHub Packages
GitHub Packages está habilitado por defecto. El `GITHUB_TOKEN` se proporciona automáticamente.

## Tecnologías Utilizadas

- **.NET 8.0**: Framework principal
- **MSTest/NUnit**: Frameworks de pruebas
- **Docker**: Containerización
- **Semgrep**: Análisis estático de seguridad
- **SonarCloud**: Análisis de calidad de código
- **DocFx**: Generación de documentación
- **GitHub Actions**: CI/CD
- **GitHub Pages**: Hosting de documentación y reportes

## Autor

Diego Castillo

## Licencia

Este proyecto es parte del Laboratorio 03 de Pruebas Estáticas de Seguridad.
