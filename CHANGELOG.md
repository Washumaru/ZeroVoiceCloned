# Registro de cambios

Lo que cambia en cada versión, en español y sin jerga. Lo que escribas aquí es lo que ve
quien recibe el aviso de actualización dentro del programa, así que se escribe pensando en
él y no en quien programó el cambio.

Formato: lo más reciente arriba. **Corregido** antes que **Nuevo**, a propósito — quien
lee esto casi siempre viene buscando si ya se arregló lo que le pasó a él.

---

## 1.0.1 — sin publicar todavía

### Corregido

- **Dos interruptores de Ajustes no hacían absolutamente nada.** «Protección de
  respiraciones» y «Corrección de formantes» se podían encender y apagar, se guardaban y
  hasta salían en las recetas — y **nunca llegaban al motor**. Ninguno de los dos existía
  más allá de la casilla. Se auditaron los ochenta mandos del programa uno por uno para
  descartar que hubiera más: eran exactamente esos dos.

  Cada uno se resolvió como tocaba, que no era lo mismo:

  **La protección de respiraciones existe ahora de verdad.** Entre frase y frase el
  cantante toma aire, y ese sonido no tiene tono: el motor no encuentra nada parecido en el
  modelo y rellena el hueco con gruñidos graves y soplidos metálicos. Ahora se detectan
  esas respiraciones en la voz original —sin tono, poca energía y ruido de banda ancha, las
  tres cosas a la vez— y se devuelven al clon con un cruce corto en los bordes. No mete al
  cantante equivocado en la mezcla: una respiración es aire pasando por una garganta
  abierta, y no lleva ni el tono ni los formantes que identifican a una persona.

  **La corrección de formantes se quitó en vez de implementarse.** Copiar los formantes del
  cantante de origen sobre el clon ya se había medido en este proyecto: acercaba el clon un
  40 % al original y lo dejaba nasal y tapado — el híbrido sin carácter. Es una regla escrita
  del proyecto, así que hacer funcionar ese interruptor habría sido empeorar el programa a
  petición de un botón. El color se sigue ajustando donde corresponde, al 15 % y por
  cantante.

- **La voz doble no sonaba nunca.** Se marcaba el tramo, se elegía la segunda voz, la
  pantalla lo daba por puesto — y en el audio final sólo estaba la voz principal.

  La causa no estaba en el dúo sino en una etapa anterior. Justo antes de renderizar hay un
  paso que reparte entre los cantantes vecinos los trocitos de voz que la detección dejó sin
  dueño. Ese paso **reconstruía cada tramo quedándose sólo con el cantante y sus tiempos**, y
  tiraba todo lo demás por el camino. Entre ese «todo lo demás» estaba precisamente la marca
  del dúo. Cuando después se preguntaba qué tramos llevan segunda voz, la respuesta era
  siempre «ninguno».

  Se llevaba por delante también los cortes hechos a mano, y podía fundir dos bloques con
  segundas voces distintas en uno solo — extendiendo un dúo a un tramo donde nadie lo puso.
  Ahora los tramos conservan lo que el usuario marcó, y la regla de cuándo se pueden fundir
  dos bloques es la misma en la línea de tiempo y en el motor, que es lo que hace que el
  resultado se parezca a lo que se ve en pantalla.

- **Ya no queda ninguna descarga pendiente: el separador de 6 pistas viaja dentro.** Era la
  única pieza que el programa se bajaba la primera vez que se usaba. Se había dado por hecho
  que ese modelo se guardaba en un almacén aparte con sus propias reglas, y que meterlo
  obligaba a llevar dentro un segundo sistema entero. Resultó que no: se comporta igual que
  los otros dos. Lo único cierto es que su archivo nunca se había llegado a descargar en la
  máquina donde se arma el paquete, así que no había nada que copiar. Ahora se empaqueta con
  los demás, y la comprobación previa a publicar cuenta seis archivos en vez de cinco, así
  que no puede volver a salir un paquete sin él.

