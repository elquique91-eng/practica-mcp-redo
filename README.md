# Práctica: Integración y Uso de Protocolo de Contexto de Modelo (MCP)

## 1. Descripción
Esta práctica consiste en la experimentación y validación del estándar **Model Context Protocol (MCP)**, conectando un agente de inteligencia artificial con herramientas externas para interactuar con el sistema de archivos local, realizar web scraping mediante automatización de navegadores, validar políticas de seguridad de sandbox y comunicarse con una base de datos PostgreSQL alojada en la nube.

- **Agente utilizado:** **Antigravity** (Google DeepMind)
- **Modelos empleados:** Claude Sonnet 4.6 (Thinking) y Gemini 3.8 Flash (Medium)
- **Versión de Antigravity:** Antigravity 2.0 (Entorno Desktop Windows)

---

## 2. Requisitos y reproducción

### Requisitos previos
1. **Sistema Operativo:** Windows 10/11 (o macOS/Linux adaptando las rutas).
2. **Node.js y npm:** Versión 18 o superior instalada (incluye `npx`).
3. **Git:** Instalado y configurado en el sistema.
4. **Google Chrome / Chromium:** Para las pruebas de navegación de Playwright.
5. **Cuenta en Supabase:** Cuenta activa con una organización y proyecto creado.

### Pasos para reproducir la práctica desde cero

1. **Clonar el repositorio:**
   ```bash
   git clone https://github.com/elquique91-eng/practica-mcp-redo.git
   cd practica-mcp-redo
   ```

2. **Descargar los navegadores de Playwright:**
   Ejecuta en tu terminal para contar con el binario de Chromium necesario:
   ```bash
   npx -y playwright install chromium
   ```

3. **Configurar el archivo de servidores MCP en Antigravity:**
   El archivo de configuración global se encuentra en:
   - **Windows:** `C:\Users\<TU_USUARIO>\.gemini\config\mcp_config.json`
   - **macOS/Linux:** `~/.gemini/config/mcp_config.json`

   > **IMPORTANTE (Cambio de ruta):** Debes reemplazar `C:\\Users\\elqui\\practica-mcp` por la ruta absoluta donde hayas clonado o ubicado tu proyecto en tu máquina.

   Escribe en `mcp_config.json`:
   ```json
   {
     "mcpServers": {
       "filesystem": {
         "command": "cmd",
         "args": [
           "/c",
           "npx",
           "-y",
           "@modelcontextprotocol/server-filesystem",
           "C:\\RUTA\\A\\TU\\CARPETA\\practica-mcp-redo"
         ]
       },
       "playwright": {
         "command": "cmd",
         "args": [
           "/c",
           "npx",
           "-y",
           "@playwright/mcp@latest",
           "--executable-path",
           "C:\\Users\\<TU_USUARIO>\\AppData\\Local\\ms-playwright\\chromium-1254\\chrome-win64\\chrome.exe"
         ]
       }
     }
   }
   ```

4. **Instalar el plugin de Supabase:**
   - Abre Antigravity.
   - Instala el plugin oficial de Supabase (`antigravity-plugins-partners/plugin/supabase`) desde el catálogo de plugins y vincula tu cuenta.

5. **Reiniciar Antigravity:**
   Cierra y abre completamente Antigravity para que reconozca los servidores MCP registrados.

---

## 3. Servidores MCP

| Servidor | Paquete / Fuente | Transporte | Herramientas Expuestas |
| :--- | :--- | :--- | :--- |
| **`filesystem`** | `@modelcontextprotocol/server-filesystem` | Stdio (Local) | `read_file`, `read_text_file`, `read_media_file`, `read_multiple_files`, `write_file`, `append_file`, `edit_file`, `create_directory`, `list_directory`, `list_directory_with_sizes`, `directory_tree`, `move_file`, `copy_file`, `search_files`, `get_file_info`, `list_allowed_directories`. |
| **`playwright`** | `@playwright/mcp@latest` | Stdio (Local) | `browser_navigate`, `browser_navigate_back`, `browser_click`, `browser_type`, `browser_fill_form`, `browser_press_key`, `browser_hover`, `browser_drag`, `browser_drop`, `browser_select_option`, `browser_snapshot`, `browser_take_screenshot`, `browser_evaluate`, `browser_run_code_unsafe`, `browser_tabs`, `browser_wait_for`, `browser_resize`, `browser_close`, etc. |
| **`supabase_mcp`** | Plugin Oficial Supabase | Remoto / SDK | `list_organizations`, `get_organization`, `list_projects`, `get_project`, `create_project`, `list_tables`, `execute_sql`, `apply_migration`, `query_logs`, `get_advisors`, `generate_typescript_types`, etc. |

