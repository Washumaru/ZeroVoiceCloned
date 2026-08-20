# Registro de cambios

Lo que cambia en cada versión, en español y sin jerga. Lo que escribas aquí es lo que ve
quien recibe el aviso de actualización dentro del programa, así que se escribe pensando en
él y no en quien programó el cambio.

Formato: lo más reciente arriba. **Corregido** antes que **Nuevo**, a propósito — quien
lee esto casi siempre viene buscando si ya se arregló lo que le pasó a él.

---

## 1.0.1 — sin publicar todavía

### Corregido

- **En el Editor faltaban los coros, el botón de escucharlos, deshacer y las voces del
  dúo.** Las líneas de tiempo se mudaron a la pantalla del Editor, y todo lo que estaba
  montado a su alrededor se quedó en IA Voz: la fila de coros y segundas voces no salía,
  con ella se perdía el botón que reproduce una región aislada para saber de quién es,
  **Ctrl+Z y Ctrl+Y no hacían nada** en la pantalla donde justamente se recorta y se
  reparte, y el desplegable de segunda voz del dúo salía vacío.

  Ninguna de las cuatro cosas se ha copiado de una pantalla a otra, que habría dejado dos
  versiones destinadas a separarse. Lo que se edita —cantantes, coros, efectos, silencios
  y el modelo y el tono de cada voz— es de la **canción**, no de la pantalla: ahora vive
  en un solo sitio y las dos pantallas leen de ahí. El historial de deshacer es uno solo y
  lo comparten.

- **Al clon le faltaba brillo justo donde el modelo deja de llegar.** El programa ya le
  presta a la voz clonada la banda alta del vocal original —es lo que le devuelve el aire
  que ningún modelo de estos sabe generar—, pero ese préstamo entraba demasiado despacio:
  tardaba una octava entera en aplicarse del todo, así que la franja intermedia se
  quedaba a medio cubrir. Medido sobre tres canciones, contra el vocal del disco:

      8 kHz    antes -7,2 dB    ahora -4,5 dB
     10 kHz    antes -3,6 dB    ahora -1,3 dB

  Ahora el préstamo entra en media octava. Se comprobó que la voz sigue siendo la del
  modelo: por debajo de 4 kHz —donde vive quién canta— no cambia nada, porque ahí no se
  toca. Y no se acortó más: probando con un cuarto de octava, en una de las tres
  canciones la voz se pasaba de brillo por encima del propio disco.

- **La cadena del Estudio le metía a la voz una curva que nadie había pedido.** Era la
  única parte del programa que no escuchaba la canción: aplicaba siempre los mismos
  números —presencia arriba, corte abajo y un quitasiseos permanente— sin mirar si esa
  voz los necesitaba.

  Se midió pasándola por encima del **vocal original de tres discos**, o sea voces que ya
  estaban perfectas. A las tres les levantaba unos 4 dB en 4-5 kHz y les quitaba 2 dB justo
  encima: un escalón de más de 5 dB que se oye como voz nasal y sin aire. Y sobre el clon
  era peor todavía, porque el clon no llega con sibilancia de sobra sino con 9 dB de menos:
  el quitasiseos estaba borrando lo poco que el modelo consigue producir.

  Ahora esas cuatro decisiones salen de comparar la voz clonada con el vocal del disco, y
  cada una tiene el mismo tope que antes: solo se sube lo que falte, y nunca por encima de
  lo que tiene el original. Si a la voz no le falta nada, la etapa se aparta.

      sobre el vocal del disco     escalón de 4 a 6 kHz    antes +5,1 / +5,2 / +5,7 dB
                                                           ahora  0,0 /  0,0 /  0,0 dB

      sobre la voz clonada         distancia al disco      antes 15,0 / 34,0 / 47,5 dB
                                                           ahora  5,9 / 29,6 / 43,7 dB

  Se comprobó además que el clon no se acerca al timbre del cantante original al hacerlo:
  la voz sigue siendo la del modelo, solo que sin la curva de más.

