# find + xargs Cheatsheet · Hoja de referencia de comandos find + xargs

> 🌍 [中文](README.md) · [Português](README.pt.md)

Una **hoja de referencia de una sola página** con 30 usos reales de `find` y `xargs`: buscar archivos por nombre, tipo, tamaño, fecha y permisos; combinar condiciones sin caer en la trampa clásica de `-o` con `-print`; actuar sobre los resultados con `-exec`, `-execdir` y `-printf`; y pasar la lista a `xargs` de forma segura (`-print0`/`-0` contra nombres con espacios, `-P` en paralelo, `-n` y `-I` para controlar los lotes). Cada entrada responde solo tres preguntas: **qué hace, un comando listo para copiar y pegar, y etiquetas de uso**. Sin parámetros inventados: las 30 entradas se **ejecutaron de verdad**, una por una, en un árbol de prueba con GNU findutils 4.9.0, y la semántica se verificó contra el manual oficial de GNU Findutils.

## Para quién es

Ingenieros de operaciones (SRE/DevOps), desarrolladores backend y cualquiera que cada vez que escribe un `find` tenga que buscar si `-exec` termina en `\;` o en `+`, o que haya visto romperse una tubería a `xargs` por un nombre de archivo con espacios. Si buscar, renombrar o limpiar archivos en lote te frena, esta hoja te lo deja claro de una vez.

## Cómo usarla

1. Ve directo a **[CHEATSHEET.md](CHEATSHEET.md)**: el cuerpo de la referencia; los comandos en inglés los entiende cualquier desarrollador.
2. Necesitas GNU findutils: viene con Linux; macOS trae la versión BSD y algunas opciones (como `-r` de xargs) difieren — la hoja marca las diferencias.
3. Sigue el orden de la hoja —conceptos básicos → filtros → lógica → acciones → xargs— y prueba cada comando a mano en un directorio de práctica. En una hora los dominas.

## Pruébalo en 30 segundos

```bash
find --version | head -1           # imprime la versión (p. ej. find (GNU findutils) 4.9.0): es la versión GNU
mkdir -p /tmp/play && cd /tmp/play # directorio de práctica: ensaya aquí todo lo destructivo
touch app.log notes.txt            # crea dos archivos y prueba la combinación de abajo al instante
```

## La combinación de oro, lista para usar

```bash
find . -name "*.txt" -print0 | xargs -0 grep -l "TODO"
```

Inmune a espacios, comillas y saltos de línea en los nombres: find separa con NUL (el único byte que un nombre de archivo nunca puede contener) y xargs `-0` lee con NUL. Para árboles de directorios ajenos, usa siempre esta pareja.

## Qué incluye

| Categoría | Contenido |
|---|---|
| 🔍 Conceptos básicos | -name por nombre / -iname sin distinguir mayúsculas / -type por tipo / -path sobre la ruta completa / -maxdepth y -mindepth (5) |
| 🎯 Filtros | -size por tamaño (con su trampa de redondeo) / -mtime y -mmin por fecha / -newer contra un archivo de referencia / -empty / -perm en tres modos / -regex (6) |
| 🧩 Lógica | Paréntesis para -o, negación con !, y la trampa de -print que se ata a la rama equivocada (1) |
| ⚙️ Acciones | -exec \; uno a uno / -exec {} + por lotes / -execdir dentro del directorio / -ok preguntando antes / -printf salida a medida / -ls listado / -delete borrar (7) |
| 🚀 xargs | Tubería básica / la pareja segura -print0 + -0 / -n límite por tanda / -I marcador / -P paralelo / -t eco / -p preguntar / -r no ejecutar si está vacío / -d delimitador / -a desde archivo / -E marca de fin (11) |

La documentación oficial enlazada es solo https://www.gnu.org/software/findutils/manual/; todos los comandos se verificaron contra el manual de GNU Findutils (capítulos `find` y `xargs`) y se ejecutaron con éxito en GNU findutils 4.9.0; los ejemplos operan únicamente sobre un directorio de práctica creado al momento, sin datos reales.

## Por qué estas 30

- **La pareja segura, primero**: una tubería común corta los nombres por cualquier espacio, y un nombre con espacios se rompe en pedazos — `-print0` + `-0` es la combinación que hay que memorizar antes que ninguna.
- **`;` y `+` difieren en órdenes de magnitud**: `-exec ... \;` lanza un proceso por archivo; `-exec ... {} +` llena una línea de comando antes de ejecutar. Con miles de archivos es la diferencia entre minutos y segundos.
- **Dos trampas, señaladas de antemano**: al añadir una acción a una expresión con `-o`, el `-print` se ata solo a la última rama y pierdes la mitad de los resultados en silencio; y `-size` redondea hacia arriba antes de comparar, así que `-size -1M` solo coincide con archivos vacíos. Ambas llevan ⚠️ y su forma correcta en la hoja.
- **Lo destructivo se ensaya**: `-delete` no tiene papelera; `-ok` y `xargs -p` son su modo de ensayo — primero la lista, luego la acción.

## Se aceptan PR

¿Un comando incorrecto, una opción obsoleta, una traducción mejorable? Los Pull Requests son bienvenidos. Mantén el formato de las entradas (una línea de explicación / bloque bash / etiquetas) y **solo comandos reales y verificados; ante la duda, déjalo fuera**.

## 📚 Más hojas de referencia

Otras hojas del mismo autor, también en chino / español / portugués:

- [Hoja de SSH](https://github.com/Ezra-Zhao/ssh-cheatsheet) — 28 comandos de conexión, claves, túneles y endurecimiento
- [Hoja de FFmpeg](https://github.com/Ezra-Zhao/ffmpeg-cheatsheet) — 28 comandos de audio y vídeo
- [Hoja de Git](https://github.com/Ezra-Zhao/git-cheatsheet) — 30 comandos diarios y de rescate
- [Hoja de Docker](https://github.com/Ezra-Zhao/docker-cheatsheet) — 28 comandos de contenedores
- [Hoja de curl](https://github.com/Ezra-Zhao/curl-cheatsheet) — 29 comandos para depurar APIs
- [Hoja de expresiones regulares](https://github.com/Ezra-Zhao/regex-cheatsheet) — 30 patrones prácticos
- [Hoja de jq](https://github.com/Ezra-Zhao/jq-cheatsheet) — 30 filtros para JSON
- [Hoja de superpoderes de terminal](https://github.com/Ezra-Zhao/terminal-superpowers-cheatsheet) — 14 herramientas de productividad
- [Hoja de kubectl](https://github.com/Ezra-Zhao/kubectl-cheatsheet) — comandos habituales de Kubernetes
- [Hoja de SQL](https://github.com/Ezra-Zhao/sql-cheatsheet) — consultas y funciones habituales
- [Hoja de Vim](https://github.com/Ezra-Zhao/vim-cheatsheet) — 30 comandos de edición, búsqueda y macros
- [Hoja de tmux](https://github.com/Ezra-Zhao/tmux-cheatsheet) — 30 comandos de sesiones, ventanas y paneles
- [Hoja de systemd](https://github.com/Ezra-Zhao/systemd-cheatsheet) — 30 comandos de servicios y registros

## Licencia

MIT © 2026 Guangyi Zhao. Véase [LICENSE](LICENSE).