---

## 4. Actividades

### Actividad A: Cálculo de Promedios de Calificaciones (MCP Filesystem)

- **Instrucción dada por el usuario:**
  > Usa unicamente las herramientas del servidor MCP filesystem.
  > 1) Lista el contenido de la carpeta datos.
  > 2) Lee datos/calificaciones.csv.
  > 3) Calcula el promedio final de cada alumno (media de los tres parciales, con dos decimales) e indica si aprueba (promedio >= 6.0).
  > 4) Escribe el resultado en salida/reporte.md como una tabla Markdown ordenada de mayor a menor promedio, y al final agrega el promedio del grupo.
  > Responde con un resumen breve de lo que hiciste. Resultado esperado:
  > Existe el archivo salida/reporte.md con ocho alumnos.
  > Seis aprueban y dos no (Diego Ruiz con 5.50 y Hector Vega con 5.33).
  > El promedio del grupo es 7.50.
  > Registra en un README: qué herramientas usó el agente y en qué orden.

- **Secuencia de herramientas utilizadas:**
  1. `filesystem:list_directory` (sobre `datos/` para identificar `calificaciones.csv`).
  2. `filesystem:read_file` (sobre `datos/calificaciones.csv` para leer los parciales).
  3. `filesystem:create_directory` (sobre `salida/` para asegurar la carpeta destino).
  4. `filesystem:write_file` (sobre `salida/reporte.md` escribiendo la tabla markdown ordenada).
  5. `filesystem:write_file` (sobre `README.md` registrando la bitácora de uso).

- **Resultado obtenido:**
  - Archivo `salida/reporte.md` generado con 8 alumnos:
    - 6 aprobados: Gabriela Luna (9.50), Carla Mendez (9.50), Ana Torres (9.00), Elena Castro (8.17), Fabian Ortiz (6.83), Bruno Salas (6.17).
    - 2 no aprobados: Diego Ruiz (5.50) y Hector Vega (5.33).
    - Promedio general del grupo: **7.50**.

- **Captura de evidencia:**
  ![Evidencia Actividad A](evidencias/Captura%20de%20pantalla%202026-11-12%20090732.png)

---

### Actividad B: Web Scraping de Entidades Federativas (MCP Playwright y Filesystem)

- **Instrucción dada por el usuario:**
  > Usa las herramientas MCP de playwright y filesystem.
  > 1) Abre en el navegador la pagina https://es.wikipedia.org/?curid=5897912
  > 2) Localiza la tabla principal con las 32 entidades federativas y extrae, para cada una: entidad, superficie en km2, poblacion y densidad (hab/km2).
  > 3) Guarda los datos en salida/entidades.csv con los encabezados entidad,superficie_km2,poblacion,densidad_hab_km2, con numeros sin separadores de miles y punto decimal.
  > 4) Toma una captura de pantalla de la pagina.
  > 5) Cierra el navegador.
  > Responde con un resumen breve: cuantas filas guardaste y que año de poblacion usa la tabla.

- **Secuencia de herramientas utilizadas:**
  1. `playwright:browser_navigate` (apertura de la URL de Wikipedia).
  2. `playwright:browser_evaluate` (inspección del DOM y extracción de las 32 entidades con limpieza de espacios duros y formato numérico).
  3. `playwright:browser_take_screenshot` (captura de la vista del navegador).
  4. `playwright:browser_close` (cierre controlado de la instancia del navegador).
  5. `filesystem:write_file` (escritura del archivo `salida/entidades.csv`).

- **Resultado obtenido:**
  - Se guardaron **32 filas** de entidades federativas más 1 fila de encabezados.
  - La tabla utiliza la columna de población del **año 2020** (Censo INEGI 2020).
  - Captura almacenada en `salida/captura_pagina.png` y datos en `salida/entidades.csv`.

- **Captura de evidencia:**
  ![Evidencia Actividad B](evidencias/Captura%20de%20pantalla%202026-11-12%20092838.png)

---

### Actividad C: Validación de Permisos y Sandbox (MCP Filesystem)