- **El detector automático de cantantes decía «2» en una canción de cinco, y lo decía muy
  seguro.** En «Diles (Full Remix)» —Bad Bunny con Farruko, Ozuna, Arcángel y Ñengo Flow—
  detectó dos voces y presentó el resultado sin ninguna señal de duda.

  La causa es de fondo y sigue ahí: la medida que decide cuántos grupos hay premia partir en
  dos. Varias voces pasadas por la misma mezcla no forman grupos separados sino un continuo,
  y cortar un continuo por la mitad siempre puntúa mejor que cortarlo en cinco. En esa
  canción K=2 saca 0,334 y K=5 saca 0,225: no es un empate que desempatar, gana de calle.

  Lo que sí se ha arreglado es que **el programa lo diga**. La curva de puntuaciones delata
  a esa canción: baja de 2 a 3 y vuelve a subir hasta 5. Si de verdad hubiera dos cantantes,
  partirlos en más empeoraría siempre; que repunte significa que arriba hay voces sin
  recoger. Ahora ese repunte baja la confianza y la pantalla propone los números que
  convendría probar — en esta canción, **5 y 6**.

  El aviso exige que el repunte tenga tamaño (un 10 %), medido para no llenar de avisos las
  canciones sanas: en «Ghost Boy», de un solo cantante, la mayor subida es del 0,05 % y no
  dispara nada.

  Y en el desplegable, **«Autodetectar» ya no dice «recomendado»**, con una nota que explica
  que si sabes cuántos artistas cantan conviene forzar ese número. Recomendar el camino que
  falla era lo peor de las dos cosas.

- **Un cantante seguía cantando la parte del siguiente durante segundo y medio.** El relevo
  se colocaba tarde, así que el arranque del cantante que entraba salía convertido con el
  modelo y el tono del que se acababa de ir. Con dos voces de registros distintos se oye
  clarísimo: la frase entra una octava fuera de sitio y se corrige sola al cabo de un rato.

  El detector de cantantes decide muy bien **quién** canta y bastante peor **dónde**: mira
  la canción en ventanas de tres segundos que avanzan de medio en medio, así que sus
  fronteras llegan con hasta un segundo de error. Había un afinador que lo corregía
  comparando el timbre de los dos lados, pero se rinde cuando la evidencia no es
  concluyente — y dos personas cantando la misma frase, en la misma sala y con la misma
  reverb, se parecen bastante.

  Faltaba mirar lo más evidente: **el registro**. En la canción donde se vio, el que sale
  canta en 294 Hz y el que entra en 174; ese salto es imposible de confundir con otra cosa,
  y el dato ya estaba calculado desde antes. Ahora el relevo se lleva al punto donde el tono
  da el salto. Medido sobre ese caso: la frontera pasa de estar **1,50 s tarde a 0,02 s**.

  Cuando los dos cantantes tienen el mismo registro no se toca nada, que es lo correcto:
  ahí esta señal no distingue, y moverse sin evidencia sería cambiar un error por otro.

- **La voz del cantante que entra sonaba unos segundos con el timbre del anterior.** En un
  relevo —el cantante 1 calla, empieza el 2— las primeras sílabas del que llegaba salían
  convertidas con el modelo y el tono del que se acababa de ir, y la cosa se corregía sola
  al cabo de un par de segundos.

  Eran tres cosas sumándose. El detector de cantantes trabaja con ventanas de tres segundos
  y saltos de medio, así que la entrada del que llega ya quedaba apuntada con hasta un
  segundo de retraso. El afinador fino que existe para arreglar justo eso **se saltaba
  siempre que hubiera una pausa entre los dos** — o sea, casi siempre, porque entre cantante
  y cantante suele haber silencio. Y el silencio que quedaba en medio, que llevaba dentro el
  arranque del que entra, se repartía **por la mitad geométrica**, sin mirar el audio.

  Ahora ese silencio se parte por donde de verdad arranca la voz que entra: se busca el
  punto más callado justo antes de su primera sílaba, saltándose la cola de reverb del que
  se va. Cada cantante empieza en su propia voz.