- **La voz original se colaba por debajo del clon en cada borde de tramo.** Se oía como si
  el cantante de verdad hubiera quedado de fondo, o como un eco con la reverb apagada.

  Lo que no reclama ningún cantante conserva la voz original, para que no desaparezca audio
  de la mezcla — eso está bien. El fallo estaba en el borde: cuando un tramo termina y no
  entra otro cantante, el programa bajaba el clon con una rampa larga mientras subía al
  cantante original en su lugar, así que durante toda la rampa **sonaban los dos**. Medido:
  el original llegaba a seis decibelios por debajo del clon.

  Y la rampa la escalaba el mando de suavizado, que es para el relevo *entre cantantes*:
  con el mando en 300 ms eran 330 milisegundos de las dos voces juntas en cada borde. En
  una canción con muchos tramos, eso es la voz original apareciendo decenas de veces por
  minuto.

  Ahora esa rampa dura 24 ms fijos y no la mueve ningún mando: lo justo para que la caída
  no chasquee, y demasiado corta para que el oído separe dos voces. El relevo entre dos
  cantantes no cambia nada — ahí el cruce largo sí tiene sentido y sigue conservando la
  potencia exacta.

- **La opción «consonantes en pasada aparte» iba sin proteger ninguna consonante.** El
  mando de protección del motor va al revés de lo que sugiere su nombre: es la fracción de
  timbre del modelo que se deja pasar donde no hay tono, así que **bajarlo protege más**, y
  a partir de 0,5 el motor se salta la etapa entera. La pasada de consonantes lo ponía
  justo en 0,5. Es decir: de las dos conversiones que costaban el doble de tiempo, la que
  existía para arreglar las consonantes era la única que iba con la protección apagada.
  Ahora usa 0,15, que deja pasar el 85 % de la articulación del cantante original.

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

- **La corrección de tono automática casi nunca se activaba, y cuando lo hacía movía las
  notas a donde no debía.** Tres fallos encadenados en la misma función:

  El detector medía la afinación del original agrupando el tono en notas, y las agrupaba
  cortando donde el tono pega un salto brusco. Un cantante que **se desliza** de una nota a
  la siguiente no pega ningún salto brusco, así que las dos notas quedaban pegadas en una
  sola cuyo centro cae entre ambas — a 30 o 40 cents de cualquier nota real, cantara como
  cantara la voz. Es decir: sobre canto natural, que siempre lleva deslizamientos, el
  detector medía su propio error y concluía «esta voz no está corregida». Medido: la misma
  voz daba 0,54 sin deslizamientos y 0,07 con ellos; ahora da 0,54 y 0,46.

  El corrector, por su parte, recorría el audio en bloques fijos de 80 milisegundos
  contados desde el principio del archivo. Las notas no empiezan ahí, así que un bloque
  cualquiera caía a caballo entre dos: se le calculaba un único tono y se desplazaba
  entero, con lo que **media nota se corregía hacia la altura de la otra**.

  Y apuntaba siempre al semitono más cercano de los doce. Una nota sesenta cents alta no
  se acerca así a la nota que se pretendía: se clava en **la de arriba**, que a menudo no
  está en la canción. Eso no se oye como robótico, se oye como desafinado.

  Ahora las notas se detectan de verdad —separando las asentadas de los deslizamientos—,
  cada una se corrige como una unidad, y cuando la canción está lo bastante clara como
  para saber sobre qué notas se mueve, la corrección apunta a ellas y no a las doce.
  Medido sobre un clon desafinado: de 26,2 cents de desviación a 4,5.

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

- **El proyecto cabe en un archivo.** Hasta ahora una sesión vivía en una carpeta con
  nombre de identificador, dentro de un directorio llamado «temporal». El trabajo de una
  tarde —repartir cantantes, recortar, ajustar veinticinco mandos— no se podía guardar en
  un sitio propio, ni llevar a otro ordenador, ni distinguir de las demás. Ahora se guarda
  como un `.zvc` y se abre donde sea: la canción, las pistas separadas, los tramos de cada
  cantante, los recortes, las regiones de efectos, la letra alineada y todos los mandos.

  **Los modelos de voz no van dentro**, y no es un descuido. Pesan 55 MB cada uno, así que
  un proyecto a tres voces se iría por encima de los 250 MB llevando copias de archivos que
  ya están en tu carpeta. Y pesa más otra razón: compartir un proyecto es compartir en qué
  trabajaste; compartir un modelo de voz es otra cosa, y no debería pasar sin querer dentro
  de un archivo que mandas «para que veas cómo quedó». Lo que sí viaja es el **nombre** de
  cada modelo, y al abrir el programa mira tu carpeta y dice cuáles faltan — antes de meterte
  en la sesión, no después.

  El audio va dentro en FLAC, sin perder una muestra: comprimir un WAV con ZIP no ahorra
  casi nada y en FLAC ocupa cerca de la mitad. Y si el proyecto trae las pistas separadas,
  al abrirlo no hay que volver a separar; si trae la letra, no hay que volver a transcribir.

