
# RunTime

## SQL Server Linux

Validemos nuestro actual escenario:  
Tienes **SQL Server en Linux** (dentro de un contenedor Docker) en una **VM local**, y quieres conectarlo a **Azure Data Factory (ADF)**.

La idea general es que **ADF no se conecta directamente a entornos on-premises** o privados, sino que necesita un puente seguro: **Self-hosted Integration Runtime (SHIR)**.

---

### 1️⃣ El problema: SHIR no corre en Linux

El **Self-hosted Integration Runtime** solo está disponible para:

- Windows (nativo)
    
- Windows Server (nativo)
    

No existe versión oficial de SHIR para Linux o contenedores Linux.  
Por eso, si quieres conectar ADF a tu SQL Server local (aunque sea Linux), **el runtime debe instalarse en algún host Windows** que pueda ver tu contenedor en la red.

---

### 2️⃣ Opciones para tu caso

#### **Opción 1 – Instalar SHIR en una máquina Windows que tenga acceso a la VM Linux**

- Instalas el SHIR en un servidor o PC Windows en tu red.
    
- Configuras el SHIR en ADF.
    
- Aseguras que desde esa máquina Windows se pueda hacer ping/conectar al SQL Server del contenedor (`ip_vm_linux:1433`).
    
- Ventajas:
    
    - Fácil de implementar.
        
    - 100% soportado por Microsoft.
        
- Desventajas:
    
    - Necesitas tener un equipo Windows encendido.
        

---

#### **Opción 2 – Usar una VM Windows como “puente” en la misma red que la VM Linux**

- Creas una VM Windows (en el mismo host físico o en Azure con VPN a tu red).
    
- Instalas el SHIR ahí.
    
- La VM Windows se conecta al contenedor SQL Server en la VM Linux.
    
- Ideal si tu entorno es todo virtualizado (Hyper-V, VMware, Proxmox, etc.).
    

---

#### **Opción 3 – SHIR en Windows dentro de un contenedor Docker**

- No hay imagen oficial, pero podrías usar un contenedor **Windows** con SHIR instalado.
    
- Esto requiere que tu host soporte **Windows Containers** (solo en Windows Server o Windows 10/11 con Hyper-V).
    
- En Linux puro, **no es viable** porque Windows Containers no se ejecutan nativamente en Docker Linux.
    

---

#### **Opción 4 – Evitar SHIR usando acceso público seguro**

- Expones SQL Server al público (muy poco recomendable) o a través de un túnel seguro:
    
    - **VPN site-to-site** desde Azure a tu red local.
        
    - **Azure ExpressRoute** (opción empresarial).
        
    - **Túnel SSH inverso** a una VM en Azure que tenga IP pública.
        
- Esto permitiría a ADF conectarse como si fuera un SQL Server en Azure, sin SHIR.
    
- Ventajas:
    
    - No necesitas un host Windows.
        
- Desventajas:
    
    - Configuración más compleja.
        
    - Riesgos de seguridad si no se protege bien.
        

---
💡 **Recomendación práctica:**

En este caso usaremos la **opción 1**, ya que es la más fácil de implementar. Para este caso de uso, **necesitas tener Windows como tu sistema operativo principal**, ya que el SHIR (Self-hosted Integration Runtime) solo se puede instalar en entornos Windows.

Si tienes restricciones para usar Windows directamente, la otra opción sería **montar el SHIR en una máquina virtual (VM) con Windows** que tenga acceso de red a tu contenedor de SQL Server. Es el camino más sencillo, soportado y estable.

De esta forma, puedes mantener tu entorno principal en Linux o en la nube, mientras aprovechas las capacidades del SHIR de forma compatible y sin complicaciones de red o compatibilidad.


## **Instalación RUNTIME**


Perfecto 🚀, ya tienes la VM Linux con SQL Server en Docker y el reenvío de puertos hacia tu host Windows (1422 → 1433 del contenedor).  
Ahora solo falta instalar y configurar el **Self-hosted Integration Runtime (SHIR)** en Windows.

---

### 🔹 1. Descargar e instalar el SHIR en Windows

1. Inicia sesión en el **portal de Azure**.
    
2. Ve a tu **Azure Data Factory (ADF)**.
    
3. En el menú lateral, selecciona **Administrar** (icono de engranaje).
    
4. Dentro de **Integration runtimes**, haz clic en **Nuevo**.
    
5. Elige **Azure Self-Hosted** y selecciona **Continuar**.
	
6. Elige **Self-Hosted** y selecciona **Continuar**.
	
7. Asignaremos el nombre por default y creamos la conexión
    
8. Validamos los datos del nuevo Runtime 

**PASO 4:** Dentro de **Integration runtimes**, haz clic en **Nuevo**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img42.png)

Como verás Integration Runtimes ya viene con un Runtime por defecto llamado: 

**Integration Runtimes en Azure Data Factory (ADF)**

---

🔧 ¿Qué es un Integration Runtime (IR)?

➡️ Es el **motor de ejecución** que permite a Azure Data Factory mover y transformar datos desde y hacia diferentes orígenes y destinos.

---

📌 TIPOS DE INTEGRATION RUNTIME:

📊 Tabla resumen:

|Tipo de IR|¿Qué hace?|¿Dónde se usa?|
|---|---|---|
|AutoResolveIntegrationRuntime|Es el runtime administrado por Azure. Decide la región automáticamente.|Para conexiones en la nube pública.|
|Self-hosted IR (SHIR)|Se instala en tu red local para acceder a datos en entornos on-premise.|Acceso a sistemas internos/privados.|

---

🧠 ¿Qué es **AutoResolveIntegrationRuntime**?

✔️ Es el **runtime por defecto** que viene ya preconfigurado en cada instancia de Azure Data Factory.

✔️ No requiere instalación ni configuración manual.

✔️ ADF decide automáticamente en qué región de Azure ejecutarlo.

✔️ Se utiliza principalmente cuando los **orígenes y destinos están accesibles públicamente** (por ejemplo: Blob Storage, SQL Azure, etc.)

