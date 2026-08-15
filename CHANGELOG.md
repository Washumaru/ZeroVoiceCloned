# Registro de cambios

Lo que cambia en cada versión, en español y sin jerga. Lo que escribas aquí es lo que ve
quien recibe el aviso de actualización dentro del programa, así que se escribe pensando en
él y no en quien programó el cambio.

Formato: lo más reciente arriba. **Corregido** antes que **Nuevo**, a propósito — quien
lee esto casi siempre viene buscando si ya se arregló lo que le pasó a él.

---

## 1.0.1 — sin publicar todavía

### Corregido

- **La última palabra de cada voz salía cortada.** La cadena de limpieza se comía hasta
  46 milisegundos del final de todo lo que procesaba. En una canción de una sola voz eso
  es el final de la última frase; con varios cantantes, el final de **cada uno** de sus
  tramos. Como el hueco se rellenaba con silencio, no sonaba a error: sonaba a que el
  cantante había soltado la palabra antes de tiempo.

- **La voz clonada salía más alta de lo que estaba en el disco.** El programa mide la
  proporción entre voz y fondo del original para ponerle al clon la misma. Pero la medía
  sobre las dos pistas ya separadas, y esas se guardan igualadas de volumen cada una por
  su lado — que es lo que hace falta para escucharlas sueltas en el editor y justo lo que
  destruye la proporción que se quería copiar. En una canción con la voz picando a 0,20 y
  el fondo a 0,90, que es lo normal, la voz acababa cuatro veces por encima de donde el
  disco la tenía. Ahora se apunta cuánto se subió cada pista y la cuenta se deshace antes
  de medir.

- **Las colas de frase se cortaban de golpe y las entradas empezaban a medio volumen.** La
  puerta de ruido tenía dos fallos a la vez. Aplicaba su corrección **dos veces** sobre
  cada instante, porque trabaja con ventanas que se solapan a la mitad: donde tocaba bajar
  6 dB bajaba 12. Y sus tiempos de reacción estaban mal calculados: el ataque, que debía
  ser de 3 milésimas de segundo, tardaba en realidad **1,3 segundos** en abrir del todo.
  Por eso cada entrada de frase salía apagada y se recuperaba sola a mitad de palabra.

- **El mando de calidez metía aspereza metálica en vez de calor.** La saturación trabajaba
  a la resolución del archivo, así que los armónicos que ella misma genera por encima de
  los 7 kHz no cabían y volvían reflejados hacia abajo convertidos en ruido que no guarda
  relación con la nota. Es un defecto que se acumula con el volumen del mando. Ahora se
  satura al cuádruple de resolución y se filtra antes de volver: medido sobre un tono de
  8 kHz, queda **1 760 veces menos** de esa suciedad (−65 dB) y la voz no se mueve ni una
  décima de decibelio.

- **El realce de aire subía ruido en las voces que no llegan tan arriba.** Dos de los
  cuatro modelos del proyecto no producen nada por encima de 7,3 kHz. El realce fijo de la
  banda de 10–12 kHz se aplicaba igual, así que en esas voces no levantaba brillo:
  levantaba el ruido que el modelo deja donde debería haber voz — y la etapa siguiente
  tenía que quitar justo lo que la anterior acababa de amplificar. Ahora se mira primero
  si hay algo ahí, y se reordenó la cadena para que el aire se recupere del original
  **antes** de realzarlo.

- **Con varios cantantes, cada voz se comparaba contra la canción entera.** Las etapas que
  corrigen el color y rescatan el nivel comparan al clon con el original. En una canción
  repartida, el clon de un cantante está callado durante los tramos del otro, y el
  original no: la comparación mezclaba peras con manzanas. Se notaba en dos cosas
  concretas — la corrección de color se iba al tope y dejaba de ser un ajuste de matiz para
  convertirse en una subida plana, y cada cantante recibía un empujón de volumen justo al
  entrar y al salir de sus frases. Ahora cada uno se compara solo contra sus propios
  tramos.

- **El cambio de cantante caía a mitad de sílaba.** Cuando dos cantantes se solapaban en la
  línea de tiempo, el reparto cortaba por el punto medio exacto. Ese instante cae donde
  cae, y muy a menudo cae dentro de una palabra: la empezaba uno y la acababa otro. No hay
  fundido que arregle eso. Ahora el corte se mueve al punto más silencioso que haya a
  150 milésimas a la redonda, que es donde el cantante ya está soltando.

