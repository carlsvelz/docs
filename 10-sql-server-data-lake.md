

# SQL Server al Data Lake

## 🚀 **Pipeline: De SQL Server al Data Lake**

Ahora que ✅ hemos creado exitosamente nuestro **runtime** y probado las conexiones entre **Azure Data Factory (ADF)** y **SQL Server local**, además de haber ejecutado una prueba con `Debug` para asegurarnos de que todo esté funcionando correctamente 🧪, es momento de dar el siguiente gran paso:

➡️ **Crear el pipeline que moverá toda nuestra base de datos de SQL Server al Data Lake.**

### 🔹 **Limpieza Inicial: Eliminando el archivo de prueba**



🧹 Antes de continuar, vamos a eliminar el archivo `SalesLT.Address.parquet` que usamos en la prueba anterior. Este paso es importante para mantener nuestro entorno limpio y evitar confusiones al validar los resultados finales del pipeline completo.

#### ✅ Pasos para eliminar el archivo `.parquet` de prueba:

1. 🖱️ Dirígete al contenedor del Data Lake donde se encuentra el archivo.
2. 🔍 Busca el archivo: `SalesLT.Address.parquet`
3. 🗑️ Selecciona el archivo y haz clic en **Delete**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img101.png)

4. 📤 Se abrirá una ventana emergente solicitando confirmación para eliminar el archivo.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img102.png)

5. ✅ Confirma la acción.

🎉 ¡Listo! El archivo fue eliminado exitosamente. Ahora estás listo para construir tu pipeline.

---

#### 📝 **Nota**

> Siempre que trabajes con pruebas previas, asegúrate de limpiar los resultados antes de pasar a procesos productivos. Esto evita datos duplicados, errores y confusiones durante la validación final.

---

## 📝 **Copiar Todas las Tablas al Data Lake** 

Ahora sí, ¡es momento de la verdad! Vamos a crear un pipeline que copie **todas las tablas** desde **SQL Server** hasta el **Data Lake** utilizando **Azure Data Factory**. 🌐💡

---

🛠️ **Pasos para Crear el Pipeline** 💻

### 1️⃣ **Crear un Nuevo Pipeline**:

* Dirígete a **Factory Resources** en el panel de Azure Data Factory.
* Haz clic en **Pipelines**, luego selecciona **Actons** y finalmente en **New Pipeline** 🆕.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img103.png)

### 2️⃣ **Añadir la Actividad Lookup**:

* En **Activities**, ve a **General** y selecciona **Lookup**.
* Arrastra el **Lookup** al panel de trabajo.
* En la sección de **Properties**, cambia el nombre del pipeline a **copy\_all\_tables** 📝.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img104.png)

---

🔍 **¿Qué Hace la Actividad Lookup?** 🤔

La actividad **Lookup** en **Azure Data Factory** se utiliza para recuperar o consultar datos desde una **fuente externa** (como una base de datos SQL o archivos). Esto nos permite obtener información útil que podemos usar más adelante en el pipeline. 🧑‍💻

**¿Cómo Ayuda a Copiar Todas las Tablas al Data Lake?** 🗂️

Para el pipeline **`copy_all_tables`**, el **Lookup** realizará lo siguiente:

1. **Consultar las Tablas Disponibles**:

   * El **Lookup** se conecta a la base de datos **AdventureWorksLT2022** y ejecuta una consulta para obtener el listado de **todas las tablas** que queremos copiar al **Data Lake**.

2. **Obtener los Nombres de las Tablas**:

   * La consulta devuelve una lista de **nombres de tablas**, lo que es crucial para procesarlas de forma dinámica, sin necesidad de definir cada tabla manualmente.

3. **Iterar sobre las Tablas**:

   * Con los **nombres de las tablas**, podemos construir un flujo **dinámico** que copie cada tabla a su archivo correspondiente en el **Azure Data Lake**.

4. **Automatización del Proceso**:

   * El uso de **Lookup** permite **automatizar** el proceso de copiar todas las tablas sin necesidad de escribir un proceso para cada tabla manualmente. El **Lookup** solo necesita configurarse para devolver los nombres de las tablas, y luego estas se procesan en un ciclo.

