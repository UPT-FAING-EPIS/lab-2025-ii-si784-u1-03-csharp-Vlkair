# Guía de Configuración Post-Instalación

## ⚠️ Configuraciones Pendientes

Después de crear el proyecto, debes configurar los siguientes elementos en tu repositorio de GitHub:

## 1. Configurar Secrets en GitHub

### Para el workflow `package_nuget.yml` (SonarCloud)

1. Ve a tu repositorio en GitHub
2. Navega a: **Settings** > **Secrets and variables** > **Actions**
3. Haz clic en **New repository secret**
4. Agrega los siguientes secrets:

#### SONAR_TOKEN
- **Nombre:** `SONAR_TOKEN`
- **Valor:** Token de autenticación de SonarCloud
- **Cómo obtenerlo:**
  1. Ir a https://sonarcloud.io
  2. Crear cuenta o iniciar sesión
  3. Ir a **My Account** > **Security**
  4. Generar nuevo token
  5. Copiar el token generado

#### SONAR_PROJECT_KEY
- **Nombre:** `SONAR_PROJECT_KEY`
- **Valor:** Clave del proyecto en SonarCloud
- **Formato:** `tu-organizacion_nombre-proyecto`
- **Cómo obtenerlo:**
  1. En SonarCloud, crear nuevo proyecto
  2. Conectar con tu repositorio de GitHub
  3. Copiar la Project Key que aparece

#### SONAR_ORGANIZATION
- **Nombre:** `SONAR_ORGANIZATION`
- **Valor:** Clave de tu organización en SonarCloud
- **Cómo obtenerlo:**
  1. En SonarCloud, ir a **My Organizations**
  2. Copiar la clave de tu organización

### Resumen de Secrets
```
SONAR_TOKEN          → Token de autenticación de SonarCloud
SONAR_PROJECT_KEY    → Clave del proyecto (ej: mi-org_bank-app)
SONAR_ORGANIZATION   → Clave de organización (ej: mi-organizacion)
```

## 2. Habilitar GitHub Pages

### Para reportes de Semgrep
1. Ir a: **Settings** > **Pages**
2. En **Source**, seleccionar: **Deploy from a branch**
3. En **Branch**, seleccionar: `gh-pages` / `/ (root)`
4. Guardar

### Para documentación con DocFx
1. El workflow crea automáticamente la rama `gh-pages-docs`
2. Una vez ejecutado el workflow, configurar:
   - **Source**: Deploy from a branch
   - **Branch**: `gh-pages-docs` / `/docs`
3. Guardar

## 3. Habilitar Permisos de Workflow

1. Ir a: **Settings** > **Actions** > **General**
2. En **Workflow permissions**, seleccionar:
   - ✅ **Read and write permissions**
3. Marcar: ✅ **Allow GitHub Actions to create and approve pull requests**
4. Guardar

## 4. Verificar Configuración de Packages

GitHub Packages está habilitado por defecto. Verifica que:
1. El `GITHUB_TOKEN` tenga permisos de escritura (configurado en paso 3)
2. Los workflows pueden publicar packages

## 5. Primer Push y Verificación

### Hacer push de todo el código
```bash
cd Bank
git add .
git commit -m "feat: Implementación completa del laboratorio 03"
git push origin main
```

### Verificar workflows
1. Ir a la pestaña **Actions** en GitHub
2. Verificar que se ejecuten los workflows:
   - ✅ Semgrep Analysis
   - ✅ Publish Documentation
   - ✅ Build and Publish NuGet Package

### Verificar resultados
- **Code Scanning:** Security > Code scanning alerts
- **GitHub Pages (Semgrep):** `https://<usuario>.github.io/<repo>/`
- **GitHub Pages (Docs):** `https://<usuario>.github.io/<repo>/docs/`
- **Packages:** Pestaña "Packages" en tu repositorio

## 6. Crear Primera Release

### Opción A: Con tag
```bash
git tag v1.0.0
git push origin v1.0.0
```

### Opción B: Manual desde GitHub
1. Ir a **Actions**
2. Seleccionar workflow **Create Release Version**
3. Hacer clic en **Run workflow**
4. Ingresar versión (ej: v1.0.0)
5. Ejecutar

### Verificar release
1. Ir a **Releases** en tu repositorio
2. Verificar que aparezca la nueva release con:
   - Archivo ZIP
   - Paquete NuGet
   - Reporte de cobertura

## 7. Opcional: Configurar Variables de Entorno

Para `docker-compose.yml`, crea un archivo `.env` en el directorio `Bank/`:

```env
# .env
APP_HOST=8080
DB_SERVER=sqlserver
DB_PORT=1433
DB_NAME=BankDB
DB_USERNAME=sa
DB_PASSWORD=YourStrong!Password
TRUST_SERVER_CERTIFICATE=True
INTEGRATED_SECURITY=False
```

## Checklist de Configuración ✅

- [ ] Secrets de SonarCloud configurados
- [ ] GitHub Pages habilitado para `gh-pages`
- [ ] GitHub Pages habilitado para `gh-pages-docs`
- [ ] Permisos de workflow configurados (Read and write)
- [ ] Primer push realizado
- [ ] Workflows ejecutándose correctamente
- [ ] Reportes visibles en GitHub Pages
- [ ] Primera release creada (opcional)
- [ ] Archivo .env creado para Docker (opcional)

## Troubleshooting

### Los workflows fallan con errores de permisos
- Verificar que los permisos de workflow estén en "Read and write"
- Verificar que los secrets estén configurados correctamente

### SonarCloud falla
- Verificar que los 3 secrets estén configurados
- Verificar que el proyecto exista en SonarCloud
- Verificar que la organización sea correcta

### GitHub Pages no se publica
- Esperar unos minutos (puede tardar en propagarse)
- Verificar que la rama correspondiente exista
- Verificar que GitHub Pages esté habilitado

### Docker Compose falla
- Crear archivo `.env` con las variables requeridas
- Verificar que Docker Desktop esté ejecutándose
- Verificar permisos de puerto 8080 y 1433

## Soporte

Para más información, consultar:
- [Documentación de GitHub Actions](https://docs.github.com/en/actions)
- [Documentación de SonarCloud](https://docs.sonarcloud.io/)
- [Documentación de DocFx](https://dotnet.github.io/docfx/)
- [Documentación de Semgrep](https://semgrep.dev/docs/)

---

**¡Configuración completada!** 🎉

Una vez completados estos pasos, tu proyecto estará completamente funcional con CI/CD, análisis de seguridad, documentación automatizada y publicación de paquetes.
