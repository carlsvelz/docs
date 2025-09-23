
# Acceso a ADLS Gen2

## 🗂️🔥 CATALOG

### La Evolución del Acceso Seguro a Datos en Databricks

---

### 🚫🔑 Credential Passthrough: **Descontinuado**

> ⚠️ **NOTA IMPORTANTE:**  
> Si antes habías trabajado con Databricks debes saber que a partir de **Databricks Runtime 15.0**, la opción de **credential passthrough** ha sido **eliminada** de la configuración de clústeres.  
> Ya no está disponible en la **UI de clústeres**.

---

### ❓ ¿Qué era **Credential Passthrough**?

🧾 Era una funcionalidad que permitía que los usuarios accedieran a **Azure Data Lake Storage (ADLS Gen2)** usando **su identidad personal** de Azure Active Directory (AAD).

|🧍‍♂️ Usuario|🔐 Acceso con su propia identidad|
|---|---|
|👤 Juan|Accede con su usuario AAD|
|👤 María|Accede con su usuario AAD|
|❌ Sin credenciales compartidas|✅ Más seguro y auditado|

📝 **Ventajas que ofrecía:**

- Evitaba el uso de **service principals** o claves compartidas

- Aumentaba la **seguridad basada en identidades**

- Permitía una **auditoría directa por usuario**


---

### 🔄 ¿Qué lo reemplazó?

#### ✨➡️ **Unity Catalog**: La Nueva Generación de Gobernanza de Accesos

Databricks recomienda **migrar** a **Unity Catalog**, que provee:

|🛠️ Característica|✅ Unity Catalog|
|---|---|
|🔐 Control de acceso granulado|✔️ Por catálogo, esquema, tabla y columna|
|🔎 Auditoría centralizada|✔️ Registro de accesos por usuario|
|🔗 Integración multi-workspace|✔️ Metastore compartido|
|🧭 Gobernanza unificada|✔️ Modelo consistente de permisos|
|🧾 Linaje de datos|✔️ Rastreabilidad de orígenes y destinos|

---

#### 💡 Comparación

|🧱 **Tecnología**|🛑 Credential Passthrough|✅ Unity Catalog|
|---|---|---|
|🎯 Nivel de control|Usuario general (AAD)|Granular: catálogo → columna|
|🌍 Alcance|Por clúster|Multi-workspace|
|📜 Auditoría|Limitada|Completa y centralizada|
|🔐 Identidad|Individual del usuario|Gestionado vía Unity + Identity Federation|
|📦 Soporte a futuro|❌ Descontinuado|✅ Soportado y en evolución|

---

📌📝 Notas

> 🌟 **Credential passthrough fue útil… pero estaba limitado.**  
> Ahora, Unity Catalog **no solo reemplaza**, sino que **mejora significativamente** el control de acceso con herramientas modernas, escalables y seguras.

> 🔐 **Unity Catalog** te permite aplicar **políticas centralizadas**, con **auditoría completa** y sin la necesidad de clústeres especiales o configuraciones complejas.

---

### ✅ Keys

🔻 **Credential Passthrough:**  
Descontinuado desde DBR 15.0, limitado y sin soporte futuro.

🔺 **Unity Catalog:**  
La solución actual y recomendada:

- ✔️ Más seguro

- ✔️ Más escalable

- ✔️ Con mejor trazabilidad



---

## 🚀✨ **Unity Catalog: La Nueva Era de la Gobernanza de Datos**

> 📚 **Catalog** es la evolución moderna del paradigma de gobernanza, metadatos y control de acceso a datos, superando tecnologías anteriores como _credential passthrough_ o el _Hive Metastore_ puro.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img159.png)

### 🧠 ¿Qué es Unity Catalog?

🔐 **Unity Catalog** es el componente de **gobernanza de datos** de **Databricks**. Centraliza en **un solo lugar** todo lo relacionado con:

|🔍 **Función**|✅ **Descripción**|
|---|---|
|🔐 Control de acceso|Gestión detallada de permisos de usuarios y grupos|
|📜 Auditoría|Registro de actividad: quién hizo qué y cuándo|
|🔁 Linaje de datos|Seguimiento del flujo de datos entre orígenes y destinos|
|🏷️ Etiquetado|Clasificación y organización de objetos de datos|
|🔎 Descubrimiento|Búsqueda y documentación de activos de datos|
|🧭 Metastore unificado|Compartido entre workspaces para consistencia y menor duplicación|