➡️ Por defecto, ADF intentará usar la **misma región** donde está creada la instancia de Data Factory.  
➡️ Si no es posible, usará la región más cercana disponible.

---

📝 NOTA:  
Si solo estás trabajando con datos que ya están en Azure (como bases de datos en la nube, almacenamiento en la nube, etc.), **NO necesitas instalar nada adicional**. El AutoResolveIntegrationRuntime hace todo el trabajo por ti.

---

💡 EJEMPLO DE USO:

Imagina que tienes una actividad de copia de datos desde una base de datos SQL en Azure hacia un Blob Storage también en Azure.

✔️ No necesitas configurar un IR manual.  
✔️ ADF usará automáticamente el **AutoResolveIntegrationRuntime**.  
✔️ Se ejecutará en la misma región (si es posible).

---

🧱 DIFERENCIA CLAVE CON SHIR:

📋 Lista comparativa:

- 🌐 **AutoResolveIntegrationRuntime**
    
    - ✅ Totalmente administrado por Azure
        
    - 🔒 No accede a redes privadas
        
    - 🚀 Ideal para conexiones 100% en la nube
        
- 🖥️ **Self-hosted Integration Runtime (SHIR)**
    
    - 🧰 Necesita instalación local
        
    - 🌐 Puede acceder a bases de datos y sistemas en red privada
        
    - 🔐 Usa una puerta de enlace para conexión segura
        

---

✔️ CONCLUSIÓN:

El **AutoResolveIntegrationRuntime** es perfecto si trabajas solo con servicios públicos en la nube.  
Si necesitas conectar con recursos locales (como servidores SQL en tu empresa), entonces debes usar el **SHIR**.

---

❗RECUERDA:

- No confundas AutoResolve con IR personalizados: solo AutoResolve está preconfigurado por defecto.
    
- No necesitas pagar extra por usar el runtime automático, viene incluido con ADF.


#### **PASO 5:** Elige **Azure, Self-Hosted** y selecciona **Continuar**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img43.png)

---

#### **PASO 6:** Elige **Self-Hosted** y selecciona **Continuar**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img44.png)

---

#### **PASO 7:** Asignaremos el nombre por default y creamos la conexión 

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img45.png)


---

#### **PASO 8:** Validamos los datos del nuevo Runtime y descargamos el instalador (archivo `.msi`) que te dará Azure.


![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img46.png)

---

#### **Keys de autenticación en Self-hosted Integration Runtime (SHIR)**

🔑 ¿Qué son las Key1 y Key2 en SHIR?

➡️ Son **claves de autenticación** generadas automáticamente por Azure Data Factory cuando creas un **Self-hosted Integration Runtime**.

➡️ Se utilizan para **registrar e identificar** una instalación local del runtime con el servicio de Azure Data Factory en la nube.

---

📦 ¿Dónde se usan estas claves?

🖥️ Cuando instalas el **Self-hosted IR** en una máquina (normalmente con Windows), el instalador te pedirá una de estas claves.

✅ Al pegar la clave en el instalador:

- Se establece una conexión segura entre tu máquina local y Azure Data Factory.
    
- El runtime queda **vinculado** con el IR que creaste en el portal de ADF.
    

---

🛡️ ¿Por qué existen dos claves?

📋 Tabla de uso de claves:

|Clave|Uso Principal|¿Se puede regenerar?|¿Interrumpe el servicio?|
|---|---|---|---|
|Key1|Se usa como clave activa o principal|✅ Sí|❌ No|
|Key2|Clave secundaria o de respaldo para rotación segura|✅ Sí|❌ No|

📝 NOTA:  
Tener dos claves permite que puedas **rotar las claves por seguridad** sin tener que detener el servicio ni perder la conexión entre tu máquina y ADF.

---

🔄 ROTACIÓN DE CLAVES: ¿Cómo funciona?

1. 🔐 Supón que estás usando **Key1** activamente.
    
2. 💡 Por razones de seguridad, decides regenerar la clave.
    
3. ✅ Vas al portal de Azure y regeneras **Key2**.
    
4. 🛠️ Luego, en tu instalación local, cambias la clave usada por la nueva **Key2**.
    
5. 🧹 Ahora puedes **regenerar Key1** sin afectar la conexión ni la ejecución de actividades.
    

✔️ Este mecanismo permite una **rotación segura**, similar a cómo funcionan las claves de conexión en muchos servicios de Azure.

---

📝 NOTA IMPORTANTE:

- Ambas claves son igual de válidas.
    
- Puedes usar cualquiera de las dos para registrar una nueva máquina con el mismo SHIR.
    
- Si pierdes ambas claves, tendrás que generar nuevas e **instalar o actualizar manualmente** los SHIR que dependan de ellas.
    

---

⚠️ SEGURIDAD:

🔒 Estas claves son **sensibles** y deben mantenerse privadas.  
📤 No las compartas ni las subas a repositorios de código.  
🔁 Rota las claves periódicamente para mantener la seguridad de tu entorno.

---

💬 RESUMEN VISUAL:

✔️ 2 claves generadas automáticamente  
✔️ Se usan en la instalación del SHIR  
✔️ Vinculan el runtime local con ADF  
✔️ Permiten rotación sin interrupción  
✔️ Claves sensibles: ¡protección máxima!

---

### 🔹 2. Instalar SHIR con tu Data Factory

1. Instalación y configuración **Microsoft Integration Runtime Configuration Manager**.
	
2.  Una vez instalado, abre la aplicación **Microsoft Integration Runtime Configuration Manager**.

#### **PASO 1:** Instalación y configuración **Microsoft Integration Runtime Configuration Manager**.

Hay dos formas de instalar el Self-hosted Integration Runtime (SHIR) en Windows:

Option 1: Express setup 👉 es la forma más rápida y automática. Descarga e instala directamente el runtime en el mismo equipo desde el portal de Azure, sin necesidad de copiar manualmente claves de autenticación. Ideal si ya estás en la máquina donde quieres tener el SHIR.

