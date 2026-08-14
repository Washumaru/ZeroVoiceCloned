# Registro de cambios

Lo que cambia en cada versión, en español y sin jerga. Lo que escribas aquí es lo que ve
quien recibe el aviso de actualización dentro del programa, así que se escribe pensando en
él y no en quien programó el cambio.

Formato: lo más reciente arriba. **Corregido** antes que **Nuevo**, a propósito — quien
lee esto casi siempre viene buscando si ya se arregló lo que le pasó a él.

---

## 1.0.1 — sin publicar todavía

### Nuevo

- **Escuchar el original y el clon a la vez.** En el comparador hay un tercer botón,
  «Las dos», que suena las dos pistas en el mismo segundo en lugar de alternar entre
  ellas. Alternando se juzga el timbre; sonando juntas salta al instante lo que el oído no
  recuerda: si el clon entra tarde, si se queda corto en una frase o si se apaga en una
  nota alta. El clon suena tal cual quedó, con la configuración con la que se clonó.

### Corregido

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
