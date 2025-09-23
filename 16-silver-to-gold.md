


# Silver → Gold

## 🚀 **Transformación de Tablas: Silver → Gold**

### Estandarización de nombres de columnas 🛠️📊

---

### 🔍 **Objetivo Principal**

Convertir los nombres de columnas de **CamelCase** a **snake_case** para mejorar la estandarización y facilitar el manejo en SQL, Spark y BI.

---

### 🔄 **Proceso Detallado**

| Paso | Descripción                                                 | Icono |
| ---- | ----------------------------------------------------------- | ----- |
| 1    | Cargar todas las tablas de la capa **Silver**               | 📥    |
| 2    | Convertir nombres de columnas de **CamelCase → snake_case** | 🔄    |
| 3    | Guardar las tablas transformadas en la capa **Gold**        | 💾    |
| 4    | Reemplazar versiones previas en Gold                        | 🔁    |

---

### 📋 **Beneficios Clave**

- 🐍 **snake_case:** estándar práctico y legible para datos.
    
- 📈 **Mejora en la lectura:** facilita entender y trabajar con los datos.
    
- ⚡ **Optimización de integración:** ideal para herramientas BI (Power BI, Synapse, Power Apps).
    
- 🔒 **Gobernanza y calidad:** asegura uniformidad y control en la capa Gold.
    

---

📝 **NOTAS**

➡️ La automatización cubre **todas las tablas existentes** en Silver, evitando procesos manuales.  
➡️ El formato Delta permite versiones y actualizaciones eficientes.  
➡️ La estructura uniforme en Gold garantiza consumo rápido y sin errores.

---

💡 **Tips para el pipeline de datos**

- Mantener consistencia en los nombres evita errores en consultas y reportes.
    
- Validar siempre las tablas después de la transformación para asegurar integridad.
    
- Documentar cualquier cambio para futuros mantenimientos.
    

---

## 🥇 **Script de Transformación: Silver ➡️ Gold**

📍 **Objetivo:** Estándar de nombres de columnas + escritura en formato Delta en la capa Gold

---

🔄 **¿Qué hace este script?**

✅ Este script toma todas las tablas del contenedor **Silver**, renombra sus columnas del formato **CamelCase** a **snake_case** (todo en minúsculas), y luego guarda los resultados como tablas Delta en la carpeta correspondiente dentro de la capa **Gold**.

---

🧾 **Ejemplo de columnas originales en Silver**

Basado en el ejemplo real que mostraste (imagen):

```
["AddressID", "AddressLine1", "AddressLine2", "City", "StateProvince", "CountryRegion", "PostalCode", "rowguid", "ModifiedDate"]
```

Estas columnas están escritas en formato **CamelCase** y fueron generadas previamente durante la transformación de **Bronze ➝ Silver**.

---

🧠 **Script completo: Silver to Gold**

```python
table_name = []

# ✅ Listar tablas desde Silver
for i in dbutils.fs.ls("abfss://silver@z2hdatalakesuffix.dfs.core.windows.net/SalesLT/"):
    if i.isDir():
        table_name.append(i.name.split("/")[0])

print(table_name)

for name in table_name:
    path = f"abfss://silver@z2hdatalakesuffix.dfs.core.windows.net/SalesLT/{name}"
    print(f"Leyendo: {path}")

    df = spark.read.format("delta").load(path)

    column_names = df.columns

    for old_col_name in column_names:
        new_col_name = "".join([
            "_" + char.lower() if char.isupper() and (i > 0 and not old_col_name[i-1].isupper()) else char.lower()
            for i, char in enumerate(old_col_name)
        ]).lstrip("_")

        df = df.withColumnRenamed(old_col_name, new_col_name)
    
    output_path = f"abfss://gold@z2hdatalakesuffix.dfs.core.windows.net/SalesLT/{name}/"
    print(f"Saving to: {output_path}")

    df.write.format("delta").mode("overwrite").save(output_path)
```

---

✅ **Explicación paso a paso**

### 1️⃣ Listar tablas desde Silver

```python
for i in dbutils.fs.ls("abfss://silver@z2hdatalakesuffix.dfs.core.windows.net/SalesLT/"):
    if i.isDir():
        table_name.append(i.name.split("/")[0])
```

🔍 Se identifican las carpetas en el directorio **Silver/SalesLT** (cada carpeta es una tabla).