---

### 🧰 Funcionalidades Principales

#### 🎯 Permisos Finos por Nivel

> Control granular de acceso desde el **catálogo** hasta la **columna**.

📁 ➤ Catálogo  
📂 ➤ Esquema  
📄 ➤ Tabla  
🔤 ➤ Columna

🧑‍💼 ➕ 👥 Usuarios y Grupos

---

#### 🔍 Auditoría y Linaje

🧾 **Logs detallados** → Quién accedió, modificó o consultó cada recurso  
🔗 **Linaje** → Visualiza el recorrido completo de los datos  
📈 Desde la fuente ➡️ Transformaciones ➡️ Destino

📝 **Nota**

> La trazabilidad completa es esencial para el cumplimiento de normativas y la confianza en los datos.

---

#### 🏷️ Descubrimiento de Datos

🎯 Permite:

- 🏷️ Etiquetar objetos de datos (tablas, vistas, modelos, funciones)

- 📘 Documentarlos con descripciones útiles

- 🔍 Hacer búsquedas rápidas e inteligentes


🧩 Ejemplo de objetos:

- 🗃️ Tablas

- 👁️‍🗨️ Vistas

- 🤖 Modelos

- ⚙️ Funciones


📝 **Nota hermosa**

> "El descubrimiento efectivo acelera el análisis, reduce duplicaciones y mejora la colaboración."

---

### 🏢 Metastore Compartido

🚫 Antes:

- Cada _workspace_ tenía su propio metastore

- 🔁 Metadatos duplicados

- ⚠️ Permisos inconsistentes


✅ Ahora con **Unity Catalog**:

- 🧠 Un solo metastore centralizado

- 🔄 Compartido entre múltiples _workspaces_

- 🧩 Permisos consistentes y más fáciles de gestionar


📝 **Nota**

> "Un metastore unificado reduce la complejidad y mejora la seguridad a escala."

---

### ✅ ¿En qué se Diferencia de lo “Anterior”?

|⚙️ **Tecnología**|❌ **Antes (Hive/Legacy)**|✅ **Ahora (Unity Catalog)**|
|---|---|---|
|🔑 Control de acceso|Basado en archivos o ACLs|Fino, hasta nivel de columna|
|🔄 Reutilización|Limitada entre workspaces|Metastore compartido|
|📜 Auditoría|Poca o sin trazabilidad|Logs y linaje completo|
|🔍 Descubrimiento|Manual y disperso|Integrado, etiquetado|
|🔐 Seguridad a escala|Difícil de mantener|Consistente y centralizada|

---

#### ✅Unity Catalog

- [✅] Centralización del gobierno de datos

- [✅] Control granular de permisos

- [✅] Auditoría y trazabilidad total

- [✅] Metastore compartido entre workspaces

- [✅] Descubrimiento ágil y documentado


---

### Keys

> 💡 **Unity Catalog** no es solo una herramienta técnica:  
> Es un **cambio de paradigma** en cómo entendemos la **seguridad, descubrimiento y gobierno de datos** en el mundo moderno del _lakehouse_.

🎯 Ideal para organizaciones que buscan:

- Escalabilidad

- Seguridad de datos

- Cumplimiento normativo

- Democratización del acceso a datos


---

## ⚠️🧩 **Limitaciones y Cosas a Tener en Cuenta en Unity Catalog**

> 🎯 Aunque **Unity Catalog** representa una mejora enorme en gobernanza de datos, su adopción y operación traen ciertos **retos técnicos y organizacionales** que debes conocer.

---

### 🛠️💡 Requiere Configuración Completa

🧱 **No es plug-and-play**: Unity Catalog necesita una arquitectura bien configurada.

🔑 **Elementos a configurar:**

- 👥 Permisos y roles

- 🔐 Identidades federadas

- 🌐 _External Locations_

- 🧾 _Storage Credentials_


📦 Todo esto **para que el acceso a los datos subyacentes sea controlado y funcional**.

