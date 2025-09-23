

# Bronze To Silver

## 🗂️ **Explorando el Catálogo de Databricks**

Ahora que hemos terminado de configurar el **Metastore**, regresemos al **Workspace** en Databricks y hagamos clic en **Catalog**, donde veremos la siguiente estructura de carpetas.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img204.png)

El **Catalog** en Databricks es una sección clave para organizar y gestionar tus datos. Aquí puedes explorar tablas, bases de datos, y colaborar con otros usuarios de manera eficiente. A continuación, te explico todo lo que puedes encontrar y cómo navegar en esta interfaz.

---

### 🌟 **Secciones Principales del Catalog**

El **Catalog** tiene varias áreas organizadas que te ayudan a encontrar lo que buscas rápidamente. 🚀

🏢 **Mi Organización**

Aquí verás todos los catálogos relacionados con una organización. Normalmente, esto incluye el catálogo **"system"** (predeterminado) y **"main"** (catálogo principal).

- 📂 **system**
    
- 📂 **main**
    

---

🔄 **Delta Shares Recibidos**

¡Aquí es donde se reciben los datos compartidos contigo! Gracias a **Delta Sharing**, puedes acceder fácilmente a datos de otros entornos.

- 📥 **samples** (Ejemplo de datos compartidos)
    

---

🏚️ **Legacy**

Aquí encontrarás los catálogos antiguos que forman parte de tu infraestructura de datos.

- 🗃️ **hive_metastore** (Catálogo tradicional de Hive)
    

---

📊 **Vista de Acceso Rápido**

### 🔍 **Quick Access**

¡Un área para navegar fácilmente! Aquí puedes encontrar:

- **Recientes**: Accede a tablas que has consultado recientemente.
    
- **Favoritos**: Tus tablas favoritas, para tenerlas siempre a mano.
    
- **Catálogos**: Lista completa de catálogos disponibles.
    

---

 📋 **Funcionalidades**

|**Función**|**Descripción**|
|---|---|
|🗂️ **Mis Catálogos**|Organiza y visualiza catálogos dentro de tu organización.|
|🔄 **Delta Shares**|Accede a los datos que otros comparten contigo usando Delta Sharing.|
|🏚️ **Legado**|Consulta antiguos catálogos como el de Hive Metastore.|

---

✔️ **Acciones Rápidas**

- ✅ **Navegar**: Haz clic en los diferentes catálogos para ver sus tablas.
    
- 🗂️ **Explorar**: Usa la barra de búsqueda para encontrar lo que necesitas rápido.
    
- 🔖 **Favoritos**: Marca las tablas que más usas para tenerlas siempre visibles.
    
- 🔄 **Recientes**: Empieza a explorar para llenar esta sección con tus tablas más usadas.
    

---

🔔 **Recuerda**:

> ¡El **Catalog** es tu centro de operaciones! Aquí puedes gestionar todas las bases de datos y explorar datos compartidos. Usa **Recientes** y **Favoritos** para tener todo a mano. ¡Explora, marca y navega fácilmente! 🧭

---

## 🔧 **Pasos para crear un catálogo en Databricks:**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img205.png)

1. **Ir a la sección "Catalog"**  
    En el panel lateral izquierdo del Workspace, haz clic en la opción **Catalog**. Esta sección te permite gestionar catálogos, esquemas y tablas de datos gobernados por Unity Catalog.
    
2. **Abrir el menú de acciones**  
    Haz clic en el ícono de **engranaje** (⚙️) o en el botón **"+" (Add data)** en la parte superior derecha del panel Catalog.
    
3. **Seleccionar "Create a catalog"**  
    En el menú desplegable que aparece, selecciona la opción **Create a catalog** (Crear un catálogo). Esta acción te permitirá definir un nuevo catálogo de datos dentro del Metastore configurado previamente.

---

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img206.png)

Ya que **has configurado tu Metastore**, el **siguiente paso lógico** es crear la capa organizativa principal dentro del Unity Catalog.


Recordemos que un **Catalog** es el primer nivel jerárquico del Unity Catalog:

```
Metastore ➡️ Catalog ➡️ Schema (Database) ➡️ Table / View
```

---

🪪 1️⃣ **Catalog name** (obligatorio)

📌 Escribe el **nombre del catálogo** que estás creando.  
🧠 Usa nombres **significativos** según el proyecto, por ejemplo:

- `ventas_catalog`
    
- `clientes_data`
    
- `proyecto_iot_2025`
    

---

🧬 2️⃣ **Type** (obligatorio)

🔽 Selecciona el tipo de catálogo. Hay dos opciones comunes:

|Tipo|Descripción|
|---|---|
|**Standard**|Usa la configuración por defecto del Metastore. Ideal para la mayoría.|
|**Managed**|Solo aparece si tienes configuraciones específicas para control total.|

📌 Selecciona **Standard** si no tienes requerimientos personalizados.

---

 ☁️ 3️⃣ **Storage location** (opcional pero recomendable)

📦 Esta sección permite **especificar una ubicación externa** para guardar las **tablas gestionadas** del catálogo.

➕ Opciones:

- **Select external location**: Escoge una ubicación externa ya registrada.
    
- **sub/path**: Especifica la **subcarpeta** dentro de ese storage (por ejemplo: `catalogos/ventas`).
    

🔗 Si no tienes una ubicación, haz clic en:

🔧 **Create a new external location**  
Esto te llevará a un flujo para crearla vinculando un contenedor de ADLS Gen2 y un Access Connector.

---

📌 📝 Notas Importantes

- Si **no eliges una ubicación externa**, el catálogo usará la ubicación por defecto del **Metastore**.
    
- Esto puede ser suficiente porque el Metastore ya tiene un ADLS Gen2 configurado.
    

---

🧭 **Flujo** 

```plaintext
🟦 Metastore (ya creado)
    └── 📁 Catalog (tú lo creas aquí)
           └── 📂 Schemas (bases de datos dentro del catalog)
                  └── 📊 Tablas / 👁️ Vistas
```