---

📌 **Notas Importantes** 

* **Automatización**: Usar **Lookup** nos ayuda a evitar la necesidad de escribir código para cada tabla, lo que hace el proceso mucho más eficiente y fácil de mantener.
* **Dinámico**: Si se agregan nuevas tablas a la base de datos, el pipeline las copiará automáticamente sin intervención manual.

---

✅ **Pasos para Configurar el Pipeline**

| **Paso**                         | **Acción Realizada** 📝                                                                 | **¿Completado?** ✅ |
| -------------------------------- | --------------------------------------------------------------------------------------- | ------------------ |
| Crear Pipeline Nuevo             | Ir a **Factory Resources > Pipelines** y crear un nuevo pipeline.                       | ✅                  |
| Configurar la Actividad Lookup   | Arrastrar **Lookup** al panel y cambiar el nombre del pipeline a **copy\_all\_tables**. | ✅                  |
| Establecer Conexión a SQL Server | Conectar el **Lookup** a la base de datos **AdventureWorksLT2022**.                     | ❌                  |
| Consultar Tablas Disponibles     | Ejecutar la consulta para obtener los nombres de las tablas.                            | ❌                  |
| Iterar sobre las Tablas          | Usar los nombres de las tablas para copiarlas al Data Lake.                             | ❌                  |

---


### 3️⃣**Configurar el Lookup**:

- **Conexión de base de datos**: 
	- **General** cambia el nombre a **look for all tables** 

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img106.png)
- **Settings** Configura un nuevo **source dataset** busca `sql` y selecciona **SQL Server** 

	![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img105.png)

	- el Lookup para que se conecte a la base de datos **AdventureWorksLT2022** en **SQL Server**.
	- En **Set Properties**, agrega el nombre `SqlServerTables`. En **Linked Service**, selecciona la conexión creada en los pasos anteriores (**onpremsqlserver**). En **Table name**, déjalo vacío, ya que no seleccionaremos una sola tabla, sino todas luego clic en `OK`

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img107.png)

- **Consulta**: Ahora, vamos a crear una consulta que debe obtener la lista de las tablas. Podrías utilizar una consulta SQL como la siguiente:

```sql
SELECT
    s.name AS SchemaName,
    t.name AS TableName
FROM sys.tables t
INNER JOIN sys.schemas s
    ON t.schema_id = s.schema_id
WHERE s.name = 'SalesLT';
```

- Esta query trae los nombres de las tablas y esquemas del esquema `SalesLT`.

- Ahora, probemos esta query en **DBeaver**: crea un nuevo script y ejecuta el código anterior.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img108.png)

- Esto seleccionará los nombres de todas las tablas en la base de datos.

⚙️ **Configuración de la actividad Lookup**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img109.png)

1. **Selecciona la opción "Query"**:
    
    - En lugar de seleccionar una tabla completa, elige la opción **Query** para ejecutar una consulta SQL personalizada. Esto te permitirá obtener los datos específicos de la base de datos según la consulta que escribas.  
        🔽 **¿Por qué?**  
        Esto es útil para trabajar con datos dinámicos o personalizados que no se limitan a una sola tabla.
        
2. **Pega tu consulta SQL**:
    
    - Copia y pega la consulta SQL que deseas ejecutar.
        
3. **Desmarcar "First row only"**:
    
    - En las opciones de la actividad, desmarca la opción **First row only**.  
        🔍 **¿Qué hace esto?**  
        Si dejas esta opción seleccionada, ADF solo traerá la primera fila de resultados. Desmarcándola, aseguras que **todas las filas** que devuelva la consulta sean obtenidas. Esto es fundamental si tu consulta retorna múltiples filas de datos.
        
4. **Activar "Preview data"**:
    
    - Haz clic en el botón **Preview data** para **previsualizar los resultados** antes de ejecutar el pipeline completo.
    ![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img110.png)
        
    - Esto es crucial para verificar que la consulta esté funcionando correctamente y que los resultados sean los esperados.