- **El que entraba podía sonar bastante más flojo o más fuerte que el que salía.** El cruce
  ya repartía bien el volumen, pero cruzaba dos grabaciones distintas, y dos clones del
  mismo verso no salen al mismo nivel. Medido sobre una canción real, el peor relevo tenía
  un escalón de **6 dB**. Ahora los dos lados se igualan en el instante del cambio y la
  corrección se desvanece medio segundo después, así que la estrofa sigue siendo más floja
  que el estribillo.

- **El «Radio de filtro» no hacía nada.** El mando llegaba hasta el motor y el motor lo
  ignoraba salvo con un algoritmo de tono concreto — y ni siquiera entonces usaba el valor
  elegido. Se movía y no cambiaba ni una muestra del resultado. Ahora suaviza de verdad el
  temblor del tono, con el radio que se le ponga.

- **Menos voz rota y soplada en las notas altas.** Fuera del registro que vio al
  entrenarse, el modelo deja de producir voz con tono y produce ruido. Medido sobre una
  canción real, en la zona aguda había **más ruido que armónicos** (−1,27 dB de relación
  armónicos/ruido, cuando el original tenía +5,16). Es lo que se oye como voz deshilachada
  o susurrada al subir, y es el defecto típico de un modelo entrenado con poco material.

  Ahora se atenúa el ruido que queda **entre** los armónicos, solo en los pasajes altos y
  solo donde de verdad se ha desplomado. Resultado: de −1,27 a **+1,86 dB**. Los graves no
  se tocan (5,59 → 5,58), porque ahí el clon ya estaba sano.

  No inventa armónicos a propósito: fabricar la voz que el modelo no produjo es lo que
  acaba sonando a robot. Solo se quita lo que no era voz.

- **La voz clonada ya no desaparece en los pasajes más agudos.** En las notas más altas
  el modelo se sale del registro que vio al entrenarse y deja de producir voz: se oía
  cómo la voz se apagaba a mitad de una frase mientras el instrumental seguía. Medido
  sobre una canción real de 211 segundos, el peor tramo perdía **21,7 dB durante 1,2
  segundos seguidos**.

  Ahora se le devuelve el nivel que el modelo no supo generar, solo donde el original
  canta y solo hasta un tope. Sobre esa misma canción: los tramos afectados pasan de 22
  a 3, y el peor de −21,7 a −12,1 dB.

  Comprobado que no estropea nada más: el aire y la aspereza quedan igual, y el temblor
  de volumen baja ligeramente (7,31 % → 7,20 %).

### Nuevo

- **La canción termina al volumen de la canción original, y ya no distorsiona en el MP3.**
  Hasta ahora lo único que protegía la mezcla final era bajarla si algún pico se pasaba.
  Eso deja dos agujeros. El primero: el pico no dice nada de lo fuerte que suena algo, así
  que la versión clonada salía más floja —o más fuerte— que la canción de la que venía, y
  había que tocar el volumen al cambiar de una a otra. El segundo, peor: entre dos
  muestras la señal sube más de lo que marca ninguna de las dos, así que un archivo que
  parece limpio puede recortar al reproducirse. Es distorsión que **no está en el WAV y sí
  en el MP3** que uno descarga.

  Ahora se mide la sonoridad real de la canción original y se lleva la mezcla ahí, y se
  deja un margen de seguridad de 1 dB contra los picos entre muestras, con un limitador
  que baja solo donde hace falta en vez de bajar la canción entera por culpa de un golpe
  de caja. Sobre una mezcla real: de −21,3 a −14,8 en volumen percibido, y de −1,7 a
  −1,0 de pico real.

  El objetivo es **la propia canción**, no el número que pide un servicio de streaming:
  igualar todo a −14 cambiaría el carácter de un disco masterizado a −8 a propósito. Y si
  llegar hasta ahí obligara al limitador a trabajar más de 6 dB, se sube menos y se acepta
  quedarse por debajo: una canción algo más floja se arregla con el mando del volumen, y
  una aplastada no se arregla con nada.

