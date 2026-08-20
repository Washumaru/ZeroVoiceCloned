# Cómo se trabaja en esto

Y la pregunta incómoda del final: por qué se publica con el doblaje sin terminar.

---

## Nada entra «a oído»

La regla es una y no tiene excepciones: **si una mejora no se puede medir, no entra.**

Suena severo para un proyecto de una persona, pero es justo al revés — es lo único que
permite trabajar solo. Sin nadie que revise, el oído se acostumbra a lo que uno mismo acaba
de hacer: a los veinte minutos de tocar un parámetro, la versión nueva siempre suena mejor.
Los números no se acostumbran.

Cuando toco algo de la cadena de audio:

1. **Se mide antes.** El estado actual, con cifras concretas: nivel por bandas, techo de
   frecuencia, desviación de tono, lo que aplique al caso.
2. **Se hace el cambio.**
3. **Se mide después, sobre el mismo material.**
4. **Si no mejora, se descarta** — aunque me guste cómo suena, aunque haya costado toda una
   tarde.

Por eso en la [lista de características](CARACTERISTICAS.md) hay tablas con decimales en
vez de adjetivos. Esos números son el motivo por el que cada cosa está ahí.

El ejemplo que mejor lo explica: durante semanas la voz clonada sonaba apagada y ningún
ajuste lo arreglaba. Medido, el volumen estaba **+0,2 dB** respecto al original — o sea,
correcto. El problema estaba en la banda de 8–12 kHz, hasta **−69,1 dB** en el peor modelo.
A oído era «suena mal»; medido era un problema concreto con una solución concreta. Sin
medir, habría seguido moviendo faders para siempre.

---

## Con qué modelos se prueba, y por qué son malos a propósito

Los modelos de voz con los que desarrollo **no son buenos**. Están entrenados con poco
tiempo y con material mediocre: notas de voz, llamadas, grabaciones a 16 kHz, audio de
Discord. Y es deliberado.

La razón es sencilla: **eso es lo que va a tener la mayoría.** Entrenar un modelo decente
lleva horas de audio limpio y horas de máquina, y la gente que descarga un estudio de
clonación de voz normalmente llega con lo que pudo entrenar en una tarde, o con un modelo
que se bajó de algún sitio.

Si el programa solo suena bien con modelos excelentes, no sirve para casi nadie.

Así que el criterio al ajustar cualquier cosa es: **que un modelo mal entrenado suene lo
mejor posible.** Con uno bueno, todo lo que se hace por el malo suma igual — al revés no
funciona. Un programa afinado contra material perfecto se derrumba en cuanto le das
material real.

De ahí sale, literalmente, la función de la que más orgulloso estoy. El injerto de banda
alta nació midiendo cuatro modelos y encontrando que dos de ellos tenían el techo de
frecuencia en 9 000 y 7 274 Hz — sordos de fábrica, incapaces de producir el aire de una
voz por mucho que se les pida. En vez de descartar esos modelos por malos, el programa les
presta esa banda del vocal original de la canción. **Recupera hasta +43 dB en un modelo que
no debería poder sonar bien.**

Esa función no habría existido nunca desarrollando con modelos buenos. Ni se habría notado
el problema.

---

## Las pruebas automáticas

**Más de mil comprobaciones** que se ejecutan antes de dar por bueno un cambio: 951 sobre
el motor de audio y 115 sobre la interfaz — 1.066 en total, y la tabla de abajo suma
exactamente eso.

No están repartidas por igual. **La cobertura sigue a las cicatrices**: las zonas con más
pruebas son las que más veces se rompieron, no las más grandes. El espacio y la
reverberación encabezan la lista porque se rompieron de más formas distintas que ninguna
otra parte.

<!-- CUENTA-PRUEBAS:INICIO (lo escribe tools/contar_pruebas.py) -->