- **La letra y sus tiempos ya vienen dentro: nada que instalar.** El programa sabía sacar
  la letra de una canción con el momento exacto de cada palabra, pero eso necesitaba un
  motor de transcripción que no viajaba en el paquete. Es decir: estaba escrito y apagado.
  Ahora va dentro, y con él se encienden cuatro cosas que dependían de saber qué se canta:

  - Los relevos entre cantantes se colocan **en frontera de palabra**, no en el punto más
    callado — que en una nota sostenida cae a mitad de sílaba.
  - La pasada de consonantes deja de recorrer la canción entera. Medido sobre una canción
    real: sólo el **27 %** del tiempo cantado es consonante, así que se ahorra el resto.
  - El índice se aparta donde borraba la articulación, y sólo ahí.
  - Sale la **letra sincronizada** en `.lrc` para karaoke y `.srt` para el vídeo.

  Tarda unos 48 segundos por canción y se hace una sola vez: queda guardado con la sesión.
  Como todo lo demás, funciona sin conexión.

- **Una «s» del clon recupera las «eses» del modelo, y no sus vocales.** Es la otra mitad
  del arreglo de pronunciación. El índice de un modelo está dominado por vocales —en
  cualquier grabación cantada ocupan casi todo el tiempo con voz—, así que al buscar el
  sonido más parecido a una consonante encontraba vocales. No es que el índice estorbe en
  las consonantes: es que ahí no tenía consonantes que ofrecer.

  Ahora, al construir el índice de una voz, se guarda junto a él **de qué tipo de sonido
  salió cada retazo**. Con esa etiqueta, cuando lo que suena es una consonante la búsqueda
  se queda sólo con los retazos que también lo son. La consonante deja de tener que elegir
  entre sonar a la persona clonada o entenderse.

  Sobre las vocales no se toca nada — ahí ya funcionaba — y si un modelo no tiene
  consonantes suficientes en su banco se busca como siempre, en vez de devolver algo lejano
  sólo porque lleva la etiqueta buena. Las etiquetas se escriben al crear el índice: los
  modelos que ya lo tienen hecho siguen funcionando igual, y para aprovechar esto hay que
  rehacérselo desde Ajustes.

- **Una canción ya separada no se vuelve a separar.** Era lo más lento del programa después
  de convertir, y se repetía todo el rato sobre el mismo audio: al volver a cargar el tema
  para probar otro modelo de voz, al cerrar y reabrir la sesión, al abrir un proyecto que
  traía la canción pero no las pistas. Ahora las separaciones se guardan y se reutilizan, y
  esas veces son instantáneas.

  Se identifica por **el contenido del audio**, no por el nombre del archivo: la misma
  canción bajada dos veces puede llamarse distinto, y dos temas distintos pueden llamarse
  igual. Y también por los ajustes, porque cada modelo de separación da pistas distintas —
  reutilizar las de uno para otro sería devolver algo que no pediste, y encima sin decirlo.

  Antes de usar lo guardado se comprueba que esté completo y que dure lo mismo que la
  canción de entrada. Si algo no cuadra, separa como si no hubiera caché: que el programa
  tarde es un fastidio, que devuelva la canción de otro es un fallo. Se guardan las veinte
  últimas, y en Ajustes se ve cuánto ocupan y se pueden tirar.

- **El dúo suena a dos personas y no a una voz gruesa.** La voz doble convierte el mismo
  pasaje con dos modelos y los suma separados por unos milisegundos. Ese desfase existía por
  un motivo correcto — sin él las dos señales, casi idénticas, se cancelan entre sí y el
  resultado sale hueco y metálico — pero era **un número fijo**, y ahí estaba el fallo: un
  retardo constante no quita esa cancelación, la deja clavada siempre en las mismas
  frecuencias. El oído no oye eso como dos personas; lo oye como un filtro, es decir, como
  el timbre de una sola voz rara. Es el sonido de un chorus barato.

  Dos tomas de verdad no se separan por un número constante: **derivan**. Ahora el desfase
  se pasea despacio, a lo largo de la frase y no dentro de la sílaba. Con eso pasan dos
  cosas a la vez, y las dos hacían falta: la cancelación se mueve y deja de leerse como
  timbre, y la segunda voz queda unos cents por encima o por debajo, cambiando poco a poco
  — que es la pista que el oído usa antes que ninguna otra para decidir que hay dos
  personas. Medido: entre 7 y 13 cents de media, con picos de 17 a 24. Es lo que separa a
  dos tomas de la misma persona.

  Lo que **no** se toca es la entonación: las dos conversiones salen del mismo audio, así
  que siguen la misma melodía. Eso es lo que las mantiene al unísono en vez de
  descoordinadas. El mando del desfase sigue sirviendo: ahora elige dónde se pasea la
  segunda voz. Y la deriva es siempre la misma para el mismo tramo, así que dos renders de
  la misma canción suenan idénticos — un efecto que cambiara en cada render no se podría
  ajustar.