**Debug:** Haz clic en el Botón **Debug** para **ejecutar el pipeline en modo prueba**, sin necesidad de publicarlo ni programarlo. Con esto puedes validar al instante si las actividades están configuradas correctamente.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img111.png)

En este caso, la actividad **Lookup** con la query devuelve todas las tablas del esquema `SalesLT`
El recuadro de la actividad **Lookup** muestra un **check verde**, indicando que la ejecución fue **exitosa**
Esto confirma que la query se ejecutó en SQL Server, que trajo los datos y que ADF pudo procesarlos sin errores

🔶 Bloque: Iconos **Input / Output**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img112.png)


Estos iconos aparecen junto al nombre de la actividad después de ejecutar el pipeline y permiten abrir ventanas con la **información detallada** de lo que ocurrió en esa actividad.

1. **➡️ Input Izquierda**
    
    - Muestra **qué configuraciones y parámetros entraron a la actividad**.
        
    - Ejemplo:
        
        - El dataset usado (`SqlServerTables`).
            
        - La query ejecutada (`SELECT ... FROM sys.tables ...`).
            
        - Si “First row only” estaba marcado o no.
            
        - Valores de parámetros (si los hubieras usado).
            
    - En pocas palabras: “lo que se le pasó a la actividad antes de ejecutarse”.
        
2. **⬅️ Output Derecha**
    
    - Muestra **los resultados devueltos por la actividad**.
        
    - En este caso, serán las filas obtenidas por la consulta SQL: una lista (array) de objetos con `SchemaName` y `TableName`.


### 4️⃣ **Usar los resultados**

Ahora que ya tienes lista la **actividad Lookup** que devuelve todas las tablas del esquema `SalesLT`, el siguiente paso es usar un **ForEach** en **Azure Data Factory** para iterar sobre cada una de esas tablas.

➕Agregar la actividad ForEach

- En  el panel de **actividades** en **Iteration & conditionals**:  
    ➜ Arrastra y suelta **ForEach** y renombralo a **ForEach Shema Table**
    ![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img113.png)
    
- Conéctalo a la salida del **Lookup** que acabas de configurar. **Selecciona el ícono verde "Success" (✅)** desde **Lookup**. Este ícono indica que puedes conectar otra actividad. Simplemente, arrastra con clic sostenido hacia **ForEach** y suelta para que se cree la conexión automáticamente.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img114.png)
>Esto activará el bloque **ForEach**, el cual recorrerá la lista de tablas devuelta por la actividad **Lookup**. En nuestro caso, como **Lookup** encontró **10 tablas**, **ForEach** ejecutará **10 iteraciones**, una por cada tabla, aplicando dentro del bucle la actividad que configuremos.

- Perfecto 👌, ahora procedamos con la configuración de **ForEach** en la pestaña **Settings**, donde vamos a definir el contenido que se va a iterar (los ítems).  

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img115.png)

1️⃣ **Ir a Settings**

- Selecciona la actividad **ForEach**.
    
- Abre la pestaña **Settings**.
    

---

2️⃣ **Items → Add dynamic content**

- En el campo **Items** aparece el mensaje _This property should be parameterized_.
    
- Aquí es donde debes indicar la colección que el ForEach recorrerá.
    
- Haz clic en **Add dynamic content** (o presiona `Alt+Shift+D`).
    

---

3️⃣ **Pipeline Expression Builder (ventana derecha)**

- Se abre la ventana del **Expression Builder** para escribir expresiones dinámicas.
    
- En la lista de la derecha selecciona la salida del Lookup (resaltado en tu captura):  
    👉 _Look for all tables → Look for all tables activity output_.
    

---

4️⃣ **Ajustar la expresión**

- Al seleccionar esa opción, ADF inserta en la caja de expresión:
    
    ```json
    @activity('Look for all tables').output
    ```
    
- Sin embargo, necesitamos que ForEach recorra el **array de filas** devuelto, no todo el objeto output.
    