---

Finalmente solo debemos agregar el nombre al catalogo ya que el Type y el Storage location están configurados en el **Metastore**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img207.png)


🖱️ Último paso

Haz clic en **Create** 💙  
¡Y listo! 🎉 Ya tendrás tu **nuevo catálogo configurado**.


Ahora demos un vistazo en **Configure catalog**  

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img208.png)

---

### 🔐 **Configuración de Acceso a un Catálogo en Databricks**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img209.png)

Este panel de configuración te permite gestionar **quién puede ver, acceder o modificar** un catálogo de datos. Es una parte clave de la **gobernanza de datos** en **Databricks + Unity Catalog**.

---

🧭 **Acceso General**

 🌍 **Workspaces con Acceso**

- ✅ **Todos los espacios de trabajo tienen acceso**  
    Esto significa que **cualquier workspace** dentro de la cuenta puede visualizar el catálogo. Ideal si trabajas con múltiples equipos.
    

---

👤 **Propietario del Catálogo**

- 👑 **Propietario Asignado**:  
    `[carlosvelasquez@zero2herox.onmicrosoft.com](mailto:carlosvelasquez@zero2herox.onmicrosoft.com)`  
    Este usuario tiene **control total** sobre el catálogo y puede:
    
    - Asignar permisos
        
    - Gestionar objetos
        
    - Modificar configuraciones de acceso
        

>📝 **NOTA**: Solo los propietarios o administradores pueden **delegar o revocar** privilegios. Esto también lo puedes controlar desde el panel https://accounts.azuredatabricks.net/ donde puedes agregar y/o quitar usuarios y asignar permisos.

---

🔐 **Privilegios de Acceso**

|🧑‍🤝‍🧑 **Grupo/Usuario**|🎯 **Permiso Otorgado**|
|---|---|
|Todos los usuarios|`BROWSE` (Explorar)|

🔍 El permiso **BROWSE** permite:

- Ver el catálogo y sus objetos
    
- 🔒 **No** permite editar, borrar ni crear objetos
    

---

📦 **Objetos del Catálogo**

- 📁 **Nombre del catálogo**: `proyecto_databricks_catalog`
    
- Este es el catálogo específico al que se le están aplicando estos privilegios.
    

---

🎛️ **Controles de Acción**

🔘 **Botones disponibles:**

|🛠️ Acción|📋 Descripción|
|---|---|
|✅ **Conceder**|Asigna un nuevo privilegio a un usuario o grupo|
|❌ **Revocar**|Quita un privilegio previamente otorgado|

🔁 Puedes usarlos para ajustar el acceso en tiempo real sin interrumpir el uso del catálogo.

---

📌 **Resumen Visual**

|Elemento|Estado / Valor|
|---|---|
|🌍 Acceso Workspace|Todos los espacios de trabajo|
|👑 Propietario|[carlosvelasquez@zero2herox.onmicrosoft.com](mailto:carlosvelasquez@zero2herox.onmicrosoft.com)|
|📦 Catálogo|`proyecto_databricks_catalog`|
|👥 Permisos|Todos los usuarios → `BROWSE`|
|🛠️ Acciones|Conceder / Revocar permisos|

---

✅ **Checklist de Seguridad**

☑ ¿El propietario tiene control total?  
☑ ¿Los permisos están limitados a lo necesario (principio de menor privilegio)?  
☑ ¿Se otorgó acceso solo a usuarios o grupos verificados?  
☑ ¿Se documentaron los cambios de privilegios?

---

📣 **KEY**:  
💡 Usa privilegios como **SELECT**, **MODIFY**, **CREATE**, y **USAGE** solo cuando sea necesario. ¡Evita dar más permisos de los que cada rol necesita! 🔒

---

 🔐 **Ventana "Grant" – Configuración de Accesos en Catálogos de Databricks**

Al hacer clic en **"Grant"**, accedes a una ventana fundamental para la **gestión de permisos** en tu catálogo de datos. Aquí te mostramos qué puedes hacer y por qué, en algunos casos, no puedes modificar esta configuración.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img210.png)

---

🧩 **¿Qué es la Ventana "Grant"?**

La ventana **"Grant"** te permite:

✅ **Asignar permisos específicos**  
👥 A **usuarios**, **grupos** o **principals**  
🔐 Controlar **qué pueden hacer** con los objetos del catálogo

---

📋 **Opciones de Configuración Dentro de "Grant"**

1️⃣ **Principals (Usuarios o Grupos)**

Selecciona a quién deseas otorgar permisos:

- 👤 Usuarios individuales
    
- 👥 Grupos de usuarios
    
- 🤖 Principals de servicio
    

---

 2️⃣ **Privilege Presets (Preajustes)**

|🔧 Tipo|📋 Descripción|
|---|---|
|🎛️ Custom|Configura permisos manualmente|
|📦 Preajustes|Usa plantillas rápidas con privilegios comunes|

---

3️⃣ **Permisos Disponibles**

|🧭 Categoría|🔑 Permisos Incluidos|
|---|---|
|🪜 **Prerequisitos**|`USE CATALOG`, `USE SCHEMA`|
|📖 **Lectura**|`EXECUTE`, `READ VOLUME`, `SELECT`|
|🏗️ **Creación**|`CREATE FUNCTION`, `CREATE SCHEMA`, `CREATE TABLE`, `CREATE VOLUME`, etc.|
|🧾 **Metadatos**|`APPLY TAG`, `BROWSE`|
|✏️ **Edición**|`MODIFY`, `REFRESH`, `WRITE VOLUME`|

---

4️⃣ **Opciones Avanzadas**

|⚙️ Opción|📝 Descripción|
|---|---|
|`ALL PRIVILEGES`|Otorga todos los permisos disponibles|
|`EXTERNAL USE SCHEMA`|Permite acceder a objetos desde motores externos|
|`MANAGE`|Permisos similares a los del propietario (gestionar, eliminar, renombrar)|

---

5️⃣ **Acciones Finales**