- **El reparto de cantantes sale limpio de fábrica.** La detección agrupa por timbre y
  acierta el «quién» mucho mejor que el «dónde»: sus fronteras caen donde cambia el
  parecido acústico, que no es donde acaba una frase. De ahí salían dos estropicios que
  había que arreglar a mano tramo por tramo — trozos de dos décimas adjudicados a otro
  cantante en mitad de un verso, que se oyen como un parpadeo de voz, y relevos colocados a
  mitad de palabra.

  Ahora el programa los corrige solo: absorbe el tramo imposible y lleva cada relevo a la
  pausa que tenga más cerca. **Y no se pasa de listo**: un tramo corto sólo desaparece
  cuando está rodeado por el mismo cantante a los dos lados, que es el caso que no ocurre
  al cantar. Si está entre dos cantantes distintos, o al principio o al final de la
  canción, se respeta — podría ser un ad-lib de verdad, y borrarlo sería peor. Todo sigue
  siendo editable en la línea de tiempo; la idea es que casi nunca haga falta.

- **Bajarse las pistas por separado.** Voz clonada, instrumental, voz original y voz
  principal sin coros, cada una en su archivo, para seguir la mezcla en otro programa. Los
  archivos ya estaban en la carpeta de la sesión; lo que faltaba eran las dos cosas que los
  hacen servibles fuera.

  **El nombre.** `converted_vocals.wav` arrastrado a un secuenciador, dentro de una carpeta
  con las pistas de otras tres canciones, es indistinguible. Ahora salen como
  «Mi canción - Voz clonada.wav», con el tema delante porque es lo que ordena la carpeta de
  descargas y así las pistas de un mismo trabajo quedan juntas.

  **La proporción entre ellas**, que es lo que de verdad faltaba. La separación deja la voz
  y el instrumental cada uno a su tope — bien para escucharlos sueltos en el editor, y una
  trampa fuera: sumados no dan la canción, porque la voz es la que más se subió. El
  programa apuntaba los dos factores, así que ahora los deshace al exportar y las pistas
  suenan unas respecto de otras como sonaban en el disco. Se puede apagar si lo que se
  quiere es una pista sola a tope. En sesiones separadas con una versión anterior, que no
  apuntaba esos niveles, el programa lo dice en vez de ofrecer un modo que no haría nada.

- **El clon pronuncia las palabras como las pronuncia el cantante original.** Es lo que
  hacía que un clon sonara a que «mastica» la letra, se come las eses o canta en otro
  idioma — y la causa tenía nombre desde el principio: el **índice**.

  El índice de un modelo es un banco de retazos de la voz de esa persona. Con el mando al
  0,75, cada instante de la canción se sustituye en un 75 % por lo más parecido que
  encuentre en ese banco. Sobre una vocal sostenida es exactamente lo que se quiere: ahí
  vive el parecido. Sobre una consonante es un desastre, porque el banco está lleno de
  vocales —en cualquier grabación cantada las vocales ocupan casi todo el tiempo— y lo más
  parecido a una «s» dentro de un montón de vocales sigue siendo una vocal. La «s» del
  cantante original se cambiaba por algo que no era una «s».

  Ahora ese mando **deja de ser un número para toda la canción**. Con la letra alineada, el
  programa sabe décima a décima si suena una vocal o una consonante, y mueve el mando por
  su cuenta: manda el índice donde está la identidad de la voz, y se aparta donde lo único
  que hacía era borrar la articulación. La protección de consonantes hace el camino
  inverso a la vez.

  **No cuesta tiempo.** Es la misma conversión de siempre con dos números que se mueven
  mientras suena, no una segunda pasada. De hecho hace casi innecesaria la opción de
  «consonantes en pasada aparte», que perseguía lo mismo por fuera convirtiendo el tema
  dos veces.

  Lo que esto **no** arregla, y conviene saberlo: un modelo cuyo techo esté en 7 kHz no
  puede producir una «s» nítida por mucho que se le aparte el índice — ahí la consonante
  se injerta del original, que es lo que ya hacía el injerto de banda alta.

