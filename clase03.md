# Clase03 - 01/09/25

Formatos de datos:

- CSV
- XML
- SQL
- JSON

Ejercicio:
Descargar y abrir y guardar un dataset en formato CSV, XML, SQL o JSON.

```python
"""
Contenido:
1) Preparación del entorno y "simulación" de descarga (creación de archivos).
2) Lectura (read) desde cada formato con pandas.
3) Operaciones básicas (selección, filtrado, agregación).
4) Guardado (to_*) a otros formatos.
5) (Opcional) Ejemplo de descarga real con Internet.
"""

import os
import sqlite3
import pandas as pd

# -------------------------------------------------------------------------
# 1) Preparación del entorno y datos "base"
# -------------------------------------------------------------------------
base_dir = "data_formats_demo"
os.makedirs(base_dir, exist_ok=True)

# Creamos un DataFrame de ejemplo con "alumnos"
alumnos = pd.DataFrame(
    {
        "id": [1, 2, 3, 4, 5, 6],
        "nombre": ["Ana", "Luis", "María", "José", "Elena", "Pablo"],
        "carrera": ["IS", "IS", "CE", "IS", "CE", "IS"],  # IS=Ing. Software, CE=Computación Eléctrica
        "semestre": [1, 3, 5, 1, 7, 9],
        "promedio": [8.5, 9.1, 7.8, 8.2, 9.7, 6.9],
    }
)

# Guardamos en varios formatos (simulación de descarga previa)
csv_path  = os.path.join(base_dir, "alumnos.csv")
json_path = os.path.join(base_dir, "alumnos.json")
xml_path  = os.path.join(base_dir, "alumnos.xml")
db_path   = os.path.join(base_dir, "universidad.sqlite")

# 1.a) CSV
alumnos.to_csv(csv_path, index=False)

# 1.b) JSON
alumnos.to_json(json_path, orient="records", force_ascii=False, indent=2)

# 1.c) XML
alumnos.to_xml(xml_path, index=False, root_name="alumnos", row_name="alumno",
               xml_declaration=True, encoding="utf-8")

# 1.d) SQL (SQLite)
conn = sqlite3.connect(db_path)
alumnos.to_sql("alumnos", conn, if_exists="replace", index=False)
conn.commit()
conn.close()

# -------------------------------------------------------------------------
# 2) Lectura desde cada formato con pandas
# -------------------------------------------------------------------------
df_csv  = pd.read_csv(csv_path)
df_json = pd.read_json(json_path)
df_xml  = pd.read_xml(xml_path)
conn = sqlite3.connect(db_path)
df_sql = pd.read_sql_query("SELECT * FROM alumnos", conn)
conn.close()

print("CSV:\n", df_csv.head(), "\n")
print("JSON:\n", df_json.head(), "\n")
print("XML:\n", df_xml.head(), "\n")
print("SQL:\n", df_sql.head(), "\n")

# -------------------------------------------------------------------------
# 3) Operaciones básicas
# -------------------------------------------------------------------------
# Selección: solo nombre, carrera y promedio
seleccion = df_csv[["nombre", "carrera", "promedio"]]

# Filtrado: alumnos con promedio >= 8.5
buen_promedio = df_csv[df_csv["promedio"] >= 8.5]

# Agregación: promedio por carrera
promedio_por_carrera = df_csv.groupby("carrera", as_index=False).agg(
    promedio_medio=("promedio", "mean")
)

print("Selección:\n", seleccion, "\n")
print("Filtrado:\n", buen_promedio, "\n")
print("Promedio por carrera:\n", promedio_por_carrera, "\n")

# -------------------------------------------------------------------------
# 4) Guardado a otros formatos
# -------------------------------------------------------------------------
salida_dir = os.path.join(base_dir, "salidas")
os.makedirs(salida_dir, exist_ok=True)

# Guardamos resultados en distintos formatos
seleccion.to_csv(os.path.join(salida_dir, "seleccion.csv"), index=False)
buen_promedio.to_json(os.path.join(salida_dir, "buen_promedio.json"),
                      orient="records", force_ascii=False, indent=2)
promedio_por_carrera.to_xml(os.path.join(salida_dir, "promedio_por_carrera.xml"),
                            index=False, root_name="resumen", row_name="carrera")

# -------------------------------------------------------------------------
# 5) Ejemplo de descarga real (solo en tu equipo con Internet):
# -------------------------------------------------------------------------
"""
import requests
url = "https://people.sc.fsu.edu/~jburkardt/data/csv/airtravel.csv"
r = requests.get(url, timeout=30)
with open("airtravel.csv", "wb") as f:
    f.write(r.content)

df_air = pd.read_csv("airtravel.csv")
print(df_air.head())
"""
```