📝 **Nota**

> "Habilitar Unity Catalog no garantiza acceso: sin configuración adecuada del almacenamiento, los datos serán inaccesibles."

---

### 🧮📦 Compatibilidad con Clústeres y Runtimes

🔄 **No todos los clústeres soportan Unity Catalog** al 100%.

📌 **Requisitos mínimos recomendados:**

- 🚀 Databricks Runtime **11.3+**

- ❗ Algunas funcionalidades requieren versiones **aún más recientes**


|🔢 Versión de Runtime|🧰 Compatibilidad con Unity Catalog|
|---|---|
|10.x o menor|❌ No compatible|
|11.3 LTS|✅ Compatibilidad básica|
|12.x en adelante|✅ Mejores funcionalidades y soporte|

📝 **Nota**

> "Antes de migrar, revisa que los clústeres usados por tu equipo soporten Unity Catalog."

---

### 📦🔄 Migración de Datos y Permisos

🔁 **Mover todo al nuevo catálogo no es automático** y puede implicar esfuerzo técnico.

👀 Cosas a tener en cuenta:

- 📂 Migrar tablas del antiguo **Hive metastore**

- 🔁 Reescribir rutas absolutas (ej. `dbfs:/`, `abfss:/`)

- ✏️ Actualizar scripts, notebooks, y jobs

- 🧾 Reconfigurar permisos y accesos existentes


📌 **Resultado esperado:**  
🎯 Tener todos los objetos y accesos alineados con el nuevo modelo de gobernanza.

📝 **Nota**

> "Cada entorno es diferente: planifica la migración como un proyecto, no como un cambio puntual."

---

### 🚦 Retos a Considerar

|⚠️ **Aspecto**|🧩 **Detalle**|
|---|---|
|🔐 Configuración inicial|Necesaria para storage, permisos, identidades y external locations|
|🔄 Compatibilidad de clústeres|No todos los runtimes soportan UC; 11.3+ recomendado|
|🔧 Migración de metadatos|Requiere mover tablas, ajustar rutas y adaptar scripts|
|👨‍💻 Reentrenamiento del equipo|Cambia el modelo de permisos y uso de recursos|

---

### ✅ Recomendaciones Finales

🔍 **Antes de migrar:**

- Revisa tus versiones de runtime

- Evalúa tu infraestructura de almacenamiento

- Haz un inventario de tablas, scripts y permisos

- Prepara un plan de migración por fases


---

## **Data Lake** a Databricks, estos son los pasos que debes seguir:

### 🚀 **¿Qué es Databricks en el Ecosistema Azure?**

🌐 **Plataforma dentro de Azure, pero… ¿De Terceros?**

Databricks es parte del ecosistema de **Azure**, pero no es un producto _nativo_ de Azure. Es un **servicio de terceros**, lo que implica ciertos detalles técnicos importantes cuando trabajas con él.

---

🤔 **¿Qué significa esto?**

Al ser un servicio de terceros, **Databricks** no se conecta de forma **nativa** a los recursos de Azure de la misma manera que otras herramientas. Esto es crucial cuando quieres trabajar con tu **Data Lake**. 🏞️

---

❌ **¿Qué problemas genera esto?**

Cuando intentas conectar tu **Data Lake** `z2hdatalakesuffix`, en el que tienes tablas en **Parquet**, **Databricks** no se conecta directamente como **Data Factory** o **Synapse Analytics** lo harían.

🔑 **En resumen:**

- **Data Factory** y **Synapse Analytics** pueden conectarse sin configuraciones complicadas.
    
- **Databricks**, al ser un servicio de terceros, requiere pasos adicionales para la conexión. ⚙️
    

---

 📊 **Comparativa de Conexión a Data Lake**

|**Herramienta**|**Conexión Directa al Data Lake**|**Pasos Requeridos**|
|---|---|---|
|**Databricks**|❌ No directa|🔧 Requiere configuración adicional|
|**Azure Data Factory**|✅ Sí|✨ Simple, sin pasos extra|
|**Synapse Analytics**|✅ Sí|✨ Simple, sin pasos extra|

---

📌 **NOTA IMPORTANTE**