✅ **Grant** – Aplica los permisos seleccionados  
❌ **Cancel** – Descarta los cambios

---

🚫 ¿Por Qué No Puedes Modificar Esta Configuración?

Recuerda que no puedes usar el botón **"Grant"**, puede deberse a uno (o más) de los siguientes motivos:

|⚠️ **Razón**|💬 **Descripción**|
|---|---|
|🔒 **No eres el propietario**|Solo los propietarios pueden editar permisos del catálogo.|
|👑 **No tienes rol de administrador**|Algunos permisos requieren ser **Workspace Admin** o **Metastore Admin**.|
|🛡️ **Restricciones organizacionales**|La política de tu empresa puede bloquear cambios en catálogos.|
|🔄 **Permisos gestionados automáticamente**|El catálogo puede heredar permisos desde otra fuente (como políticas globales).|
|🐞 **Error técnico o visual**|Puede ser un bug temporal. Intenta recargar la página o cerrar sesión.|


---

Finalmente haz clic en Next y Save

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img211.png)


### 📦**External Location**

Aunque ya configuraste correctamente el contenedor `metastore` en el Unity Catalog de **Databricks**, **esto no da acceso automático** a otros contenedores del mismo Data Lake, como `bronze`. Cada contenedor requiere su propia configuración de **credenciales** y **ubicaciones externas (external locations)** para poder ser accedido desde Databricks.

Pero antes de proceder a crear un **External Location** vamos a crear una nueva credencial que reutilizaremos en el contenedor Bronze y el resto de contenedores que usaremos más adelante. 

**Haz clic en ➕ y luego en Create a credential**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img212.png)

Ahora simplemente le asignamos un nombre y copiamos el **Resource ID** en En el campo **Access Connector Id** y clic en **Create**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img195.png)


![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img213.png)


### **Acceder desde Databricks (External Location)**:

- En **Databricks**, ve a la sección de **"Catalog"** como ya vimos anteriormente.

- Selecciona **"Create an external location"**.
![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img162.png)

- Completa los siguientes campos:

- **External location name**: Un nombre representativo (ej. `bronze_external_location`).
	
- **Storage type**: **Azure Data Lake Storage**.
	
- **URL**: Usa la URL del contenedor que bronze:
	
	```
	abfss://bronze@z2hdatalakesuffix.dfs.core.windows.net/
	```
	
- Finalmente seleccionamos la credencial que recién creamos `my-credential`
	
	![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img164.png)
	
- Finalmente clic en **Create**
![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img214.png)

Tambien puedes probar la conexión en **Test conection**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img215.png)

## 🌟 **Workspace en Databricks** 

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img172.png)

Databricks tiene una interfaz muy completa para gestionar tus proyectos y recursos. En la pestaña _Workspace_, puedes organizar tu trabajo y colaborar con otros usuarios.

---

### 🚀 **1. Navegación de Carpetas** 📂

La **navegación de carpetas** en la parte izquierda es tu mapa de recursos. Así puedes encontrar rápidamente todo lo que necesitas:

|**Sección**|**Descripción**|
|:-:|:--|
|🏠 **Home**|Aquí están tus proyectos y archivos personales.|
|📚 **Repos**|Tus proyectos y repositorios de código (como los de Git).|
|🔄 **Shared**|Carpeta donde puedes compartir archivos con otros usuarios.|
|👤 **Users**|Cada usuario tiene su propia carpeta aquí.|
|⭐ **Favorites**|Marca archivos importantes o útiles como favoritos.|
|🗑️ **Trash**|Todos los archivos eliminados van aquí.|

---

### 📊 **2. Vista de Usuario** 👨‍💻👩‍💻

La vista central muestra los archivos de un **usuario específico** (en este caso, el de "[carlosvelasquez@zero2herox.onmicrosoft.com](mailto:carlosvelasquez@zero2herox.onmicrosoft.com)").

📝 **NOTA**: Si la carpeta está vacía, verás el mensaje "**This folder is empty**".

---

### 🛠️ **3. Opciones de Filtro y Búsqueda** 🔍

Puedes organizar tus archivos usando filtros o buscando por:

🔹 **Nombre**  
🔹 **Tipo**  
🔹 **Propietario**  
🔹 **Fecha de Creación**

---

### 💬 **4. Botones de Acción** ⚙️

### ✨ **Acciones Principales**:

|**Acción**|**Descripción**|
|:-:|:--|
|➕ **Crear**|Crea nuevos proyectos, notebooks o carpetas.|
|🔗 **Compartir**|Comparte archivos o recursos con otros usuarios.|
|⚙️ **Más Opciones**|Ajustes adicionales como enviar feedback y configuración.|

---

### 🗂️ **5. Otras Secciones en el Menú Lateral** 🔧

En el menú lateral también puedes encontrar otros recursos súper útiles para tu trabajo:

|**Sección**|**Descripción**|
|:-:|:--|
|⚙️ **Jobs & Pipelines**|Automatiza tareas con trabajos y pipelines.|
|💻 **Compute**|Accede a los clústeres y recursos de computación.|
|📊 **SQL**|Editor de SQL, consultas, y SQL Warehouses.|
|🛒 **Marketplace**|Descubre e integra aplicaciones y servicios adicionales.|

---

✅ **Resumen de Funciones Clave**:

🔹 **Espacio personal**: Guarda tus proyectos en tu carpeta "Home".  
🔹 **Colaboración**: Comparte recursos y colabora con otros usuarios.  
🔹 **Automatización**: Utiliza jobs y pipelines para agilizar procesos.  
🔹 **SQL**: Gestiona y consulta bases de datos fácilmente.  
🔹 **Computación**: Accede a recursos de clústeres para procesamiento de datos.

---

🎯 **Consejos para Maximizar tu Productividad** 💡

- 💼 **Organiza tu trabajo**: Crea carpetas y subcarpetas para mantener tus proyectos ordenados.
    
- 💬 **Colabora fácilmente**: Utiliza la función de compartir para que otros usuarios puedan ver o editar tus archivos.
    