- **El detector de coros no detectaba coros.** Lo único que miraba era cuánta energía
  quedaba tras separar la voz principal — y ahí siempre queda energía, porque ninguna
  separación es perfecta: una nota potente con reverb dejaba un resto que se marcaba como
  «segunda voz» sin que hubiera nadie más cantando.

  Medido sobre dos canciones, de lo que marcaba como coro sólo el **0 %** y el **9 %** tenía
  de verdad dos voces. Fuera de lo que marcaba había más segundas voces que dentro. Y el
  resultado lo decidía un número escrito a mano: moverlo una décima multiplicaba por siete
  lo detectado, y dos décimas lo borraban entero.

  El detector nuevo busca lo que de verdad distingue a un coro: que el resto **tenga su
  propia nota**, que la **sostenga**, y que esa nota forme un **intervalo musical** con la
  voz principal — terceras, cuartas, quintas, sextas, octavas. El ruido de una separación no
  canta en cuartas. Sobre las mismas dos canciones: **69 %** y **72 %** de acierto.

  Cada región detectada dice ahora en qué se basa —«dos voces, 5ª justa»— y con cuánta
  confianza, para que se pueda comprobar en vez de creer.

  El programa tenía **dos** detectores de coros: uno para la línea de tiempo y otro que
  reparte las segundas voces durante la mezcla. Se midieron los dos con dos varas —de lo que
  marca, cuánto es de verdad una segunda voz; y de las que hay, cuántas llega a marcar— y el
  nuevo gana en las dos, en las dos canciones probadas:

      canción A    antes  0 % y  0 %      ahora  66 % y 55 %
      canción B    antes  9 % y  4 %      ahora  73 % y  7 %

  Así que ahora los dos sitios usan el mismo, y el de energía se ha retirado del programa.

  **Se le escapan coros a propósito, y conviene saberlo.** Exige las tres señales a la vez, y
  ante la duda deja el tramo como voz principal. Perder un coro flojo significa que se clona
  como si fuera el cantante, que es molesto; marcarlo de más deja **sin clonar** un trozo de
  la voz principal, y entonces aparece la voz original del disco en mitad de una frase. De
  los dos errores, el segundo se oye muchísimo más.

- **Ningún enlace de YouTube se descargaba.** Al pegar cualquier dirección salía «No se
  pudo descargar ese video», daba igual el vídeo: público, conocido y sin restricción
  ninguna. La ficha del vídeo (título, duración) sí se leía bien; lo que YouTube cortaba
  era la descarga del audio en sí, rechazándola por venir con la firma del navegador.
  Ahora el audio se pide con la firma de la aplicación de Android, que YouTube sí atiende,
  y quedan otras dos de respaldo por si esa también se cierra.

- **La pantalla se quedaba en blanco justo al empezar a clonar.** Se pulsaba el botón, y en
  vez de la tarjeta que dice qué está haciendo el motor aparecía una pantalla vacía y
  congelada, con el programa entero inservible hasta reiniciarlo. Por dentro el trabajo
  seguía —la canción se clonaba y se guardaba igual—, pero no había forma de verlo.

  Un icono de esa tarjeta se quedó sin declarar al partir en trozos el archivo más grande
  de la interfaz. Es un fallo que no se ve al construir el programa: sólo revienta al
  ejecutarse, y como esa tarjeta **sólo aparece al clonar**, todo lo demás funcionaba. Se
  añadió una comprobación automática que revisa los cuarenta archivos de la interfaz y
  caza este tipo de error antes de que llegue a nadie; encontró otro igual escondido en la
  librería de voces.

- **Recargar el navegador en Cola o en Doblaje devolvía un texto de programador.** Esas dos
  pantallas se llamaban igual que dos direcciones internas del motor, así que al recargar
  ganaba el motor y salía un galimatías en vez de la aplicación.

- **El separador de seis pistas se estaba comiendo la guitarra y el piano.** Quien elegía
  `htdemucs_6s` en Ajustes —el modelo que la propia pantalla recomienda «por si quieres
  las pistas sueltas»— recibía la canción **sin guitarra y sin piano**, y sin ningún aviso.

  El motivo: ese modelo devuelve seis pistas y los otros dos devuelven cuatro, y el
  programa las cogía por su posición en la lista en vez de por su nombre. Sumaba las tres
  primeras como instrumental y daba la cuarta por voz. Con cuatro pistas eso funciona por
  casualidad; con seis, la guitarra y el piano están en las posiciones quinta y sexta y no
  entraban en la suma. El programa pagaba los minutos de separar seis instrumentos para
  tirar dos.

  Ahora cada pista se busca por su nombre y el instrumental es la suma de **todo** lo que
  no es voz, sea cual sea el modelo y sean cuantas sean sus pistas.