🔑 **Recuerda** que la conexión de Databricks al Data Lake no es tan sencilla como otras herramientas nativas de Azure. Aunque no es imposible, **requiere más pasos** y configuración.

---

✔️ **¿Por qué es útil saber esto?**

- 🛠️ **Evitar sorpresas** al trabajar con Databricks.
    
- 📈 **Optimizar tiempos** al comprender cómo conectar estos servicios correctamente.
    
- 🔄 **Mejorar la integración** de Databricks con otros productos de Azure.

 Entoces como lo sulucionamos Access Connector for Azure Databricks

### 🌐 **Solución: Access Connector for Azure Databricks**

🔑 **¿Qué es el Access Connector?**

El **Access Connector** es una herramienta de **Azure Databricks** diseñada específicamente para facilitar la conexión entre Databricks y los servicios de **Azure Storage** (como **Data Lake**). Usando este conector, podemos establecer una conexión segura y eficiente sin la necesidad de configurar pasos complejos manualmente. 🛠️✨

---

📊 **Proceso Simplificado con Access Connector**

|**Paso**|**Acción**|
|---|---|
|**1. Habilitar Access Connector**|Activar la herramienta en Azure Databricks.|
|**2. Configuración de Permisos**|Asignar los permisos adecuados en Azure AD.|
|**3. Configurar Conexión**|Conectar Azure Databricks con el Data Lake usando el Access Connector.|
|**4. Verificación**|Validar la conexión y accesibilidad de los datos.|

---

 🎯 **Ventajas del Access Connector**

- **🔒 Seguridad Mejorada**: Al usar un **Access Connector**, aseguras una conexión más segura entre **Databricks** y tu **Data Lake**, con autenticación de Azure Active Directory.
    
- **⚡ Integración Fácil**: Simplifica la conexión entre los servicios de Azure sin necesidad de pasos complicados.
    
- **📈 Escalabilidad**: Permite manejar grandes volúmenes de datos sin sacrificar rendimiento.
    

---

## 📝 **Creación de Access Connector for Azure Databricks en el grupo de recursos de Azure** 


---

### **Pasos para crear un Azure Data Factory en el Portal de Azure**

1. **Inicia sesión en el Portal de Azure**  
    👉 [https://portal.azure.com](https://portal.azure.com/)
    
2. **Busca el servicio "Access Connector for Azure Databricks"**
    
    - En la barra superior de búsqueda escribe **Access Connector for Azure Databricks**.
        
    - Selecciona **Access Connector for Azure Databricks** en los resultados.
        
3. **Crear un nuevo Access Connector for Azure Databricks**
    
    - Haz clic en **+ Crear** o **+ Add**.
        

---

**Sección: Project details**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img176.png)

Aquí defines a qué suscripción y grupo de recursos estará ligado el servicio.

- **Subscription**:  
    Seleccionas la suscripción de Azure donde se van a facturar y administrar los recursos. En tu caso está marcada como **Azure subscription 1**.
    
- **Resource group**:  
    El grupo de recursos donde quieres guardar este Data Factory. Ya seleccionaste `databricks_access_connector_suffix`.  
    (Los grupos de recursos sirven para organizar y administrar en conjunto todos los recursos relacionados con un proyecto: bases de datos, ADF, almacenamiento, etc.)
    


---

El siguiente paso es pasar directo a **Review + Create** y finalmente **Create** para desplegarlo.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img177.png)

---

👉 Con eso ya tienes tu **Access Connector for Azure Databricks** y listo para empezar a mover/transformar datos.

Al revisar el grupo de recursos de Azure correspondiente al proyecto, podrás confirmar que el nuevo servicio ha sido creado y ya se encuentra correctamente integrado

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img178.png)





## 🌐 **Accounts - Acceso a Azure Databricks**

### 1️⃣ **Ingresa a tu Cuenta de Azure Databricks**

🔑 **Accede a [Azure Databricks - Login](https://accounts.azuredatabricks.net/login)**

* Inicia sesión con tu **correo** y **contraseña**.

---

### 2️⃣ **Accede al Portal de Administración de Cuentas**

Después de iniciar sesión, serás redirigido a la página de administración de cuentas de **Azure Databricks**:

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img179.png)

