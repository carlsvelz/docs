

# Compute

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img150.png)

---

## ⚙️ ¿Qué es la sección **Compute**?

Es donde gestionas los **recursos de cómputo** que necesitas para ejecutar tus notebooks, tareas automatizadas (jobs), consultas SQL y modelos de machine learning. En pocas palabras, aquí creas y administras los clústeres que te dan la potencia para trabajar con datos.

---

### 🧩 Pestañas dentro de la sección Compute:

1. **All-purpose compute**
    
    - Aquí creas **clústeres interactivos**, ideales para notebooks colaborativos y exploración de datos en tiempo real.
        
    - Se pueden compartir entre usuarios y se mantienen activos mientras los necesitas.
        
2. **Job compute**
    
    - Clústeres dedicados a la ejecución de **jobs automáticos o programados**.
        
    - Son efímeros: se inician cuando comienza un trabajo y se apagan al terminar, optimizando costos.
        
3. **SQL Warehouses**
    
    - Infraestructura especializada para ejecutar **consultas SQL** con alto rendimiento.
        
    - Son ideales para dashboards o herramientas de BI conectadas a Databricks.
        
4. **Vector Search**
    
    - Permite crear índices para hacer **búsquedas vectoriales** (por ejemplo, para IA generativa o motores de recomendación).
        
    - Aparece si tienes activada esta funcionalidad (en vista previa o edición específica).
        
5. **Pools**
    
    - Agrupan instancias previamente activadas para reducir el tiempo de inicio de clústeres.
        
    - Útil cuando necesitas ejecutar jobs frecuentemente y no quieres esperar a que un clúster arranque desde cero.
        
6. **Policies**
    
    - Aquí defines políticas para controlar cómo se crean los clústeres.
        
    - Puedes limitar el tipo de máquinas, tamaño, duración, etc., según necesidades o presupuestos del equipo.
        
7. **Apps**
    
    - Muestra aplicaciones o integraciones desplegadas dentro del entorno de Databricks.
        
    - No siempre está activo, depende de tu configuración.
        
8. **Lakebase Postgres**
    
    - Una nueva integración con PostgreSQL para facilitar consultas y sincronización de datos.
        
    - Aún puede estar en versión preview o restringida a ciertas ediciones.
        

---

### 🛠️ Otras funcionalidades destacadas en la pantalla:

- **Create with Personal Compute**: Te permite iniciar un clúster personal rápidamente con una configuración predeterminada.
    
- **Create compute**: Abre el asistente para configurar tu propio clúster (tipo de instancia, número de nodos, políticas, etc.).
    
- **Filtros y columnas**: Puedes buscar, ordenar o filtrar clústeres por nombre, creador, política, uso de recursos, etc.
    

---

### ✅ KEYS:

La sección **Compute** es el corazón operativo de Databricks. Aquí defines con cuánta potencia vas a trabajar, cómo se usa esa potencia (interactiva o automática), y cuánto control tienes sobre su comportamiento. Organizar bien esta parte es clave para balancear rendimiento y costos.



---

## **Create Compute**

Ahora nos posicionamos en la pestaña **All-purpose compute** dentro de la sección **Compute** de Databricks. Aquí es donde puedes crear clústeres interactivos para trabajar con notebooks, probar código, explorar datos y colaborar con otros usuarios.

---

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img151.png)

### 🟨 Menú desplegable de políticas de cómputo

📍**Haz clic:** Parte superior derecha, junto al botón azul "Create compute"

Este menú desplegable permite seleccionar **la política con la cual crear tu recurso de cómputo**, y es clave para definir cómo se comportará ese clúster. Las opciones que se muestran son:

- **Create with Personal Compute**  
    → Crea un clúster solo para ti. Nadie más podrá conectarse ni compartirlo. Ideal para trabajo individual, pruebas o desarrollo personal.
    
- **Create with Power User Compute**  
    → Pensado para usuarios avanzados que requieren más control sobre la configuración. Suele permitir elegir más tipos de instancias o configuraciones personalizadas.
    
- **Create with Shared Compute**  
    → Crea un clúster que puede ser compartido entre varios usuarios. Útil para equipos que colaboran en los mismos notebooks o procesos.
    