| Área | Comprobaciones |
|---|---:|
| Espacio y reverberación | 37 |
| Letra y tiempos por palabra | 34 |
| Doblaje e intervenciones | 33 |
| Pronunciación: índice y protección por fotograma | 33 |
| Limpieza del reparto de cantantes | 31 |
| Relevos entre cantantes | 31 |
| Voz doble y duetos | 27 |
| Detección de intérpretes | 25 |
| test_acceso_remoto | 24 |
| Velocidad y escala de la afinación | 24 |
| Consonantes en pasada aparte | 22 |
| test_coros | 21 |
| Índice por tipo de sonido | 21 |
| No volver a separar lo ya separado | 21 |
| Exportar las pistas por separado | 21 |
| Niveles de voz e instrumental | 21 |
| Equilibrio contra una canción de referencia | 20 |
| El proyecto en un archivo | 20 |
| Unir cantantes que son la misma persona | 19 |
| Volumen del máster y pico real | 19 |
| Recetas de mandos guardadas | 19 |
| Edición de tramos y silencios | 19 |
| Cadena del Estudio y balance de la mezcla | 18 |
| Respiraciones devueltas del original | 18 |
| Registro del modelo, tramo a tramo | 17 |
| Datos de la pista | 17 |
| Reparación del tono antes de clonar | 16 |
| test_realce_medido | 16 |
| Plan de efectos | 16 |
| Color tonal | 16 |
| Mezcla y máster | 15 |
| Video e imagen | 15 |
| test_cola_render | 14 |
| test_doblaje_juego | 14 |
| test_stems_partes | 14 |
| test_relevo_en_hueco | 13 |
| test_doblaje_tomas | 11 |
| test_pegar_a_saltos_de_tono | 11 |
| Afinación | 11 |
| Precisión de los efectos | 10 |
| test_respiracion_cantidad | 10 |
| Dicción y consonantes | 10 |
| Importación por enlace | 10 |
| test_enlaces_sitios | 9 |
| Huella del equipo y licencia | 9 |
| test_sin_clonar | 9 |
| test_video_doblaje | 9 |
| Quitar sala del original | 9 |
| Reparación del clon (agudos y caídas) | 9 |
| Dinámica y respiración | 8 |
| Cruces entre cantantes | 7 |
| test_doblaje_precision | 7 |
| Atadura de la licencia al equipo | 7 |
| test_doblaje_fronteras | 6 |
| Aislamiento de las pruebas | 5 |
| Efectos aplicados al render | 5 |
| Análisis de efectos del tema | 4 |
| Lo marcado a mano sobrevive al render | 4 |
| test_endpoints_responden | 4 |
| test_injerto_rampa | 3 |
| test_video_no_se_borra | 3 |
| Interfaz: progreso, tiempos, historial, exportación, avisos previos | 115 |
| **Total** | **1066** |

<sub>951 sobre el motor de audio en 61 grupos y 115 sobre la interfaz en 11. Tabla generada por `tools/contar_pruebas.py` leyendo el código: no se escribe a mano, así que no puede quedarse desfasada.</sub>

<!-- CUENTA-PRUEBAS:FIN -->

### El doblaje aparece con 33 y por eso mismo está «en pruebas»

Mirando la tabla, 33 parece mucho: es la segunda cifra más alta. **Y sin embargo es
justamente el motivo de la advertencia.** Hay que leerlo en contexto.

Esos 33 son de las funciones con más comprobaciones propias, sí — pero es que **el doblaje
no es una función, es un sistema nuevo entero** dentro de la aplicación. Tiene su propia
detección de intervenciones, su propio reparto de personajes, su sincronía con la imagen,
sus avisos de entrada, su grabación y su montaje sobre el vídeo.

Compáralo con lo que costó que la otra mitad del programa funcionara bien:

| | Comprobaciones | Grupos |
|---|---:|---:|
| La cadena de audio (separación, clonación, reparto, tratamiento) | 795 | 51 |
| El doblaje y el vídeo | 98 | 8 |

**795 comprobaciones repartidas en 51 grupos** hicieron falta para que clonar una voz sobre
una canción funcione de verdad. El doblaje tiene una ambición parecida y va con 98, que ya
no es poco: la sincronía de las tomas, el encaje entre hablantes y el vídeo tienen su
propia red. Sigue siendo la parte más joven del programa.

No es que esté sin probar: está probado en su lógica —que los tramos se calculen bien, que
los avisos caigan donde toca, que el reparto sea el correcto— y todas esas comprobaciones
pasan. Lo que no tiene todavía es **la cobertura que se ganó el resto a base de romperse**,
ni el uso real de gente distinta con vídeos distintos, que es lo único que enseña dónde
falla de verdad un sistema así.

Por eso la marca sigue puesta. Cuando ese número se parezca al de la cadena de audio y haya
horas de uso ajeno detrás, se quita.

### Y cuando una falla

**El cambio no se publica.** No hay «lo dejo así de momento» ni «ya lo miro luego»: o pasa,
o no sale de aquí.

Y cada vez que aparece un fallo que las pruebas no vieron venir, lo primero es **escribir la
prueba que lo habría cazado**, antes de arreglarlo. Así el mismo fallo no puede volver dos
veces. Buena parte de ellas nacieron exactamente así.