---

### 3️⃣ **Verifica los Permisos del Usuario**

Asegúrate de que tu usuario tenga permisos de **administrador** en **Azure y Databricks**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img181.png)

---

### 4️⃣ **Accede al *Unity Catalog***

Haz clic en el botón de **Catalog** para acceder al **Unity Catalog**:

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img182.png)

---

🔑 **Repasemos ¿Qué es el *Unity Catalog*?**

El **Unity Catalog** es una herramienta esencial que permite gestionar, organizar y compartir datos en plataformas como **Databricks**.

**¿Por qué es importante?**

* 📊 Controla el acceso a los datos.
* 🔒 Gobierna los datos en toda tu infraestructura.

---

📚 **Componentes Clave del Unity Catalog**

El Unity Catalog está organizado en **3 niveles principales**:

1. **🗂️ Metastore**: El contenedor principal que organiza todo.
2. **📁 Catálogos**: Unidades dentro del Metastore, que contienen esquemas.
3. **📑 Tablas/Vistas**: Los datos reales que se utilizan para análisis.

---

💡 ***Metastore* - El Guardián de los Datos**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img183.png)


Un **metastore** es como una “base” que contiene catálogos, esquemas, tablas y vistas.

🔑 **Dentro de un Metastore:**

* **🗂️** Se organizan **Catálogos**.
* **📁** Cada catálogo puede tener **Esquemas** (bases de datos).
* **📑** Los esquemas contienen **Tablas y Vistas**.

**🔍 Notas:**

* **🌍 Región**: El **metastore** está ubicado en una región específica (por ejemplo, `eastus`).
* **🔄 Actualización**: Los **metastore** se actualizan en tiempo real.

---

📊 **Estructura del *Catalog***

Cada **Catalog** contiene los datos organizados lógicamente. Aquí tienes cómo se ve:

| **Nombre**               | **Región** | **Ruta**      | **Creado en** | **Actualizado en** |
| ------------------------ | ---------- | ------------- | ------------- | ------------------ |
| metastore\_azure\_eastus | eastus     | /path/to/data | 09/11/2025    | 09/11/2025         |

🔍 **Notas Importantes**:

* **🖥️ Nombre**: Cada **metastore** tiene un nombre único.
* **📍 Región**: Indica la ubicación geográfica del **metastore** (por ejemplo, `eastus`).
* **🕒 Fechas**: Las fechas de **creación** y **actualización** son claves para saber cuándo se actualizó por última vez.

---

### 🔧 **¿Cómo Funciona el *Unity Catalog*?**

1️⃣ **Crear un Metastore**

* **Ve a la interfaz de Unity Catalog**.
>	**📝NOTA:** Antes de proceder con el siguiente paso, debes eliminar el Metastore que viene por defecto en este caso `metastore_azure_eastus` . 
* Solo debes seleccionarlo luego busca los tres puntos en la zona izquierda y clic en **Delete**. Sin este paso previo no podrás crear un nuevo Metastore. 

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img203.png)


* Ahora que haz eliminado el metastore anterior solo **Haz clic en "Crear Metastore"** para comenzar a organizar tus datos.
	![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img184.png)

---

🚀 **Metastore en Databricks**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img185.png)

Esta es la **pantalla principal** para crear un **Metastore** dentro del **Unity Catalog** de **Databricks**.

---

🧩 **Secciones Clave del Formulario**

| 🏷️ Campo               | 📌 Descripción                                                                                                          | 🔒 Obligatorio |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------- | -------------- |
| **Name**                | Nombre único para tu Metastore. ⚠️ Debe ser identificable y claro, como `metastore_proyecto_ventas`.                    | ✅ Sí           |
| **Region**              | Región geográfica donde se alojará el Metastore. Solo podrás  asignar un solo _workspaces_ de esta región al Metastore. | ✅ Sí           |
| **ADLS Gen 2 path**     | Ruta de tu cuenta de almacenamiento ADLS Gen2 para guardar tablas gestionadas.                                          | ✅ Sí           |
| **Access Connector Id** | ID del Access Connector de Azure. Este recurso permite a Databricks acceder de forma segura al ADLS.                    | ✅ Sí           |