Option 2: Manual setup 👉 es un proceso más flexible. Te permite descargar el instalador (.msi), ejecutarlo en cualquier servidor/PC Windows y, durante la instalación, registrar el runtime usando las keys de autenticación que te da Azure. Es útil cuando instalas en otro equipo distinto o cuando prefieres más control en el proceso.

✔️Express setup es rápido y directo para la máquina actual, mientras que el Manual setup es la opción recomendada si lo instalarás en un servidor dedicado o en una máquina distinta a donde estás navegando el portal.

👉En nuestro caso usaremos la **Opción Express Setup** solo debes hacer clic sobre esta opción descarga el instalador solo debes proceder con la instalación automática y express

---

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img47.png)

---

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img48.png)

---

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img49.png)

En la ventana verás la confirmación de que terminaste con éxito la instalación mediante el **Express Setup** del **Self-hosted Integration Runtime (SHIR)** 🚀.

El asistente hizo automáticamente estos pasos:

1. **Loading configuration** → cargó la configuración de tu runtime.
    
2. **Downloading Integration Runtime (Self-hosted)** → descargó los binarios necesarios.
    
3. **Installing Integration Runtime (Self-hosted)** → instaló el servicio en tu máquina Windows.
    
4. **Registering Integration Runtime (Self-hosted)** → registró tu runtime con Azure Data Factory, por lo que ya aparece vinculado en el portal.
    

👉 Ahora, tu SHIR ya está instalado, registrado y listo para usarse.  
El siguiente paso es **crear un Linked Service en ADF** que use este runtime y apunte a tu SQL Server (ejemplo: `localhost,1422`) para probar la conexión.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img50.png)

---


![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img51.png)

¡Perfecto! Ahora solo debes ejecutar la aplicación recién instalada y podrás ver la pantalla **Microsoft Integration Runtime Configuration Manager**, y confirma que tu **Self-hosted Integration Runtime (SHIR)** ya está instalado y **conectado correctamente al servicio de Azure Data Factory**.

Los puntos clave de lo que ves:

- ✅ **Self-hosted node is connected to the cloud service** → el runtime está enlazado a tu Data Factory (`z2h-adf-suffix`).
    
- 🔗 **Integration Runtime: integrationRuntime1** → este es el nombre del runtime que creaste en el portal.
    
- 💻 **Node: Z2H** → es el nombre de tu máquina Windows que está actuando como host del SHIR.
    
- 🔐 **Data Source Credential** → indica que las credenciales que uses para conectarte a tus orígenes de datos (ej. SQL Server en Linux) se almacenan localmente en esta máquina y están sincronizadas con el runtime.
    
- 📦 Opciones de **backup/import** → sirven para exportar e importar las credenciales si necesitas migrar o restaurar el runtime en otra máquina.
    

👉 Tu SHIR ya está activo y vinculado.  
El siguiente paso es ir al portal de Azure Data Factory y crear un **Linked Service** que use este runtime para conectarse a tu SQL Server en Linux (`localhost,1422`).


### 🔹 3. Configurar la conexión al SQL Server (puerto 1433)

1. Desde ADF, crea una actividad **Copy Data**
    
2. En **Servidor**, pon la IP o nombre de tu host Windows (ejemplo: `127.0.0.1,1433`.
    
3. Usuario y contraseña: las credenciales que definiste al levantar SQL Server en Docker.
    
4. Selecciona el runtime recién creado (SHIR).
    
5. Haz clic en **Probar conexión**.


#### **Crea una actividad Copy Data**

**¿Qué es la actividad Copy Data en Azure Data Factory?**

---

🚚 **Copy Data** es una de las actividades más importantes y comunes en ADF.  
Sirve para **mover datos desde un origen hasta un destino (sink)**, ya sea en la nube o local (on-premises).

---

🔍 ¿Qué hace exactamente?

➡️ Conecta con un origen de datos (ej: SQL Server, Blob Storage, API, etc.)  
➡️ Lee los datos (usando consulta o tabla completa)  
➡️ Mapea y transforma columnas (si es necesario)  
➡️ Escribe los datos en un destino (sink)  
➡️ Usa un Integration Runtime (AutoResolve o SHIR) para ejecutar la actividad

---

📦 **Soporta múltiples escenarios:**

📋 Lista de capacidades clave:

- 🌐 Orígenes y destinos en la nube o en red privada
    
- ⚙️ Control de formato (JSON, CSV, Parquet, Avro, etc.)
    
- 🗺️ Mapeo de columnas personalizado
    
- 🚀 Paralelismo y particionado (para rendimiento)
    
- 🔁 Cargas completas o incrementales
    
- 🧯 Opciones de tolerancia a errores (fault tolerance)
    

---

🛠️ ¿Cuándo usarla?

✔️ Ideal para:

- Migraciones simples
    
- Integraciones periódicas
    
- ETL ligeros (solo movimiento de datos)
    
- Cargas rápidas sin transformación compleja
    

❌ NO es ideal para:

- Procesos complejos de transformación de datos (usa Data Flows o Synapse en ese caso)
    

---

📝 NOTA:  
Toda actividad **Copy Data** requiere un Integration Runtime:

- 🌐 **AutoResolveIntegrationRuntime** si todo está en la nube
    
- 🏠 **Self-hosted IR (SHIR)** si el origen o destino está en red privada/local
    

---

🧪 ESCENARIO: **Tu entorno SQL en contenedor Linux (puerto 1422)**

Pasos detallados para ejecutar una Copy Data desde ese SQL:

🔢 **0) PRERREQUISITOS (verificados):**

✔️ SHIR instalado y conectado  
✔️ Reenvío de puertos configurado:  
➡️ Windows:1422 ➝ Linux Container:1433  

---

📋 **1) Crear pipeline con actividad Copy Data**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img52.png)

➡️ Ve a la sección **Author > Pipelines > New Pipeline**  
➡️ Arrastra `drag and drop` la actividad **Copy Data**  al panel


📋 **2) Pestaña General**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img53.png)

Ahora en la  **pestaña General** de la actividad _Copy data_ en tu pipeline define el **primer paso de configuración de la actividad**, que es básicamente darle identidad y control a la ejecución:

- **Name** → Asigna el nombre _Copy address table_ (será la etiqueta con la que identifiques esta actividad dentro del pipeline).
    
- **Description** → opcional, puedes documentar qué hace esta copia (ej. “Copia la tabla Address de SQL Server hacia Blob Storage”).
    
- **Activity state** → lo dejaremos en **Activated**, lo que significa que la actividad se ejecutará cuando corra el pipeline. Si la pusieras en **Deactivated**, quedaría como parte del pipeline pero se saltaría en la ejecución.
    
- **Timeout** → configurado en `0.12:00:00`, que significa 12 horas máximo antes de que la actividad falle por tiempo excedido.
    
- **Retry** y **Retry interval (sec)** → 0 reintentos, con intervalo de 30 segundos (si pusieras por ejemplo 3, intentaría 3 veces más en caso de error, esperando 30 segundos entre intentos).
    
- **Secure output** → desmarcado; si lo activas, la salida de esta actividad no se mostraría en logs/monitor para proteger datos sensibles.
    

👉 Este es solo el arranque: aquí defines cómo se llama la actividad y cómo se comporta ante errores o tiempos de espera.  
El siguiente paso será ir a la pestaña **Source** para configurar desde dónde vas a copiar los datos (tu SQL Server en Linux a través del SHIR).


📋 **3) Pestaña Source**

Ahora vamos a la **pestaña Source** de la actividad _Copy data_, donde definimos **de dónde vienen los datos**.  Los pasos para configurarlo con tu SQL Server en Linux (vía SHIR) son:

---
1. En **Source dataset**, haz clic en **+ New**.
    
2. Se abre la ventana de selección de tipo de dataset → busca **SQL Server**
    
3. Haz clic en **Continue**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img54.png)
    
4. Ahora configura tu dataset:

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img56.png)

---

Muy bien 🙌, ahora en la ventana **Set properties** al crear el **dataset de origen (Source dataset)**.

Aquí defines cómo ADF se va a conectar a tu SQL Server. Los pasos son:

---

🔹 Configuración del Source dataset

1. **Name**
    
    - Escribe un nombre descriptivo, en este caso **address**, que corresponde a la tabla que vas a copiar.
        
    - Este nombre es el identificador del dataset dentro de ADF (no necesariamente el mismo nombre de la tabla, pero conviene mantenerlo claro).
        
2. **Linked service**
    
    - Ahora selecciona el **Linked Service** que conecta ADF con tu SQL Server on-prem.
        
    - Haz clic en **+ New** y:
        
        - Elige tipo **SQL Server**.

3. **Ahora completa los siguientes campos**

- Nombre: `onpremsqlserver`
    
- (Opcional) Descripción: `Conexión SQL local desde contenedor`
    
- Integration Runtime: `integrationRuntime1` (tu SHIR)
    
- Versión: `2.0 (Recommended)`
    
- Server name: `localhost`
    
- Database name: `AdventureWorksLT2022`
    
- Authentication type: `SQL authentication`
    
- User name: `sa`

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img57.png)


4. 🔐 **Configurar Azure Key Vault en ADF**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img58.png)

- Haz clic en **"Azure Key Vault"** (en vez de escribir la contraseña manual).
    
- En **AKV linked service**, selecciona o crea un nuevo linked service hacia tu Key Vault.
 
![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img59.png)

- **Selecciona** `Azure Key Vault` como tipo de Linked Service.
    
- **Asigna** el nombre, por ejemplo: `AzureKeyVault1`.
    
- **Elige** el método **"From Azure subscription"**.
    
- **Selecciona** la **suscripción de Azure** correspondiente.
    
- **Escoge** el **Key Vault**, por ejemplo: `key-vault-suffix`.
    
- **Usa** la opción de **autenticación con Managed Identity**.
    
- **Prueba** la conexión para verificar que funcione correctamente.
    
- **Haz clic** en **Create** para guardar el Linked Service.

---

Ahora verás este error **Loading Fail**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img60.png)

Si haces clic en **More** verás los detalles del error

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img61.png)

---

🔧¿Qué significa este error?

Este error indica que Azure Data Factory **no pudo cargar o recuperar los secretos almacenados en el Azure Key Vault** especificado. Por eso no aparecen las opciones disponibles en el desplegable de **Secret name**.

▶️Posibles causas comunes:

1. **Permisos insuficientes**
    
    - La identidad utilizada por Azure Data Factory (Managed Identity o Service Principal) **no tiene permisos de acceso (get/list) configurados en el Key Vault**.
        
2. **Nombre del Azure Key Vault incorrecto**
    
    - El Linked Service de Azure Key Vault seleccionado (`AzureKeyVault1`) puede estar mal configurado o apuntar a un Key Vault que no existe o no es accesible.
        
3. **Problemas de red o configuración**
    
    - Restricciones de red o políticas de firewall que impiden a ADF acceder al Key Vault.
        
4. **Error temporal o conexión interrumpida**
    
    - Puede ser un problema temporal en la conexión o servicio.

🧰¿Cómo resolver el problema?

Debes asignar los permisos necesarios para el usuario o la identidad gestionada de Azure Data Factory a través de **IAM (Identity and Access Management)** en lugar de usar Access policies:

1. Ve a la opción **Access control (IAM)** del Key Vault.
    
2. Haz clic en **Add role assignment**.

¿Qué significa esto?

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img62.png)

- El usuario tiene permisos administrativos completos sobre el Key Vault.
    
- Sin embargo, para que Azure Data Factory (o cualquier otro servicio) pueda acceder a los secretos almacenados en el Key Vault, **la identidad gestionada** del servicio también debe tener los permisos adecuados asignados aquí.
    
- El error "Forbidden" indica que la identidad que intenta acceder a los secretos no tiene los permisos necesarios.

¿Qué revisar o hacer?

Verifica cuál es la **identidad gestionada** o el principal de servicio que usa Azure Data Factory para acceder al Key Vault.

- Entra al portal de Azure.
    
- Navega a tu **Azure Data Factory**.
    