- 🚀 **Usa filtros**: Encuentra rápidamente lo que necesitas aplicando filtros en el nombre, tipo o propietario de los archivos.

---
## 🛠 **¿Cómo crear un notebook correctamente?**

1. **Ir a tu carpeta personal**  
    En Databricks → _Workspace_ → _Users_ → _tu usuario_. (Evitas mezclar con recursos compartidos).
    
2. **Crear notebook**
    
    - Haz clic derecho sobre tu **Usuario** luego en **Create** → seleccionar **Notebook**. 
     ![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img173.png)
    
1. **Configurar lenguaje predeterminado**  
    El notebook tomará por defecto Python
    
2. **Adjuntar compute**  
    Al crearlo, normalmente ya se adjunta automáticamente al compute que usaste más recientemente. Si no tienes cluster o compute creado, tendrás que crearlo antes de ejecutar celdas. 
    
3. **Activar características útiles del editor**
    
    - **Tabs para notebooks y archivos**: en _Settings > Developer_ puedes activar la experiencia de pestañas para navegar entre múltiples notebooks y archivos. 
        
    - **Split view**, **minimap**, búsqueda avanzada, entre otras, son ya parte de la experiencia mejorada.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img216.png)

---

## ⚡️ ¿Qué es “Serverless Compute” en **Azure Databricks**?

Al hacer clic en la pestaña **Connect**, encontrarás dos opciones principales para conectar tu notebook:

* ✅ **Serverless**
* 🛠️ **Tu clúster personalizado** (como el que creaste en pasos anteriores)

> Ahora bien, ¿**cuál deberías usar**?
> ¡Buena pregunta! Vamos a resolverla antes de seguir.

---

### 👀 Vistas en Databricks

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img174.png)

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img175.png)

Aquí puedes ver tanto el **Serverless compute** como el clúster **"Z2h cluster..."** que creaste anteriormente. Ambos están disponibles como opciones para tu notebook.

---

## ☁️ **¿Qué es Serverless Compute?**

Una forma de ejecutar notebooks sin tener que crear, iniciar o administrar clústeres manualmente.

---

### 🔑 **Características Clave**

* ⚙️ **Sin aprovisionamiento manual**: No tienes que configurar nada, Databricks se encarga de todo.
* ⚡ **Inicio rápido**: Los notebooks se conectan más rápido a Serverless que a un clúster tradicional.
* 🔁 **Escalabilidad automática**: Los recursos se asignan según demanda.
* 🧼 **Ambiente limpio**: Cada ejecución se da en un entorno aislado.

---

### 🧩 **Requisitos para poder usarlo**

| ✅ Requisito                   | 📌 Detalle                                                            |
| ----------------------------- | --------------------------------------------------------------------- |
| 🔐 Unity Catalog habilitado   | Necesario para gobernanza unificada                                   |
| 🌍 Región compatible de Azure | No todas las regiones lo soportan actualmente                         |
| 🔧 Funcionalidad activada     | Debe estar habilitada en los ajustes de cuenta (“Feature enablement”) |

📝 **NOTA**: Si no ves la opción “Serverless”, verifica estos 3 puntos.

---

### 📊 **Ventajas Operativas**

* ✅ Menor mantenimiento de clústeres
* ⚙️ No más configuración de Spark avanzada (ideal para usuarios no técnicos)
* 📈 Escala sola, sin intervención manual
* 🕒 Ahorro de tiempo al no tener que iniciar clústeres

---

### ⚠️ **Limitaciones del Serverless Compute**

| 🚫 Limitación                  | 🔍 Explicación                                                              |
| ------------------------------ | --------------------------------------------------------------------------- |
| 🔧 Sin Spark configs avanzados | No puedes personalizar la configuración de Spark                            |
| 📜 No soporta init scripts     | No puedes ejecutar scripts automáticos al iniciar el entorno                |
| ⏱️ Límites de inactividad      | Puede cerrarse si está inactivo por un período de tiempo                    |
| 💾 Recursos limitados          | Memoria y cómputo disponibles pueden ser menores que en un clúster dedicado |

📝 **Ideal para**: notebooks interactivos, análisis ligeros, pruebas, demos, exploración de datos.

---

## ⚙️ **“Mi Cluster” vs “Serverless Compute”**

### Tu escenario actual:

Tenemos el cluster que creamos en pasos enteriores:

* 🛠️ Un clúster propio: `"Z2h cluster 2025‑09‑11 ..."`
* ⚡ La opción de usar **Serverless**

Entonces... ¿cuál usar?

---

### 🆚 Comparación

| 💡 Característica        | ⚡ Serverless               | 🛠️ Clúster Personalizado      |
| ------------------------ | -------------------------- | ------------------------------ |
| Configuración manual     | ❌ No                       | ✅ Sí                           |
| Tiempo de inicio         | ⚡ Muy rápido               | 🕓 Puede tardar varios minutos |
| Escalabilidad            | ✅ Automática               | ⚠️ Manual                      |
| Control de Spark configs | ❌ No disponible            | ✅ Completo                     |
| Uso de init scripts      | ❌ No soportado             | ✅ Soportado                    |
| Ideal para               | Pruebas, notebooks simples | Procesos complejos, producción |
| Gestión de recursos      | 🔄 Dinámico                | 🧑‍💻 Bajo tu control directo  |

---

### 🎯 **¿Cuál deberías elegir para tu notebook?**

| 📌 Escenario                                           | ✅ Recomendación           |
| ------------------------------------------------------ | ------------------------- |
| 📊 Análisis exploratorio rápido o presentación         | **Serverless**            |
| 🧪 Desarrollo inicial de modelos                       | **Serverless**            |
| 🛠️ ETLs complejas o procesos en producción            | **Clúster Personalizado** |
| 🔧 Necesitas Spark configs o scripts de inicialización | **Clúster Personalizado** |

---

### ✅ **Conclusión**

> Usaremos **Serverless Compute** ya que nos brinda facilidad, rapidez y menos mantenimiento.
> Usaremos el **clúster personalizado** en proyectos futuros

