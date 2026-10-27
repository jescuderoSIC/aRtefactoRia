# aRtefactoRia

Página web que graba el sonido de otra pestaña del navegador, lo guarda en formato Opus (`.opus`) y lo transcribe a texto con Whisper, todo en tu propio ordenador.

## Archivos

| Archivo | Qué hace |
|---|---|
| `grabadora-transcripcion.html` | Grabadora + transcripción. Necesita servidor local (ver abajo). |
| `grabadora.html` | Solo grabadora. Funciona abriéndola con doble clic. |
| `grabadora-video.html` | Graba vídeo de la pantalla, una pestaña o la cámara (MP4/WebM). Funciona abriéndola con doble clic. |
| `dictado-local.html` | Dictado por voz con el micrófono. Funciona con doble clic. |

## Requisitos

- **Chrome o Edge** de escritorio (Windows, Mac o Linux). Firefox y Safari no permiten capturar el audio de otras pestañas, y en el móvil no funciona.
- **Python 3**, solo para lanzar el servidor local de la versión con transcripción.
- **Conexión a internet** la primera vez que transcribas, para descargar el modelo. Después queda guardado en el navegador.

## 1. Comprobar que tienes Python

Abre una terminal:

- **Windows:** pulsa `Windows + R`, escribe `cmd` y pulsa Intro.
- **Mac:** abre la app *Terminal* (en Aplicaciones → Utilidades).
- **Linux:** abre tu terminal habitual.

Escribe:

```
python --version
```

Si sale algo como `Python 3.12.1`, ya lo tienes. Si da error, prueba con `python3 --version` (Mac y Linux) o `py --version` (Windows) y usa ese nombre en los pasos siguientes.

Si no tienes Python, descárgalo de https://www.python.org/downloads/. En Windows, durante la instalación marca la casilla **“Add Python to PATH”**.

## 2. Lanzar el servidor

1. Pon `grabadora-transcripcion.html` en una carpeta, por ejemplo `Documentos\grabadora`.
2. En la terminal, entra en esa carpeta:

   ```
   cd Documentos\grabadora
   ```

   En Mac y Linux las barras van al revés: `cd Documentos/grabadora`.

   Truco en Windows: abre la carpeta en el Explorador, escribe `cmd` en la barra de direcciones y pulsa Intro; se abre una terminal ya situada ahí.

3. Lanza el servidor:

   ```
   python -m http.server 8000
   ```

   (o `python3 -m http.server 8000` / `py -m http.server 8000`, según el paso 1).

   Verás un mensaje parecido a `Serving HTTP on :: port 8000`. Deja esa ventana abierta mientras uses la página.

## 3. Abrir la página

En Chrome o Edge, ve a:

```
http://localhost:8000/grabadora-transcripcion.html
```

Para que Windows no te pida permiso de red, puedes usar `python -m http.server 8000 --bind 127.0.0.1` y abrir `http://127.0.0.1:8000/grabadora-transcripcion.html`.

## 4. Parar el servidor

En la ventana de la terminal, pulsa `Ctrl + C`. Para volver a usar la página, repite los pasos 2 y 3.

## Cómo usar la grabadora

1. Pon el audio que quieras grabar en otra pestaña.
2. Pulsa el botón redondo de grabar.
3. En el diálogo del navegador elige **Pestaña de Chrome**, selecciona la pestaña y deja activado **“Compartir también el audio de la pestaña”**.
4. Para terminar, pulsa el botón de parar o “Dejar de compartir” en la barra del navegador.
5. En la grabación, pulsa **Descargar .opus** para guardarla donde quieras.
6. Elige modelo e idioma en el apartado *Transcripción* y pulsa **Transcribir**. Al terminar puedes copiar el texto o descargarlo como `.txt` o como subtítulos `.srt`.

Las grabaciones solo viven en la memoria de la página: descárgalas antes de cerrarla.

### Modelos de transcripción

| Opción | Velocidad | Precisión | Descarga |
|---|---|---|---|
| Muy rápido | Alta | Básica | Pequeña |
| Equilibrado | Media | Buena | Media |
| Preciso | Baja | Muy buena | Grande |

Si tu navegador tiene WebGPU disponible, la transcripción usa la tarjeta gráfica y va bastante más rápida. La página indica qué está usando.

## Problemas frecuentes

**“Address already in use” al lanzar el servidor.** El puerto 8000 está ocupado (quizá tienes otro servidor abierto). Usa otro número: `python -m http.server 8080` y abre `http://localhost:8080/grabadora-transcripcion.html`.

**La página dice que hay que abrirla desde un servidor.** La has abierto con doble clic (la dirección empieza por `file://`). Usa la dirección `http://localhost:8000/...` del paso 3.

**No se graba sonido.** En el diálogo de compartir hay que elegir una *pestaña* (no una ventana) y activar la opción de compartir audio.

**La transcripción tarda mucho.** Prueba con el modelo “Muy rápido” o “Equilibrado”. En ordenadores sin WebGPU, una grabación larga puede tardar más que su propia duración.

**La descarga del modelo falla.** Comprueba la conexión a internet y vuelve a pulsar Transcribir. Solo hace falta la primera vez para cada modelo.

## Extra: convertir grabaciones .webm antiguas a .opus

Con [ffmpeg](https://ffmpeg.org/download.html) instalado, sin pérdida de calidad:

```
ffmpeg -i grabacion.webm -c:a copy grabacion.opus
```
