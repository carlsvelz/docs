# Service Databricks

## 📝 **Creación de Databricks en el grupo de recursos de Azure** 

---

### **Pasos para crear un Azure Data Factory en el Portal de Azure**

1. **Inicia sesión en el Portal de Azure**  
    👉 [https://portal.azure.com](https://portal.azure.com/)
    
2. **Busca el servicio "Databricks"**
    
    - En la barra superior de búsqueda escribe **Databricks**.
        
    - Selecciona **Databricks** en los resultados.
        
3. **Crear un nuevo Databricks**
    
    - Haz clic en **+ Crear** o **+ Add**.

---

**Sección: Project details**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img145.png)

Aquí defines a qué suscripción y grupo de recursos estará ligado el servicio.

- **Subscription**:  
    Seleccionas la suscripción de Azure donde se van a facturar y administrar los recursos. En tu caso está marcada como **Azure subscription 1**.
    
- **Resource group**:  
    El grupo de recursos donde quieres guardar este Data Factory. Ya seleccionaste `z2h-dataengineering-projec-suffix`.  
    (Los grupos de recursos sirven para organizar y administrar en conjunto todos los recursos relacionados con un proyecto: bases de datos, ADF, almacenamiento, etc.)
- **Pricing Tier**: 
	Los _tiers_ o niveles de suscripción de **Azure Databricks**, donde aparecen opciones como _Standard_, _Premium (+ Role‑based access controls)_, _Trial (Premium‑14‑Days Free DBUs)_. A continuación veamos detalladamente qué significa cada opción, qué diferencias hay, y qué implicaciones de roles, precio y esto con actualización 2025.

---

**¿Qué es “DBU” y qué es Azure/Databricks?**

- **DBU** significa _Databricks Unit_. Es la unidad de procesamiento que usa Databricks para facturar el uso de cómputo (clusters, notebooks, trabajos, etc.). Cuantos más recursos usas más DBUs consumes.
    
- Azure Databricks tiene distintos **niveles o SKU** (Standard, Premium, etc.) que determinan qué funciones tienes disponibles, especialmente en seguridad, gestión de acceso, auditoría, etc.
    

---

**Niveles**

En la imagen tienes tres opciones principales:

1. **Standard (Apache Spark, Secure with Microsoft Entra ID)**
    
2. **Premium (+ Role‑based access controls)**
    
3. **Trial (Premium – 14‑Days Free DBUs)**
    

---

**Qué ofrece cada nivel (a 2025)**

1. *Standard*

Este nivel es el más básico que permite ejecutar cargas de trabajo con Apache Spark, integrándose con Microsoft Entra ID (antiguo Azure AD) para la autenticación, pero con capacidades limitadas en cuanto a control de acceso fino, auditoría avanzada, etc. En resumen:

- Puedes autenticar usuarios con Entra ID, single sign‑on, etc.
    
- Acceso básico a los recursos del workspace o clúster.
    
- No se incluyen muchas de las funciones de seguridad y gobernanza avanzada.
    
- Costos más bajos por DBU comparados con Premium.
    

2. *Premium (+ Role‑based access controls)*

Este nivel añade características de seguridad y gestión de acceso mucho más finas. “Role‑based access controls” (RBAC) significa que puedes definir permisos basados en roles / grupos para controlar quién puede hacer qué, sobre qué recursos (notebooks, clusters, jobs, tablas, etc.). Algunas diferencias clave:

- En Premium puedes **activar control de acceso en el workspace**, de modo que no todos los usuarios vean o manipulen todos los objetos. 
    
- Permisos sobre **notebooks, clusters, jobs, tablas** (lectura, escritura, ejecución, eliminación) según rol del usuario o grupo. 
    
- Auditoría: logs de quién hizo qué y cuándo.
    
- Funcionalidades adicionales de seguridad: autenticación condicional (“conditional authentication”), pasar credenciales de AD (Azure AD credential passthrough), políticas de clúster, reglas de acceso de IP, etc. 
    
- Costos mayores por DBU que en Standard, ya que estás pagando por capacidades más avanzadas de gestión, seguridad y gobernanza. 
    

3. *Trial (Premium – 14 Days Free DBUs)*

- Es una versión de prueba del nivel Premium: durante 14 días tienes DBUs gratis para explorar las funciones de Premium, incluir RBAC, acceso condicional, auditoría, etc.
    
- Tiene limitaciones de tiempo y posiblemente de capacidad: tras los 14 días, debes escoger Standard o Premium pagando por uso.
    
- Ideal para evaluar si realmente necesitas las capacidades avanzadas antes de comprometerte con Premium.
    

---

**Detalles / implicaciones de “Role‑Based Access Control” (RBAC)**

- Bajo Premium, puedes definir **roles/grupos** y asignar permisos específicos a esos roles. Por ejemplo, que un usuario pueda ejecutar notebooks, pero no modificarlos; o que tenga acceso solamente a ciertas tablas. Esto permite aplicar el principio de mínimo privilegio. 
    