📸 **Ejemplo visual**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img235.png)

---

### 2️⃣ Cargar cada tabla desde Silver

```python
df = spark.read.format("delta").load(path)
```

📥 Se lee cada tabla Delta individualmente desde la capa **Silver**.

---

### 3️⃣ Renombrar columnas a snake_case

```python
for old_col_name in column_names:
    new_col_name = "".join([
        "_" + char.lower() if char.isupper() and (i > 0 and not old_col_name[i-1].isupper()) else char.lower()
        for i, char in enumerate(old_col_name)
    ]).lstrip("_")

    df = df.withColumnRenamed(old_col_name, new_col_name)
```

🧠 Este bloque convierte nombres como:

|🧾 Antes (CamelCase)|🔁 Después (snake_case)|
|---|---|
|AddressID|address_id|
|ModifiedDate|modified_date|
|PostalCode|postal_code|
|CountryRegion|country_region|

---

🎯 **¿Por qué es importante este cambio?**

Transformar los nombres de columna a **snake_case** es mucho más que un tema visual. Tiene beneficios clave para todo tu pipeline de datos:

|✅ Beneficio|💡 Razón|
|---|---|
|🔁 Consistencia|`snake_case` es el estándar más usado en SQL, Python, Spark, APIs, y lagos de datos.|
|⚠️ Evita errores en BI|Herramientas como Power BI, Power Apps o Excel pueden fallar con CamelCase o mayúsculas.|
|🧠 Más legible|Nombres como `customer_id` o `modified_date` son más fáciles de leer y entender a simple vista.|
|🔍 Mejores queries SQL|No necesitas usar comillas para referenciar columnas (`"CustomerName"` vs `customer_name`).|
|🛠️ Simplifica transformaciones|Es más fácil automatizar procesos si los nombres siguen un patrón común.|
|🤝 Colaboración fluida|Todo el equipo entiende y trabaja sobre un formato estándar y mantenible.|

---

🔁 **¿Cómo se hace la conversión?**

- Se agrega un guion bajo (`_`) antes de cada letra mayúscula (excepto si está precedida por otra mayúscula).
    
- Se convierte todo a minúsculas.
    
- Se elimina el guion bajo inicial con `.lstrip("_")`.
    
- El resultado final es `snake_case`.
    

📝 **Nota técnica:** Este método puede sobrefragmentar siglas (`SKUCode` → `s_k_u_code`). Se puede mejorar con expresiones regulares (`re.sub()`).

---

### 4️⃣ Guardar en Gold

```python
df.write.format("delta").mode("overwrite").save(output_path)
```

📦 La tabla resultante se guarda en:

```
abfss://gold@z2hdatalakesuffix.dfs.core.windows.net/SalesLT/<table_name>/
```

📌 Se utiliza formato Delta Lake

♻️ Modo `overwrite` asegura que se reemplace la versión anterior si ya existe.

📸 **Ejemplo visual**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img236.png)

---

📝 **NOTAS**

|⚠️ Consideración|💡 Detalle|
|---|---|
|✔ Solo renombra columnas|No afecta los valores de las celdas.|
|❌ No modifica fechas|Si `ModifiedDate` venía con la hora, así se queda.|
|⚠️ Manejo de siglas|`ProductSKU` se convertiría en `product_s_k_u`.|
|❌ No maneja campos complejos|No aplica a structs, arrays, maps.|

---

✅ **RESUMEN**

|🧩 Paso|Acción|
|---|---|
|1️⃣ Listar tablas|Desde Silver/SalesLT|
|2️⃣ Leer tabla|Se carga en un DataFrame desde Silver|
|3️⃣ Renombrar columnas|De CamelCase ➡️ snake_case (minúsculas)|
|4️⃣ Guardar como Delta|En Gold/SalesLT/|

---

🔍 **Validación rápida**

📂 Verifica que las tablas estén en Gold:

```python
dbutils.fs.ls("abfss://gold@z2hdatalakesuffix.dfs.core.windows.net/SalesLT/")
```

🔎 Consulta columnas ya renombradas:

```python
df = spark.read.format("delta").load("abfss://gold@z2hdatalakesuffix.dfs.core.windows.net/SalesLT/Address/")
df.columns
# ['address_id', 'address_line1', ..., 'modified_date']
```

---

🎓 **Recomendaciones para mejoras futuras**