- **El interruptor de respiraciones no llegaba al modo de una sola voz.** Se apagaba en
  Ajustes, se guardaba, se veía apagado — y al clonar con una voz el motor seguía
  devolviendo las respiraciones del original, porque la pantalla nunca le mandaba ese dato.
  En multivoz sí funcionaba, así que apagarlo cambiaba el resultado o no según por qué
  botón se hubiera entrado.

  Ni ese interruptor ni el mando nuevo de cantidad se guardaban al cerrar, además: quien lo
  apagaba a propósito se lo encontraba encendido al volver a abrir el programa.

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

- **La vista previa de una voz salía con eco y sala aunque los tuvieras apagados.** No
  llegaba al motor ninguno de tus interruptores: la llamada mandaba sólo desde qué
  segundo preescuchar, y lo que no se manda el programa lo daba por activado. Así que la
  preescucha copiaba la sala del original, el color y el autotune medido pasara lo que
  pasara, y después la mezcla final —que sí los respetaba— sonaba distinta.

  Una vista previa está para predecir el render. Si aplica cosas que el render no va a
  aplicar, engaña, y es peor que no tenerla. Ahora manda exactamente lo que tienes puesto.

- **La pantalla de subida dice de dónde acepta enlaces, y aclara lo del vídeo.** Debajo
  del campo aparecen los sitios soportados, y la lista la manda el motor: no puede quedarse
  prometiendo un sitio que ya no acepta.

  Y se dice una cosa que antes había que adivinar: **el vídeo tiene que ser un archivo
  tuyo.** Es la confusión natural — si el programa acepta enlaces y acepta vídeos,
  cualquiera supone que le puede pegar el enlace de un vídeo y recuperarlo entero con la
  voz clonada. De un enlace sólo se trae el audio.

- **El campo de enlaces acepta ahora SoundCloud, Bandcamp y Suno, además de YouTube.**
  Se pega la dirección igual que antes y el resto del camino es el mismo: se descarga, se
  separa y se clona sin más pasos.

  Suno necesitó camino propio: la herramienta que usa el programa para todo lo demás se
  niega en redondo con esa web. Ahora se resuelve leyendo la página del tema, que sirve
  el audio ella misma. Probado con un enlace real de los cortos, de los que empiezan por
  `suno.com/s/`.

  **Vimeo y Audiomack se quedaron fuera a propósito.** Vimeo falla hoy en todos los
  vídeos que se probaron —es un problema del sitio, no de un vídeo concreto— y aceptar un
  enlace para luego no poder traerlo es peor que decir que no desde el principio.

  La lista de sitios sigue siendo cerrada. No es desconfianza hacia quien lo usa: sin esa
  puerta, ese campo de texto se convierte en un descargador de cualquier cosa de internet
  desde tu propio ordenador.

- **La consola ya no se inunda al descargar.** La barra de progreso escribía cientos de
  líneas de «12.4% of 4.05MiB» y enterraba las dos que se leen de verdad: qué se está
  bajando y si salió bien.

- **El doblaje salía desplazado y bajo, aunque hubieras grabado con el tiempo exacto.**
  Dos cosas a la vez, y las dos nuestras.

  Al pulsar Grabar, la imagen arranca **un segundo antes** de la frase — hace falta para
  ver la entrada y coger aire. Tú hablas cuando toca, así que tu toma llega con ese
  segundo de silencio dentro… y se colocaba entera al principio del hueco. Tu voz entraba
  un segundo tarde y por el final se salía y se cortaba. Ahora ese silencio se quita antes
  de montar, dejando un pelín de aire para no descabezar la primera sílaba.

  Y sonaba muy baja porque nada igualaba el nivel: un micrófono casero está muy por debajo
  de un vocal de disco. Ahora cada toma se lleva al nivel que tenía la voz original en ese
  mismo tramo, con tope de 18 dB — subir más solo levantaría el ruido de la habitación.