---

## Por qué esto hace que todo tarde tanto

Conviene decirlo entero, porque es la consecuencia directa de lo de arriba: **lo mismo que
hace fiable al programa es lo que lo hace lento.**

Una función no es «escribirla». Es escribirla, medirla contra lo anterior, cubrirla con
pruebas, encontrar los casos donde falla, arreglarlos, y volver a medir. La parte de
escribir suele ser la más corta. Y todo eso sale del rato que queda después del trabajo y
de los estudios: noches sueltas y fines de semana, con semanas enteras en las que no toco
nada.

Súmale con qué cuento:

- **No hay equipo.** Nadie con quien repartir el trabajo, nadie a quien preguntarle por qué
  algo suena raro, nadie que revise lo que hice.
- **No hay probadores.** Ni un grupo cerrado, ni versiones previas repartidas. El único que
  prueba soy yo.
- **Hay una computadora. La mía.** Un solo Windows, una sola tarjeta, una sola combinación
  de todo. Cuando digo que algo está probado, está probado **ahí**.

Ese último punto es el más limitante y el más incómodo de reconocer. Un fallo que solo
aparece con otra tarjeta gráfica, otra versión de Windows o un audio con una peculiaridad
que yo no tengo a mano, **no lo puedo encontrar por mucho tiempo que le dedique**. No es
falta de ganas: es información a la que no tengo acceso.

Por eso los reportes valen tanto aquí. No son una queja: son literalmente la única forma de
ver lo que mi equipo no puede enseñarme.

**Y por eso entre una versión y la siguiente puede pasar mucho más tiempo del que parezca
razonable.** Podría publicar el triple de rápido saltándome las mediciones y las pruebas —
y publicaría el triple de cosas rotas. Prefiero tardar.

---

## La pregunta del millón: ¿por qué publicar con el doblaje sin terminar?

Es lo primero que me preguntaría yo, así que va con respuesta directa.

**Qué pasó.** Trabajando en el modo de doblaje, algo se rompió de forma inesperada: parte
del trabajo se dañó y no hubo manera de recuperarlo. Me tocó volver atrás, a un punto
anterior, y rehacer camino ya recorrido. Es de las cosas que en un equipo se resuelven en
una tarde entre varios, y que solo, con las horas que dejan un empleo y unos estudios, se
convierte en semanas.

**Qué opciones había**, y ninguna era buena:

1. **Retrasar todo el programa** hasta terminar el doblaje. Habría dejado parado durante
   meses lo que ya funciona y está medido: la separación, la clonación, el reparto de voces,
   toda la cadena de tratamiento.
2. **Quitar el doblaje** y no mencionarlo. La función existe, se puede usar y a mucha gente
   le sirve tal como está. Esconderla habría sido tirar trabajo que funciona.
3. **Publicarla diciendo exactamente en qué punto está.**

Elegí la tercera.

**Y por eso está marcada en todas partes**: dentro del programa, con una etiqueta *EN
PRUEBAS* junto al título y un aviso permanente en su pantalla; en el LÉEME; en la página de
inicio; en la descripción de la tienda; y en la hoja de ruta, donde el doblaje aparece en
«en medición» y no en «ya está». Es la única función del programa con esa marca, y no está
puesta por prudencia genérica: está puesta porque es verdad.

**Lo que no voy a hacer** es cobrarla como si estuviera terminada y esperar a que se note.
Si estás comprando principalmente por el doblaje, quiero que lo sepas antes de pagar,
aunque eso me cueste la venta. Prefiero un comprador menos que uno que pagó esperando otra
cosa.

### Qué falta para quitarle esa etiqueta

Para poder decir que está terminado, no basta con que funcione en mi equipo con mis vídeos.
Hace falta:

- Que la sincronía aguante en material variado: vídeos largos, con cambios de ritmo, con
  varias personas hablando encima.
- Cubrirlo con pruebas automáticas al nivel del resto de la cadena.
- Uso real de gente que no sea yo, que es justo lo que ahora mismo no tengo.

Cuando eso esté, la etiqueta desaparece y se dirá en el registro de cambios. Sin fecha:
prefiero no dar ninguna antes que dar una y fallarla.

---

## Lo que esto significa para ti

Que cuando el programa dice que algo funciona, hay una medición detrás. Y que cuando algo no
está terminado, **lo dice él mismo antes de que lo descubras tú**.

Es la única forma de vender un programa hecho por una persona sola sin pedirle a nadie un
acto de fe.