- En el menú de la izquierda, busca y selecciona **Managed identities** o **Identidad gestionada**.
    
- Aquí verás si tienes una **identidad gestionada habilitada** (normalmente un principal de servicio que Azure crea para tu Data Factory).
    
- Copia el nombre o el ID de esta identidad.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img63.png)

¡Perfecto! Tu instancia de **Azure Data Factory** (`z2h-adf-suffix`) **tiene habilitada su identidad gestionada del sistema** (`System assigned`), y su **Object (principal) ID** es:

```
d17a4c4c-236a-474a-a542-c5803b115a03
```

✅ ¿Qué significa esto?

Que Azure Data Factory ya tiene una identidad propia que puedes usar para asignarle permisos a otros recursos, como el **Azure Key Vault**. Y esto es justo lo que debes hacer para resolver el error de acceso a secretos que vimos antes.

---

✅ ¿Qué sigue? (resumen de acciones)

Ahora, ve al **Key Vault** y haz lo siguiente:

1. Entra al **Key Vault**.
    
2. Ve a la opción **Access control (IAM)**.
    
3. Haz clic en **+ Add > Add role assignment**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img64.png)
    
4. Configura:
    
    - **Rol**: `Key Vault Secrets User` (o cualquier rol que permita leer secretos).
        
    - **Asignado a**: Busca por el ID o nombre del principal `Azure Data Factory` (puedes usar el nombre del recurso o pegar ese ID) luego haz clic en Next

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img65.png)

Estás a punto de completar la **asignación de rol** para que tu Azure Data Factory (`z2h-adf-suffix`) pueda acceder a los secretos del **Key Vault**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img66.png)

🔍 Observaciones importantes:

- Selecciona correctamente el tipo: `Managed identity`.
	
- Selecciona Data Factory (V2) 
    
- En **search by name** Elige como miembro: `z2h-adf-suffix` (tu instancia de Data Factory) y haz clic en el objeto luego clic en **Select**
	
- Luego continúa con:
    
    - 🔹 `Next > Review + assign`
        
    - 🔹 `Review + assign` nuevamente para confirmar
        

---

🎯 Resultado esperado

Una vez que se asigne correctamente el rol, la **identidad gestionada de Azure Data Factory** tendrá permiso para **acceder a los secretos** del Key Vault, y el error de “Forbidden” debería desaparecer. Ahora podrás seleccionar el **password**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img67.png)

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img68.png)


¡Perfecto! Ahora que ya se ve el nombre del **secreto (`password`)** en la interfaz de configuración de la conexión con **Azure Key Vault**, significa que el problema de permisos ya se resolvió. ✅

---

✅ ¿Qué confirma esto?

- El secreto **ya carga correctamente** desde el Key Vault.
    
- La identidad gestionada de **Azure Data Factory (`z2h-adf-suffix`)** ya tiene permisos suficientes, probablemente porque le asignaste correctamente el rol `Key Vault Secrets User`.
    
- Ya puedes usar el secreto (por ejemplo, una contraseña) en tu conexión segura.


Antes de finalizar la configuración de la conexión en ADF, puedes probarla desde el Runtime hacia la base de datos. Simplemente completa los campos de conexión requeridos y haz clic en **(Test)** para confirmar que la conexión desde el Runtime esté funcionando correctamente.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img70.png)

---

**Próximos Pasos**

Para que tu ADF pueda conectarse al SQL Server desde el Self-hosted IR, sigue estos pasos:

1. En el **Linked Service** dentro del portal ADF:
        
    - Pon **Encrypt = Optional** (en vez de “Mandatory”).
        
    - Marca la casilla **Trust server certificate**.
        
    - Guarda la configuración aunque el botón “Test connection” muestre error; esa prueba no siempre refleja la conexión real por parte del SHIR.

2. Haz clic en **“Test connection”** para validar que todo funcione correctamente.
    
3. Si todo está bien, haz clic en **"Save"** para guardar la configuración.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img69.png)



---

✅ Selección de tabla 

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img71.png)

Ahora podemos ver la configuración de una **Table name**  y ya puedes:

- Ver el **linked service `onpremsqlserver`** correctamente seleccionado.
    
- Confirmar que se conecta mediante el **Integration Runtime `integrationRuntime1`**, que está **activo y saludable** (ícono verde ✅).
    
- Ver que **la lista de tablas se carga automáticamente** desde la base de datos `AdventureWorksLT2022`, lo cual solo es posible si:
    
    - El **SQL Server está accesible** desde el IR.
        
    - El **usuario (`sa`) tiene permisos para leer metadatos**.
        
    - Los **drivers y configuración del IR están correctos**.

Ahora solo debes seleccionar la tabla **SalesLT.Address** desde el menú de opciones y hacer clic en **OK** para guardar la conexión.




Ahora podemos ver parte de la configuración de la **actividad de origen (`Source`)**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img72.png)



Ahora haz clic en **`Preview data`**, para validar que:

- El Integration Runtime puede conectarse a SQL Server.
    
- El dataset `Address` está correctamente configurado.
    
- Puedes ver los datos **antes de ejecutar la actividad**, lo cual ayuda mucho en tareas de debugging y validación previa.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img73.png)


📋 **4) Pestaña Sink**

Ahora vamos a crear una sincronización con **Azure Data Lake**. Recordemos que se crearán las tres capas: **bronce**, **plata** y **oro**. En este caso, comenzaremos con la configuración de la **capa bronce**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img74.png)

Perfecto. En la pestaña **Sink** vamos a prepararnos para crear un dataset de destino (sink dataset) que apunte a **Azure Data Lake Storage (ADLS)** para almacenar la **capa bronce** de la arquitectura de datos.

---

🧱 ¿Qué es la Capa Bronce?

En un _Data Lakehouse_, la **capa bronce** almacena los datos **en crudo (raw)**, tal como vienen de la fuente (en este caso SQL Server).  
No se transforman, solo se copian. Su objetivo es conservar fidelidad y trazabilidad.

---

🛠️ Pasos para configurar el Sink hacia Azure Data Lake – Capa Bronce