---

Ok, antes de continuar con con el completado del formulario debemos realizar un pasos extras. En primer lugar debemos ubicarnos en el storage de Azure en este caso en `z2hdatalakesuffix` y crear un nuevo contenedor al que llamaremos `metastorage` 

Para crear un Blob Container en Azure sigue estos pasos:

1. **Acceder a la opción de Blob Containers**:
    
    - En la barra lateral izquierda de tu cuenta de almacenamiento, selecciona **Storage browser**.
        
    - Dentro de esta sección, haz clic en **Blob containers** para visualizar los contenedores de blobs existentes.
        
2. **Crear un nuevo contenedor**:
    
    - Haz clic en el botón **+ Add container** que aparece en la parte superior de la pantalla.
        
3. **Configurar el contenedor**:
    
    - En el panel emergente que aparece a la derecha, se te solicitará que ingreses el **nombre** del contenedor. En este caso, se ha ingresado el nombre **metastorage**.
        
    - En la sección de **Anonymous access level**, selecciona la opción **Private (no anonymous access)** para garantizar que el contenedor no permita acceso anónimo.
        
4. **Finalizar la creación**:
    
    - Después de configurar el contenedor, haz clic en el botón **Create** para crear el Blob Container.
        

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img192.png)


Ahora debemos gestionar el control de acceso en un recurso de Azure:

1. **Acceder a "Access Control (IAM)"**:
    
    - En la barra lateral izquierda de tu cuenta de almacenamiento, selecciona **Access Control (IAM)**, que te permitirá gestionar permisos y roles de acceso.
        
2. **Agregar una asignación de rol**:
    
    - Dentro de la sección "Access Control (IAM)", haz clic en el botón **+ Add** y luego selecciona **Add role assignment**. Esto te permitirá asignar un rol a un usuario, grupo, o identidad administrada para controlar el acceso a los recursos.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img187.png)

En la sección de "Role" (Rol), se utiliza un cuadro de búsqueda para buscar el rol específico: **"Storage Blob Data Contributor"**. Este rol permite la lectura, escritura y eliminación de datos en los contenedores de Blob Storage en Azure.
    
1. **Descripción del rol**: Este rol permite acceso de **lectura, escritura y eliminación** a los contenedores y datos de Azure Storage Blob.
    
2. **Categoría y tipo de rol**:  Este rol pertenece a la categoría **"Storage"** y es un **rol incorporado** (**BuiltInRole**).
    

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img188.png)



Ahora clic en **Next**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img189.png)


        
3. En **Assign access to** (Asignar acceso a), selecciona **"Managed identity"**.
        
4. **Selecciona la identidad administrada de Databricks**:
    
    - En el cuadro **Select managed identities** (Seleccionar identidades administradas), busca la identidad administrada asociada con Databricks. Deberás encontrar algo como:
        
        - **Access Connector for Azure Databricks**
            
    - Esta identidad administrada se utiliza para permitir que el **Unity Catalog** de Databricks tenga acceso al almacenamiento de Azure.
        
5. **Selecciona los miembros**:
    
    - En la sección **Members** (Miembros), haz clic en **"Select members"** (Seleccionar miembros).
        
    - Busca la identidad de **Databricks** que hayas seleccionado previamente en este caso **databricks_access_connector_suffix**
    
	    ![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img190.png)
        
3. **Revisión y asignación**:

	![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img191.png)
    
    - Una vez seleccionados los miembros, haz clic en **"Review + assign"** (Revisar y asignar).
        
    - Revisa los detalles y confirma que todo esté correcto.
        
    - Finalmente, haz clic en **"Assign"** para completar el proceso de asignación del rol.
        

**Resultado Final:** *Acceso de Unity Catalog de Databricks a Azure Blob Storage* 🚀

📝 **Resumen**
Después de seguir estos pasos, el **Unity Catalog** de **Databricks** podrá interactuar con el contenedor de **Azure Blob Storage** a través del **databricks_access_connector**. Esto permitirá realizar operaciones de lectura, escritura y eliminación, todo gestionado bajo el rol **"Storage Blob Data Contributor"**.

---

🛠️ **Pasos que realizamos**