- También con Premium se habilitan **listas de control de acceso (ACLs)** sobre objetos del workspace (notebooks, clústeres, jobs…). En Standard esto suele estar deshabilitado o muy limitado. 
    
- En Unity Catalog, además, hay una característica que está en _Beta_ a 2025: **Attribute‑Based Access Control** (ABAC). Esta permite usar etiquetas y atributos de datos/usuario para hacer acceso condicional/dinámico, no solo roles estáticos. 
    

---

**Costos y trade‑offs**

*Ventajas de Premium:*

- Mucha más seguridad y capacidad de cumplimiento normativo.
    
- Mejor control de quién accede a qué, lo que reduce riesgo de fugas de datos o accesos indebidos.
    
- Funcionalidades útiles para ambientes productivos/multigrupo/grandes empresas.
    

*Desventajas:*

- Costo por DBU mayor. El precio por unidad de procesamiento sube. 
    
- Mayor complejidad operativa: gestión de roles, permisos, auditorías, configuración de políticas de seguridad.
    
- Posible que algunas funciones avanzadas estén en preview (Beta) o no disponibles en todas las regiones inmediatamente.
    

---

**Cuándo conviene cada uno**

| Situación                                                                                                             | Nivel recomendado              |
| --------------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| Proyecto pequeño, pocos usuarios, pocos requisitos de seguridad / gobernanza                                          | Standard puede ser suficiente. |
| Empresa mediana o grande, varios equipos, requisitos regulatorios, necesidad de control fino, auditoría, cumplimiento | Premium.                       |
| Solo quieres evaluar funciones, hacer pruebas antes de decidir                                                        | Trial de Premium.              |

🔍**Tabla comparativa: Azure Databricks – Standard vs Premium vs Trial (2025)**

|Característica / Tecnología|🟦 Standard|🟨 Premium (+ RBAC)|🧪 Trial (Premium - 14 días)|
|---|---|---|---|
|**Apache Spark**|✅ Sí|✅ Sí|✅ Sí|
|**Microsoft Entra ID (Azure AD)**|✅ Básico (autenticación)|✅ Avanzado (autenticación + autorización)|✅ Avanzado|
|**Delta Lake**|✅ Compatible|✅ Compatible|✅ Compatible|
|**Delta Live Tables (DLT)**|✅ Básico (modo estándar)|✅ Con monitoreo y calidad de datos avanzada|✅ Acceso completo durante prueba|
|**Apache Iceberg (en Unity Catalog)**|⚠️ Limitado (requiere Unity Catalog habilitado)|✅ Totalmente compatible|✅ Compatible durante prueba|
|**Unity Catalog (gobernanza de datos)**|❌ No|✅ Sí|✅ Habilitado durante prueba|
|**Control de acceso basado en roles (RBAC)**|❌ No|✅ Sí|✅ Sí|
|**Listas de Control de Acceso (ACLs)**|❌ No|✅ Para notebooks, clústeres, jobs, etc.|✅ Habilitado|
|**Attribute-Based Access Control (ABAC)** _(Preview)_|❌ No|✅ Sí (en preview 2025)|✅ Sí (durante prueba)|
|**Auditoría y logs de acceso**|⚠️ Limitado (solo logs generales)|✅ Completos (acciones por usuario, objeto, etc.)|✅ Habilitado|
|**Passthrough de credenciales de Azure AD**|❌ No|✅ Sí (secure access a Data Lake, SQL, etc.)|✅ Durante prueba|
|**Enforce IP access lists**|❌ No|✅ Sí|✅ Sí (temporalmente)|
|**Enforce workspace admin roles**|❌ No|✅ Sí|✅ Durante prueba|
|**Cluster Policies (restricción de clústeres)**|❌ No|✅ Sí|✅ Habilitado|
|**Job compute isolation (seguridad en ejecución)**|⚠️ Parcial|✅ Sí (clusters separados por job)|✅ Sí|
|**Precio por DBU (referencial)**|💲 Bajo|💲💲 Medio–Alto|✅ Gratis 14 días (luego pagarás Premium)|

En neustro caso, si no has usado previamente el trial de Premium, selecciona esta opción para no incurrir en gastos. Ahora, si ya no tienes esta opción, selecciona Premium, ya que lo usaremos tanto para este proyecto como para proyectos posteriores donde veremos opciones avanzadas

Una vez completado estos pasos clic en **Review + Create** y finalmente **Create** para desplegarlo.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img146.png)


👉 Con eso ya tienes tu **Databricks desplegado** y listo para empezar  a transformar datos.

Al revisar el grupo de recursos de Azure correspondiente al proyecto, podrás confirmar que el nuevo servicio ha sido creado y ya se encuentra correctamente integrado

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img147.png)

Ahora, simplemente hacemos clic en **Launch Workspace** para acceder al panel administrativo.


![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img148.png)

---

## 🧭 **Tu Workspace en Databricks**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img149.png)