> 🔍 Estas "políticas" no solo definen permisos, sino también límites de recursos, tipos de instancias permitidas, tiempo de vida del clúster, etc.

---

### 🟨 Botón **Create compute**

📍**Ubicación:** Parte central inferior

Este botón es el **acceso directo para iniciar un nuevo clúster** desde cero. Al hacer clic, se abre un asistente donde puedes definir:

- Nombre del clúster
    
- Tipo de máquina y número de nodos
    
- Versión del entorno (Databricks Runtime)
    
- Políticas aplicadas
    
- Configuraciones avanzadas como autoscaling, acceso a librerías, etc.
    

> 💡 Este botón aparece cuando no tienes ningún clúster activo, como en la imagen que indica “No compute”.

---

🔚 En resumen:

Puedes crear tu entorno de ejecución en Databricks usando distintas políticas de cómputo. Puedes elegir si el clúster será personal, compartido o avanzado

Ahora haz clic en **“Create compute”** para empezar a trabajar. Este paso es esencial, ya que sin un clúster activo no puedes ejecutar notebooks ni consultas.

---

## Formulario para **crear un nuevo clúster**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img152.png)

Perfecto. Ahora que estas en el formulario para **crear un nuevo clúster** en Databricks y tu objetivo es crear **un clúster personal, pequeño y de prueba**, lo cual es ideal para desarrollo ligero o validación de código. Vamos a revisar **cada sección paso a paso**, explicando qué significa y cómo configurarlo eficientemente para nuestro proyecto.

---

### 🧾 **1. General**

|Campo|Descripción|Recomendación para pruebas|
|---|---|---|
|**Compute name**|Nombre del clúster. Databricks genera uno automático con fecha/hora.|Puedes dejar el nombre por defecto o poner algo como `"test-cluster"` para identificarlo fácilmente.|
|**Policy**|Determina las reglas aplicadas al clúster (recursos máximos, configuración permitida, etc.).|Si ves “Unrestricted”, significa que no hay restricciones. Ideal para pruebas rápidas.|

---

### ⚙️ **2. Performance**

Aquí defines la potencia y capacidades del clúster:

#### ✅ Machine learning (opcional)

- **Si se activa**, el clúster incluye bibliotecas de ML preinstaladas (MLflow, scikit-learn, etc.).
    
- **Para pruebas simples**, puedes **dejarlo desactivado**, a menos que estés haciendo algo de ML.
    

#### ⚡ Databricks runtime

- Es la versión del entorno base (Scala + Spark + librerías).
    
- En la imagen está seleccionado: **`16.4 LTS`**, lo cual es una versión estable y con soporte extendido.
    
- **Está bien dejarlo así.**
    

#### 🧠 Photon acceleration (opcional)

- Acelera el procesamiento en ciertas cargas de trabajo SQL y DataFrame.
    
- **Puedes dejarlo activado**, no genera sobrecostos si no se usa.
    

#### 🖥️ Worker type

- El tipo de máquina virtual que se usará como **nodo trabajador** del clúster.
    
- Por default se usa: **`Standard_D4ds_v5` (16 GB RAM, 4 cores)**
    
- **Para reducir costos**, puedes cambiar a algo más pequeño como `Standard_D2ds_v5` (8 GB, 2 cores), si está disponible.
    

#### 🔄 Autoscaling (habilitado)

- Escala automáticamente el número de nodos según la carga.
    
- **Ideal para pruebas**, ya que puedes empezar con 2 nodos y crecer hasta 8 solo si es necesario.
    

#### ⏱️ Terminate after X minutes of inactivity

- Si no se usa el clúster por el tiempo definido, se apaga automáticamente para **evitar costos innecesarios**.
    
- En la imagen está configurado en **120 minutos**, lo cual está bien para pruebas. Puedes bajarlo a 30 si quieres mayor control de costos.
    

#### 🧬 Advanced performance (plegable)

- Configuraciones más técnicas como tipo de disco, GPU, etc.
    
- Puedes **ignorarlo** para este caso de prueba.
    

---

### 🏷️ **3. Tags**

- Puedes asignar etiquetas clave/valor para organizar y rastrear costos o recursos.
    
- **No es obligatorio**. Puedes dejarlo vacío.
    