1️⃣  **Crear el Azure Blob Storage Container**

- En el portal de **Azure**, crea un contenedor en **Azure Blob Storage** donde se almacenarán los datos.
    

2️⃣ **Establecer un Rol en Azure**

- Asegúrate de que el rol **"Storage Blob Data Contributor"** esté asignado al servicio o a la entidad de seguridad que usará Databricks.
    

🔑 **¿Por qué?**  
Este rol garantiza que **Databricks** pueda realizar operaciones de:

- 📄 **Lectura** (read)
    
- ✏️ **Escritura** (write)
    
- 🗑️ **Eliminación** (delete)
    

3️⃣ **Conecta el Contenedor de Azure con Databricks**

- Desde **databricks_access_connector**, configura la conexión entre **Azure Blob Storage** y el catálogo de datos de Databricks.
    

---

 🔄 **Diagrama de flujo del proceso**

```plaintext
  [Azure Blob Storage]  --->  [databricks_access_connector]  --->  [Unity Catalog Databricks]
        |                                                             |
   🚪🔑 (Rol Storage Blob Data Contributor)                       📁 (Operaciones de CRUD)
```

---

🚨 **NOTA IMPORTANTE**

El acceso correcto al contenedor de **Azure Blob Storage** es **crucial** para que el **Unity Catalog** de **Databricks** pueda realizar las tareas necesarias sin interrupciones. Asegúrate de seguir cada paso cuidadosamente y de que las credenciales y permisos estén correctamente configurados. 🔐

---

📋 **Checklist** ✔️

|**Paso**|**Acción**|
|---|---|
|🛠️ **Paso 1**|Configurar Unity Catalog|
|🌐 **Paso 2**|Crear contenedor en Azure Blob Storage|
|🔑 **Paso 3**|Asignar rol "Storage Blob Data Contributor"|
|🔗 **Paso 4**|Conectar Azure Blob Storage con Databricks|
|✅ **Paso 5**|Verificar operaciones CRUD|

---

🛠️ **Formulario Metastore en Databricks**

Para completar el proceso de **crear un metastore** en Databricks según el formulario, sigue estos pasos:

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img193.png)

 1️⃣ **Especifica el Nombre del Metastore**

- En el campo **Name**, ingresa el nombre de tu metastore. Este será el identificador de tu metastore en Databricks.  
    Ejemplo: **metastore_suffix**.
    

2️⃣ **Selecciona la Región**

- En el campo **Region**, selecciona la región donde deseas que se cree tu metastore.
    
- La región elegida determinará en qué ubicación física se almacenarán los datos.
    
- En este caso, está seleccionado **eastus2**, pero puedes elegir otra región que te convenga.
    

3️⃣ **Configura la Ruta de ADLS Gen 2 (Opcional)**

- En el campo **ADLS Gen 2 path**, puedes especificar una ruta en **Azure Data Lake Storage Gen 2** para almacenar los datos gestionados del metastore.
    
- Asegúrate de que la ruta esté correctamente estructurada:
    

```
abfss://metastore@z2hdatalakesuffix.dfs.core.windows.net/
```

4️⃣ **Proporciona el ID del Conector de Acceso**

- En el campo **Access Connector Id**, deberás ingresar el identificador del conector de acceso.

	En tu grupo de recursos `z2h-dataengineering-project-suffix` y selecciona `databricks_access_connector_suffix`
	
	![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img194.png)
	Luego en la pestaña **Overview** copia el **Resource ID**
	
	![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img195.png)

- Este es un vínculo directo a tu recurso de acceso de Databricks, el cual debería tener el siguiente formato:
    

```
/subscriptions/{sub-id}/resourceGroups/{rg-name}/providers/Microsoft.Databricks/accessConnectors/{connector-id}
```

- Copy/Paste en el formulario.
![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img196.png)
    

 **Crear el Metastore**

- Una vez que hayas completado todos los campos, haz clic en el botón **Create** para crear tu metastore. Luego selecciona el Workspace que vamos a usar para este metastore en este caso `z2h-databricks-suffix` y luego clic en **Assign**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img197.png)

---


Luego clic

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img198.png)

Luego verás el siguiente mensaje de felicitaciones

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img199.png)