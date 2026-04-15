# Unidad 6

## Bitácora de proceso de aprendizaje

#### Actividad 1

Decisiones visuales que reconozco

Composición:

La imagen ocupa todo el espacio con líneas que fluyen de un extremo a otro sin dejar zonas vacías. No hay un punto focal único, sino una distribución uniforme.

Densidad:

Hay zonas donde las líneas están más juntas, generando mayor intensidad visual, y otras más dispersas que permiten “respirar”.

Dirección del movimiento:

Las líneas siguen trayectorias curvas, como si estuvieran guiadas por fuerzas invisibles. Parecen simular viento o corrientes.

Color:

Se utilizan degradados suaves entre colores cálidos (naranjas, rojos) y fríos (azules), generando contraste y profundidad.

Ritmo:

El ritmo se percibe en la repetición de líneas, pero con pequeñas variaciones que evitan que se vea mecánico.

Repetición y variación:

Las líneas se repiten en forma, pero cambian en dirección, grosor o color, lo que hace que la pieza se sienta orgánica.

Creo que esta obra está basada en un sistema de flow fields, donde:

Existe un campo vectorial como una “grilla invisible” que define la dirección en cada punto.
Muchas partículas o agentes recorren ese campo siguiendo esas direcciones.
Cada agente deja un rastro línea.
Se aplican pequeñas variaciones aleatorias en:

-color
-grosor
-elocidad

El color podría depender de la posición o del tiempo.




#### Actividad 2




-¿Qué es un agente autónomo

Un agente autónomo es una entidad que puede moverse y tomar decisiones por sí misma dentro de un entorno. No depende de que alguien le diga exactamente qué hacer en cada momento, sino que sigue ciertas reglas internas que le permiten reaccionar a lo que ocurre a su alrededor.A diferencia de un objeto pasivo (como una pelota que solo cae por gravedad), un agente autónomo tiene “comportamiento”. Por ejemplo, puede buscar un objetivo, evitar obstáculos o seguir a otros agentes.

-¿Qué es una steering force?

Una steering force es una fuerza que guía el movimiento de un agente según una intención o comportamiento. No es solo una fuerza física, sino una forma de traducir decisiones en movimiento. Por ejemplo:

Ir hacia un objetivo (seek)
Huir de algo (flee)
Llegar suavemente a un punto (arrive)

En pocas palabras:
 Es la forma en que un agente “decide hacia dónde moverse”.

Diferencia entre steering force y fuerzas físicas (gravedad, viento, fricción)

Fuerzas físicas (gravedad, viento, fricción):

Son externas al objeto.
No tienen intención.
Siempre actúan igual según las leyes físicas.
Ejemplo: la gravedad siempre tira hacia abajo.

-Steering force:

Es interna al agente. Representa una intención o comportamiento. Puede cambiar dependiendo del contexto. Ejemplo: un agente puede decidir perseguir algo o evitarlo.

-En resumen:

Las fuerzas físicas “empujan”,
las steering forces “dirigen con intención”.

¿Por qué esto es útil para diseñar comportamiento visual?

Estas ideas son importantes porque permiten crear sistemas visuales que parecen vivos, en lugar de simples animaciones mecánicas.

En vez de animar todo manualmente, puedes definir reglas como:

seguir un punto
evitar otros agentes
moverse en grupo

Y el resultado es un comportamiento emergente, es decir, patrones complejos que surgen de reglas simples.

Esto es muy útil en arte generativo porque:

Hace que las visuales sean dinámas y no repetitivas. Permite interacción (por ejemplo, con el mouse o el sonido). Da la sensación de que el sistema “reacciona” y tiene vida propia. Idea clave final

No estamos animando objeto
Estás diseñando comportamientos.



#### Actividad 3

-¿Cómo está construido el campo de flujo?

El campo de flujo está construido como una grilla (matriz 2D) que cubre toda la pantalla. Cada celda de esa grilla contiene un vector que indica una dirección. Esa dirección no es aleatoria totalmente, sino que se genera usando Perlin Noise, lo que hace que el campo tenga continuidad y se vea fluido en lugar de caótico.

-¿Qué representa cada celda o vector?

Cada celda representa una dirección de movimiento en ese punto del espacio. Es como si el espacio estuviera lleno de pequeñas flechas invisibles que le dicen a los agentes hacia dónde moverse.

-¿Cómo usa un agente su posición para consultar el campo?

El agente toma su posición (x, y) y la convierte en una posición dentro de la grilla:

Divide su posición por la resolución del campo
Obtiene la celda correspondiente
Consulta el vector guardado en esa celda

Es como preguntar:
“¿Qué dirección debo seguir aquí donde estoy?”

-¿Cómo se convierte ese vector en una decisión de movimiento?