---

### 📦 **4. Advanced** (plegable)

- Aquí puedes configurar librerías, scripts de inicio, variables de entorno, etc.
    
- **No es necesario tocarlo para un clúster de pruebas.**
    

---

### 🧾 **5. Panel derecho – Resumen**

|Sección|Qué muestra|
|---|---|
|**Summary**|Rango estimado de recursos según la configuración (memoria y núcleos).|
|**Data access**|Muestra que el clúster podrá acceder a los datos gestionados por **Unity Catalog**.|
|**Price**|Estimación del costo por hora en DBUs (Databricks Units). Esto varía según tu contrato cloud.|

También puedes alternar entre **UI y JSON**, para ver la configuración en formato de código.

---

## **clúster personal**


![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img153.png)

Ahora crearemos un **clúster personal, de bajo costo, en modo single-node**, con una versión estable de Spark y aceleración activada. Además, estableceremos un apagado automático muy corto (10 min), lo que **minimiza riesgos de cobros innecesarios**. Es una configuración excelente para pruebas pequeñas o notebooks exploratorios.

## **Advanced**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img154.png)

En esta sección **Advanced** es para la configuración de un clúster en Databricks, donde puedes definir algunos parámetros adicionales para personalizar el comportamiento del clúster. Aquí te explico cada parte:

---

#### 1. **Access mode**

- **Auto**: El clúster se gestiona automáticamente, lo que significa que Databricks decide el nivel de acceso a los recursos según sea necesario (normalmente la opción predeterminada).
    
- **Manual**: Aquí puedes gestionar manualmente los recursos y configuraciones de acceso. Esto te da más control, pero es más adecuado para configuraciones avanzadas o específicas que no requieren la gestión automática.
    

> **Recomendación**: En un entorno de prueba y sin configuraciones específicas de seguridad, **deja el modo en "Auto"**. Esto es ideal para no complicarte con configuraciones adicionales que no son necesarias en este caso.

#### 2. **Logging**

- Esta opción permite configurar el registro de logs para el clúster.
    
- **Logging** permite rastrear eventos, errores y actividades dentro del clúster, y es útil si deseas realizar auditorías o monitorizar el uso.
    

> **Recomendación**: Para pruebas básicas, **no necesitas cambiar esta opción**, ya que no se requiere un registro detallado a menos que estés buscando depurar problemas complejos.

#### 3. **Init scripts**

- Los **init scripts** son secuencias de comandos que se ejecutan cuando el clúster se inicia, lo que te permite instalar librerías adicionales, configurar variables de entorno o realizar otras personalizaciones.
    
- Esta opción te deja añadir scripts personalizados para modificar tu entorno antes de usarlo.
    

> **Recomendación**: Para pruebas sencillas, **no es necesario utilizar init scripts** a menos que estés buscando preconfigurar algo específico para tu proyecto.

#### 4. **JDBC/ODBC**

- Estas configuraciones permiten establecer conexiones mediante los protocolos JDBC o ODBC a otras bases de datos o sistemas externos.
    
- Se usa cuando deseas conectar Databricks con bases de datos externas como MySQL, PostgreSQL, o incluso otros entornos en la nube.
    

> **Recomendación**: **No es necesario para un clúster de prueba**, a menos que vayas a conectar con una base de datos externa de forma constante.

---

### 🔐 **Acceso y Seguridad**

En la parte inferior, ves un candado al lado de **Standard** en "Access mode". Esto indica que el acceso a esta configuración está restringido por políticas o permisos en tu cuenta. No podrás cambiarlo si no tienes los privilegios necesarios.

---

✅ **KEYS**

La sección **Advanced** te permite personalizar detalles adicionales del clúster, pero para pruebas sencillas, **puedes dejar estas opciones tal como están**. Configura el acceso en modo **Auto**, no es necesario habilitar logging o scripts de inicialización, y no necesitas conectar a bases de datos externas.

## ✅ ¿Qué hacer ahora?

Una vez revisado todo:

- Haz clic en **Create** para iniciar el clúster.
    
- Espera unos minutos hasta que cambie a estado `Running`.
    
- ¡Listo! Ya puedes abrir un notebook y empezar a probar código.
    

---