🧠 **Recomendación**:
Ahora por temas de costos comenzaremos con **Serverless** para notebooks pequeños o pruebas, y migraremos a un clúster para trabajos más avanzados o críticos más adelante.

---

## ⚙️ **Cómo acceder desde tu notebook al Lake Bronze** 🌊

Si ya tienes configurado lo necesario (workspace Premium, Unity Catalog habilitado, Managed Identity con permisos correctos, ADLS Gen2 con HNS y Metastore configurado), ahora es el momento de acceder a tu **Lake Bronze** desde un notebook en **Databricks**.

---

### 📑 **Pasos Conceptuales para Leer Datos desde tu Notebook**

Antes de profundizar en los detalles del código, entendamos el **flujo general**:

---

🔌 **Usando Spark para Leer desde ADLS Gen2**

1. **Acceder a un archivo Parquet**  
    El método más común para leer datos desde tu **Lake Bronze** es usar Spark con el formato adecuado.
    
    Aquí está la idea conceptual de cómo sería el código (¡sin copiar y pegar directamente, solo para entender el flujo!):
    
    > `spark.read.format("parquet").load("<ruta usando abfss://...>")`
    
    Esto leería el archivo en el contenedor de **Azure Data Lake Storage Gen2 (ADLS Gen2)**, siempre y cuando la **External Location** esté correctamente configurada con permisos adecuados.
    
    📝 **Nota**: Esta operación solo funcionará si tienes los permisos correctos sobre la _External Location_.
    

---

🔄 **Alternativa: Usando Tablas Externas Registradas en Unity Catalog**

2. **Acceder a Datos a Través de Tablas Externas**  
    Si registraste previamente una tabla externa en **Unity Catalog** que apunte a ese contenedor de **Lake Bronze**, el proceso es aún más sencillo.
    
    Aquí el flujo sería algo como:
    
    > `SELECT * FROM <catalog>.<schema>.<tabla_externa>`
    
    En este caso, **Databricks** se encargará de gestionar la búsqueda de archivos en **ADLS Gen2** y hacer todo el trabajo por ti.
    

---

🧩 **Resumen del Flujo**

1. **Conexión a ADLS Gen2 usando Spark**
    
    - Usas **`spark.read.format("parquet")`** con la ruta correcta.
        
    - **Requiere permisos en la External Location.**
        
2. **Acceder mediante tablas registradas en Unity Catalog**
    
    - Usas **`SELECT`** en una tabla externa.
        
    - **Databricks encuentra los archivos por ti**.
        

---

🚀 **¿Qué Necesitas Para Que Esto Funcione?**

|🔑 **Requisito**|📝 **Descripción**|
|---|---|
|🌐 **Workspace Premium**|Requiere tener un **workspace Premium** activo.|
|🔐 **Unity Catalog habilitado**|Debes tener **Unity Catalog** activado y configurado.|
|🛠️ **Managed Identity**|Configura una **Managed Identity** con permisos correctos.|
|📦 **ADLS Gen2 con HNS**|**ADLS Gen2** debe estar habilitado con **Hierarchical Namespace (HNS)**.|
|🗂️ **Metastore configurado**|El **Metastore** debe estar configurado y listo.|

---

### 🧐 **¿Por qué usar tablas registradas en Unity Catalog?**

Registrar tablas externas en **Unity Catalog** es una práctica recomendada ya que:

- **Automatiza la búsqueda de archivos**: No tienes que gestionar las rutas manualmente.
    
- **Mejora la seguridad y el control de acceso**: Puedes aplicar permisos más específicos a nivel de catalogo, esquema y tabla.
    
- **Facilita el manejo de datos**: Hace que el trabajo con datos sea más organizado y accesible.

---

### 🚀 **Cómo acceder a archivos `.parquet` desde un notebook**

Ya configuraste correctamente tu entorno, así que es hora de **validarlo con pruebas prácticas.**

---

🧪 **1. Leer el archivo `.parquet` directamente desde ADLS con Pyspark**

```python
%python
spark.read.format("parquet").load("abfss://bronze@z2hdatalakesuffix.dfs.core.windows.net/SalesLT/Address/Address.parquet").show(10)
```

🔍 **Explicación del código:**

| Línea de código                | ¿Qué hace?                                                                                                                                        |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| `%python`                      | Indica que el bloque de código se ejecutará en Python.                                                                                            |
| `spark.read.format("parquet")` | Usa Spark para leer datos en formato **Parquet**.                                                                                                 |
| `.load("abfss://...")`         | Carga el archivo desde **ADLS Gen2** usando el esquema `abfss://`, que es típico para Data Lakes con autenticación mediante **Managed Identity**. |
| `.show(10)`                    | Muestra los **primeros 10 registros** en el notebook. Ideal para validar lectura.                                                                 |

📸 Resultado esperado:

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img217.png)

✅ Si todo está bien configurado (External Location, permisos, Unity Catalog), verás los datos del archivo Parquet.

---

🧱 **2. Crear un esquema y tabla externa en Unity Catalog con SQL**

```sql
-- Create schema within the valid catalog
CREATE SCHEMA IF NOT EXISTS proyecto_databricks_catalog.bronze_schema;
```

🔍 **¿Qué hace este bloque?**

| Componente                                  | Explicación                                                                   |
| ------------------------------------------- | ----------------------------------------------------------------------------- |
| `CREATE SCHEMA`                             | Crea un **nuevo esquema (namespace)** dentro del catálogo.                    |
| `IF NOT EXISTS`                             | Previene errores si el esquema ya existe.                                     |
| `proyecto_databricks_catalog.bronze_schema` | El esquema se crea dentro del catálogo llamado `proyecto_databricks_catalog`. |

📝 Este paso organiza tus objetos en Unity Catalog y te permite tener una estructura como:
**`<catalog>.<schema>.<table>`**

---

🗂️ Crear la tabla externa

