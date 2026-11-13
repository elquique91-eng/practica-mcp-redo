# Instrucciones de la Práctica MCP

Este documento recopila de manera cronológica todas las instrucciones proporcionadas por el usuario durante la sesión de trabajo con Antigravity y los servidores MCP (**Filesystem**, **Playwright** y **Supabase**).

---

## 1. Consulta inicial del entorno
> cual es la variante de antigravity que estoy usando?

---

## 2. Configuración inicial del servidor MCP Filesystem
> "mcpServers": {
> "filesystem": {
> "command": "cmd",
> "args": ["/c", "npx", "-y",
> "@modelcontextprotocol/server-filesystem",
> "C:\\Users\\elqui\\practica1-mcp"]
> }
> }

---

## 3. Confirmación de configuración
> configurala en antigravity

---

## 4. Ejercicio 1 - Procesamiento de Calificaciones (MCP Filesystem)
> Usa unicamente las herramientas del servidor MCP filesystem.
> 1) Lista el contenido de la carpeta datos.
> 2) Lee datos/calificaciones.csv.
> 3) Calcula el promedio final de cada alumno (media de los tres parciales, con dos decimales) e indica si aprueba (promedio >= 6.0).
> 4) Escribe el resultado en salida/reporte.md como una tabla Markdown ordenada de mayor a menor promedio, y al final agrega el promedio del grupo.
> 
> Responde con un resumen breve de lo que hiciste. Resultado esperado:
> Existe el archivo salida/reporte.md con ocho alumnos.
> Seis aprueban y dos no (Diego Ruiz con 5.50 y Hector Vega con 5.33).
> El promedio del grupo es 7.50.
> Registra en un README: qué herramientas usó el agente y en qué orden.

---

## 5. Notificación de reinicio de Antigravity
> ya reinicie antigravity.

*(Posteriormente se corrigió la ruta de `practica1-mcp` a `practica-mcp` y se repitió la instrucción tras un reinicio).*

> ya esta reiniciado

---

## 6. Ejercicio 2 - Scraping web y extracción (MCP Playwright y Filesystem)
> Usa las herramientas MCP de playwright y filesystem.
> 1) Abre en el navegador la pagina https://es.wikipedia.org/?curid=5897912
> 2) Localiza la tabla principal con las 32 entidades federativas y extrae, para cada una: entidad, superficie en km2, poblacion y densidad (hab/km2).
> 3) Guarda los datos en salida/entidades.csv con los encabezados entidad,superficie_km2,poblacion,densidad_hab_km2, con numeros sin separadores de miles y punto decimal.
> 4) Toma una captura de pantalla de la pagina.
> 5) Cierra el navegador.
> 
> Responde con un resumen breve: cuantas filas guardaste y que año de poblacion usa la tabla.

---

## 7. Configuración de Playwright MCP
> si,configura el json

> parece que me falto agregar esto anteriormente en la configuracion "playwright": {
> "command": "cmd",
> "args": ["/c", "npx", "-y", "@playwright/mcp@latest"]
> }
> }
> }

> ya reinicie 

> ya esta reiniciado otra vez

---

## 8. Ejercicio 3 - Pruebas de Seguridad y Permisos (Sandbox / Whitelist MCP Filesystem)
> Usa unicamente las herramientas del servidor MCP filesystem.
> 1) Llama a list_allowed_directories.
> 2) Intenta leer un archivo que este fuera de la carpeta de la practica (en Windows: C:/Windows/win.ini; en macOS o Linux: /etc/hosts).
> 3) Intenta listar el directorio padre de tu carpeta permitida.
> 
> No uses ninguna otra via si falla. Reporta textualmente que respondio el servidor en cada intento

---

## 9. Exploración e Instalación de Supabase MCP
> busca como instalar el mcp supabase,y dame ideas de algun problema que resolver con aquel para probarlo

---

## 10. Ejercicio 4 - Migración de Datos a Supabase (MCP Filesystem + Supabase MCP)
> continua con la primera idea

> ya cree el proyecto,llamado proyecto 1

---

## 11. Documentación y subida a GitHub
> en la subcarpeta de instructiones de la carpeta practica-mcp añade todas las instrucciones que te di en esta conversacion,y subelo a mi repositorio de github "practica mcp" que ya acaba de ser creado