-El proceso es:

El agente obtiene el vector del campo (desired).
Lo escala según su velocidad máxima (maxspeed).
Calcula la diferencia entre ese vector y su velocidad actual → (steering).
Limita esa fuerza con maxforce.
Aplica esa fuerza a su aceleración.

 -En pocas palabras:
El agente ajusta su movimiento poco a poco para alinearse con la dirección del campo.

-Parámetros importantes del sistema

resolution:

Define el tamaño de cada celda.
Alta resolución → más detalle
Baja resolución → movimiento más simple
maxspeed:
Velocidad máxima del agente.
Controla qué tan rápido se mueve.
maxforce:
Qué tan fuerte puede girar o cambiar de dirección.
Bajo - movimiento suave
Alto - cambios bruscos
cantidad de agentes:
Más agentes → mayor densidad visual
Menos agentes → más vacío y claridad
Modificación realizada

Modificación: aumenté el maxforce.

Efecto visual:

Los agentes giran más rápido.
El movimiento se vuelve más caótico y menos fluido.
Se pierde un poco la sensación de “corriente natural”.

Conclusión:
El maxforce afecta directamente qué tan orgánico o agresivo se ve el sistema.

¿Qué tipo de movimiento produce este algoritmo?

Produce un movimiento fluido, continuo y orgánico, parecido a:

-corrientes de agua
-viento
-humo
-fluidos invisibles

¿Qué sensaciones visuales sugiere?

Calma y fluidez (cuando los parámetros son suaves)
Hipnosis o repetición relajante
También puede volverse caótico si se aumentan velocidades o fuerzas

Se siente como un sistema “vivo”, no mecánico.

¿En qué tipo de pieza musical funcionaría?

Este tipo de sistema funcionaría muy bien con:

música ambiental
electrónica suave
lo-fi
música experimental

Porque el movimiento continuo y orgánico acompaña sonidos que no son bruscos, sino envolventes.

Idea clave final

El flow field no anima directamente a los agentes…
Diseña un “campo de decisiones” que todos siguen.


#### Actividad 4



1. Reglas básicas del sistema

El comportamiento del sistema se basa en tres reglas simples:

-Separación (Separation):

Cada agente evita acercarse demasiado a los demás.
Hace que no se amontonen ni colisionen.

-Alineación (Alignment):

Cada agente intenta moverse en la misma dirección que sus vecinos cercanos.
Genera movimiento grupal coherente.

-Cohesión (Cohesion):

Cada agente intenta acercarse al centro del grupo.
Mantiene unido al conjunto.

 2. Parámetros que controlan estas reglas
    
-desiredSeparation:

Distancia mínima entre agentes (afecta separación).

-neighborDistance:

Distancia para considerar vecinos (afecta alineación y cohesión).
-maxspeed:

Velocidad máxima de cada agente.
-maxforce:

Qué tan fuerte puede cambiar de dirección.
pesos de las fuerzas (mult):

En el código:
```js
sep.mult(1.5);
ali.mult(1.0);
coh.mult(1.0);
````

 Estos valores determinan qué regla domina el comportamiento.

 3. Modificación realizada

Modificación: aumenté el peso de separación:

```js
sep.mult(2.5);

Efecto visual:
```

Los agentes se alejan más entre sí. El grupo se vuelve más disperso. Se pierde un poco la sensación de “bandada unida”.

Conclusión:

La separación alta rompe la cohesión del grupo.


Modificación 2: aumenté cohesión:

coh.mult(2.0);

Efecto:

Los agentes se agrupan más.
Se forman clusters o núcleos.
Movimiento más compacto.

4. Comportamiento emergente observado

Dependiendo de los parámetros, el sistema puede verse:

Fluido: cuando hay buen balance entre reglas
Compacto: cuando domina cohesión
Disperso: cuando domina separación
Nervioso: cuando maxforce es alto
Caótico: cuando todo está desbalanceado

 En el caso base del ejemplo:
El comportamiento es fluido y relativamente estable, simulando una bandada natural.

 ¿Qué atmósfera visual produce?

Produce una atmósfera:

Orgánica
Viva
Natural
Dinámica

Se siente como observar:

-aves volando
-peces en cardumen
-multitudes en movimiento

Da la sensación de inteligencia colectiva.

-¿Con qué tipo de música funcionaría?

Este sistema funcionaría muy bien con:

música electrónica rítmica
house o techno suave
música instrumental dinámica

Porque:

El ritmo puede sincronizarse con el movimiento del grupo Cambios en la música podrían afectar los parámetros (más caos o más orden)

Idea clave final

Cada agente sigue reglas simples…
pero juntos crean comportamiento complejo e “inteligente”.



## Bitácora de aplicación 


## Bitácora de reflexión
