# Aplicación Web OpenEMR FHIR/API Client Explorer

Este proyecto de ejemplo demuestra cómo interactuar con las APIs OAuth2, FHIR y REST estándar de OpenEMR utilizando varios tipos de concesión OAuth2. Proporciona una implementación de referencia funcional para desarrolladores que crean aplicaciones que se integran con la infraestructura API de OpenEMR.

- El código está diseñado para ser educativo, mostrando las mejores prácticas para el registro de clientes OAuth2, gestión de tokens e interacción con APIs.
- Usé la IA ChatGPT para ayudar a refinar el código, asegurando que cumple con los estándares de OpenEMR, incluyendo ayuda para crear este README. ¡Seguro que podría haberlo usado hace 40 años! 😄
- He dedicado este proyecto a la comunidad de OpenEMR para ayudarte a aprender y experimentar con autenticación OAuth2, recursos FHIR y endpoints de API estándar.

---

## 📌 Propósito del Proyecto

Este explorador está construido para desarrolladores de nivel intermedio para aprender y experimentar con las APIs protegidas por OAuth2 de OpenEMR — incluyendo endpoints FHIR y estándar (no-FHIR). El objetivo es permitir a los desarrolladores:

- Registrar nuevos clientes públicos, confidenciales o JWT
- Autenticarse usando varios tipos de concesión OAuth2
- Consultar recursos API en tiempo real como `Patient`, `Encounter`, etc.
- Aprender con ejemplos para futuros proyectos de integración

---

## 🚀 Características

### ✅ Tipos de Concesión OAuth2 Soportados
- **Authorization Code** (con PKCE para clientes públicos)
- **Client Credentials** (para aplicaciones sistema-a-sistema)
- **Refresh Token** (actualiza automáticamente el token de acceso cuando expira)
- **JWT Client Credentials** (`client_secret_post`) — Soporta tanto par de claves en línea como URI JWKS

### ✅ Registro de Clientes
- Registra automáticamente clientes `confidential`, `public` o `JWT` con OpenEMR
- Registro JWT:
  - Auto-genera par de claves RSA (`private.pem`, `public.pem`)
  - Usa `jwks` embebido o `jwks_uri`, dependiendo de la configuración
  - Las rutas JWKS y PEM se guardan en `client_JWT.json`
- No necesitas ir a la línea de comandos ni usar comandos shell — keygen usa PHP OpenSSL compatible con OpenEMR
- Soporte CLI y navegador (`--regen` o `?regen=1`)
- Respaldo inteligente: Si `use_keys_file` es false, SSL no es requerido y `jwks_uri` se omite

### ✅ Modos de API
- **FHIR API**: Usa `/apis/default/fhir` de OpenEMR
- **Standard API**: Usa `/apis/default/api`

### ✅ Comportamiento de UI
- El tipo de concesión se actualiza automáticamente cuando cambia el tipo de cliente:
  - `JWT` fuerza `client_credentials`
  - `public` fuerza `authorization_code`
  - `confidential` soporta los tres (`authorization_code`, `client_credentials`, `refresh_token`)
- La lista de recursos se actualiza dinámicamente basándose en el cliente/scopes seleccionados

---

## ⚙️ Configuración Dinámica con `$GLOBALS['ApiConfig']`

En lugar de usar `define()`, el explorador usa un array de configuración global dinámico que se ajusta automáticamente al cambiar sitios o tipos de concesión.

```php
$GLOBALS['ApiConfig'] = [
  'JWKS_LOCATION_URL'        => "{$base_path}/clients_keys/{$site}_jwks.json",
  'AUTHORIZATION_ENDPOINT'   => "{$base_path}/oauth2/default/authorize",
  'TOKEN_ENDPOINT'           => "{$base_path}/oauth2/default/token",
  'LOGOUT_REDIRECT_URI'      => "{$base_path}/oauth2/default/logout.php",
  'REGISTER_CLIENT_ENDPOINT' => "{$base_path}/oauth2/default/registration",
  'FHIR_SERVER_URL'          => "{$base_path}/apis/default/fhir",
  'API_SERVER_URL'           => "{$base_path}/apis/default/api",
  'REDIRECT_URI'             => "{$app_path}/oeApiExplorer.php"
];
```

Usa los valores en cualquier parte mediante:

```php
$GLOBALS['ApiConfig']['FHIR_SERVER_URL']
```


**💡 Notas:**
- Si `$use_keys_file = true`: El archivo JWKS debe servirse via HTTPS con un certificado válido
- Si es `false`: Usa JWKS en línea, evita problemas de SSL y permisos de archivos

---

## 🧪 Instrucciones de Uso

1. **Edita `config.php`**
   - Configura tu dominio OpenEMR, rutas de API y la opción `$use_keys_file`

2. **Ejecuta `client_register.php`**
   - Registra clientes JWT, confidenciales y públicos
   - CLI:  
     ```bash
     php client_register.php --regen
     ```
   - Navegador:  
     ```
     https://tu-openemr/modules/oe-module-api-explorer/client_register.php?regen=1
     ```

3. **Explora via `oeApiExplorer.php`**
   - Elige tipo de cliente → el tipo de concesión se sincroniza automáticamente
   - Selecciona FHIR o Standard API
   - Escoge un recurso (ej. Patient, Encounter)

---

## 🔐 Manejo de Tokens

- Los tokens se almacenan en `$_SESSION`
- Se actualizan automáticamente cuando `refresh_token` está presente
- Los clientes JWT usan `lcobucci/jwt` para firmar solicitudes con el `private.pem` generado
- JWKS se expone vía URI o embebido en el registro del cliente dependiendo de `$use_keys_file`

---

## 📁 Resumen de Archivos

| Archivo                   | Propósito                                                    |
|---------------------------|--------------------------------------------------------------|
| `client_register.php`     | Registra clientes y genera pares de claves / JWKS            |
| `oeApiExplorer.php`       | UI para explorar recursos API via interfaz interactiva       |
| `oauth_client.php`        | Lógica de intercambio y actualización de tokens OAuth2       |
| `config.php`              | Define configuraciones específicas del entorno y flags       |
| `src/JwkService.php`      | Genera par de claves RSA y construye JWKS para clientes JWT  |
| `client_*.json`           | Almacena credenciales de clientes registrados                |

---

## 🔧 Requisitos

- OpenEMR 7+ con APIs OAuth2 y FHIR habilitadas
- PHP 7.4+ / 8.x con extensión OpenSSL
- Backend MySQL o MariaDB
- HTTPS recomendado para producción y requerido si `$use_keys_file = true`

---

## 🙌 Contribuciones

PRs y feedback son bienvenidos.  
Esta herramienta fue creada para empoderar a la comunidad de desarrolladores de OpenEMR.

---

© 2025 Jerry Padgett — [sjpadgett@gmail.com](mailto:sjpadgett@gmail.com)

## 📜 Licencia

Este proyecto es un ejemplo contribuido por la comunidad proporcionado con fines educativos.  
No es un módulo oficial de OpenEMR. Eres libre de usarlo, modificarlo y extenderlo según sea necesario.
---

## 🌐 Contexto Multi-Sitio y Consciente de Concesión

Este explorador ahora soporta múltiples sitios OpenEMR y ajusta dinámicamente comportamientos clave:

- El desplegable de sitio te permite apuntar a `localhost`, `docker` o dominios remotos
- Cada combinación de sitio + tipo de concesión almacena un archivo de cliente único:
  - `client_docker_client_credentials.json`
  - `clients_keys/docker_jwks.json`
- El explorador reconstruye rutas y claves al vuelo cuando cambias de contexto

## ⚙️ Configuración Dinámica con $GLOBALS['ApiConfig']

Los defines estáticos han sido reemplazados con un array de configuración global seguro en tiempo de ejecución:

```php
$GLOBALS['ApiConfig']['FHIR_SERVER_URL'] = "{$base_path}/apis/default/fhir";
```

Esto permite que `config.php` se recargue en cualquier momento con nuevo `$base_path` o `$app_path`, y todas las referencias se actualizan en vivo.

## 📁 Almacenamiento de Clientes

- Los clientes y JWKS se almacenan en `/clients_keys`
- Si falta, el directorio se crea automáticamente con permisos seguros
- Las claves PEM y archivos JWKS se nombran por sitio

## 🛡️ Manejo de Sesiones

- Las sesiones se inician explícitamente en todos los puntos de entrada
- Asegura que el estado (`client`, `grant`, `site`, `tokens`) persista a través del redireccionamiento Auth Code