- Por eso, debes agregar `.value` al final:
    
    ```json
    @activity('Look for all tables').output.value
    ```
    
![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img116.png)
    

✅ Esto asegura que ForEach iterará sobre cada elemento de la colección (cada tabla encontrada en el esquema).

- Lookup devuelve algo así:
    
    ```json
    {
      "count": 3,
      "value": [
        { "SchemaName": "SalesLT", "TableName": "Customer" },
        { "SchemaName": "SalesLT", "TableName": "Product" },
        { "SchemaName": "SalesLT", "TableName": "SalesOrderHeader" }
      ]
    }
    ```
    
- `@activity('Look for all tables').output.value` → entrega directamente el array dentro de `value`.
    
- ForEach recorrerá ese array → 3 iteraciones en este ejemplo.
    

---

👉 Con esto ya tienes el ForEach listo para iterar por cada tabla.  

---

### 5️⃣ **Configurar la Actividad Copy Data dentro del ForEach**

Una vez que tenemos el ciclo **ForEach**, que recorre todas las tablas obtenidas mediante el **Lookup**, debemos editar su contenido para añadir la actividad **Copy Data**.

A continuación, agregaremos y configuraremos la actividad **Copy Data** dentro del ciclo **ForEach**, con el objetivo de copiar cada tabla desde **SQL Server** hacia el **Data Lake**.

---

🧭 **Paso a paso:**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img119.png)

---

#### 1. **Accede al interior del ForEach**:

* Haz clic sobre el contenedor **ForEach**, y luego en el ícono del **lápiz ✏️** dentro del contenedor **ForEach Schema Table**.

* También puedes hacer clic en la pestaña **Activities (0)** en la parte inferior y luego en el lápiz de la columna **Activity**.

* Esto abrirá el diseño interno del ciclo **ForEach**, donde podrás definir qué actividades se ejecutarán para cada tabla.

---

#### 2. **Agregar la actividad Copy Data**:

* En el panel de actividades dentro del **ForEach**, selecciona **Copy Data** y arrástrala al área de trabajo.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img120.png)

* Cambia el nombre de la actividad a **Copy Each Table**.

---

#### 3. **Configurar la Fuente (Source)**:

* En la pestaña **Source** de la actividad **Copy Data**, selecciona **New** para crear un nuevo **dataset** de **SQL Server**. Completa los campos con los siguientes valores:

  * [ ] **Name**: `SqlServerCopy`
  * [ ] **Linked service**: `onpremsqlserver`
  * [ ] **Table name**: déjalo vacío

* Luego haz clic en **OK** para guardar los cambios.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img121.png)

* En el campo **Query**, haz clic en **Add dynamic content**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img122.png)

Esto abrirá el editor de expresiones dinámicas. Allí ingresaremos la siguiente expresión:

```json
@{concat('SELECT * FROM ', item().SchemaName, '.', item().TableName)}
```

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img130.png)

📌 ¿Qué hace esta expresión?

* `item()` hace referencia al **elemento actual** del ciclo **ForEach** (una tabla).
* `item().SchemaName`: obtiene el nombre del esquema.
* `item().TableName`: obtiene el nombre de la tabla.
* `concat(...)`: concatena el texto para formar una sentencia SQL válida del tipo:

```sql
SELECT * FROM schema_name.table_name
```

✅ **Resultado esperado**:
Con esta configuración, la actividad **Copy Data** ejecutará dinámicamente una consulta distinta para cada tabla devuelta por el **Lookup**, permitiéndote copiar múltiples tablas sin tener que configurarlas una por una.

Haz clic en **OK** para guardar la expresión.

---

#### 4. **Configurar el Sink (Destino)**:

* En la pestaña **Sink**, selecciona el **New** de destino correspondiente al **Data Lake**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img124.png)

Selecciona **Parquet**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img131.png)

Configura los campos de la siguiente forma:

* [ ] **Name**: `parquetTables`
* [ ] **Linked service**: `AzureDataLakeStorage1`
* [ ] **File path**: `Bronze`

Haz clic en **OK** para guardar los cambios.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img125.png)









