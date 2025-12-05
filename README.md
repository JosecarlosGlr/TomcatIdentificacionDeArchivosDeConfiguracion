# Tomcat: Identificación de archivos de configuración

!(Archivos clave)[https://raw.githubusercontent.com/JosecarlosGlr/TomcatIdentificacionDeArchivosDeConfiguracion/refs/heads/main/3.png]


## 1. conf/server.xml

Es el archivo central de configuración del servidor Tomcat.

✔️ ¿Qué define?

- Conectores (Connectors)
- Puertos HTTP/HTTPS (por defecto 8080)
- Timeouts, número de hilos, protocolos NIO/APR
- Hosts virtuales (Host)
- Dominios o aplicaciones alojadas
- Directorio webapps o rutas personalizadas
- Engines y Services
- Arquitectura interna de Tomcat
- Mapeo de peticiones desde un conector hacia un host

✔️ ¿Qué puedes configurar?

- Puerto de acceso
- HTTPS / TLS
- Número de hilos del servidor
- Compresión GZIP
- Hosts virtuales
- Paths de despliegue

## 2. conf/web.xml

Es el descriptor de despliegue global.
Aplica por defecto a todas las aplicaciones web.

✔️ ¿Qué define?

- Parámetros globales de servlets
- Configuración de sesiones
- Filtros globales
- Listeners
- MIME mappings
- Páginas de error generales
- Tiempo de expiración de sesión
- Cada aplicación puede sobrescribir esto con su propio WEB-INF/web.xml.

## 3. conf/tomcat-users.xml

Controla la autenticación y roles para acceder a la consola de administración.

✔️ ¿Qué define?

- Usuarios
- Roles
- Acceso a /manager y /host-manager

✳️ Ejemplo de roles típicos

- manager-gui → acceder al Manager Web
- admin-gui → acceder al Host Manager

✔️ Solo afecta a:

- Consola web de administración
- Despliegue remoto con credenciales

## 4. conf/context.xml

Define opciones comunes para todos los contextos (aplicaciones).

✔️ ¿Qué define?

- Recursos (DataSources JDBC)
- Parámetros por defecto para todas las apps
- Configuraciones de seguridad
- Permisos de acceso a ficheros
- Habilitar/deshabilitar reloading
- Configuración de cookies, sessions, etc.

Cada aplicación puede tener su propio META-INF/context.xml que sobrescribe este.

                        ┌──────────────────────────┐
                        │       server.xml         │
                        │  - Conectores            │
                        │  - Hosts                 │
                        │  - Servicios             │
                        └───────────┬──────────────┘
                                    │
                                    │ Define estructura
                                    ▼
                      ┌────────────────────────────┐
                      │         Host(s)             │
                      │  (mapean aplicaciones)      │
                      └───────────┬─────────────────┘
                                  │
                                  ▼
                    ┌──────────────────────────────┐
                    │       context.xml (global)    │
                    │   - DataSources               │
                    │   - Config. comunes apps      │
                    └───────────┬───────────────────┘
                                │ Sobrescribible por:
                                ▼
                ┌────────────────────────────────────────┐
                │   WEB-INF/web.xml (por aplicación)     │
                │  - Servlets, filtros, listeners        │
                │  - Sesiones, errores, MIME             │
                └────────────────────────────────────────┘


Separado del flujo de ejecución:

                ┌─────────────────────────┐
                │   tomcat-users.xml      │
                │  - Usuarios y roles     │
                │  - Acceso a Manager     │
                └─────────────────────────┘
