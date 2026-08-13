# Qué hace por dentro

Esta es la lista larga: los sistemas que tiene ZeroVoiceCloned y **por qué existe cada
uno**. Casi todos nacieron de un problema concreto que se pudo medir, así que donde hay
números, son medidos.

Si solo quieres saber qué es el programa, el [README](README.md) es más corto.

---

## El recorrido de una canción

![La pantalla de Estudio](capturas/estudio.png)

Sueltas un archivo —o pegas un enlace, o grabas con el micrófono— y a partir de ahí:

1. **Separación** de voz e instrumental.
2. **Detección** de cuántas personas cantan y en qué tramos.
3. **Clonación** de cada voz con el modelo que elijas.
4. **Adaptación** del resultado a la canción original.
5. **Mezcla, máster y exportación.**

Los pasos 4 y 5 son donde está casi todo lo que sigue, y son los que diferencian «sustituir
una voz» de «que suene como si esa persona hubiera cantado la canción».

---

## Separación y reparto de voces

### Tres modelos de separación
`htdemucs_ft` (máxima calidad), `htdemucs` (más rápido) y `htdemucs_6s`, que además
separa guitarra y piano. Con tres perfiles de velocidad frente a precisión.

### Detección de cantantes sin internet
Agrupación por **K-Means sobre MFCC**: se mide el color espectral de cada tramo y se
agrupan los que pertenecen a la misma persona. No usa modelos pesados ni servicios
externos.

Dos detalles que costaron encontrarse:

- Se agrupa sobre **la voz ya aislada**, no sobre la mezcla. Con el instrumental delante,
  la guitarra y la batería contaminan la medición y aparecen «cantantes» que no existen.
- **No se fuerza el número de grupos hacia arriba.** Al intentar detectar más cantantes de
  los que hay, un mismo intérprete se parte en dos y la canción sale con dos voces
  distintas en el mismo verso.

---

## Multi-voz: duetos, coros y segundas voces

![La pantalla de IA Voz](capturas/ia-voz.png)

Esta es la parte más difícil del programa, y la que más veces se rehízo.

### Coros y segundas voces separados del cantante principal

Demucs devuelve **una sola pista de voces** con el cantante principal, las armonías, las
voces dobladas y el coro mezclados. Y RVC da por hecho que hay exactamente una voz: HuBERT
ve fonemas superpuestos y el detector de tono salta de un cantante a otro.

Por eso los estribillos con coro salían como un balbuceo, y **ningún parámetro de RVC lo
arreglaba** — no era un problema de ajuste, era un problema de entrada.

La solución es partir la pista antes de clonar, combinando dos pistas independientes y sin
depender de nada externo:

- **Máscara armónica sobre la melodía predominante.** Se busca el tono dominante de cada
  instante y se marca como principal lo que cae sobre ese tono y sus armónicos; lo demás,
  no. Funciona incluso en mono y con armonías centradas en la imagen.
- **Anchura estéreo.** Lo que no está centrado es acompañamiento. Solo se aplica si la
  pista tiene anchura real, para no estropear una grabación mono.

El corte está **deliberadamente sesgado hacia el cantante principal**, y la razón importa:
si se cuela un poco de coro en la voz principal, esa parte simplemente se convierte también
y no pasa nada. Si se cuela voz principal en el coro, el **cantante original queda audible
por debajo del clon** — el efecto de voz doblada que este sistema existe para evitar.

Las dos capas suman exactamente la original, muestra por muestra. El coro se devuelve **sin
tocar**: no se clona, se conserva.

### Duetos dentro del mismo verso

Cuando dos personas cantan **a la vez**, no se elige una: se marca ese tramo como dueto y se
pasan **dos modelos distintos sobre el mismo trozo de audio**, con un desfase de unos
**18 ms** entre ambos. Ese desfase no es un adorno — sin él las dos copias se suman en fase
y aparece filtrado en peine: un sonido metálico, hueco, como cantar dentro de un tubo.

El reparto se marca **por tramo y no por cantante**, que es lo que permite que la misma
persona use un modelo en la estrofa y otro en el estribillo.

Hay un control de **tolerancia de duetos simultáneos** para decidir cuándo dos voces cuentan
como cantando a la vez y cuándo son la misma persona con dos matices.

### Una línea de tiempo exclusiva
Dos cantantes no pueden ocupar el mismo instante por accidente. Cuando eso pasaba, las dos
voces clonadas se pisaban y salía un sonido grave y deformado —«voz de demonio»— que era
imposible de diagnosticar escuchando el resultado: sonaba a fallo del modelo y era un fallo
del reparto.