---


#### 5. **Estructura de carpetas en el Data Lake**

Ahora debemos generar la siguiente **estructura de carpetas en el Data Lake** para guardar los archivos `.parquet`:

```
bronze/SchemaName/TableName/TableName.parquet
```

Ejemplo concreto:

```
bronze/SalesLT/Address/Address.parquet
```

Para lograr esto en **Azure Data Factory (ADF)** al configurar el **Sink (Destino)** de tu actividad **Copy Data**, debes usar contenido dinámico (Dynamic Content) en la propiedad **File path** del dataset de destino (tipo Azure Data Lake Storage Gen2).

---

#### ✅ **Configurar la Ruta Dinámica en el Sink:**

Ahora haz clic **Open** en la pestaña **Sink** (el que apunta al Data Lake **ParquetTables**), en el campo y lo que te llevará a nueva pestaña correspondiente al Dataset **ParquetTables**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img127.png)

🧭 Pasar los valores desde el pipeline

1. Vuelve a la actividad **Copy Each Table** en tu pipeline.
    
2. En la pestaña **Sink**, al usar el dataset `parquetTables`, verás que se piden los parámetros `schemaname` y `tablename`.
    
![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img128.png)

Una vez creados los parámetros `schemaname` y `tablename` dentro del dataset `parquetTables`, ahora debemos pasar sus valores dinámicamente desde la actividad **Copy Each Table**, para generar rutas dinámicas por tabla y esquema.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img129.png)


1. En la pestaña **Sink** de la actividad **Copy Each Table**, asegúrate de tener seleccionado el dataset `parquetTables`.
    
2. En la sección **Dataset properties**, verás los dos parámetros que creaste:
    
    - `schemaname`
        
    - `tablename`
        


✅ **Configurar los parámetros del Sink (Dataset `parquetTables`)**

---

🔶 **Paso 1: Configurar el parámetro `schemaname`**

1. Ve a la pestaña **Sink** de la actividad **Copy Each Table**.
    
2. Asegúrate de que el **Sink dataset** seleccionado sea `parquetTables`.
    
3. En la sección **Dataset properties**, ubica el campo llamado `schemaname`.
    
4. Haz clic en el enlace **"Add dynamic content"** debajo del campo `Value`.
    
5. En la ventana del **Pipeline Expression Builder**, escribe o selecciona la siguiente expresión:
    
    ```json
    @item().SchemaName
    ```
    
1. Haz clic en **OK** para guardar la expresión.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img132.png)

✅ Con esto, el parámetro `schemaname` tomará el valor dinámico de la propiedad `SchemaName` para cada tabla que se está procesando en el ciclo `ForEach`.

---

🔷 **Paso 2: Configurar el parámetro `tablename`**

1. En la misma sección de **Dataset properties**, ahora ubica el campo `tablename`.
    
2. Haz clic en **"Add dynamic content"** justo debajo del campo `Value`.
    
3. En la ventana del **Expression Builder**, escribe o selecciona la expresión:
    
    ```json
    @item().TableName
    ```
    
1. Haz clic en **OK** para guardar la expresión.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img133.png)

✅ Ahora, el parámetro `tablename` también se llenará dinámicamente para cada tabla en el `ForEach`.

---

🎯 Resultado

Con ambos parámetros configurados dinámicamente, tu dataset `parquetTables` tendrá suficiente información para construir rutas del estilo:

```
bronze/SalesLT/Address/Address.parquet
```

Todo de manera automática, para **cada tabla** obtenida en el `Lookup`.

🧭 **Paso a paso para crear la ruta dinámica en el Sink (Data Lake)**

🎯 Objetivo:

Lograr que los archivos `.parquet` se guarden con esta estructura:

```
bronze/SchemaName/TableName/TableName.parquet
```

Ejemplo:

```
bronze/SalesLT/Address/Address.parquet
```

🔸 Paso 1: Abrir el Sink Dataset

En la actividad **Copy Each Table**:

1. Ve a la pestaña **Sink**.
    