**Este panel es el workspace principal de Azure Databricks**, es decir, el espacio de trabajo desde donde comenzamos a desarrollar nuestras soluciones de análisis de datos, ciencia de datos e inteligencia artificial. Desde aquí podemos crear notebooks, ejecutar trabajos, explorar datos con SQL, entrenar modelos de machine learning y conectarnos a diversas fuentes de datos. Todo esto dentro de una interfaz colaborativa y altamente integrada con el ecosistema de Azure.

Aquí es donde empieza todo. Piensa en esto como tu cuaderno digital, tu laboratorio y tu centro de control, todo en uno. Ahora te explico para qué sirve cada parte del menú de la izquierda:

---

>**NOTA:** 
>El menú y las funcionalidades de Databricks pueden variar según la edición que estés usando (Standard, Premium o Enterprise), el proveedor cloud (Azure, AWS, GCP) y las características que estén habilitadas en tu entorno, como Unity Catalog, SQL Warehouses o herramientas de machine learning. Por ejemplo, la edición Premium ofrece más controles de seguridad y administración, mientras que en Azure puedes ver integraciones específicas con servicios de Microsoft. Además, algunos ítems del menú pueden aparecer o desaparecer según tu rol de usuario o si estás en una vista previa de funciones nuevas.

### 🔴 **New**

¿Quieres empezar algo nuevo? Este botón es como un lienzo en blanco. Desde aquí puedes crear notebooks, cargar datos, arrancar trabajos o hasta lanzar experimentos de machine learning. Es el primer paso para cualquier proyecto.

---

### 📁 **Workspace**

Este es tu escritorio personal (o el de tu equipo). Aquí guardas tus notebooks, scripts, dashboards... todo lo que vayas construyendo. Puedes organizarlo por carpetas como si fuera tu nube personal dentro de Databricks.

---

### 🕒 **Recents**

¿Abriste algo hace un rato y no recuerdas dónde lo dejaste? Aquí lo tienes. Es como tu historial de navegación, pero solo con lo importante.

---

### 📚 **Catalog**

Aquí están todas las bases de datos y tablas que tienes a mano. Es tu biblioteca de datos. Si estás usando Unity Catalog, además tendrás control de accesos, permisos y versiones en un solo lugar.

---

### 🔄 **Jobs & Pipelines**

Cuando algo necesita ejecutarse automáticamente (como un ETL diario, un entrenamiento de modelo o una limpieza de datos), lo configuras aquí. Todo queda programado y monitorizado sin que tengas que estar pendiente.

---

### ⚙️ **Compute**

Aquí vives y mueres por los clústeres. Si quieres ejecutar algo, necesitas potencia, y esta es la sección para crear, escalar o apagar tus entornos de cómputo.

---

### 🛒 **Marketplace**

¿Buscas datasets interesantes, notebooks listos para usar o herramientas de terceros? Aquí puedes explorar y conectar recursos externos sin salir de Databricks.

---

##  **SQL — Donde los analistas se sienten en casa**

- **SQL Editor**: Es tu editor SQL, puro y duro. Ideal para los que quieren consultas rápidas sin abrir un notebook.
    
- **Queries**: Aquí se guardan tus consultas. Piensa en ello como tu colección personal.
    
- **Dashboards**: Transforma esas consultas en algo visual y compartible. Perfecto para mostrar KPIs o resultados a otros equipos.
    
- **Genie**: Tu asistente personal que convierte lenguaje natural en SQL (sí, como magia, pero con datos).
    
- **Alerts**: Configura notificaciones si los resultados de tus consultas cambian o superan ciertos valores.
    
- **Query History**: Un registro de todas las consultas que se han lanzado. Ideal para seguir la pista de lo que se ha hecho.
    
- **SQL Warehouses**: Infraestructura optimizada solo para ejecutar SQL, más rápido y eficiente que un clúster general.
    

---

## 🛠️ **Data Engineering — La parte más “backstage” del show**

- **Job Runs**: Aquí revisas qué se ejecutó, cuándo y cómo fue. Si algo falló, este es el primer lugar para mirar.
    
- **Data Ingestion**: Es como una rampa de carga de datos. Desde aquí traes información desde archivos, bases de datos externas o sistemas conectados.
    

---

## 🤖 **AI/ML — Si creas modelos**

- **Playground**: Como su nombre indica, un espacio para probar cosas, rápido y sin presiones.
    
- **Experiments**: Aquí haces seguimiento a tus pruebas de modelos. Compara resultados, versiones y elige al ganador.
    
- **Features**: Un almacén de características reutilizables para tus modelos. Útil cuando trabajas en equipo.
    
- **Models**: Gestionas todo lo que has entrenado. Versiones, métricas, estados... todo queda registrado.
    
- **Serving**: ¿Tienes un modelo que funciona? Desde aquí lo despliegas y lo pones en producción con un endpoint listo para usar.
    

---

>**NOTA:** Databricks no es solo notebooks y código, es una plataforma completa donde puedes ir desde la exploración de datos hasta la puesta en producción sin salir del mismo entorno.

---