- **El tono se repara antes de clonar, no después.** El motor decide qué nota canta el clon
  a partir del tono que detecta en el original, y ese detector se equivoca siempre de las
  mismas dos maneras: salta una octava durante unas décimas —lo que se oye como un gallo o
  un tartamudeo metálico— y se queda mudo a mitad de nota, que es lo que hace que la voz
  se apague en los agudos mientras el instrumental sigue. Hasta ahora los dos defectos se
  tapaban después, sobre el audio ya generado. Ahora se arreglan antes: sobre 25 segundos
  de una canción real, 5 instantes devueltos a su octava y 47 recuperados de quedarse
  mudos.

  Un salto de octava que el cantante hace **de verdad** no se toca: la corrección se
  propone mirando el entorno y se confirma mirando el espectro del audio en ese instante,
  así que gana lo que de verdad está sonando. Y de propina la clonación va más rápida,
  porque el motor ya no tiene que buscar el tono por su cuenta.

- **La voz al centro y la sala abierta.** La voz salía idéntica en los dos altavoces contra
  un instrumental que sí es estéreo, y eso se oye como una voz pegada al frente, delante
  de la canción en lugar de dentro de ella. Ahora la voz sigue en el centro —que es donde
  va en cualquier disco— y lo que se abre es la reverberación que la lleva. No se copia el
  estéreo del vocal original a propósito: ahí dentro está la voz del cantante original, y
  meterla en los lados sería colar a la persona equivocada en la mezcla.

- **Quitarle la sala a la voz antes de clonarla** (en Ajustes, encendido). El motor sabe
  imitar una voz, no una habitación: con la reverb de la grabación pegada intenta convertir
  las dos cosas a la vez, y de ahí salen las colas metálicas y las palabras borrosas. Se le
  da la voz seca para que la analice y **la sala se le devuelve después**, medida del
  original. Lo que sale por el altavoz no queda más seco: queda más limpio. Sobre una toma
  que ya venía seca no se toca nada.

- **Consonantes en una pasada aparte** (en Ajustes, apagado). Dos ajustes del motor tiran
  en direcciones opuestas: los valores que dejan las vocales con el timbre de la persona
  clonada son los que dejan las eses y las tes blandas o metálicas, y al revés. Con esto se
  convierte cada tramo dos veces, una afinada para cada cosa, y se coge de cada una la
  parte en la que es mejor. **Tarda el doble**, y por eso viene apagado: es para cuando la
  voz ya gusta y lo único que falla son las consonantes.

- **Escuchar la comparación con el instrumental.** Un interruptor en el comparador añade
  la pista de fondo, para oír cómo queda la voz dentro de la canción en vez de aislada.
  Entra y sale sin parar lo que está sonando. Viene apagado: con la música encima cuesta
  más notar las diferencias de timbre, que es para lo que sirve comparar.

- **Los enlaces del proyecto, dentro del programa.** En Ajustes aparecen la documentación,
  la página de compra y la biblioteca de descargas de quien ya compró. Antes había que
  acordarse de una dirección web.

- **Escuchar el original y el clon a la vez.** En el comparador hay un tercer botón,
  «Las dos», que suena las dos pistas en el mismo segundo en lugar de alternar entre
  ellas. Alternando se juzga el timbre; sonando juntas salta al instante lo que el oído no
  recuerda: si el clon entra tarde, si se queda corto en una frase o si se apaga en una
  nota alta. El clon suena tal cual quedó, con la configuración con la que se clonó.

### Cambiado

- **Las letras del programa son más grandes y se leen mejor.** Casi todo el texto corría
  entre 11 y 12 píxeles, y las descripciones que explican para qué sirve cada mando eran
  precisamente lo más pequeño y lo más apagado de la pantalla. Ahora el cuerpo normal está
  en 14, ningún texto baja de 12, el gris de las explicaciones es más claro y los títulos
  de sección dejan la letra de máquina de escribir —que en una frase larga en mayúsculas
  cuesta de leer— y usan la misma tipografía que el resto.

- **El suavizado del cambio de cantante se decide solo.** Antes era un número fijo para
  toda la canción, y el mismo número no puede valer para un relevo en una pausa y para
  otro a mitad de frase: en la pausa se queda corto y suena a corte, y a mitad de frase se
  pasa y se oyen las dos voces a la vez. Ahora cada relevo elige su duración según lo que
  suene ahí. El mando de Ajustes sigue mandando, pero ahora mueve el rango entero: con
  30 ms va de 18 a 140, con el doble va de 36 a 280.