2. Haz clic en el botón **Open** junto al dataset de salida llamado `parquetTables`.



![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img135.png)

🔶 Esto abrirá una nueva pestaña con la configuración del dataset `parquetTables`.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img134.png)

🔸 Paso 2: Configurar la Ruta del Archivo Dinámicamente

En la pestaña del dataset `parquetTables`:

1. Ve a la pestaña **Connection**.
    
2. En el campo **File path**, en la parte de **Directory**, haz clic en **Add dynamic content**.
    
3. En el editor de expresiones, escribe lo siguiente:
    

```json
@{concat(dataset().schemaname, '/', dataset().tablename)}
```

📌 ¿Qué hace esta expresión?

- `dataset().schemaname` → toma el valor del parámetro `schemaname`.
    
- `'/'` → separa las carpetas.
    
- `dataset().tablename` → toma el valor del parámetro `tablename`.
    

✅ Resultado:  
Esto crea un path como:

```
bronze/SalesLT/Address
```

🔸 Paso 3: Definir el Nombre del Archivo `.parquet`

En el mismo dataset, en el campo **File name**:

1. Haz clic en **Add dynamic content**.
    
2. Ingresa la siguiente expresión:
    

```json
@{concat(dataset().tablename, '.parquet')}
```

📌 ¿Qué hace esta expresión?

- Toma el nombre de la tabla (`tablename`).
    
- Le concatena la extensión `.parquet`.
    

✅ Resultado:  
El archivo se nombrará dinámicamente como:

```
Address.parquet
```
---

📦 Resultado final en Data Lake:

Con esta configuración, al ejecutar la actividad **Copy Each Table**, los archivos se copiarán al Data Lake siguiendo esta estructura:

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img136.png)

```
bronze/
└── SchemaName/
    └── TableName/
        └── TableName.parquet
```

Ejemplo:

```
bronze/SalesLT/Address/Address.parquet
```



Ahora que hemos terminado de crear el Pipeline solo debemos guardar los cambios

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img137.png)



 **Prueba del Pipelin(Opcional)**

Una vez que todo esté configurado, puedes utilizar el modo **Debug** en Azure Data Factory para ejecutar el pipeline de manera de prueba. Esto te permitirá verificar que cada actividad esté configurada correctamente y que el pipeline esté copiando las tablas de SQL Server al Data Lake de manera exitosa.

---

### 6️⃣ Ejecutar el pipeline con **Trigger now**

Este paso es clave para validar si tu pipeline está funcionando correctamente una vez que ha sido **publicado**. Ahora te explico cómo hacerlo:

---

1. Asegúrate de haber publicado tu pipeline

Antes de poder ejecutar tu pipeline con un trigger manual, debes hacer clic en **"Publish all"** (en la esquina superior izquierda) para guardar y publicar los cambios realizados en tu pipeline.

---

2. Haz clic en **"Add trigger"**

En la parte superior del editor, ubica el botón **Add trigger** (ícono de rayo) y haz clic en él.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img138.png)


---

4. Selecciona la opción **Trigger now**

Se desplegará un pequeño menú. Haz clic en la opción **Trigger now**, como se muestra en la imagen.  
🔸 Esta opción ejecutará el pipeline inmediatamente, usando su configuración actual y sin necesidad de crear un trigger programado.

5. Confirma la ejecución del pipeline

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img139.png)

📌 En la ventana **Pipeline run**, verás un mensaje que dice:

**"Trigger pipeline now using last published configuration."**

Esto indica que el pipeline se ejecutará con la última versión publicada y sin ningún parámetro adicional (en este caso, no hay registros en la tabla de parámetros).

---

6. Haz clic en **OK** para continuar

✅ Solo necesitas confirmar haciendo clic en el botón azul **OK** (como se resalta en la imagen).

Esto lanzará la ejecución del pipeline inmediatamente y verás una ventana emergente con el estado **Running** lo que confirma que el Pipeline esta en ejecución.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img140.png)

---

#### 🔍 6. Monitorea la ejecución

➡️ Dirígete a la sección **"Monitor"** en el menú izquierdo de Azure Data Factory para verificar el progreso del pipeline.