- **El programa ya sabe qué se está cantando, y no sólo cuánta energía hay.** Es el
  cambio más de fondo de esta versión. Hasta ahora todo —el reparto de cantantes, los
  relevos, la dicción, la pasada de consonantes— trabajaba mirando energía y tono, sin
  enterarse de qué palabra sonaba. Varias cosas fallaban exactamente por eso. Ahora se
  transcribe la voz una vez por canción, se apunta cuándo suena cada palabra, y el resto
  del programa lo lee.

  Lo que cambia con eso:

  - **Los relevos entre cantantes se colocan en frontera de palabra.** Antes se buscaba el
    punto más callado alrededor del cambio, y en una nota larga sostenida el punto más
    callado está *dentro* de la palabra: el corte quedaba técnicamente en el sitio más
    silencioso y aun así partía la sílaba.
  - **La pasada de consonantes deja de convertir la canción entera dos veces.** Sabía que
    unos ajustes van bien a las vocales y otros a las consonantes, pero no dónde estaba
    cada cosa, así que hacía el tema completo por duplicado — y por eso venía apagada, que
    tarda el doble. Con la letra delante sabe qué trozo lo necesita de verdad.
  - **Sale la letra sincronizada**, en `.lrc` para karaoke y `.srt` para el vídeo.

  Se hace **antes de clonar**, porque las dos primeras cosas se deciden durante el render.
  Y todo lo que la usa sigue funcionando igual que antes si no se hace: es opcional, no un
  paso nuevo obligatorio.

  Corre en su propio entorno (`venv_alineacion/`), como ya pasa con la desreverberación:
  el motor de transcripción necesita una versión de numpy que rompería Demucs.

- **Saber de antemano dónde se va a forzar la voz.** Cuando la canción pide notas que el
  modelo nunca vio, el motor extrapola y saca voz rota, soplada o afónica. El programa ya
  repara parte de eso, pero reparar tiene un límite y la única forma de enterarse era
  escuchar el render entero. Ahora, junto al tono de cada cantante, hay un botón que dice
  qué tramos se salen del registro del modelo, cuánto y hacia dónde — con el tono que
  tengas puesto ya aplicado, que es lo que de verdad le llega al motor.

  **No se transporta tramo a tramo, y es a propósito.** Bajar el puente una octava para
  que entre en el modelo haría que el clon cantase ese puente por debajo del instrumental:
  dejaría de sonar forzado para sonar equivocado, que es peor. Lo que se propone es
  cambiar de modelo para esa voz, que es lo que sí arregla la causa.

- **Una canción de referencia para el equilibrio del máster.** El programa ya resolvía
  *cuánto* suena la mezcla, pero no decía nada de *cómo* se reparte esa energía entre
  graves, medios y agudos — y ese reparto es lo que hace que una mezcla suene casera al
  lado de un disco. Ahora se suelta un tema que suene como quieres que suene el tuyo y el
  programa mide la diferencia banda a banda y la corrige.

  Se copia **la forma de la curva, nunca el volumen**: las dos canciones se comparan
  referidas a sus propios medios, así que lo que se traslada es «ésta lleva 3 dB más de
  graves en relación con su voz» y jamás «ésta suena más fuerte». Tampoco se copia la
  dinámica, ni el estéreo, ni la reverberación: eso está en la mezcla y en el arreglo, y
  un ecualizador no puede traerlo — intentarlo sólo destruye lo que había.

  Va a media fuerza y ninguna banda se mueve más de 4 dB. Una diferencia mayor no es un
  desequilibrio que arreglar; es que las dos canciones no se parecen, y entonces el
  programa lo dice en vez de deformar la tuya para acercarla. La corrección se puede
  **ver dibujada antes de aplicarla**.