```sql
-- Create external table (parquet in bronze container)
CREATE TABLE IF NOT EXISTS proyecto_databricks_catalog.bronze_schema.address
USING PARQUET
LOCATION 'abfss://bronze@z2hdatalakesuffix.dfs.core.windows.net/SalesLT/Address/Address.parquet';
```

🔍 **¿Qué hace este bloque?**

| Línea                                               | Explicación                                                                      |
| --------------------------------------------------- | -------------------------------------------------------------------------------- |
| `CREATE TABLE IF NOT EXISTS`                        | Crea una nueva tabla externa solo si no existe aún.                              |
| `proyecto_databricks_catalog.bronze_schema.address` | Define el nombre completo de la tabla (`address`) dentro del esquema y catálogo. |
| `USING PARQUET`                                     | Indica que los datos están almacenados en formato **Parquet**.                   |
| `LOCATION 'abfss://...'`                            | Ruta absoluta al archivo en **ADLS Gen2** (Lake Bronze).                         |

📸 Vista esperada en notebook:

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img218.png)

---

🔍 **3. Verificar tabla en Unity Catalog**

Una vez creada la tabla, puedes ir a la pestaña **Catalog** en el panel izquierdo y navegar:

* Catálogo: `proyecto_databricks_catalog`
* Esquema: `bronze_schema`
* Tabla: `address`

📸 Ejemplo visual:

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img219.png)

✅ Si ves esta estructura, todo fue creado correctamente.

---

📊 **4. Consultar la tabla con SQL**

```sql
SELECT * FROM proyecto_databricks_catalog.bronze_schema.address LIMIT 10;
```

🔍 **¿Qué hace?**

| Elemento                                                 | Explicación                                                                                |
| -------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| `SELECT *`                                               | Recupera **todas las columnas** de la tabla.                                               |
| `FROM proyecto_databricks_catalog.bronze_schema.address` | Indica exactamente la tabla completa que quieres consultar, incluyendo catálogo y esquema. |
| `LIMIT 10`                                               | Muestra solo los **primeros 10 registros** para evitar sobrecargar el notebook.            |
📸 Vista esperada en notebook:

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img220.png)

✅ Esto confirma que Unity Catalog puede acceder y consultar correctamente el archivo `.parquet`.

---

🧠 **Resumen final**

| 🔢 Paso | 🛠️ Acción                        | ✅ Resultado Esperado                                    |
| ------- | --------------------------------- | ------------------------------------------------------- |
| 1️⃣     | Leer `.parquet` directo con Spark | Validación básica de acceso al Lake Bronze              |
| 2️⃣     | Crear esquema en Unity Catalog    | Organización para tus objetos de datos                  |
| 3️⃣     | Registrar tabla externa           | Tabla visible en el catálogo con datos enlazados a ADLS |
| 4️⃣     | Ejecutar `SELECT` SQL             | Verifica lectura vía Unity Catalog                      |


## **¿Qué son las Capas Bronze, Silver y Gold?** 🧐

Antes de continuar, ya hemos creado el contenedor **Bronze**, donde almacenamos los datos crudos tal como llegan desde las fuentes de origen. Estos datos pueden estar desordenados o incompletos, pero nos permiten tener un historial completo y confiable.

 **1️⃣ Contenedor **Bronze**:**

- **¿Qué tiene?**: Datos crudos, tal cual llegan desde las fuentes. Pueden estar desordenados o incompletos.
    
- **Procesamiento**: No se realiza ningún procesamiento, solo se almacenan tal como están.
    
- **¿Para qué?**: Para almacenar todo el historial de datos sin modificaciones, garantizando una base confiable de origen.
    

---

**2️⃣ Contenedor **Silver**:**

- **¿Qué tiene?**: Datos parcialmente limpios, transformados y enriquecidos.
    
- **Procesamiento**:
    
    - Se eliminan duplicados
        
    - Se validan valores nulos
        
    - Se realizan transformaciones básicas
        
- **¿Para qué?**: Perfecto para análisis intermedios o operativos, donde los datos están lo suficientemente procesados para ser útiles.
    

---

**3️⃣ Contenedor **Gold**:**

- **¿Qué tiene?**: Datos completamente refinados, agregados y listos para su consumo final.
    
- **Procesamiento**:
    
    - Optimización total
        
    - Datos bien estructurados y organizados
        
- **¿Para qué?**: Para informes estratégicos, dashboards, y modelos de machine learning, proporcionando insights clave para la toma de decisiones.
    

---

**El flujo de los datos** 🔄

1️⃣ **Bronze** ➡️ 2️⃣ **Silver** ➡️ 3️⃣ **Gold**

Cada capa garantiza que los datos sean procesados y transformados en su versión más útil y óptima para el análisis y las decisiones empresariales. ¡El flujo de trabajo va de lo más crudo a lo más refinado! 

---

### **Pasos para crear los contenedores Silver y Gold** 🛠️

Para continuar con la creación de los contenedores, sigue los mismos pasos que usaste para crear **Bronze**. 

---

**Creación de los contenedores Silver y Gold** 📂

1️⃣ **Dirígete a tu Storage**: 📍 `z2hdatalakesuffix`
2️⃣ **Crea los contenedores Silver y Gold**: Solo repite los pasos de creación de contenedores que usaste para **Bronze**.
3️⃣ **¡Listo para avanzar!** 💡

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img221.png)

**Nota**: Asegúrate de que todo esté bien configurado para la correcta creación de los contenedores.

---

### **External Locations para Silver y Gold** 🌐

¡Ahora configuramos las ubicaciones externas! Aquí están los detalles para Silver y Gold. De nuevo, sigue los pasos previos para **Bronze**, solo ajustando los nombres y URLs correspondientes.

---

#### **External Location Silver** 💎

* **External Location Name**: silver\_external\_location
* **URL**:

  ```
  abfss://silver@z2hdatalakesuffix.dfs.core.windows.net/
  ```
* **Storage Credential**: my-credential

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img222.png)

---

#### **External Location Gold** 🏆