📌 Aquí podrás:

* Visualizar si todo se ejecutó correctamente.
* Identificar si hubo errores.
* Verificar duración, origen y estado de ejecución.

✅ Para hacerlo, navega a:
**`Monitor > Pipeline runs`**

🔄 Aquí observarás el estado actual de tus ejecuciones:

* 🟡 **In progress**: En ejecución
* ✅ **Succeeded**: Ejecución completada exitosamente
* ❌ **Failed**: Ejecución fallida

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img141.png)

---

📊 ¿Qué información verás en esta sección?

🧾 En esta vista encontrarás un historial de ejecuciones del pipeline. Por ejemplo:

📝 **Ejemplo de ejecución:**

| Campo              | Valor                                             |
| ------------------ | ------------------------------------------------- |
| 🏷️ Pipeline       | `copy_all_tables`                                 |
| 🕒 Hora de inicio  | 03/09/2025 - 11:52:31 AM                          |
| 🔄 Estado          | `In progress` (en progreso al momento de captura) |
| ⏱️ Duración actual | 24 segundos                                       |
| 🔘 Trigger type    | `Manual trigger`                                  |
| 🆔 Run ID          | Identificador único para esta ejecución           |

✔️ Esta vista permite:

* Dar seguimiento al estado del pipeline en tiempo real.
* Validar la duración de ejecución.
* Identificar el origen del trigger.
* Obtener detalles útiles para monitoreo y depuración (**debugging**).

---

#### ✅ Actualización: Pipeline completado

📥 Luego de actualizar la vista, se puede verificar que el pipeline `copy_all_tables` **ya finalizó su ejecución**.

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img142.png)

📌 Detalles de la ejecución final:

* ▶️ Ejecutado manualmente el **3 de septiembre de 2025 a las 11:52:31 AM**
* ⏹️ Finalizado a las **11:53:24 AM**
* ⏱️ Duración total: **53 segundos**
* ✅ Estado: **"Succeeded"** (completado sin errores)
* 🧩 Triggered by: **Manual trigger**
* 🆔 Run ID: identificador único generado automáticamente

---

📝 **NOTAS:**

* Esta sección es crucial para el **monitoreo post-ejecución**.
* Permite detectar errores de forma temprana o confirmar que el proceso corrió exitosamente.
* Ideal para entornos de **despliegue**, **QA**, y **producción** donde la trazabilidad y visibilidad son clave.

---

💡 **TIP**: Puedes hacer clic sobre cualquier ejecución para ver un desglose completo de las actividades dentro del pipeline, con tiempos individuales y logs detallados.



### 📂 **Data Lake Validación**  
━━━━━━━━━━━━━━━━━━━

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img143.png)

✅ Una vez que el pipeline finaliza correctamente, pasamos a validar que los datos hayan llegado al destino correcto: nuestro Data Lake.

📍 Ingresamos al recurso de almacenamiento `z2hdatalakesuffix`  
📍 Navegamos al **Storage Browser**  
📍 Seleccionamos el contenedor **bronze**  
📍 Entramos a la carpeta **SalesLT** (donde configuramos que se guarden las tablas)

✔️ Allí encontramos las siguientes carpetas, que representan las tablas copiadas:

- 📁 Address
    
- 📁 Customer
    
- 📁 CustomerAddress
    
- 📁 Product
    
- 📁 ProductCategory
    
- 📁 ProductDescription
    
- 📁 ProductModel
    
- 📁 ProductModelProductDescription
    
- 📁 SalesOrderDetail
    
- 📁 SalesOrderHeader
    

📝NOTA: La hora de modificación de todas estas carpetas coincide con la ejecución del pipeline, lo que confirma que la operación se realizó de forma exitosa.

━━━━━━━━━━━━━━━━━━━  
📄 **VALIDACIÓN FINAL: CONTENIDO DE LAS CARPETAS**  
━━━━━━━━━━━━━━━━━━━

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img144.png)