### Los relevos entre cantantes, sin costuras

Tres controles que existen porque cortar y pegar voces deja marcas audibles:

- **Fundido de relevo** (30 ms por defecto): el cruce entre un cantante y el siguiente.
  Sin él se oye el corte.
- **Contexto de arranque** (0,5 s): audio real que se le da a RVC **antes** de cada bloque
  para que el detector de tono llegue ya enganchado. Un bloque que empieza en frío empieza
  desafinado.
- **Mezcla de envolvente**: cuánto se conserva la dinámica del original frente a la que
  produce RVC.

---

---

## Clonación

![El motor de clonación](capturas/motor-clonacion.png)

### Cuatro algoritmos de tono
**RMVPE** (el mejor: cero desafinación en agudos y falsetes), **Crepe** (excelente en canto
muy limpio, más pesado), **Harvest** (precisión media, el tradicional de RVC) y **PM**
(ultrarrápido).

### Transporte automático al registro del modelo
Un modelo entrenado con una voz grave no puede cantar un tema agudo por mucho que se le
pida: no vio esas notas al entrenarse. El programa mide el registro real del modelo y el de
la canción, y propone el transporte en semitonos que la mete dentro de lo que el modelo
sabe hacer.

### Reconstrucción del índice perdido
Si tienes el `.pth` de un modelo pero perdiste su `.index`, **se puede reconstruir**, y no
hay que reentrenar nada.

El índice no son pesos aprendidos: es un almacén de vectores HuBERT que la inferencia
consulta por vecino más cercano. Se puede recalcular en cualquier momento **a partir de
cualquier grabación de esa persona**.

Y la grabación no tiene que ser cantando: una nota de voz, una llamada o un audio de
WhatsApp sirven. El índice fija **timbre**, no estilo — guarda cómo el tracto vocal de esa
persona colorea el sonido, no qué notas alcanza. Por el mismo motivo da igual que la
grabación suene a teléfono: HuBERT ignora la ecualización de la grabación.

---

## Vocal Enhance y Modo Estudio

Dos interruptores en la pantalla de clonación que encienden, cada uno, una cadena entera.

### Vocal Enhance — «copia la canción original sobre tu voz clonada»

La voz clonada sale limpia y seca, como grabada en una cabina. La canción está en un
espacio, con su eco y su color. Poner una encima de la otra suena exactamente a lo que es:
dos grabaciones distintas pegadas.

Vocal Enhance mide el vocal original y se lo aplica al clon: **la sala, el eco y el color**.
Y no es todo o nada — hay un control de **cuánto copiar**, de 0 a 100 %, porque una canción
muy procesada puede querer menos y una balada seca puede querer todo.

Por debajo son varios de los sistemas de la siguiente sección funcionando juntos: sala,
familia tonal, ataque de frases y autotune.

### Modo Estudio Profesional y efectos DSP

La cadena que se aplica **sobre cada voz clonada por separado**, no sobre la mezcla final:
filtro de graves, de-esser, compresión y limpieza. Es la diferencia entre una voz que se
sienta en la mezcla y una que flota encima.

Aparte va la **reverb del bus**, que se aplica al conjunto y no a cada voz — mandar dos
cantantes a la misma sala es lo que hace que suenen en el mismo sitio.

---

## Lo que hace que suene como la canción, no como un pegote

Aquí está la parte que no se ve en ningún botón y es la que más trabajo lleva detrás. Es
también lo que enciende Vocal Enhance.

### Injerto de banda alta

El problema, medido el 29 de julio de 2026 convirtiendo el mismo fragmento con cuatro
modelos distintos:

| | Banda 8–12 kHz | Techo |
|---|---|---|
| Vocal original (objetivo) | −12,3 dB | 15 972 Hz |
| Modelo A | −37,1 dB | 16 000 Hz |
| Modelo B | −36,9 dB | 15 455 Hz |
| Modelo C | −56,3 dB | 9 000 Hz |
| Modelo D | −69,1 dB | 7 274 Hz |

Esa banda es donde vive el «sonido de micrófono profesional»: el aire, la respiración y la
sibilancia. Sin ella la voz suena apagada y afónica **aunque el volumen sea correcto** — y
lo era, +0,2 dB respecto al original. Por eso ningún fader arreglaba el problema, y por eso
costó tanto entender qué fallaba.

La causa: dos de esos modelos se entrenaron con audio de banda limitada —grabaciones a
16 kHz, notas de voz, llamadas, capturas de Discord— y su decodificador **nunca aprendió a
producir esas frecuencias**. Ningún parámetro de inferencia las puede recuperar.