1. 🔘 Haz clic en **“+ New”** 

Esto abre el asistente para crear un **dataset nuevo de destino**.

---

3. 📥 Crea un nuevo servicio **Azure Data Lake Storage Gen2** para agregarlo al grupo de recursos

🪣 **Pasos para crear Azure Data Lake Storage Gen2 en el portal de Azure**

✅**Inicia sesión** en el portal de Azure y haz clic en **Create a Resource**
    
✅En la barra de búsqueda, escribe y selecciona **"Storage accounts"**.
    
✅Haz clic en **“+ Create”** o **“+ Nueva cuenta de almacenamiento”**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img77.png)
    
✅Configura lo básico:**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img78.png)
    
- **Subscription**: Selecciona la suscripción deseada.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img79.png)
	
- **Resource group**: Usa el existente.
	
- **Storage account name**: `z2hdatalakesuffix` No olvides cambiar el suffix por un mombre único (solo minúsculas y sin caracteres especiales).
	
- **Region**: Selecciona una región cercana a tus servicios.
	
- **Performance**: Generalmente **Standard**.
	
- **Redundancy**: Por defecto **Locally-redundant storage (LRS)**.
	
✅**Habilita Data Lake Gen2:**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img80.png)
    
- Ve a la pestaña **"Advanced"**.
	
- Activa la opción **“Enable hierarchical namespace”** → Esto convierte tu cuenta en un **Data Lake Gen2**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img81.png)

✅Haz clic en **“Review + create”** y luego en **“Create”**.

Una vez completado este paso podemos pasar el siguiente paso

7. 📂 Selecciona el tipo de dataset

En el asistente:

- Selecciona **Azure Data Lake Storage Gen2** y clic en **Continue**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img75.png)
    
- Luego selecciona un **formato de archivo**:
    
    - **DelimitedText** (CSV) ✅ ideal para la capa bronce
        
    - **JSON** o **Parquet** (si ya piensas en eficiencia o analytics, pero usualmente van en plata/oro)

En nuestro caso seleccionamos **Parquet** como formato para el dataset de salida (_sink_) en tu pipeline lo cual es una excelente decisión, incluso para la **capa bronce**, por varias razones técnicas.

✅ ¿Por qué usar **Parquet** en la capa bronce?

Aunque CSV es más simple, **Parquet** ofrece beneficios clave incluso en la capa bronce:

|Ventaja|Descripción|
|---|---|
|**Columnar**|Permite leer solo columnas necesarias → eficiencia en consultas.|
|**Compresión**|Archivos más pequeños → menor almacenamiento.|
|**Tipado**|Mantiene tipos de datos → evita problemas con fechas, números, etc.|
|**Compatibilidad**|Compatible con Spark, Synapse, Databricks, Power BI, etc.|

🔹 Próximo paso

Haz clic en **"Continue"** para seguir con la configuración del dataset Parquet.

---

3. 🔗 Crea el **Linked Service**

El linked service será la conexión a tu Azure Data Lake.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img76.png)

- Créalo:
    
    - Elige la cuenta de almacenamiento **z2hdatalakesuffix**
        
    - Usa **key**.
        
    - Prueba la conexión ✅.
	    
    - Create

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img82.png)

---

Ahora **crearemos el contenedor `bronze` en Azure Data Lake Storage Gen2**, paso necesario para configurar un **sink dataset en Azure Data Factory (ADF)** y guardar allí los archivos de la capa bronce.

---

✅ Pasos para crear el contenedor `bronze`:

1. **Ir a la cuenta de almacenamiento** en el portal de Azure (por ejemplo, `z2hcdatalakesuffix`).
    
2. En el menú lateral izquierdo, navegar a:
    
    - **Data storage** > **Containers**
        
3. Hacer clic en **➕ Add container** en la parte superior.
    
4. En el panel derecho:
    
    - **Nombre**: Escribir `bronze` (este será el contenedor lógico donde se almacenarán los archivos).
        
    - **Access level**: Dejar en `Private` (por defecto, ya que no necesitas acceso anónimo para ADF).
        
5. Hacer clic en el botón **Create**.
    

---

📌 ¿Por qué es importante este paso?

El contenedor `bronze` es el lugar donde Azure Data Factory escribirá los archivos **Parquet** provenientes de tu origen de datos (por ejemplo, SQL Server). Sin este contenedor, el flujo de datos no tendrá dónde guardar la información extraída.

Este contenedor es parte de tu arquitectura de datos en capas:

- **Bronce**: Datos crudos (sin transformar).
    
- **Plata**: Datos limpios y enriquecidos.
    
- **Oro**: Datos listos para análisis o consumo.
    

---


![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img84.png)

---

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img86.png)

---


Ahora se debe **definir el _sink dataset_**, específicamente para **escribir datos en formato Parquet dentro de Azure Data Lake Storage Gen2**, en la **capa bronce** de tu arquitectura.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img83.png)

---

📝 ¿Qué estás configurando aquí?

Estás creando un dataset de salida que:

- Tendrá el nombre `address_parquet`.
    
- Utilizará el **servicio vinculado (linked service)** `AzureDataLakeStorage1` (es decir, tu cuenta de Data Lake Gen2).
    
- Guardará archivos en formato **Parquet**.
    
- Estás especificando la **ruta destino** donde se escribirá el archivo.
    

---

🧭 Detalle de los campos:

|Campo|Descripción|
|---|---|
|**Name**|Nombre interno del dataset (`address_parquet`).|
|**Linked service**|Conexión hacia tu cuenta de Azure Data Lake Storage Gen2.|
|**File path**|Ruta lógica dentro del Data Lake: puedes definir el contenedor (File system), subcarpetas (Directory) y nombre del archivo (`File name`). Ejemplo: `bronze/saleslt/address.parquet`.|
|**Import schema**|En este caso está marcado como `None`, ya que el esquema vendrá desde el _source_ (la base de datos SQL).|

---

✅ ¿Qué sigue?