- **Instrucción dada por el usuario:**
  > Usa unicamente las herramientas del servidor MCP filesystem.
  > 1) Llama a list_allowed_directories.
  > 2) Intenta leer un archivo que este fuera de la carpeta de la practica (en Windows: C:/Windows/win.ini; en macOS o Linux: /etc/hosts).
  > 3) Intenta listar el directorio padre de tu carpeta permitida.
  > No uses ninguna otra via si falla. Reporta textualmente que respondio el servidor en cada intento

- **Secuencia de herramientas utilizadas:**
  1. `filesystem:list_allowed_directories`
  2. `filesystem:read_file` (con ruta `C:/Windows/win.ini`)
  3. `filesystem:list_directory` (con ruta `C:\Users\elqui`)

- **Respuestas textuales reportadas por el servidor:**
  1. `list_allowed_directories`:
     ```text
     Allowed directories:
     C:\Users\elqui\practica-mcp
     ```
  2. Intento de lectura de `C:/Windows/win.ini`:
     ```text
     Access denied - path outside allowed directories: C:\Windows\win.ini not in C:\Users\elqui\practica-mcp
     ```
  3. Intento de listar carpeta padre `C:\Users\elqui`:
     ```text
     Access denied - path outside allowed directories: C:\Users\elqui not in C:\Users\elqui\practica-mcp
     ```

- **Capturas de evidencia:**
  ![Evidencia Actividad C - 1](evidencias/Captura%20de%20pantalla%202026-11-12%20093310.png)
  ![Evidencia Actividad C - 2](evidencias/Captura%20de%20pantalla%202026-11-12%20093315.png)

---

## 5. Actividad D: Migración a Base de Datos en la Nube con Supabase MCP

### Servidor seleccionado
- **Servidor MCP:** `supabase_mcp` (Plugin oficial de Supabase para Antigravity / `@supabase/mcp-server-supabase`).

### Quién lo publica
- Publicado y respaldado oficialmente por **Supabase Inc.** en asociación con el equipo de **Google Antigravity** como plugin asociado (`antigravity-plugins-partners/plugin/supabase`).

### Tarea resuelta
A partir de los datos locales extraídos en la Actividad B (`salida/entidades.csv`):
1. El agente leyó localmente el archivo CSV mediante `filesystem:read_file`.
2. Consultó y detectó el proyecto activo del usuario llamado **`proyecto 1`** (`ref: oehihqpiuxzjvdffphlc`) en Supabase vía `supabase_mcp:list_projects`.
3. Ejecutó una migración DDL mediante `supabase_mcp:execute_sql` creando la tabla:
   ```sql
   CREATE TABLE IF NOT EXISTS entidades_federativas (
       id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
       entidad TEXT NOT NULL UNIQUE,
       superficie_km2 BIGINT NOT NULL,
       poblacion BIGINT NOT NULL,
       densidad_hab_km2 NUMERIC NOT NULL,
       created_at TIMESTAMPTZ DEFAULT NOW()
   );
   ```
4. Insertó los **32 registros** de las entidades federativas directamente en la nube.
5. Ejecutó consultas SQL analíticas de comprobación:
   - Total de registros: **32**
   - Población nacional total: **126,014,024 habitantes**
   - Superficie total: **1,962,833 km²**
   - Densidad promedio nacional: **296.91 hab/km²**

### Por qué se considera confiable
1. **Autoría oficial:** Es desarrollado y mantenido directamente por el equipo de ingeniería de Supabase, lo que garantiza compatibilidad estricta con las versiones de PostgreSQL 15, 16 y 17.
2. **Seguridad y Control de Acceso:** No expone las claves maestras de Postgres al modelo; las peticiones se autentican vía OAuth / Personal Access Tokens (PAT) con alcance granular y permiten modo de sólo lectura (`--read-only`).
3. **Manejo de Transacciones y Tipado:** Implementa validaciones con esquemas Zod en todas las entradas y utiliza la API de gestión oficial de Supabase, aislando la ejecución y mitigando inyecciones no controladas.

- **Capturas de evidencia:**
  ![Evidencia Actividad D - 1](evidencias/Captura%20de%20pantalla%202026-11-12%20204808.png)
  ![Evidencia Actividad D - 2](evidencias/Captura%20de%20pantalla%202026-11-12%20204814.png)
  ![Evidencia Actividad D - 3](evidencias/Captura%20de%20pantalla%202026-11-12%20204821.png)
