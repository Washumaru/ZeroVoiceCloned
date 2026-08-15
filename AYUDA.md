# Ayuda y problemas frecuentes

Ordenado por lo que más se pregunta. Si tu caso no está aquí,
[escríbelo](https://github.com/Washumaru/ZeroVoiceCloned/issues) y lo añado.

---

## Antes de empezar

### ¿De dónde saco los modelos de voz?

**El programa no trae ninguno, y no los crea.** Es un estudio: pone la voz que tú le des
sobre la canción que tú le des.

Un modelo de voz son **dos archivos**: un `.pth` (el modelo) y un `.index` (el timbre).
Los dejas en la carpeta `models\` del programa y aparecen solos en la lista.

Los modelos RVC se entrenan aparte, con herramientas hechas para eso, o se descargan de
comunidades donde la gente comparte los suyos. Entrenar uno propio con tu voz es un proceso
distinto y más largo que usar este programa.

### Me falta el archivo `.index`, ¿sirve igual?

Sí, y además **se puede reconstruir sin reentrenar nada**. En Ajustes hay una opción para
generarlo a partir de cualquier grabación de esa persona.

No hace falta que esté cantando: una nota de voz, una llamada o un audio de WhatsApp
valen. El índice guarda **timbre**, no estilo — cómo suena esa garganta, no qué notas
alcanza. Por eso tampoco importa que la grabación suene a teléfono.

---

## Al abrir el programa

### «Windows protegió su PC»

Sale siempre la primera vez. Es lo que Windows muestra ante cualquier programa que no ha
pagado una firma digital; no dice que el archivo tenga nada malo, dice que todavía no lo ha
visto lo suficiente.

**Más información → Ejecutar de todas formas.**

Si prefieres comprobar antes que el archivo es el que se publicó y nadie lo tocó por el
camino, en [SHA256.txt](SHA256.txt) están las huellas y cómo verificarlas en PowerShell.

### El antivirus lo bloquea o lo borra

Por el mismo motivo: ejecutable grande, sin firma, desconocido. Si tu antivirus lo pone en
cuarentena, tendrás que permitirlo a mano. Las huellas del apartado anterior sirven también
para esto.

### Se abre una ventana negra llena de texto

Es el programa funcionando: **no la cierres**. El estudio se abre solo en tu navegador al
cabo de unos segundos. Cuando termines, cerrar esa ventana es la forma de apagarlo del todo.

### Tarda mucho en abrir la primera vez

Normal. El primer arranque carga las bibliotecas de audio y los modelos: alrededor de medio
minuto, y más en un equipo lento o con el antivirus revisando por primera vez. Mientras
tanto verás una pantalla que cuenta los segundos. Los siguientes arranques son más rápidos.

### Se abrió el navegador pero dice que el programa se cerró

Significa que el motor no está en pie. Casi siempre es que se cerró la ventana negra. Vuelve
a abrir `ZeroVoiceCloned.exe` y esa pantalla desaparece sola, sin recargar nada.

---

## Al procesar una canción

### Salen líneas rojas en la consola y aun así termina

En la **versión de prueba** es normal: no incluye las partes de vídeo y doblaje, así que al
consultarlas aparecen errores. No afectan al resultado — fíjate en que justo debajo sale
`Separación completada con éxito`.

La regla para distinguir: **rojo y termina** → es eso. **Rojo y se detiene** → eso sí es un
fallo, cuéntamelo.

### «No se pudo descargar ese video» al pegar un enlace

El problema está en YouTube, no en el programa. YouTube cambia con frecuencia cómo sirve el
audio y bloquea las descargas desde ciertas conexiones. Suele ir y venir sin motivo
aparente.

Mientras tanto, descarga el audio por tu cuenta y súbelo como archivo: el resultado es
idéntico.

### Tarda mucho en separar o clonar

Depende sobre todo de si hay tarjeta gráfica. Sin ella todo corre en el procesador:
funciona, pero cada canción se toma su tiempo.

Para ir más rápido: elige el modelo de separación `htdemucs` en vez de `htdemucs_ft`, y el
perfil **Rápido**. Y usa la **prueba de 8 segundos** antes de procesar el tema entero — es
lo que evita esperar minutos para descubrir que el modelo elegido no era el bueno.

### La voz clonada suena apagada, como sin aire

Casi siempre es el modelo, no el programa. Un modelo entrenado con grabaciones de baja
calidad —notas de voz, llamadas, capturas de Discord— nunca aprendió a producir las
frecuencias altas, y eso no lo arregla ningún ajuste.

Enciende **Vocal Enhance**: entre otras cosas, toma esa banda alta del vocal original de la
canción y se la injerta al clon. Es exactamente el caso para el que se hizo.

### La voz clonada desafina en los agudos

Usa **RMVPE** como algoritmo de tono, que es el que mejor aguanta agudos y falsetes. Y
revisa el **transporte**: si la canción está fuera del registro que el modelo vio al
entrenarse, no puede cantarla por bien ajustado que esté todo lo demás.

### Las eses y las tes salen blandas o con un chasquido metálico

Enciende **Consonantes en una pasada aparte**, en Ajustes. Los valores que hacen que las
vocales suenen a la persona clonada son los que estropean las consonantes, así que con una
sola conversión hay que elegir. Con esto se convierte dos veces y se coge de cada pasada lo
que hace mejor. Tarda el doble, así que déjalo para cuando la voz ya te guste y lo único
que falle sean las consonantes.

### La voz suena con las palabras borrosas y colas metálicas

Si la canción original lleva mucha reverberación, comprueba que **Quitar la sala antes de
clonar** esté encendido en Ajustes. El motor sabe imitar una voz, no una habitación: con la
sala pegada intenta convertir las dos cosas a la vez. La sala se le devuelve después, así
que el resultado no queda más seco.

### Un cantante entra más flojo o más fuerte que el que sale

Ya se corrige solo: los dos lados de cada relevo se igualan en el instante del cambio. Si
aún lo notas, sube el **Suavizado del cambio de cantante** en Ajustes — ese mando ya no fija
la duración del cruce, escala el rango que el programa elige para cada relevo.

### La canción exportada suena más floja que el original

No debería: el programa mide el volumen real de tu canción y deja la mezcla ahí. Si se queda
por debajo es a propósito, y es el caso en que la mezcla tiene los picos muy por encima de
su volumen medio — subirla más obligaría al limitador a comerse los golpes, y saldría al
volumen pedido pero aplastada. Se prefiere una canción un poco más floja, que se arregla con
el mando del volumen.

### El estribillo con coros sale como un balbuceo

Comprueba que la separación de voz principal y coros esté activa. El coro debe pasar **sin
clonar**; si entra en la clonación, RVC ve varias voces a la vez y no sabe cuál seguir.

### En un dueto suena metálico, como dentro de un tubo

Eso es filtrado en peine: dos voces idénticas sumadas en fase. Ajusta la **tolerancia de
duetos simultáneos** en Ajustes — probablemente se está marcando como dueto algo que es una
sola persona.

### Detectó más cantantes de los que hay

Baja la sensibilidad de detección. Al forzar más grupos de los que existen, un mismo
intérprete se parte en dos y la canción sale con dos voces distintas en el mismo verso.

---

## Archivos y espacio

### ¿Dónde queda lo que produzco?

Lo que exportas, donde tú lo guardes. Los archivos de trabajo van a `temp\`, dentro de la
carpeta del programa: se pueden borrar cuando quieras, son intermedios.

Tus ajustes y tu licencia **no** viven ahí, sino en tu perfil de usuario. Por eso puedes
mover el programa, reinstalarlo o actualizarlo sin volver a configurar nada.

### Ocupa mucho espacio

El programa son unos 3 GB porque lleva dentro todo lo que usa, incluidos los modelos de
separación. A cambio no descarga nada y funciona sin conexión.

La carpeta `temp\` crece con el uso. Vaciarla de vez en cuando es seguro.

### ¿Cómo lo desinstalo?

Borra la carpeta. No toca el registro ni aparece en «Agregar o quitar programas». Si además
quieres borrar tus ajustes y tu licencia, elimina la carpeta `ZeroVoiceCloned` de tu perfil
de usuario (`%APPDATA%`).

---

## Cómo reportar un fallo para que sirva

Con tres datos se arregla; sin ellos, casi nunca:

1. **Qué estabas haciendo** cuando pasó.
2. **La canción**: duración y formato.
3. **Las últimas líneas de la ventana negra**, tal cual salen.

Se reporta en [Issues](https://github.com/Washumaru/ZeroVoiceCloned/issues). Se leen todos,
uno por uno.