- **Grabar ya no clona: se pone la voz cuando tú quieras.** Antes, al soltar cada toma, se
  convertía con el modelo del personaje ahí mismo. Eso obliga a esperar —y a veces la
  conversión falla— por grabaciones que ibas a repetir de todas formas. Ahora se graba del
  tirón y cada intervención tiene su botón **Poner voz**, más uno de **Poner voz a todas**
  para dejarlo hecho al final. Tu grabación se guarda aparte de la convertida, así que se
  puede reintentar sin perder la interpretación.

- **El marcador acusaba de entrar tarde a quien entraba clavado.** La imagen arranca un
  segundo antes de la frase, así que la toma llega con ese silencio delante — y la nota lo
  contaba como retraso. Medido sobre una toma perfecta: **29 puntos y «entras a 1000 ms»**
  antes, **100 y 0 ms** ahora.

- **Grabar con una tecla, la que tú elijas.** Doblando se mira la imagen, no el ratón:
  buscar el botón mientras corre el clip es perder la entrada. La misma tecla arranca y
  suelta, sobre el clip que tengas elegido. Viene puesta en la **R** y se cambia pulsando
  «Grabar con R» y la tecla nueva. Se recuerda entre sesiones — el espacio sigue siendo
  play/pausa y no se puede reasignar a grabar.

- **La voz doblada sube hasta el volumen del clip original.** Se mide el volumen que tenía
  la voz en ese tramo y la toma se lleva ahí. Y se mide solo donde **suena**: promediando
  el archivo entero mandan tus silencios, así que una toma con pausas se quedaba corta
  aunque el ajuste dijera que cuadraba.

- **Mientras doblas un clip, lo demás se calla.** Los otros tramos de ese personaje y los
  de los demás. Doblando una frase, oír la anterior entrando por detrás solo despista — y
  el segundo de margen antes de la entrada solía traer la cola de quien acababa de hablar.

- **La grabación se detiene sola al acabar el clip**, y la onda de tu voz **se dibuja
  mientras hablas**, cayendo en su hueco en tiempo real. Antes había que soltar a mano
  justo al terminar la frase, y soltar tarde metía en la toma el arranque del siguiente.

- **El tramo de un hablante seguía marcado cuando el siguiente ya hablaba.** Se veía en la
  onda: la zona roja del hablante 1 se metía dentro de la primera frase del 2. Doblando es
  fatal — grabas la frase del 1 sobre un hueco que incluye el arranque del 2, y al montar
  tu voz le tapa la entrada.

  No era un error de cálculo sino la resolución de la herramienta: el análisis mira la
  canción en ventanas de tres segundos, así que sus fronteras llegan con hasta un segundo
  de retraso. Ahora, donde cambia el turno, la costura se lleva **al punto más callado que
  hay entre las dos voces**, que es donde de verdad cambia. Dos frases seguidas de la misma
  persona no se tocan: eso no es un relevo.

  Y como ninguna detección automática acierta siempre: **se puede volver a calcular** con
  el número de hablantes que le digas, y **cambiar a mano quién dice cada intervención**
  desde su propia fila.

- **Cargabas un vídeo en Doblaje y te decía que esa sesión no tenía vídeo.** Con el mp4
  delante. El límite de la versión de prueba, además de acortar el audio, **borraba el
  archivo original** — y de ahí es de donde sale la imagen. Ahora ese archivo se recorta
  en vez de borrarse: el límite se cumple igual (en el disco no queda material completo) y
  la imagen de los segundos que sí se pueden doblar se conserva.

- **En Doblaje se oía el instrumental en vez de las voces.** Al revés de lo que hace
  falta: doblando se imita a alguien, así que hay que oír cómo lo dice, con qué intención
  y en qué milésima entra. El instrumental tapaba justo eso. Ahora suena la voz original
  mientras trabajas, y el instrumental entra al final, al montar, que es donde la voz
  nueva tiene que convivir con la música.

