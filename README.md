# practica-mcp

## Registro de Uso de Herramientas MCP Filesystem

En esta práctica se utilizaron exclusivamente las herramientas del servidor MCP `filesystem`.

### Herramientas utilizadas y orden de ejecución:

1. **`filesystem:list_directory`**
   - **Ruta:** `C:\Users\elqui\practica-mcp\datos`
   - **Propósito:** Listar el contenido del directorio `datos` para identificar el archivo `calificaciones.csv`.

2. **`filesystem:read_file`**
   - **Ruta:** `C:\Users\elqui\practica-mcp\datos\calificaciones.csv`
   - **Propósito:** Leer el contenido de las calificaciones de los alumnos.

3. **`filesystem:create_directory`**
   - **Ruta:** `C:\Users\elqui\practica-mcp\salida`
   - **Propósito:** Asegurar la existencia del directorio destino `salida` antes de generar el reporte.

4. **`filesystem:write_file`**
   - **Ruta:** `C:\Users\elqui\practica-mcp\salida\reporte.md`
   - **Propósito:** Escribir la tabla con los promedios finales ordenados de mayor a menor, el estado de aprobación y el promedio general del grupo.

5. **`filesystem:write_file`**
   - **Ruta:** `C:\Users\elqui\practica-mcp\README.md`
   - **Propósito:** Registrar este documento con las herramientas y el orden exacto en el que fueron ejecutadas.