- **Lo que se guarda va en 24 bits, y el FLAC exportado también.** La mezcla final se
  guardaba en 16 bits, y como el programa la vuelve a leer para aplicarle los efectos del
  Estudio y la guarda otra vez, cada vuelta añadía su propio ruido de redondeo. El FLAC de
  la exportación —el formato «sin pérdida»— salía también en 16 bits, tirando ocho bits
  por el camino.

- **El menú ya no lleva a la portada, y hay un botón de inicio aparte.** Estaba como una
  pestaña más entre las secciones del programa, así que pulsarla por costumbre te sacaba
  de la aplicación a mitad de una sesión. Ahora es un botón con su propio icono en el
  extremo izquierdo, separado de las pestañas: se lee como una salida y no como un
  destino más.

- **Fuera el atajo de descarga en WAV y el importador de canciones.** La exportación de
  arriba ya ofrece todos los formatos con etiquetas y carátula, así que el atajo repetía
  —peor— algo que ya existía, y dos botones que hacen lo mismo obligan a pararse a decidir
  cuál es cuál.

---

## 1.0.0 — primera versión pública

Primera versión que sale del taller. Todo lo de abajo es nuevo por definición; a partir de
aquí, esta lista dirá solo lo que cambió.

### Qué hace

- **Separación** de voz e instrumental con Demucs v4, en tres modelos a elegir.
- **Reconocimiento de intérpretes**: detecta cuántas personas cantan y en qué tramos, e
  incluso reparte duetos que se solapan dentro del mismo verso.
- **Clonación** con RVC y cuatro algoritmos de extracción de tono.
- **Tratamiento de la voz**: afinación, dicción y consonantes, corrección de formantes,
  protección de respiraciones, espacio y reverberación, dinámica, color tonal e injerto de
  banda alta.
- **Editor multipista** para silenciar tramos, ajustar regiones de efectos y comparar
  versiones.
- **Exportación** en MP3, M4A, FLAC y WAV con carátula y etiquetas, o del vídeo con la voz
  nueva.
- **Doblaje** sobre vídeo con avisos de entrada — *función en pruebas, ver abajo*.
- **Reconstrucción del índice** de un modelo de voz cuando falta el archivo `.index`.
- Todo el proceso ocurre en tu computadora. Ni una muestra de audio sale de tu disco.

### Cómo se instala

No se instala. Se descomprime el ZIP y se abre `ZeroVoiceCloned.exe`. El programa trae
dentro todo lo que usa —incluidos los modelos de separación— así que no descarga nada y
funciona sin conexión desde el primer momento.

### Lo que conviene saber

- **El modo de doblaje no está terminado.** Funciona y se puede usar, pero no acumula
  todavía pruebas suficientes para darlo por bueno en todos los casos.
- **La aceleración por tarjeta gráfica va por DirectML**, que sirve con cualquier tarjeta
  compatible con DirectX 12: AMD, Intel y también NVIDIA. La vía propia de NVIDIA (CUDA)
  no viaja dentro de esta versión.
- El modelo de separación de 6 pistas (`htdemucs_6s`) es el único que no viene incluido:
  se descarga la primera vez que se elige.
- El ejecutable no tiene firma digital comprada, así que Windows mostrará el aviso
  «Windows protegió su PC» la primera vez.

---

<!--
CÓMO SE ESCRIBE UNA ENTRADA NUEVA

## 1.0.1 — 12 de marzo de 2026

### Corregido
- La exportación en FLAC perdía la carátula cuando la imagen era mayor de 2 MB.

### Nuevo
- ...

### Cambiado
- ...

Dos reglas que valen más que el formato:

  1. Se escribe lo que el usuario NOTA, no lo que se tocó por dentro. «Se refactorizó el
     módulo de mezcla» no le dice nada a nadie; «la voz ya no se adelanta un instante al
     entrar en el estribillo» sí.
  2. Si algo se rompió y se arregló, se dice que estaba roto. Esconderlo funciona hasta
     que alguien nota que su problema desapareció sin que nadie lo mencionara, y entonces
     se pregunta qué más no se está contando.
-->