- **Decir cuántos actores hay, en vez de fiarlo todo a la detección automática.** Cuando
  se equivoca de número reparte mal *todas* las intervenciones y no hay forma de
  arreglarlo salvo empezar de cero. Ahora se elige a mano junto al botón de detectar,
  igual que en IA Voz.

- **La frase que hay que decir, en pantalla.** Si la sesión tiene la letra sincronizada,
  cada intervención enseña su texto sobre la imagen mientras la doblas. Antes había que
  escuchar el tramo tres veces para aprenderse la frase antes de poder decirla.

- **El Modo Doblaje ya es un programa entero por su cuenta.** Antes te plantaba un cartel
  mandándote a IA Voz a pulsar un botón y volver. Ahora carga su propio vídeo, lo separa y
  detecta quién habla sin salir de la pantalla. Y sí: separa voz e instrumental, así que el
  doblaje conserva la música y los efectos del original — solo cambia la voz.

  **Cada personaje, con la voz que tú le pongas.** Grabas todas las intervenciones con tu
  micrófono y cada una sale con el modelo que le hayas asignado a ese personaje: una escena
  de tres actores se dobla en solitario sin que suenen los tres igual.

  **Y no se pisan.** Ajustar una toma la encajaba en la duración de *su* frase, lo cual
  vale cuando alrededor hay silencio y falla justo en el caso difícil: si el actor 1 tiene
  dos segundos y el actor 2 interrumpe a los 2,1, una toma un poco larga seguía sonando
  encima del otro. Ahora se mide el sitio real que hay **hasta la siguiente entrada** y se
  respeta; si eliges no ajustar, el programa te avisa de cuántas invaden en vez de que lo
  descubras oyéndolo.

  **Con nota.** Cada toma recibe tres puntuaciones medidas contra la interpretación
  original: cuándo entraste, cómo repartiste la intensidad y qué forma le diste a la
  entonación. No se puntúa el parecido de voz a propósito — no tienes la voz de ese actor y
  esa nota no diría nada de lo que has hecho.

  **El vídeo, a lo suyo.** Se recorta a 30 segundos al cargarlo: doblar es grabar frase a
  frase, y media escena es una sesión que se termina y un proyecto que no engorda. Al
  descargar se elige resolución, y hay una opción de limpiar la imagen para material bajado
  de redes: quita los bloques de la compresión y afila el contorno. **No inventa detalle** —
  un vídeo malo deja de parecer roto, no se vuelve uno bueno.

- **Escuchar cada pista por separado dentro del editor.** Cada carril —voz, instrumental,
  batería, bajo, guitarra, piano— tiene ahora su botón de reproducir. Suena esa pista
  sola, y suena **con los silencios que le hayas marcado puestos**: lo que se oye es lo
  que va a quedar, que es justo lo que hacía falta para saber si un corte está bien
  puesto. Marcar o quitar un tramo mientras suena se oye al momento, sin volver a darle
  al play.

  Encima de las ondas hay una regla: un clic lleva el cursor a ese instante, y el cursor
  cruza todos los carriles a la vez. Es lo que permite ver —y oír— que el silencio que
  acabas de marcar en la batería cae donde entra la voz.

  Suena un carril cada vez. Dos pistas a la vez ya es la mezcla, y para eso está el
  reproductor de abajo.

- **Dejar un tramo suelto sin clonar.** En la tarjeta de cada cantante, debajo del selector
  de segmento, hay una casilla nueva: **«No clonar este tramo»**. El tramo elegido sale con
  la voz original de la canción y **los demás segmentos de ese mismo cantante se clonan
  igual**.

  No es lo mismo que quitarle el modelo al cantante, que es lo único que se podía hacer
  hasta ahora: eso afecta a sus ocho o diez tramos a la vez. Esto es para el caso normal de
  que una frase suelta salga mal —un grito, un ad-lib, una palabra que el modelo no coge— y
  se prefiera dejarla como estaba antes que pelearse con ella.

  Por dentro no hay ningún camino nuevo: el tramo marcado sale del reparto, y todo lo que no
  reclama ningún cantante ya se mezclaba con la voz original. Lo único que hubo que añadir
  es protegerlo del relleno de huecos, que existe para lo contrario —que no quede voz sin
  clonar por un despiste del detector— y se lo habría devuelto a un cantante vecino,
  deshaciendo en silencio lo que se acababa de marcar.

