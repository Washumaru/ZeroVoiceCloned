# ZeroVoiceCloned

**Cambia la voz de cualquier canción, sin subir nada a internet.**

Separa la voz del instrumental, reconoce a cada intérprete y lo reemplaza por el modelo de
voz que elijas. Todo el procesamiento ocurre en tu computadora: ni una muestra de audio
sale de tu disco.

![versión](https://img.shields.io/badge/versión-1.0.0-red)
![sistema](https://img.shields.io/badge/Windows-10%20u%2011-lightgrey)
![prueba](https://img.shields.io/badge/pruébalo-gratis-brightgreen)

---

## Descarga

**[⬇ Descargar la versión de prueba](https://github.com/Washumaru/ZeroVoiceCloned/releases/latest)**
· 1,3 GB · Windows 10 u 11

Descomprime todo el ZIP en una carpeta tuya y abre `ZeroVoiceCloned.exe`. No hay
instalación, no descarga nada y funciona sin conexión: el programa trae dentro todo lo que
usa, incluidos los modelos de separación.

> **La primera vez, Windows mostrará el aviso «Windows protegió su PC».** Es lo que le sale
> a cualquier programa sin firma digital comprada; no dice que el archivo sea peligroso,
> dice que Windows todavía no lo ha visto lo suficiente. Pulsa **Más información →
> Ejecutar de todas formas**.
>
> Si prefieres comprobar antes que el archivo es el que se publicó aquí y nadie lo ha
> tocado, en [SHA256.txt](SHA256.txt) están las huellas y cómo verificarlas.

---

## Qué hace

| | |
|---|---|
| **Separa** | voz e instrumental de cualquier canción |
| **Reconoce** | quién canta en cada tramo, y reparte las voces por separado |
| **Clona** | sustituye cada voz por el modelo que elijas, con cuatro algoritmos de tono |
| **Ajusta** | afinación, dicción, formantes, espacio, respiraciones y mezcla |
| **Exporta** | audio con formato y etiquetas, o el vídeo con la voz nueva |
| **Dobla** | vídeo con avisos de entrada — *función en pruebas, lee más abajo* |

Todo local. Sin cuenta, sin suscripción, sin conexión obligatoria y sin límite mensual.

---

## La versión de prueba

Procesa **los primeros 30 segundos** de cada canción, con la **misma calidad** que la
versión completa: sin ruido añadido, sin marcas de agua y sin bajar el volumen. Escuchas
exactamente lo que obtendrías al comprar.

Esos 30 segundos no son un recorte del resultado: la canción se corta **antes** de
procesarla, así que el resto del tema no se separa ni se guarda en ningún momento.

No incluye: exportar la canción completa, exportar el vídeo, repartir varias voces en un
mismo tema ni montar doblajes.

**[Versión completa — 25 USD, pago único](https://washumaru.gumroad.com/l/ZeroVoiceCloned)**
· una clave, tres computadoras, actualizaciones incluidas.

---

## Antes de comprar, lee esto

**Este programa no está terminado, y no va a estarlo.** La separación de audio y la
clonación de voz cambian cada pocos meses; uno que se declarara acabado estaría empezando a
quedarse atrás.

Detrás hay **una sola persona**. Más de un año de trabajo, pero en el tiempo que dejan un
empleo y unos estudios: noches sueltas y fines de semana, con tramos enteros sin tocarlo.
Entre una versión y la siguiente puede pasar más tiempo del que parezca razonable, y no voy
a prometer fechas que después no pueda cumplir.

**El modo de doblaje no está terminado.** Funciona y se puede usar, pero no acumula todavía
pruebas suficientes para darlo por bueno en todos los casos: la sincronía puede desplazarse
o un tramo puede quedar mal repartido. Si vas a comprar principalmente por esa función,
tenlo en cuenta.

Por eso la prueba es gratis y sin límite de tiempo: no te pido que confíes en que funciona,
te pido que lo compruebes en tu equipo antes de pagar. En
[SOBRE-EL-PROYECTO.txt](SOBRE-EL-PROYECTO.txt) está el contexto completo.

---

## Cómo se comprueba lo que se afirma

Nada de la cadena de audio se cambia «a oído y listo». Hay **376 comprobaciones
automáticas** que se ejecutan antes de dar por bueno un cambio —300 sobre el motor de audio
y 76 sobre la interfaz— y la cobertura es más alta justo en las partes que más veces se
rompieron: la cobertura sigue a las cicatrices, no al tamaño del código.

---

## Requisitos

- Windows 10 u 11 y unos 8 GB de RAM
- Unos 3 GB de disco (el ZIP son 1,3 GB y se descomprime a algo menos de 3)
- **Tarjeta gráfica opcional.** Si la hay y es compatible con DirectX 12 —AMD, Intel o
  NVIDIA— la aceleración se activa sola por DirectML. La vía propia de NVIDIA (CUDA) no
  viaja dentro de esta versión: una NVIDIA funciona igual, por DirectML, pero no a la
  velocidad que daría CUDA.

Desarrollado y probado en un solo equipo, con tarjeta AMD. No hay probadores. Puede haber
fallos que solo aparezcan con una combinación de hardware que no tengo delante — si te
pasa, [cuéntalo](https://github.com/Washumaru/ZeroVoiceCloned/issues) con lo que hacías, la
duración y el formato de la canción, y las últimas líneas de la ventana negra.

---

## Uso responsable

Esta herramienta es para tu propia voz, para la de gente que te dio permiso, o para
material del que tengas derechos. No la uses para hacer pasar a nadie por quien no es, ni
para engañar, difamar o estafar, ni para saltarte los derechos de una canción. Lo que hagas
con ella es responsabilidad tuya. Las condiciones están en [TERMINOS.txt](TERMINOS.txt).

---

## Construido sobre trabajo de otros

Este programa no reinventa la separación de audio ni la conversión de voz: las usa. Se
apoya en proyectos de código abierto y en modelos publicados por sus autores, todos bajo
licencias que permiten este uso. Es de justicia decir cuáles:

- **[Demucs](https://github.com/adefossez/demucs)** — separación de fuentes. MIT.
- **[RVC](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI)** y
  `rvc-python` — conversión de voz. MIT.
- **[fairseq](https://github.com/facebookresearch/fairseq)** y **HuBERT** — representación
  del habla. MIT.
- **[RMVPE](https://github.com/Dream-High/RMVPE)** — extracción de tono.
- **[PyTorch](https://pytorch.org)**, **[FAISS](https://github.com/facebookresearch/faiss)**
  y **[librosa](https://librosa.org)** — la base de cálculo y análisis.

El trabajo propio está en lo que hay alrededor: la cadena de tratamiento de la voz, el
reparto por intérprete, el injerto de banda alta, la corrección de dicción, la interfaz y
que todo eso funcione junto en una computadora normal, sin configurar nada.

**Los créditos completos y los textos de licencia de los 112 componentes que viajan dentro
están en [CREDITOS-Y-LICENCIAS.txt](CREDITOS-Y-LICENCIAS.txt)**, aquí mismo y también
dentro del programa. Puedes revisarlos sin descargar nada.

---

## Historial

Lo que cambia en cada versión, en [CHANGELOG.md](CHANGELOG.md).

---

<sub>ZeroVoiceCloned · hecho por [Washumaru](https://github.com/Washumaru) · el código del
programa es cerrado; lo que se publica aquí son las descargas, las condiciones y los
créditos.</sub>
