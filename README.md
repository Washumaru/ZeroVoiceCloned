# ZeroVoiceCloned

Estudio local de clonación de voz. Separa una canción, cambia la voz de cada cantante
con RVC y devuelve la mezcla con las etiquetas y la carátula puestas. Todo en el equipo,
sin subir nada a internet.

## Puesta en marcha

```bash
instalar.bat
```

Crea los dos entornos de Python, instala la interfaz y comprueba que todo importa.
Necesita **Python 3.11** y **Node.js** en el PATH.

Después hacen falta los modelos, que no viajan con el proyecto:

```
models/<TuVoz>.pth          el modelo RVC
models/<TuVoz>.index        el índice (opcional, pero sin él el parecido baja)
models/dereverb/Reverb_HQ_By_FoxJoy.onnx
```

Para arrancar: ejecuta `launcher.pyw`, que levanta el backend y la interfaz y abre la
ventana. O a mano:

```bash
venv/Scripts/python.exe server.py          # backend en el 5000
npm --prefix soundwave-ui run dev          # interfaz en el 3000
```

## Estructura

| Carpeta | Qué hay |
|---|---|
| `server.py` | La API. Es grande; lo nuevo va en módulos aparte. |
| `vocal_*.py` | Una familia de efectos por archivo: espacio, color, afinación, envolvente, dicción. |
| `master_dsp.py` | Los mandos del Estudio sobre el máster: ecualizador, compresor, paneo. |
| `master_loudness.py` | Sonoridad (LUFS, BS.1770-4), pico real y limitador. Cierra la mezcla. |
| `f0_hibrido.py` | Repara el contorno de tono —octavas, huecos, temblor— antes de que RVC lo use. |
| `transiciones.py` | Las costuras entre cantantes: dónde se corta, cuánto dura el cruce y a qué nivel. |
| `dos_pasadas.py` | Reparte un tramo entre dos conversiones: una para vocales, otra para consonantes. |
| `alineacion.py` | Qué se canta y cuándo. El dato que consumen las costuras, la doble pasada y la letra. |
| `pronunciacion.py` | Curvas de `index_rate` y `protect` por fotograma: el índice manda en vocales y se aparta en consonantes. |
| `master_referencia.py` | Acerca el equilibrio tonal del máster al de una canción de referencia. |
| `registro_tramos.py` | Qué tramos piden notas fuera del registro que el modelo aprendió. |
| `recetas.py` | Juegos de mandos guardados con nombre. |
| `voice_*.py` | Adaptación al modelo, librería de voces y niveles entre cantantes. |
| `track_meta.py` | Formatos de exportación, etiquetas y carátula. |
| `stems_export.py` | Las pistas por separado, con nombre y en la proporción del disco. |
| `proyecto.py` | El `.zvc`: la sesión entera en un archivo. Los modelos no viajan dentro. |
| `soundwave-ui/` | La interfaz: React + Vite + Tailwind. |
| `models/` | Tus `.pth` y `.index`, más el ONNX de desreverberación. |
| `data/voices/` | La librería de voces guardadas. |
| `tests/` | Pruebas. Se ejecutan sin levantar el servidor. |
| `tools/` | Utilidades: generar el logo, el trabajador de desreverberación, migrar el proyecto. |
| `docs/superpowers/` | Diseños y planes de cada tanda de trabajo. |

## Comandos

```bash
venv/Scripts/python.exe -m unittest discover -s tests -t .    # pruebas de Python
node --test soundwave-ui/src/lib/*.test.js                     # pruebas de JS
venv/Scripts/python.exe -m PyInstaller --noconfirm launcher.spec
venv/Scripts/python.exe tools/generar_logo.py                  # regenerar iconos y carátula
```

## Dos cosas que sorprenden y no son errores


**Hay tres entornos virtuales.** El del proyecto necesita numpy 1.26 y torch 2.4.1+cpu
para Demucs y DirectML. `audio-separator`, que quita la reverb antes de clonar, exige
numpy 2.x y torch 2.13. Y `faster-whisper`, que saca la letra con sus tiempos, arrastra
`ctranslate2`, que también pide numpy 2.x. Ninguno de los tres cabe con los otros, así que
cada uno vive en el suyo y se les habla por línea de comandos:

| Entorno | Para qué | Frontera |
|---|---|---|
| `venv/` | Todo lo demás | — |
| `venv_dereverb/` | Quitar la sala antes de clonar | `tools/dereverb_worker.py` |
| `venv_alineacion/` | Letra y tiempos por palabra | `tools/alinear_worker.py` |

Los dos aislados son opcionales: sin ellos el programa arranca y funciona, sólo que sin
esas dos etapas. Se montan con `python -m venv <nombre>` y su `requirements-*.txt`.

**Nunca uses el Python del sistema.** Siempre `venv/Scripts/python.exe`. Y antes de
instalar cualquier cosa, comprueba con `pip install --dry-run` que no arrastra otra
versión de numpy o de torch: es la forma más rápida de dejar el proyecto sin arrancar.

## Reglas de la casa

- **Todo se aplica por déficit.** Se mide el original, se mide el clon, y solo se añade
  lo que falta. Aplicar el valor absoluto duplica el efecto, porque RVC ya arrastra
  parte de la expresión de la fuente.
- **Nada de emparejar el espectro bin a bin.** Destruye la identidad del clon. Medido.
- **La prueba antes que el código**, y verificando sobre canciones reales.
- La consola de Windows va en cp1252: sin flechas ni símbolos raros en los `print()`.
- `server.py` arranca sin recarga automática: hay que reiniciarlo a mano tras tocarlo.