- **La ventana de arranque enseña cuánto está gastando el programa.** Junto a los avisos
  de Backend y Frontend aparece ahora una tercera cifra: procesador, memoria y, si el
  equipo tiene tarjeta gráfica dedicada, memoria de vídeo. Al pasar el ratón por encima se
  desglosa en tres: el motor —con todo lo que va lanzando por su cuenta al separar o
  clonar—, la interfaz y la propia ventana.

  Es el gasto **del programa**, no el del equipo. Con el navegador y media docena de cosas
  abiertas, el número del administrador de tareas no dice si el motor está trabajando o
  atascado; éste sí. Sirve sobre todo para saber si merece la pena tocar el perfil de
  velocidad o si conviene cerrar algo antes de empezar una canción larga.

- **El programa se puede usar desde el móvil, la tablet o la tele.** El ordenador sigue
  haciendo todo el trabajo —separar, clonar y mezclar—; el aparato de al lado es sólo la
  pantalla. Desde él se carga cualquier canción, por archivo o por enlace, se elige la voz,
  se preescucha y se descarga el resultado.

  En **Ajustes** aparece la dirección a la que hay que ir, un código para escanear con la
  cámara y un PIN de seis cifras. Se escribe una vez por aparato y no lo vuelve a pedir.

  Dos cosas que conviene saber:

  · **El PIN no es un adorno.** El motor siempre ha escuchado a toda la red, así que hasta
    ahora cualquier aparato conectado a la misma wifi podía entrar y hacer de todo — subir
    y borrar voces, ver las canciones del disco, lanzar renders. Ahora hace falta el PIN,
    salvo desde el propio ordenador. Los aparatos conectados se ven en una lista y se
    pueden echar de uno en uno.

  · **Grabar con micrófono sigue siendo cosa del ordenador.** Los navegadores no dan acceso
    al micrófono fuera de una conexión segura, y una dirección de red local no lo es. No es
    un permiso que se pueda conceder: la función no está. El programa lo dice en vez de
    ofrecer un botón que iba a fallar.

  Es para tu red. Abrir el puerto del router hacia internet deja el motor entero expuesto
  con seis cifras delante, y eso es otra cosa.

- **La pantalla se adapta a un móvil.** El camino de clonar entero —cargar la canción,
  elegir voz y tono, preescuchar, clonar, la cola y descargar— cabe y se usa con el dedo.
  El menú de arriba se desplaza en vez de salirse, y las cabeceras se apilan en lugar de
  pelearse por el ancho: medido en una pantalla de 375 píxeles, la página se salía 64 y
  aparecía una barra de desplazamiento horizontal que no debería existir. El editor
  multipista se ve y se puede tocar, y avisa de que para recortar fino conviene el
  ordenador.

- **Una cola: sueltas diez canciones y te vas.** Hay una pestaña nueva, «Cola». Se elige la
  voz, se sueltan los archivos y el programa los va haciendo de uno en uno. No mejora ni un
  decibelio del resultado: lo que multiplica es cuántos haces, porque hasta ahora cada
  canción eran minutos de espera con una persona delante mirando la barra.

  Tres cosas que se decidieron a propósito:

  · **La cola sobrevive a cerrar el programa.** Vive en disco, no en la pantalla. Si
    Windows reinicia por una actualización a mitad de la tercera canción, al volver a abrir
    están las siete que quedaban y la tercera vuelve a estar pendiente — si se cortó a
    medias, no está hecha. Una cola que se evapora al reiniciar es peor que no tenerla,
    porque para entonces quien la dejó puesta ya se fue a dormir confiando en ella.

  · **Los ajustes viajan con cada canción.** Se copian al añadirla. Encolar cinco con una
    voz y cambiar de voz para la sexta deja las cinco primeras con la que tenían.

  · **Una que falle no para las demás.** Es justo el caso para el que existe: se dejan diez
    de noche, una viene con el audio roto, y las otras nueve tienen que estar por la mañana.

