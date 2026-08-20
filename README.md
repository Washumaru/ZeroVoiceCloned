# ZeroVoiceCloned

**Cambia la voz de cualquier canción, sin subir nada a internet.**

Separa la voz del instrumental, reconoce a cada intérprete y lo reemplaza por el modelo de
voz que elijas. Todo el procesamiento ocurre en tu computadora: ni una muestra de audio
sale de tu disco.

![versión](https://img.shields.io/badge/versión-1.0.0-red)
![sistema](https://img.shields.io/badge/Windows-10%20u%2011-lightgrey)
![prueba](https://img.shields.io/badge/pruébalo-gratis-brightgreen)
![tienda](https://img.shields.io/badge/versión%20completa-25%20USD-blue)

---

## Descarga

**[⬇ Descargar la versión de prueba](https://github.com/Washumaru/ZeroVoiceCloned/releases/latest)**
· 1,3 GB · Windows 10 u 11

**[🛒 Comprar la versión completa — 25 USD, pago único](https://washumaru.gumroad.com/l/ZeroVoiceCloned)**
· ya a la venta

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

## Míralo funcionando

**[▶ Ver la demostración del sistema multi-voz](capturas/multivoz-demo.mp4)** — una canción
con dos cantantes, cada uno sustituido por un modelo distinto.

Conviene saber con qué está hecha, porque es lo que hace la prueba interesante:

**Las dos voces son modelos mal entrenados.** No es una demostración con material de
laboratorio: son de los que recuperé, entrenados con poco tiempo y con audio mediocre — de
los que suenan apagados y deshilachados en cuanto suben. Suenan bien **por lo que el
programa hace con ellos**, no porque los modelos sean buenos.

Y esto se puede poner en números. Los dos peores modelos de este proyecto, medidos antes y
después de pasar por la cadena:

| | Techo de frecuencia | Banda 8–12 kHz |
|---|---|---|
| Modelo A, crudo | 7 276 Hz | −69,0 dB |
| Modelo A, procesado | **15 914 Hz** | **−26,1 dB** |
| Modelo B, crudo | 8 376 Hz | −56,3 dB |
| Modelo B, procesado | **15 920 Hz** | **−26,6 dB** |

Un modelo que se apagaba a los 7 276 Hz —sordo de fábrica, incapaz de producir el aire de
una voz— sale por encima de los 15 900. Son **+43 dB** recuperados en la banda donde vive
el brillo. Y no es una mejora que haya que buscar con lupa: **a oído la diferencia es
enorme**, de voz apagada y tapada a voz que se sostiene delante de la mezcla.

De eso se encarga **Vocal Enhance**: le presta la banda alta que el modelo no sabe
producir, le copia la sala y el color de la canción, le devuelve el ataque de las frases,
le quita el ruido que mete en las notas altas y le tapa los huecos donde se queda mudo.
Todo eso es un interruptor: no hay que entender nada de lo anterior para usarlo.

Y ese es el objetivo al que sigo apuntando — **que alguien sin ni idea de edición musical
pueda hacer su música**. Cada mejora se mide contra ese listón, no contra lo que suena bien
en manos de quien ya sabe mezclar.

> **Sobre la canción de la demostración.** Es un tema conocido y con derechos de autor, y
> se usa aquí **como prueba real**, no como producto: una canción comercial de verdad, con
> su mezcla, sus coros y sus dos voces, que es lo único que demuestra que esto funciona
> fuera de un caso preparado. No se distribuye la canción ni se reclama ningún derecho
> sobre ella, y todos los derechos son de sus autores. Las voces que se ponen encima son
> mías.

---

## Qué hace

| | |
|---|---|
| **Separa** | voz e instrumental de cualquier canción |
| **Reconoce** | quién canta en cada tramo, y reparte las voces por separado |
| **Clona** | sustituye cada voz por el modelo que elijas, con cuatro algoritmos de tono |
| **Ajusta** | afinación, dicción, espacio, respiraciones y mezcla |
| **Transcribe** | la letra con sus tiempos, y con ella coloca los relevos y las consonantes |
| **Masteriza** | al volumen del disco original, o al de la plataforma que elijas |
| **Exporta** | audio con etiquetas, las pistas por separado, el vídeo, o la letra en `.lrc` |
| **Guarda** | el proyecto entero en un archivo, y los mandos como recetas reutilizables |
| **Importa** | por archivo o por enlace de YouTube, SoundCloud, Bandcamp y Suno |
| **Edita** | un carril por instrumento, con zoom, recortes copiables y escucha por pista |
| **Dobla** | vídeo con avisos de entrada, voz por personaje y nota por toma — *en pruebas* |

Todo local. Sin cuenta, sin suscripción, sin conexión obligatoria y sin límite mensual.

**[→ La lista larga: qué hace por dentro y por qué](CARACTERISTICAS.md)** — el injerto de
banda alta, la separación de coros, los duetos dentro del mismo verso, la reparación del
contorno de tono, los relevos entre cantantes, la letra con sus tiempos, el índice que
distingue una ese de una vocal, el máster y la reconstrucción del índice perdido, con las
mediciones que los motivaron.

---

## La versión de prueba

**Es el programa entero**, no una demostración recortada. Procesa **los primeros 30
segundos** de cada canción con la **misma calidad** que la versión completa: sin ruido
añadido, sin marcas de agua y sin bajar el volumen. Escuchas exactamente lo que obtendrías
al comprar.

Esos 30 segundos no son un recorte del resultado: la canción se corta **antes** de
procesarla, así que el resto del tema no se separa ni se guarda en ningún momento.

No incluye: exportar la canción completa, exportar el vídeo, repartir varias voces en un
mismo tema ni montar doblajes.

### Verás errores en rojo en la consola, y son normales

La versión de prueba **no incluye** las partes del programa que se encargan del vídeo y
del doblaje. No están capadas: no viajan dentro del archivo, y por eso ocupa menos.

Como consecuencia, al procesar una canción la ventana negra puede escupir varias líneas
rojas —`ModuleNotFoundError`, `500 Internal Server Error`— cuando la pantalla consulta por
esas funciones. **No afectan al resultado.** Fíjate en que justo debajo aparece
`Separación completada con éxito`: el trabajo termina igual.

Preferiría que en lugar de ese error dijera «esto está en la versión completa», y lo dirá.
Mientras tanto, la regla para saber si algo va mal de verdad es simple:

- **Rojo y el proceso termina** → es esto, ignóralo.
- **Rojo y el proceso se detiene** → eso sí es un fallo.
  [Cuéntamelo](https://github.com/Washumaru/ZeroVoiceCloned/issues) con lo que hacías, la
  duración y el formato de la canción, y las últimas líneas de la ventana.

En la versión completa no aparecen: esas partes sí están dentro.

---

## La versión completa

**[Comprar — 25 USD, pago único](https://washumaru.gumroad.com/l/ZeroVoiceCloned)** · una
clave, tres computadoras, actualizaciones incluidas. Sin suscripción y sin caducidad.

Quita el límite de 30 segundos y añade lo que la prueba no trae: exportar la canción
completa, exportar el vídeo con la voz nueva, repartir varias voces en un mismo tema —con
sus duetos por tramo— y el modo de doblaje.

Al comprar recibes la clave en el propio recibo. Se pega en **Ajustes**, dentro del
programa, y solo hace falta conexión en ese momento: después funciona sin internet, sin
cuenta y sin caducidad. Reinstalar o formatear la misma computadora no gasta cupo; el
detalle está en [LICENCIA-Y-ACTIVACION.md](LICENCIA-Y-ACTIVACION.md).

No hay que volver a descargar nada distinto: es el mismo programa, con las partes de vídeo
y doblaje incluidas.

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

**El autotune todavía no queda como en el original, y llevo desde el principio con eso.**
El programa mide el tratamiento de tono de la voz original y se lo aplica al clon — sin
eso, una voz clonada natural encima de una producción muy corregida suena a dos grabaciones
distintas pegadas.

**Durante mucho tiempo esto no llegaba a igualar al original, y aquí decía exactamente
eso.** El motivo resultó ser que se estaba midiendo una sola cosa de las tres que hacen
falta: cuánto se corrige, pero no con qué rapidez entra la corrección ni hacia qué notas.
La segunda es la que importaba — dos voces pueden acabar igual de pegadas a la nota y sonar
completamente distintas según si el tono salta de golpe al empezar cada nota o se desliza
hasta ella. El clon acababa igual de afinado y conservaba sus propios deslizamientos
naturales encima de una producción que saltaba de golpe.

Las tres se miden ahora, y está detallado en
[qué hace por dentro](CARACTERISTICAS.md). **No lo doy por cerrado**: la medida de
velocidad tiene un suelo por debajo del cual no distingue, y la de la escala solo se puede
aplicar en producciones ya muy corregidas — en canto natural no hay forma fiable de sacarla
de un vocal aislado, así que ahí se dice que no se sabe en vez de adivinar. Sigue siendo la
parte del programa que más veces he tocado, y cada intento se mide contra el original.

**Y con un modelo bien entrenado, esa etapa sobra.** Un modelo aprende exactamente lo que
se le dio: si se entrenó con la persona **cantando** y con el autotune ya puesto, esa
corrección forma parte de la voz que produce — sale afinada a máquina sola, sin pedírselo.

En ese caso, **deja el autotune de Vocal Enhance apagado**. Aplicarle corrección a una voz
que ya viene corregida no la mejora: la afina dos veces, y eso sí se oye — metálica, con
los saltos de nota marcados, ese sonido de exceso que delata el proceso.

Y se puede, porque **Vocal Enhance no es todo o nada**. Cada parte lleva su propio
interruptor —copiar la sala, copiar el color tonal, copiar el autotune— más un control de
cuánto se copia en general. Puedes apagar solo el autotune y quedarte con lo demás, bajar
la intensidad, o **apagar Vocal Enhance entero** y quedarte con la conversión limpia. Si tu
modelo ya trae puesto lo que esa etapa iba a añadir, esa es la opción correcta.

Dicho en corto: la corrección de tono existe para los modelos que **no** la traen. Si el
tuyo la trae, el programa estorba menos cuanto menos toca. Y de paso, ahí está la razón por
la que un modelo bien entrenado vale más que cualquier ajuste: no hay parámetro que
sustituya a un modelo que aprendió a cantar.

Por eso la prueba es gratis y sin límite de tiempo: no te pido que confíes en que funciona,
te pido que lo compruebes en tu equipo antes de pagar. En
[SOBRE-EL-PROYECTO.txt](SOBRE-EL-PROYECTO.txt) está el contexto completo.

---

## Control real, no un botón de «hazlo bonito»

**El motor de tono se elige por separado, y puede ser distinto para cada modelo.** Hay
cuatro —RMVPE, Crepe, Harvest y PM— y no es una preferencia estética: un modelo entrenado
con voz muy limpia rinde con Crepe, y otro con material sucio aguanta mejor con RMVPE.
Como se asigna por voz, en una canción con dos cantantes cada uno puede ir con el suyo.

**El tono se calcula solo, si el modelo tiene su índice.** El programa mide el registro
real del modelo y el de la canción, y propone el transporte en semitonos que mete el tema
dentro de lo que ese modelo sabe cantar. Un modelo grave no puede hacer un tema agudo por
mucho que se le insista: nunca vio esas notas.

Y aquí está la ventaja de **crear el índice dentro del programa**: ese cálculo necesita el
`.index`, y muchos modelos que circulan por ahí lo han perdido. En vez de dejarte sin la
función, el programa lo reconstruye desde cualquier grabación de esa persona —vale una nota
de voz— y a partir de ahí el tono automático funciona. No hay que reentrenar nada ni buscar
el archivo original.

**Y para juzgar el resultado hay un comparador A/B**: la voz original y la clonada en el
mismo segundo, cambiando de una a otra al instante, o **sonando las dos a la vez**. Ese
tercer modo enseña lo que alternando no se oye — si el clon entra tarde, si se queda corto
en una frase o si se apaga en una nota alta.

---

## Cuando algo falla, te dice qué pasó

La mayoría de los programas de este tipo fallan de una de dos formas: se caen enteros, o
devuelven algo raro sin decir por qué. Aquí hay tres capas para que eso no ocurra.

**Avisa antes de gastar tu tiempo.** Antes de ponerse a procesar comprueba lo evidente —que
el archivo se pueda abrir, que tenga audio, que dure lo suficiente— y lo dice en el momento,
no después de diez minutos de trabajo.

**Un fallo no tumba la canción.** Si un cantante falla al clonarse, esa parte sale con la
voz original y **el resto del tema se termina igual**. Pierdes una voz, no la sesión entera.

**Y te dice exactamente qué pasó.** Al terminar aparece un aviso, *«Cantantes que salieron
SIN clonar»*, con el motivo de cada uno: si le faltaba modelo asignado, o si fue un error
del motor — y en ese caso, hasta el punto exacto del programa donde ocurrió.

Hay además un botón que **copia todos los detalles al portapapeles** para que puedas
pegarlos al pedir ayuda. Antes ponía «mira la consola del programa», y casi nadie sabe qué
es la consola ni dónde está: el fallo se quedaba sin diagnosticar por no poder señalarlo.

---

## Con qué se ha probado

No solo con canciones cómodas. El programa se ha ido puliendo contra los casos que lo
rompían:

- **Canciones en varios idiomas.**
- **Ocho cantantes en un mismo tema**, repartidos cada uno por su cuenta.
- **Hombres y mujeres en la misma canción**, que obliga a que cada voz lleve su propio
  transporte y su propio modelo.
- **Voces muy agudas y voces muy graves**, que son los dos extremos donde los modelos se
  rompen de formas distintas.

**Los cubre todos, y ninguno al 100 %.** Falta trabajo y quedan mejoras pendientes en cada
uno de esos frentes. Pero ya no son casos que tumben el resultado: se resuelven, y suenan
bien. Pueden sonar mejor, y en eso sigo.

---

## Cuando un enlace falla

Se aceptan enlaces de **YouTube, SoundCloud, Bandcamp y Suno**. La lista es cerrada a
propósito: la herramienta que hay debajo soporta más de mil setecientos sitios, y sin esa
puerta el campo de texto se convierte en un descargador de cualquier cosa de internet
desde tu propio ordenador.

Si pegas un enlace y dice que no se pudo descargar, **el problema no suele estar en el
programa**. YouTube en particular cambia con frecuencia cómo sirve el audio y bloquea las
descargas según la conexión, el vídeo o el momento del día.

**No hay una solución de verdad.** Lo que funciona es volver a intentarlo —a veces al
segundo o tercer intento entra— o descargar el audio por tu cuenta y subirlo como archivo,
que da exactamente el mismo resultado.

Lo digo aquí y no en la letra pequeña porque es el fallo con el que más gente se va a
encontrar, y porque no quiero que nadie concluya que el programa está roto por algo que
pasa del otro lado.

---

## Cómo se comprueba lo que se afirma

Nada de la cadena de audio se cambia «a oído y listo». Hay **más de mil comprobaciones
automáticas** que se ejecutan antes de dar por bueno un cambio —951 sobre el motor de audio
y 115 sobre la interfaz, 1.066 en total— y la cobertura es más alta justo en las partes que más veces se
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

## Documentación

| | |
|---|---|
| [Qué hace por dentro](CARACTERISTICAS.md) | La lista larga de sistemas y por qué existe cada uno |
| [Cómo se trabaja en esto](COMO-TRABAJO.md) | Cómo se mide cada cambio, y por qué se publica con el doblaje sin terminar |
| [Ayuda y problemas frecuentes](AYUDA.md) | De dónde salen los modelos de voz, el aviso de Windows, por qué suena apagado, qué hacer si algo falla |
| [La clave: cómo funciona](LICENCIA-Y-ACTIVACION.md) | Qué gasta cupo y qué no, cambiar de computadora, formatear |
| [Sobre el proyecto](SOBRE-EL-PROYECTO.txt) | Quién hay detrás, cómo se trabaja, por qué se vende |
| [Créditos y licencias](CREDITOS-Y-LICENCIAS.txt) | Los 112 componentes que viajan dentro |
| [Condiciones de uso](TERMINOS.txt) | Términos, incluida la política de devoluciones |
| [Historial de versiones](CHANGELOG.md) | Qué cambia en cada versión |

---

<sub>ZeroVoiceCloned · hecho por [Washumaru](https://github.com/Washumaru) · el código del
programa es cerrado; lo que se publica aquí son las descargas, las condiciones y los
créditos.</sub>
