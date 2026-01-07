## OpenEMR API Explorer - Guía de Instalación

## Propósito
Implementación de referencia educativa para desarrolladores que crean aplicaciones que se integran con las APIs OAuth2, FHIR y REST estándar de OpenEMR.

## Características
- Soporte de FHIR y Standard API
- Registro de clientes OAuth2 (clientes JWT, Confidenciales, Públicos)
- Soporte de múltiples tipos de concesión (Authorization Code + PKCE, Client Credentials, JWT Bearer, Refresh Token)
- Explorador de API interactivo con interfaz basada en navegador
- Soporte de configuración multi-sitio
- Generación automática de claves JWT y configuración JWKS
- UI inteligente que sincroniza las selecciones de cliente y concesión
- Población de recursos basada en scopes
- Funcionalidad de auto-actualización de tokens

## Requisitos
- OpenEMR 7.02+
- PHP 8.1+ con extensión OpenSSL
- Certificado SSL (solo requerido si se usa el modo jwks_uri)

## Pasos de Instalación
1. Clona o descarga el proyecto en tu directorio de módulos de OpenEMR
- Repositorio: https://github.com/sjpadgett/oe-module-api-explorer
- Asegúrate de que la estructura del directorio sea `/openemr/modules/oe-module-api-explorer/` o dos niveles por encima del directorio raíz de openemr para el enrutamiento.
2. Navega a `/openemr/modules/oe-module-api-explorer/`
3. Edita `config.php` si es necesario (la configuración predeterminada de localhost funciona para la mayoría de las configuraciones)
4. Accede al explorador en `oeApiExplorer.php`
5. Registra clientes desde dentro de la aplicación (Registrar creará los 3 tipos de clientes y claves requeridas):
    - Selecciona el Sitio API deseado del desplegable
    - Haz clic en el botón "Register Clients"
    - Repite para cada sitio que quieras usar

## Inicio Rápido
1. Abre `oeApiExplorer.php` en tu navegador
2. Selecciona Sitio API y haz clic en "Register Clients" (haz esto para cada sitio que planees usar)
3. Selecciona tipo de cliente, tipo de concesión, tipo de API y recurso
4. Haz clic en "Fetch" para probar las llamadas API

## Configuración
- Edita `config.php` para agregar sitios OpenEMR adicionales
- Por defecto soporta localhost, docker y sitios demo remotos
- JWKS puede servirse en línea (predeterminado) o vía archivo (requiere SSL)

## Archivos Clave
- `client_register.php` - Registro de clientes y claves
- `oeApiExplorer.php` - Interfaz principal del navegador
- `oauth_client.php` - Manejadores del flujo OAuth2
- `config.php` - Configuración del entorno
- `clients_keys/` - Credenciales de clientes y claves generadas

## Enlaces
- Repositorio GitHub: https://github.com/sjpadgett/oe-module-api-explorer
- Autor: Jerry Padgett (sjpadgett@gmail.com)
- Licencia: Uso educativo, no es un producto oficial de OpenEMR

## Soporte
Esta es una herramienta educativa contribuida por la comunidad. Para problemas o contribuciones, usa el repositorio de GitHub.
