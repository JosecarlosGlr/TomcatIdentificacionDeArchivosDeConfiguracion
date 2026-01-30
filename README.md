# Tomcat identificación de archivos de configuración

## 1. Localización en el Sistema
Los archivos de configuración principales de Tomcat se encuentran en el directorio `$CATALINA_HOME/conf`.

![Listado de archivos de configuración](https://raw.githubusercontent.com/JosecarlosGlr/TomcatIdentificacionDeArchivosDeConfiguracion/refs/heads/main/3.png)

---

## 2. Análisis de Archivos Clave

### ⚙️ `conf/server.xml`
Es el **archivo central** de configuración del servidor. Define la arquitectura física y el comportamiento del contenedor.

* **Función:** Configurar los componentes del servidor (Puertos, Conectores, Motores).
* **Elementos configurables:**
    * **Conectores:** Puertos de escucha como HTTP (8080), HTTPS (8443) y AJP (8009).
    * **Hosts:** Definición de dominios virtuales (ej. `localhost`, `app.empresa.com`).
    * **Rendimiento:** Ajuste de hilos (`maxThreads`) y tiempos de espera (`connectionTimeout`).

### 🌐 `conf/web.xml`
Es el **Descriptor de Despliegue Global**. Actúa como una plantilla base para todas las aplicaciones web del servidor.

* **Función:** Establecer valores por defecto para Servlets y JSPs.
* **Elementos configurables:**
    * **Tipos MIME:** Define cómo el navegador debe interpretar archivos (ej. pdf, json, html).
    * **Sesiones:** Tiempo de expiración de sesión por defecto (ej. 30 minutos).
    * **Páginas de Error:** Gestión global de errores 404 o 500.
    > **Nota Importante:** Cada aplicación puede tener su propio archivo `WEB-INF/web.xml` que **sobrescribe** estas configuraciones globales.

### 🛡️ `conf/tomcat-users.xml`
Es la base de datos de usuarios basada en XML (*MemoryRealm*).

* **Función:** Gestionar la autenticación básica y la autorización mediante roles.
* **Elementos configurables:**
    * **Usuarios:** Definición de credenciales (usuario/contraseña).
    * **Roles:** Asignación de permisos específicos.
        * `manager-gui`: Acceso al Manager Web.
        * `admin-gui`: Acceso al Host Manager.

### 📦 `conf/context.xml`
Define el contexto de ejecución de las aplicaciones web.

* **Función:** Configurar recursos externos y comportamientos específicos de la aplicación.
* **Elementos configurables:**
    * **Recursos (DataSources):** Configuración de conexiones a bases de datos (Pool JDBC).
    * **Válvulas (Valves):** Reglas de filtrado, como restricción de acceso por IP.
    * **Session Management:** Configuración de persistencia de sesiones.

---

## 3. Mapa Visual de Dependencias

El siguiente esquema ilustra la jerarquía de configuración. Los archivos superiores (globales) definen el entorno para los inferiores (específicos).
            
            
               [ server.xml ] ──────────────────────────┐
                (Define Puertos, Hilos y Hosts)         │
                        │                               │
                        ▼                               │
                   [ Host ] (ej. localhost)             │
                        │                               │
                        ▼                               │
                 [ APLICACIÓN WEB ] ◄───────────────────┼─── [ context.xml ]
                        │   ▲                           │    (Recursos DB y Válvulas)
                        │   │                           │
                        │   └───────────────────────────┼─── [ web.xml ]
                        │                                    (MIME types y Sesiones)
                        │
                        │   ┌──────────────────────┐
                        └──►│ tomcat-users.xml     │
                            │ (Usuarios y Roles)   │
                            └──────────────────────┘