- **El editor tiene un carril por instrumento.** Debajo de voz e instrumental aparecen ahora
  la batería, el bajo, la guitarra, el piano y el resto — cada una con su onda y sus
  silencios. Se puede callar la batería sólo en el puente y dejar sonando al resto de la
  banda, que antes era imposible: el instrumental es la suma ya hecha y de ahí no se puede
  quitar un instrumento.

  Cuáles salen depende del modelo con el que se separó: `htdemucs` da batería, bajo y otros;
  `htdemucs_6s` añade guitarra y piano. Las sesiones separadas con una versión anterior no
  los tienen, y el programa lo dice en vez de aceptar un recorte que se iba a ignorar.
  También se pueden bajar sueltas desde la pantalla de exportar.

- **Acercar la línea de tiempo.** Hasta ahora cuatro minutos de canción se editaban siempre
  en el mismo ancho de pantalla, así que la precisión de cada recorte la decidía cuántos
  píxeles había. Ahora se acerca hasta ocho veces, con una sola barra de desplazamiento para
  todos los carriles —dos barras separadas dejarían de mirar al mismo instante de la canción
  en cuanto se moviera una— y los nombres de las pistas se quedan quietos a la izquierda. Al
  acercar se pide una onda más fina, no la misma estirada.

- **Copiar los recortes de un carril y pegarlos en otro.** Silenciar los mismos diez tramos
  en la voz y en el instrumental era hacer el trabajo dos veces.

- **Preescuchar por donde va sonando.** El botón de vista previa de una sola voz cogía
  siempre los ocho segundos más fuertes de la canción, que es un estribillo casi siempre.
  Sirve para la primera escucha y no para la segunda: si lo que se quiere comprobar es cómo
  canta el modelo el puente, o esa frase concreta que queda rara, no había forma de pedirlo.
  Hay un segundo botón, **DESDE DONDE SUENA**, que convierte el punto en el que esté el
  reproductor. Debajo se dice desde qué minuto salió la última.

- **Cuánta respiración del original, cantante a cantante.** La protección de respiraciones
  presta el aire de la persona original allí donde el motor sólo sabe gruñir. Cuánto se nota
  ese préstamo depende de lo lejos que esté el timbre del modelo del de esa persona: a tope,
  con un modelo lejano, se oye como un pegote de otra grabación. Antes era todo o nada y
  para toda la canción, así que en un dueto había que elegir a quién le sonaba bien. Ahora
  hay un mando general en Ajustes y otro en la tarjeta de cada cantante, y a cero se apaga
  sólo para esa voz.

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

- **Decirle al programa que dos cantantes detectados son la misma persona.** La detección
  agrupa por timbre y se equivoca casi siempre en el mismo sentido: **parte a una persona en
  varias**. Una voz que grita en el estribillo y susurra en la estrofa tiene dos timbres
  bastante distintos, y acaban en montones separados. Es el error cómodo de cometer: juntar
  a dos personas suena mucho peor que separar a una.

  Se podía sortear asignando el mismo modelo a todos esos montones, pero deja tarjetas de
  sobra que mantener, tonos que igualar uno a uno y una costura en cada relevo entre dos
  trozos de la misma voz. Ahora se marcan los que sean el mismo y se unen en uno: los tramos se
  reetiquetan, se renumeran sin dejar huecos y los que quedan pegados se funden. El modelo
  que ya tenían asignado viaja con ellos.

  Y no hay que volver a analizar la canción — ni para esto ni para probar otro número de
  voces. Las huellas del análisis se guardan con la sesión, así que reagrupar cuesta
  segundos en vez de repetir los minutos del análisis completo.

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

- **Editor va justo al lado de IA Voz en el menú.** Son los dos lados del mismo trabajo
  —se analiza y se clona en uno, se recorta y se reparte en el otro— y se va y se vuelve
  entre ellos todo el rato, mientras que Doblaje se hace una vez y no vuelve. Tenerlos
  separados por Doblaje obligaba a saltárselo en cada ida y vuelta.

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