🔍 Finalmente, ingresamos dentro de una de las carpetas generadas, en este caso:  
📁 **Address**

✔️ Dentro de ella encontramos el archivo:  
📄 `Address.parquet`  
📅 Fecha: 9/3/2025 - 11:53:13 AM  
🔥 Access tier: Hot  
📦 Tipo: Block blob  
📏 Tamaño: 35.08 KiB

📝NOTA: Este archivo en formato `.parquet` contiene los datos de la tabla `Address` extraídos desde la base de datos SQL y almacenados de forma óptima para análisis o transformación posterior.

━━━━━━━━━━━━━━━━━━━  
💡 **¿Qué logramos hasta ahora?**  
━━━━━━━━━━━━━━━━━━━

✔️ Automatizamos la extracción y carga de todas las tablas del esquema `SalesLT` desde SQL a Data Lake.  
✔️ Utilizamos un pipeline parametrizado que recorre las tablas dinámicamente.  
✔️ Validamos la ejecución en tiempo real y monitoreamos el proceso desde Azure Data Factory.  
✔️ Confirmamos que los datos ya se encuentran organizados en nuestro contenedor `bronze`.

---

### **Flujo del Pipeline:**

- **Lookup** obtiene las tablas.
    
- **ForEach** (u otra actividad de iteración) se usa para procesar cada tabla.
    
- **Copy Activity** se utiliza para copiar los datos de cada tabla al Data Lake.
    

---

Análisis:

1. **Lookup**  
    ✔️ Correcto, esta actividad se usa para obtener la lista de tablas desde la fuente.
    
2. **ForEach**  
    ✔️ Correcto, es la actividad que permite iterar sobre cada tabla obtenida por el Lookup.
    
3. **Copy Activity**  
    ✔️ Correcto, es la actividad que efectivamente realiza la copia de datos desde la tabla fuente al Data Lake.
    

---
### 🚀 **¿Está completo el flujo?** 🚀

El flujo básico para copiar múltiples tablas de una fuente a un Data Lake está correctamente planteado ✅.

---

🔍 **Pero, según buenas prácticas y escenarios reales, se podrían agregar más elementos para mayor robustez:**

➡️ **Control de errores (Error handling):**  
❓ ¿Qué pasa si falla alguna tabla?  
➡️ Agrega actividades para manejo de errores o lógica condicional para continuar o alertar.

➡️ **Validaciones o transformaciones:**  
🛠️ En algunos pipelines se incluye una actividad previa o posterior para validar o transformar datos.

➡️ **Parámetros dinámicos:**  
🎛️ Para hacer el pipeline más reusable y dinámico.

➡️ **Triggers:**  
⏰ Para automatizar la ejecución (aunque aquí fue manual, puede estar presente).

---

📋 **Resumen en tabla:**

|Elemento|¿Incluido?|Comentarios|
|---|---|---|
|Lookup|✅|Obtiene las tablas desde la fuente|
|ForEach|✅|Itera sobre cada tabla|
|Copy Activity|✅|Copia los datos al Data Lake|
|Control de errores|❌|Recomendado para robustez|
|Validaciones/Transform|❌|Opcional, pero útil para limpieza o ajuste|
|Parámetros dinámicos|❌|Facilita reutilización|
|Triggers|❌|Automatiza ejecución|

---

📝 **NOTAS:**

- Este flujo es **básico y funcional**, ideal para principiantes en Ingeniería de Datos en la nube.
    
- Más adelante, replicaremos este proyecto a nivel de código Python para que aprendas desde cero y con un stack open source cómo replicar todo esto 100% en Python y SQL.
	
- Las herramientas No-Code como Azure Data Factory abstraen el código, pero para entender a fondo y controlar mejor los pipelines, es fundamental aprender a programar los flujos.
	
- Conocer Python para construir pipelines te da flexibilidad y poder para optimizar y personalizar tu solución.

---

✅ **Conclusión:**  
Este pipeline básico funciona y es el primer paso para entender el proceso. Pero para flujos más robustos y reales, implementar manejo de errores, validaciones, parametrización y automatización es clave.