|🧪 Mejora|🛠️ Sugerencia|
|---|---|
|Mejor manejo de siglas|Usar `re.sub(r'(?<!^)(?=[A-Z])', '_', name).lower()`|
|Transformación de fechas|Añadir lógica para truncar hora en campos `*_date`|
|Logging de procesos|Registrar tablas procesadas con timestamp|
|Validación estructural|Verificar existencia de columnas críticas|

---

### 🟡**Contenedor Gold**

📦 Los datos se encuentran ahora en formato **Delta** en el contenedor Gold:

📁 `abfss://gold@.../SalesLT/...`

📝 Ahora puedes **verificar** en el contenedor **Gold** las tablas exportadas en formato **Delta**, lo que indica que el script fue ejecutado exitosamente ✅

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img237.png)

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img238.png)


### ✅ **Validando los resultados**

🔍 **Verificación de datos en Gold**

Después de ejecutar el script de transformación, puedes validar que las columnas han sido renombradas correctamente a `snake_case` y que los datos están en formato Delta en la capa **Gold**

```python
spark.read.format("delta").load("abfss://gold@z2hdatalakesuffix.dfs.core.windows.net/SalesLT/Address/").show(10)
```

✔ Al revisar los datos en Gold, notarás que los nombres de columna ya siguen el formato `snake_case` y los datos permanecen sin modificación, excepto por el renombramiento de columnas.

📸 **Salida**

```plaintext
+----------+-------------------+-------------+--------+----------------+--------------+-----------+--------------------+---------------+
|address_id|      address_line1|address_line2|    city|  state_province|country_region|postal_code|             rowguid| modified_date |
+----------+-------------------+-------------+--------+----------------+--------------+-----------+--------------------+---------------+
|         9|  8713 Yosemite Ct.|         NULL| Bothell|      Washington| United States|      98011|268af621-76d7-4c7...|     2006-07-01 |
|        11|1318 Lasalle Street|         NULL| Bothell|      Washington| United States|      98011|981b3303-aca2-49c...|     2007-04-01 |
|        25|   9178 Jumping St.|         NULL|  Dallas|           Texas| United States|      75201|c8df3bd9-48f0-465...|     2006-09-01 |
|        28|   9228 Via Del Sol|         NULL| Phoenix|         Arizona| United States|      85004|12ae5ee1-fc3e-468...|     2005-09-01 |
|        32|  26910 Indela Road|         NULL|Montreal|          Quebec|        Canada|    H1Y 2H5|84a95f62-3ae8-4e7...|     2006-08-01 |
|       185|    2681 Eagle Peak|         NULL|Bellevue|      Washington| United States|      98004|7bccf442-2268-46c...|     2006-09-01 |
|       297|    7943 Walnut Ave|         NULL|  Renton|      Washington| United States|      98055|52410da4-2778-4b1...|     2006-08-01 |
|       445| 6388 Lake City Way|         NULL| Burnaby|British Columbia|        Canada|    V5A 3A6|53572f25-9133-4a8...|     2006-09-01 |
|       446|  52560 Free Street|         NULL| Toronto|         Ontario|        Canada|    M4B 1V7|801a1dfc-5125-486...|     2005-08-01 |
|       447|  22580 Free Street|         NULL| Toronto|         Ontario|        Canada|    M4B 1V7|88cee379-dbb8-433...|     2006-08-01 |
+----------+-------------------+-------------+--------+----------------+--------------+-----------+--------------------+---------------+
```

✅ Como puedes ver, los nombres de columna están correctamente en `snake_case` y los datos se han preservado exactamente como estaban en Silver.

📝 **Nota:** En esta versión del script, la columna `modified_date` no ha sido truncada a nivel de hora; simplemente conserva el valor original tal como estaba en Silver. Si deseas truncar la hora y conservar solo la fecha, puedes añadir una transformación adicional en el futuro.

---

📸 **Ejemplo visual**

![](../../../../static/img/0%20-%20Programs/1%20-%20DataEngineering/6%20-%20Azure/img239.png)

### 🎉 **¡Transformación completada!**

Tus tablas ahora están en la capa 🥇 **Gold**, con nombres de columnas en `snake_case`, listas para ser consumidas por:

✨ Power BI  
✨ Azure Synapse  
✨ Power Apps  
✨ Modelado Gold en arquitecturas Medallion