Lo que sí se puede hacer es **tomarlas prestadas del vocal original de la canción**, que sí
las tiene. Eso es el injerto: se cruza por encima de los ~7 kHz y se pega el aire del
original sobre el clon. Recupera hasta **+43 dB** en esa banda sin traer nada de la
identidad del cantante original, porque a esas frecuencias ya no hay información de quién
canta, solo textura.

### Copiar la sala del original
La voz clonada sale seca de estudio y la canción está en un espacio concreto. Se mide la
reverberación del vocal original y se aplica sobre el clon, para que no suene pegado encima
de la mezcla.

### Familia tonal
Se copia el **color** del vocal original: dónde tiene el cuerpo, dónde la presencia, cómo
está repartida la energía. Sin esto, dos voces clonadas de canciones distintas suenan
iguales entre sí y distintas a su canción.

### Desreverberación previa
Si el vocal original viene con mucha sala, esa sala entra en la clonación y se acumula. Se
quita antes de clonar, y después se devuelve la cantidad medida.

### Ataque de las frases
El clon tiende a suavizar los comienzos de frase. Se le devuelve el ataque del original
para que las entradas no suenen desganadas.

### Detección de autotune
Se mide **cuánta corrección de tono lleva el vocal original** para replicarla. Una canción
con autotune marcado y un clon perfectamente natural encima suenan a dos producciones
distintas pegadas.

### Dicción y consonantes
Busca **dónde el clon no dice bien lo que dice el original** — consonantes que se comen,
sílabas que se pierden — y lo corrige por tramos, en lugar de aplicar un ajuste global.

### Protección de respiraciones y corrección de formantes
Las respiraciones se mantienen naturales en lugar de convertirse en ruido sintético, y el
tracto vocal se ajusta al cambiar de registro hombre ↔ mujer para que no aparezca el efecto
ardilla.

### Volumen por cantante
Cada voz clonada se nivela por separado contra el original. En una canción con dos
intérpretes, uno puede quedar tapado si se aplica una sola ganancia a todo.

---

## Estudio, mezcla y edición

- **Editor multipista**: silenciar tramos, ajustar las regiones donde se aplica cada efecto
  y comparar versiones sin salir de la sesión.
- **Efectos por tramos**: los efectos no son globales; se marcan sobre el mapa de la
  canción.
- **Prueba de 8 segundos** antes de procesar el tema entero. Es lo que evita esperar
  minutos para descubrir que el modelo elegido no era el bueno.
- **Metadatos**: carátula, título y qué modelos se usaron, escritos en el archivo
  exportado.
- **Exportación** en MP3, M4A, FLAC y WAV.

---

## Vídeo y doblaje

- **Vídeo 1:1**: entra un vídeo, sale el mismo vídeo con la voz clonada dentro. Sin
  recodificar la imagen.
- **Doblaje**: el vídeo corre sin la voz original y avisa **tres segundos antes** de cada
  intervención, indicando a quién le toca. Grabas leyendo la pantalla.

> **El doblaje no está terminado.** Funciona, pero no acumula todavía pruebas suficientes
> para darlo por bueno en todos los casos. Está marcado como tal dentro del programa.

*Ambas funciones son de la versión completa: no viajan dentro del archivo de prueba.*

---

## Cosas que no se ven pero se notan

- **Aceleración por tarjeta gráfica** con DirectML: AMD, Intel y también NVIDIA, sin
  instalar ni configurar nada.
- **Importación por enlace** de YouTube, o grabación directa con el micrófono.
- **Tiempo restante real**, calculado a partir de lo que va tardando, no una barra que
  avanza inventando.
- **Vigilancia de memoria**: el programa anota cuánta lleva gastada, para que un cierre
  inesperado deje una explicación en vez de un misterio.
- **La sesión sobrevive al navegador**: el motor vive aparte; si cierras la pestaña a mitad
  de un proceso, al volver está todo donde lo dejaste.
- **Todo local**: ni una muestra de audio sale de tu disco. La única conexión que el
  programa abre por su cuenta es la comprobación de versión nueva, y se puede apagar.

---

## Cómo se comprueba todo esto

**376 comprobaciones automáticas** —300 sobre el motor de audio y 76 sobre la interfaz— que
se ejecutan antes de dar por bueno un cambio. La cobertura es más alta justo en las partes
que más veces se rompieron: la cobertura sigue a las cicatrices, no al tamaño del código.

Ninguna medición de las de arriba es una impresión. Cada una salió de comparar números
antes y después, y por eso se pueden escribir aquí con decimales.