* **External Location Name**: gold\_external\_location
* **URL**:

  ```
  abfss://gold@z2hdatalakesuffix.dfs.core.windows.net/
  ```
* **Storage Credential**: my-credential

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img223.png)

---

✅ **¡Recuerda!**

* **Pasos similares** para la creación de **External Locations**.
* 💡 **Consejo**: Verifica que las credenciales estén bien configuradas para ambos entornos.
* 📝 **¡Todo a su tiempo!**: Una vez creados estos, estarás listo para continuar con los análisis.

---


## 💿✨ **Capa Silver - Transformaciones** 

> 🔄 ¡De _bronze_ a _silver_! Ahora que ya tienes tus contenedores **Silver** y **Gold** listos en tu **Data Lake**, y los **External Locations** bien definidos en **Databricks**, es hora de meterle mano a los datos 🧠.

---

### 🔧 ¿Qué vamos a hacer?

Transformaremos archivos **Parquet** ubicados en la capa **Bronze** y los guardaremos como archivos **Delta** en la capa **Silver**, corrigiendo el formato de columnas **fecha** ⏳.

>NOTA: Aunque esta transformación es básica, puedes adaptar la lógica a los requisitos de futuros proyectos

---

### ✅ **Pasos del Script de Transformación**

|Nº|Acción|Estado|
|---|---|---|
|1️⃣|Inicializar una lista vacía de tablas|☑️ Completado|
|2️⃣|Listar los directorios (tablas) en el contenedor _bronze_|☑️ Completado|
|3️⃣|Importar funciones necesarias de PySpark|☑️ Completado|
|4️⃣|🔁 Para cada tabla:|En progreso|
||➤ Leer archivo Parquet correspondiente|☑️|
||➤ Detectar columnas con fechas y convertirlas|☑️|
||➤ Guardar como Delta Table en _silver_|☑️|

---

### 👀 **Ejemplo real**

📂 Revisemos una de las tablas en **Dbeaver** donde podrás ver que contiene columna `ModifiedDate` con valores como:

```
2006-07-01 00:00:00.000
2007-04-01 00:00:00.000
...
```

📌 Esta columna está en formato `timestamp`, incluye:

- 📅 Fecha → `yyyy-MM-dd`
    
- ⏰ Hora → `00:00:00.000` (inútil en este contexto)
    
![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img224.png)

---

### ❓ ¿Por qué transformar `timestamp` → `date`?

> 💡 Convertir la columna de fecha es clave para tener datos más limpios, eficientes y fáciles de consumir.

---

🛠 **Razones de la transformación**

|🔍 Motivo|✅ Detalle|
|---|---|
|1️⃣ Formato más limpio|Se elimina la hora que **no aporta valor** (`00:00:00.000`)|
|2️⃣ Conversión de zona horaria|Usa `from_utc_timestamp(..., "UTC")` si aplica zona horaria|
|3️⃣ Mejora compatibilidad|Herramientas como Power BI y Tableau **leen mejor fechas simples**|
|4️⃣ Estandarización medallion|Silver = datos estandarizados ✨|

---

### ⚠️ ¿Qué pasa si **NO** haces la transformación?

🚫 Riesgos de mantener el `timestamp`:

- Más espacio de almacenamiento 💾
    
- Usuarios confusos al ver `00:00:00.000` 🧍‍♂️❓
    
- Agrupamientos incorrectos (`group by ModifiedDate` ➤ incluye la hora 😨)
    

---

📝 NOTA IMPORTANTE

> 📌 Siempre que migres datos a una capa **Silver**, pregúntate:
> 
> **¿Estoy dejando solo lo esencial? ¿Estoy estandarizando bien los datos?**

✔️ Menos es más → una fecha limpia es **más útil, clara y eficiente**.

---

🎯 Conclusión Final

🧹 Transformar la columna `ModifiedDate` te permite:

- ✅ Limpiar el dato innecesario (hora)
    
- ✅ Estandarizar la estructura
    
- ✅ Mejorar interoperabilidad con otras herramientas
    
- ✅ Evitar errores de análisis y agrupamiento
    

> **Una buena capa Silver es la base de un análisis de datos exitoso.** 🚀


## ⚙️✨ Script de Transformación a la Capa Silver

📍 _De archivos Parquet (🟤 Bronze) a tablas Delta (🟡 Silver)_

---

### 🧾 **Objetivo General**

> Transformar archivos **Parquet** almacenados en el contenedor **🟤 Bronze**, aplicando limpieza y formateo de fechas, y guardar los resultados como **🟡 tablas Delta** en el contenedor **Silver**.

🔧 Conversión de columnas tipo `timestamp` → `yyyy-MM-dd`.

---

### 🧠 Script Completo en PySpark

```python
# 1️⃣ Inicializar lista vacía de nombres de tabla
table_name = []

# 2️⃣ Obtener nombres de carpetas (tablas) en bronze
for i in dbutils.fs.ls("abfss://bronze@z2hdatalakesuffix.dfs.core.windows.net/SalesLT/"):
    if i.isDir():
        table_name.append(i.name.split('/')[0])
        
# Listamos la tablas
table_name

# 3️⃣ Importar funciones necesarias
from pyspark.sql.functions import from_utc_timestamp, date_format
from pyspark.sql.types import TimestampType

# 4️⃣ Procesar cada tabla
for i in table_name:
    path = f"abfss://bronze@z2hdatalakesuffix.dfs.core.windows.net/SalesLT/{i}/{i}.parquet"
    df = spark.read.format('parquet').load(path)
    columns = df.columns

    # 🔄 Convertir columnas tipo fecha
    for col in columns:
        if "Date" in col or "date" in col:
            df = df.withColumn(
                col,
                date_format(
                    from_utc_timestamp(df[col].cast(TimestampType()), "UTC"),
                    "yyyy-MM-dd"
                )
            )

    # 💾 Guardar como tabla Delta en silver
    output_path = f"abfss://silver@z2hdatalakesuffix.dfs.core.windows.net/SalesLT/{i}/"
    df.write.format("delta").mode("overwrite").save(output_path)
```