1. Completar la ruta de destino (`File path`) en tu Data Lake. Solo debes hacer clic en el icono del folder y seleccionar **From root**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img85.png)
    
2. Selecciona **bronze**
![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img87.png)

---

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img88.png)

 3. Y haz clic en **OK** para confirmar la creación del dataset.


---

💡 Con este paso terminamos de configurar la **sincronización a la capa bronce**, donde los datos se almacenan **tal como vienen del origen**, sin transformaciones.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img89.png)

---

### 🔹 4. Debug

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img90.png)

Es hora de ejecutar un **Debug** en un **pipeline** que contiene una actividad `Copy data` 

---

🔍 ¿Qué es el botón **Debug**?

El botón **Debug** te permite **ejecutar el pipeline inmediatamente** para probarlo **sin necesidad de publicarlo ni de configurar un trigger**. Es ideal para validar que:

- La configuración del origen y destino (sink) está correcta.
    
- Las conexiones funcionan.
    
- La transformación (si hubiera) se realiza como se espera.
    

---

✅ En este caso, ¿qué se va a probar?

Al hacer clic en `Debug`, ADF va a:

1. Conectarse al origen (SQL Server on-premises, vía Integration Runtime).
    
2. Leer la tabla `Address`.
    
3. Escribir los datos en formato **Parquet** en el contenedor **`bronze`** de tu **Azure Data Lake Storage Gen2**.
    

---

🧪 Resultado esperado del Debug

- Si todo está correcto: verás un **ícono verde con una paloma (✔️)** junto a la actividad.
    
- Si hay errores (conexión, formato, permisos, etc.): aparecerá una **equis roja (❌)** con detalles del error al hacer clic.
    

---

📌 Importante:

- Esta ejecución **no persiste cambios en producción** hasta que publiques.
    
- Es útil para desarrollar y corregir en tiempo real sin necesidad de esperar despliegues.
    

#### **Ejecución Debug**

Ahora, simplemente haz clic en 'Debug' para ejecutar el pipeline. Verás en la parte inferior el estado de ejecución del pipeline y su monitoreo.

---

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img91.png)





Después de terminar el debug verás un error que se muestra en la ejecución de **Debug** indicando claramente un problema con el entorno de ejecución **Self-hosted Integration Runtime (IR)**. El cual puedes verificar en el icono de **mensaje**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img92.png)

---

#### ❌ **Error: Java Runtime Environment (JRE) no encontrado**

**Mensaje completo del error:**

> `Java Runtime Environment cannot be found on the Self-hosted Integration Runtime machine. It is required for parsing or writing to Parquet/ORC files.`

---

📌 **¿Qué significa esto?**

* Estás intentando escribir datos en **formato Parquet**, lo cual requiere que **Java esté instalado** en la máquina que ejecuta el **Self-hosted Integration Runtime (IR)**.

* El error indica que el archivo **`jvm.dll`** no se encuentra, lo que confirma que **no hay una instalación válida de Java** o que no está configurada correctamente.

---

✅ **Solución paso a paso**

1. **Instalar Java (JDK o JRE) en la máquina del Self-hosted IR:**

* Descarga e instala **Java (JDK 8 o superior)** desde el siguiente enlace:
 [Java Downloads](https://download.oracle.com/java/17/archive/jdk-17.0.12_windows-x64_bin.exe)

2. **Configura las variables de entorno en esa máquina:**

* Crea la variable `JAVA_HOME` apuntando a la carpeta raíz del JDK instalado.
* Agrega `JAVA_HOME\bin` al **`PATH`** del sistema.

---

🔧 **¿Qué son las variables de entorno?**

Las **variables de entorno** son configuraciones del sistema operativo que permiten a los programas (como Azure Data Factory) encontrar componentes clave, como **Java**, y ejecutar tareas necesarias como la serialización de datos en formatos como **Parquet** y **ORC**.

---

🛠️ **Paso a paso para configurar `JAVA_HOME` y `PATH`**

> ⚠️ **Realiza estos pasos en la máquina donde está instalado el Self-hosted Integration Runtime.**

---

✅ 1. **Agrega `%JAVA_HOME%\bin` al `PATH`**

1. Abre **Variables de entorno**.

2. Busca la variable llamada `Path` en **"Variables del sistema"** y haz clic en **Editar...**.

3. En la ventana de edición, haz clic en **"Nuevo"** y agrega:

```
%JAVA_HOME%\bin
```

4. Acepta todos los cambios (OK, OK, OK).

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img93.png)

✅ 2. **Configura o verifica la variable `JAVA_HOME`**

1. En la sección de **Variables del sistema**, asegúrate de que exista una variable llamada `JAVA_HOME`, con un valor como:
2. 
![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img94.png)

```
C:\Program Files\Java\jdk-XX
```

(Sustituye `jdk-XX` con la carpeta real de tu instalación de Java).

2. Abre la ventana de **CMD** y ejecuta el siguiente comando para verificar la instalación de Java:

```
java -version
```

Si todo está bien, deberías ver la versión de **Java** instalada.

3. **Reinicia el servicio del Self-hosted Integration Runtime** para que detecte la nueva configuración.



---

🎯 **¿Por qué es importante?**

ADF necesita ejecutar procesos **Java** internamente para trabajar con archivos **Parquet u ORC**, y sin estas variables correctamente configuradas, **no podrá encontrar ni usar Java**, lo que resulta en el error que viste.

---

📝 **Nota**

Este error es **específico para formatos como Parquet y ORC**, ya que **Azure Data Factory** necesita Java para manejar la **serialización/deserialización** de estos formatos.

#### 🎉 **La ejecución del debug fue exitosa!**

Si ejecutas nuevamente el 'Debug', ahora verás que la ejecución ha sido **exitosa**. Ahora vamos a revisar qué sucedió.

---

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img95.png)

🔍 **Detalles de la Ejecución**

1. **Actividades en ADF:**

   * 🚀 Como puedes ver, el **pipeline** en **Azure Data Factory** tiene la actividad **`Copy data`** y su correspondiente actividad **`Copy address table`**.
   * ✅ Ambas actividades tienen el **ícono verde con la paloma (✔️)**, lo que indica que **se ejecutaron correctamente**.