- **Destino de entrega elegible.** Además de la canción original, que sigue siendo lo que
  se usa por defecto, se puede apuntar a Spotify/YouTube (−14 LUFS), Apple Music (−16) o
  club (−8). No se cambió el comportamiento normal a propósito: igualar a −14 porque lo
  pide un servicio cambiaría el carácter de un disco masterizado a −8 queriendo, y lo que
  se pidió fue cambiar una voz, no remasterizar la canción de nadie.

- **Deshacer alcanza ya al cambio de voz y de tono.** Ctrl+Z revertía recortes, tramos y
  efectos, pero no el modelo asignado a un cantante ni su tono — que son justo las dos
  cosas que se tantean una y otra vez hasta dar con la buena. Un deshacer que funciona a
  medias es peor que no tenerlo, porque enseña a no fiarse de él.

- **El estado del modelo se dice donde se elige el modelo.** Hasta dónde llega un modelo
  —su techo— y si tiene índice deciden en silencio lo bien que puede sonar un clon, y las
  dos cosas sólo se veían en Ajustes, es decir en una pantalla a la que se entra cuando
  algo ya salió mal. Ahora aparecen debajo del desplegable, tanto en una voz como en cada
  cantante del reparto: un modelo que se apaga en 7 kHz lo avisa antes de clonar. No es
  una alarma ni hay nada que encender —el injerto de banda alta ya compensa ese techo
  siempre—, es para que un clon apagado se atribuya al modelo, que es lo que se puede
  cambiar, y no a un mando de la voz.

- **Comparar a ciegas.** En el comparador, un interruptor tapa las etiquetas: los botones
  pasan a decir A y B, y cuál es cuál se sortea en cada ronda. Se elige la que más gusta y
  sólo entonces se revela. Sabiendo cuál es el clon se le buscan defectos, y sabiendo cuál
  es el original se le perdonan — y quien compara es justo quien acaba de pasarse la tarde
  ajustando el clon y quiere que haya salido bien. El programa lleva la cuenta de las
  rondas, y con menos de tres lo dice claro: con tan pocas, cualquier resultado es
  casualidad.

- **Recetas: los mandos con los que una canción quedó bien, guardados con un nombre.** El
  programa tiene veinticinco mandos entre la voz, el motor y la mezcla. Acertar con ellos
  costaba una tarde, y esa tarde se perdía: al día siguiente no había forma de repetir el
  resultado salvo acordarse de veinticinco números. Ahora se guardan con nombre desde
  Ajustes, se vuelven a poner de una vez, y se pueden llevar a otro equipo o pasárselas a
  otra persona como un archivo suelto.

  **Antes de aplicar se enseña qué va a cambiar**, mando a mando, con el valor de ahora y
  el que va a quedar. Sustituir a ciegas veinticinco números sobre una configuración que
  costó trabajo es justo el momento en el que una herramienta pierde la confianza de quien
  la usa.

  Una receta guarda preferencias, nunca la canción: los tramos de cada cantante, los
  silencios y el reparto de modelos se quedan como están. Y una receta de una versión
  anterior a la que le falten mandos se aplica igual — los que no trae se quedan como los
  tengas, y el panel dice cuántos son.

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

- **El clon copia también CÓMO de rápido se afina el original, no solo cuánto.** Es lo que
  el propio manual admitía que no se lograba: «se nota que el clon está menos afinado a
  máquina que la voz que sustituye». La causa no era la cantidad de corrección — era otra
  cosa que no se estaba mirando.

  Dos voces pueden acabar igual de pegadas a la nota y sonar completamente distintas según
  **cómo llegan** a ella. Si el tono salta de golpe al empezar cada nota, se oye a máquina;
  si se desliza, no. El programa copiaba la profundidad de la corrección y le dejaba al
  clon sus propios deslizamientos naturales, así que el clon acababa igual de afinado
  encima de una producción que saltaba de golpe. Sonaban a dos grabaciones distintas por
  ese detalle y por ningún otro.

  Ahora se mide también esa velocidad en el original y se le da la misma al clon.

  No es una medida exacta y conviene saberlo: tiene un suelo de unos 12 milisegundos —por
  debajo de eso la ventana de análisis ya no distingue— y en un caso extremo de laboratorio
  se sale de sitio. Sirve porque lo que hay que acertar es la diferencia entre «entra de
  golpe» y «entra deslizándose», y eso lo separa por un factor de siete.

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
- Los tres modelos de separación viajan dentro, incluido el de 6 pistas: no hay ninguna
  descarga pendiente y el programa funciona sin conexión desde el primer momento.
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