---

### ✅ **Explicación Paso a Paso**


|🧩 Paso|Acción|Detalle|
|---|---|---|
|1️⃣|Inicialización|Se crea una lista vacía para almacenar los nombres de tablas|
|2️⃣|Lectura en Bronze|Se listan carpetas en el contenedor _Bronze_|
|3️⃣|Imports|Se importan funciones para convertir timestamps|
|4️⃣|Loop por tabla|Se procesan una por una: lectura ➜ transformación ➜ guardado|
|5️⃣|Lectura de Parquet|Carga del `.parquet` a un DataFrame|
|6️⃣|Conversión de fechas|Columnas con `Date`/`date` → `yyyy-MM-dd`|
|7️⃣|Guardado en Silver|Se guarda el DataFrame como tabla Delta en modo `overwrite`|

---

✅ 1. Inicialización de una lista vacía para nombres de tablas

```python
table_name = []
```

Se crea una lista vacía `table_name` para almacenar los nombres de las tablas a procesar.

---

✅ 2. Revisión del contenedor *bronze* para identificar tablas

```python
for i in dbutils.fs.ls("abfss://bronze@.../SalesLT/"):
    if i.isDir():
        table_name.append(i.name.split('/')[0])
```

📌 Se accede al directorio `SalesLT` del contenedor *bronze* y se recorre su contenido:

* Solo se agregan los **directorios (tablas)**.
* Se eliminan las barras finales (`/`) usando `split`.

✔Listamos las **tablas**

```python
table_name
```

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img226.png)

---

✅ 3. Importación de funciones de PySpark

```python
from pyspark.sql.functions import from_utc_timestamp, date_format
from pyspark.sql.types import TimestampType
```

🔧 Estas funciones permiten:

- Convertir de UTC
    
- Dar formato a fechas
    
- Hacer casting a `TimestampType`
    

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img228.png)

---

✅ 4. Procesamiento de cada tabla identificada

📸 **Ejemplo visual*

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img229.png)

```python
for i in table_name:
```

🔁 Se inicia un bucle para procesar cada tabla individualmente.

---

✅ 5. Lectura del archivo Parquet correspondiente

```python
path = f".../{i}/{i}.parquet"
df = spark.read.format('parquet').load(path)
```

📂 Se construye la ruta del archivo `.parquet` y se carga en un DataFrame Spark.

---

✅ 6. Conversión de columnas que contienen 'Date' o 'date'

```python
for col in columns:
    if "Date" in col or "date" in col:
        df = df.withColumn(
            col, 
            date_format(
                from_utc_timestamp(df[col].cast(TimestampType()), "UTC"),
                "yyyy-MM-dd"
            )
        )
```

🔎 Se transforma toda columna que tenga "Date"/"date":

| 🔧 Transformación                | Explicación                               |
| -------------------------------- | ----------------------------------------- |
| `cast(TimestampType())`          | Convierte el valor a tipo `timestamp`     |
| `from_utc_timestamp()`           | Ajusta zona horaria desde UTC             |
| `date_format(..., "yyyy-MM-dd")` | Deja solo la parte de la **fecha limpia** |
📝 **Nota:** Esto garantiza que los datos se ajusten al formato estándar esperado por herramientas de BI y otras capas aguas abajo.

---

✅ 7. Escritura del DataFrame procesado como tabla Delta

```python
output_path = f".../silver@.../SalesLT/{i}/"
df.write.format("delta").mode("overwrite").save(output_path)
```

💾 Los datos transformados se escriben en **Silver** en formato **Delta Lake**, reemplazando lo anterior si existía.

---

### ⚠️ Si NO se hace la transformación de fecha…

| 🚨 Problema                              | 💥 Impacto                                     |
| ---------------------------------------- | ---------------------------------------------- |
| `timestamp` con `00:00:00.000`           | Ocupa más espacio y puede confundir a usuarios |
| `group by ModifiedDate` incluye la hora  | Agrupaciones incorrectas por fecha+hora        |
| Problemas con herramientas como Power BI | Interpretaciones erróneas de las fechas        |
| Dificultad de análisis en capas Gold     | Estructura inconsistente y difícil de depurar  |

---

✅ Conclusión

> Se transforma la columna `ModifiedDate` (y similares) para:
>
> * Limpiar la hora innecesaria (`00:00:00.000`)
> * Mejorar legibilidad y eficiencia
> * Aumentar compatibilidad con herramientas de BI
> * Estandarizar los datos en la arquitectura medallion


### ✔**Contenedor Silver**

📦 Los datos se encuentran ahora en formato **Delta** en el contenedor Silver:

📁 `abfss://silver@.../SalesLT/...`

📝 Ahora puedes **verificar** en el contenedor **Silver** las tablas exportadas en formato **Delta**, lo que indica que el script fue ejecutado exitosamente ✅


![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img230.png)

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img231.png)


### ✅**Validando los resultados**

🔍 Comparativa: Bronze vs Silver

```python
# Bronze
spark.read.format("parquet").load("abfss://bronze@.../Address/Address.parquet").show(10)

# Silver
spark.read.format("delta").load("abfss://silver@.../Address/").show(10)
```

✔ Al revisar los datos en Silver, verás que `ModifiedDate` ya no tiene hora → solo fecha (`yyyy-MM-dd`).

📸 **Ejemplo visual**

> `2006-07-01` ✅ vs `2006-07-01 00:00:00.000` ❌

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img234.png)

Como puedes observar el campo `ModifiedDate` ahora contiene el formato correcto


### 📌**Bronze to Silver**

Por último, puedes guardar el Notebook asignándole el nombre _Bronze to Silver_.

🔖 Esto te permitirá mantenerlo identificado como parte del flujo de ingestión y transformación de datos en la arquitectura medallion.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img232.png)


### ✅ ¡Transformación completada con éxito!

📌 ¡Tus datos están ahora transformados y listos para el análisis en la capa 🟡 **Silver**!