---

2. **Ejecución del Debug:**

   * 📊 En el panel inferior, el **estado del pipeline** está marcado como **"Succeeded"** (Ejecutado con éxito), lo que significa que **la prueba de ejecución fue exitosa**.
   * 💬 El mensaje **"Pipeline status: Succeeded"** confirma que **no hubo problemas** y que los datos fueron copiados sin errores.

---

3. **Duración y Runtime:**

   * ⏱️ La duración de la ejecución fue de **19 segundos**, lo que indica que el proceso de copiar datos fue rápido y eficiente.
   * 🔄 El runtime **`integrationRuntime1`** muestra que se utilizó el **Integration Runtime autoalojado** para ejecutar el pipeline.

---

🚀 **¿Qué significa esto?**

1. **Verificación exitosa del debug:**

   * 🎯 Al hacer clic en **Debug**, el proceso se ejecutó **sin problemas** y los datos fueron copiados correctamente de un origen (base de datos `SalesLT.Address`) a un destino (Azure Data Lake).

2. **Sin errores:**

   * ✅ Al ver el ícono verde con la paloma, puedes estar seguro de que **no hubo errores** relacionados con conexiones, configuración o permisos.

3. **El proceso de debug no afecta la producción:**

   * 🛡️ El **debug** es útil para verificar el pipeline **sin realizar cambios permanentes**. Este proceso no afecta los datos ni los entornos de producción hasta que se decida **publicar** los cambios.

---

🏞️ **Validación del Contenedor "Bronze" en Azure Data Lake Storage**

Ahora, vamos a validar si el archivo **`SalesLT.Address.parquet`** fue creado correctamente en el contenedor **"bronze"** de **Azure Data Lake Storage**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img96.png)

 📁 **Detalles del archivo creado:**

1. **Archivo creado:**

   * 📂 El archivo **`SalesLT.Address.parquet`** está presente en el contenedor **bronze**.
   * 📥 Este archivo es el resultado de la actividad **Copy Data** en el pipeline, que copió los datos de la tabla `SalesLT.Address` en formato **Parquet**.

2. **Detalles del archivo:**

   * **Tamaño:** El archivo tiene un tamaño de **35.08 KiB**.
   * 🔥 El archivo está en el **tier "Hot"**, lo que significa que es fácilmente accesible para **operaciones frecuentes** de lectura/escritura.

---

✅ **Confirmación del proceso exitoso:**

* El archivo **Parquet** fue generado correctamente y almacenado en el contenedor adecuado en **Azure Data Lake Storage** (en la zona **"bronze"**), tal como se esperaba.

---

📝 **¿Qué significa esto?**

1. **Validación de éxito:**

   * El archivo **`SalesLT.Address.parquet`** confirma que el proceso de **transformación y carga de datos** en formato Parquet se completó **correctamente**.

2. **Confirmación de que el pipeline funcionó bien:**

   * 🎉 El **debug en ADF** fue exitoso, y ahora tienes el archivo Parquet almacenado y listo para su uso en **Azure Data Lake**.

---

🎯 **Resumen:**

* El **debug fue exitoso** 🏆, lo que significa que los datos fueron copiados sin errores.
* El archivo **`SalesLT.Address.parquet`** está presente en el contenedor **bronze** y listo para ser utilizado.
* ✅ Todo está funcionando perfectamente.


#### 🚀 **Paso Final: Publicar Cambios en Azure Data Factory**

🎯 **Publish**

Una vez que hayas terminado de crear y configurar tu pipeline, datasets y servicios vinculados, el siguiente paso clave es **publicar** los cambios. Esto es esencial para que los recursos estén disponibles en el entorno de producción.

🟡 **Haz clic en:**
🔘 **Publish all (5)** – Esto indica que hay 5 cambios pendientes de publicar.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img97.png)

---

✅ ¿Qué sucede al publicar?

Al hacer clic en **Publish all**, estás:

* 💾 **Guardando permanentemente** los cambios en el entorno de Azure Data Factory.
* ⏱️ **Habilitando la ejecución programada o manual** de tus pipelines.
* 🧩 **Activando** los nuevos datasets y linked services configurados.

---

👁️‍🗨️ **Pantalla de Proceso de Publicación**

Haz clic en el botón de **Publishing** para ver el resumen de los cambios:

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img98.png)

Aquí verás todos los elementos pendientes por publicar:

📋 **Cambios pendientes:**

| Tipo de elemento  | Nombre             | Estado     |
| ----------------- | ------------------ | ---------- |
| 📦 Pipeline       | `pipeline1`        | 🆕 Nuevo   |
| 📄 Dataset        | `address`          | 🆕 Nuevo   |
| 📄 Dataset        | `address_parquet`  | 🆕 Nuevo   |
| 🔗 Linked service | `onpremssqlserver` | ✏️ Editado |
| 🔐 Linked service | `AzureKeyVault1`   | 🆕 Nuevo   |

---

✅ Acción Final

🔘 **Haz clic en:**
🔵 **Publish**

Este botón aplicará todos los cambios al entorno **en vivo**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img99.png)

---

🎉 Resultado

✨ ¡Listo! Ahora tus cambios han sido publicados y están activos.

🟢 Tus pipelines, datasets y servicios vinculados ya están listos para:

* 🏁 Ejecutarse manualmente
* ⏰ Programarse automáticamente
* 📦 Integrarse en flujos más complejos

---

📝 NOTA IMPORTANTE

> Siempre **publica** los cambios en ADF luego de editar o crear nuevos recursos. Si no los publicas, seguirán solo en tu entorno de desarrollo y no estarán disponibles para su ejecución.


---


#### 🧩 ¿Qué sigue?

🔧 En los próximos pasos, comenzaremos a construir el pipeline paso a paso:

* 📁 **Creación del pipeline**
* 🔗 **Configuración de orígenes y destinos**
* 📤 **Copiado de tablas completas**
* 🔁 **Automatización del proceso para múltiples tablas**
