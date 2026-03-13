# Unidad 4


## Bitácora de proceso de aprendizaje

####  Actvidad 1 
Lo que mas me  llamo la atencion de la obra de memo anque me vi tambien primer video era que el jugaba con patrones geometricos y formas en relacion de la musica para generar un impacto visual secuencial contruido de manera en que cuando el timbre de una nota musical se sentia relajante se generaba ondas muy bonitas  que hacian llamar mucho la atencion ademas de  que me parecia muy satisfactorio de ver las figuras entrar y salir al agua y esos patrones que hacian generar un rombo se ven muy increibles


####  Actvidad 2 

-En la simulación se observa una línea que esta compuesta con dos círculos en los extremos que gira continuamente alrededor de su centro hacia la derecha indefinidamente basicamente esto ocurre gracias a esta nueva variable introducida por el docente llamada ``Angle()`` la interaccion ocurre porque se declaro esta funcion ``keyPressed()`` la cual es llamada al presionar una tecla del teclado por obvias razones donde se incremanta el valor de angle  0.1 porque esta asi declarado en la interaccion que el programa propones 


-El Origen del sistema de coordenadas se translada a la pantalla porque en cada frame se esta usando la funcion ``translate(width/2, height/2)``para mover el orgen del sistema de coordenadas al centro del canvas esto pasa porque por defecto el origen del sistema de coordfenadas en p5.js esta en la esquina superior izquierda de la pantalla si el objeto rotara alrededor de ese punto pareceria como si girara al rededor de la esquina, al trasladar el origen a la mitad del canvas esto genera que visualmente esta rotando en el punto medio del canvas lo que le da una estatica visual mas atractiva y natural al hacer dicho procedimiento.


-La relacion entre el sitema de coordenadas y la funsion ``rotate()`` ambos comparten que lo que en realidad se modificas la orientacionn del sistema de coordenadas y luego todos los objetos que se dibujen se veran afectados por esa rotacion, en conclusionel sistema de coordenadas es como un marco de referencia y que cuando se dibuja se rota ese marco todos los elementos dibujados sobre el aparecen rotados

-A pesar de que los elementos se dibujen en las coordenadas  (0,0) aparecen rotados ya que como dije anteriormente al estar vinculados dentro de una referencia especifica y esa referencia ya tenga unas modificaciones en un giro establecido  pues todos los elementos nuevos que se dibujen aparecerean instanciados por sus modifiaciones de sus elementos padres asi como el ejemplo que puse del marco

-Basado en los conceptos que ya conocemos del marco 101 acerca del la relacion entre velocidad, aceleracion, posicion y basicamente en esta simulacion se aplican dichos pasos primero se calcula la acelaracion que apunta hacia el mouse, despues la aceleracion modifica la velocidad, la velocidad modifica la posicion y la posicion determina donde se dibuja el objeto, esto se puede obsevar en el metodo ``update ()`` donde se confirma lo que acabe de decir se calcula la direccion hacia el mouse, esa direccion se convierte en aceleracion, la aceleracion se suma a la velocidad, y la velocidad se suma a la posicion, este proceso ocurre en cada frame generando un movimiento suave y continuo 

- La función ``heading()`` devuelve el ángulo de un vector en radianes con respecto al eje horizontal.

En este caso se utiliza:

```js
let angle = this.velocity.heading();
```

Esto sognifica que primero se obtine su angulo de direccion en su vector velocidad, lo cual nos ayuda a que ese angulo rote el objeto grafico, haciendo que el rectangulo siempre apunte hacia la direccion donde se esta moviendo

de esta forma esto no hace que el rectagulo unicamente se mueva si no que tambien oriente su direccion visual segun su direccion



-Basicamente push y pop guardan y restauran el estado del sistema de transformaciones

Esto es útil porque las transformaciones como translate() y rotate() afectan todo lo que se dibuja después. Al usar push() y pop(), se puede aplicar una transformación solo a un objeto específico sin afectar el resto de la escena.

-La función rectMode(CENTER) cambia la forma en que se dibujan los rectángulos. Por defecto, los rectángulos se dibujan usando la esquina superior izquierda como referencia. Con rectMode(CENTER), el rectángulo se dibuja desde su centro. Esto es importante cuando se aplica una rotación, porque si el rectángulo rota alrededor de su centro el movimiento se ve natural. Si rotara desde una esquina, el objeto parecería girar de forma incorrecta. 

¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad? 

-El vector de velocidad indica la dirección en la que el objeto se está moviendo.

Cuando se calcula:

angle = velocity.heading()

se obtiene el ángulo de ese vector.

Luego el sistema hace: Traslada el sistema de coordenadas a la posición del objeto. Rota el sistema de coordenadas según el ángulo de la velocidad. Dibuja el rectángulo. Como resultado, el rectángulo queda alineado con la dirección del movimiento.

Si se dibujara el vector de velocidad en un papel, este apuntaría exactamente hacia la misma dirección en la que apunta el rectángulo en la simulación.


#### Actividad 3

Usé el marco Motion 101 donde el movimiento se calcula a partir de fuerzas que generan aceleración. La aceleración modifica la velocidad y la velocidad modifica la posición. Las teclas de flecha aplican fuerzas horizontales que aceleran el vehículo. Para que el vehículo apunte hacia la dirección del movimiento, se obtiene el ángulo del vector de velocidad usando heading() y se rota el sistema de coordenadas antes de dibujar el triángulo.


```js
/// vehiculo basado en Motion 101

class Mover {
  constructor() {
    this.mass = 1;
    this.position = createVector(width / 2, height / 2);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.topspeed = 5;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    // motion 101
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);

    this.acceleration.mult(0);
  }

  show() {

    // ángulo según dirección de movimiento
    let angle = this.velocity.heading();

    push();
    translate(this.position.x, this.position.y);
    rotate(angle);

    stroke(0);
    strokeWeight(2);
    fill(150);

    // vehículo triangular
    triangle(-15, -10, -15, 10, 20, 0);

    pop();
  }

  checkEdges() {

    if (this.position.x > width) {
      this.position.x = width;
      this.velocity.x *= -0.5;
    }

    if (this.position.x < 0) {
      this.position.x = 0;
      this.velocity.x *= -0.5;
    }

    if (this.position.y > height) {
      this.position.y = height;
      this.velocity.y *= -0.5;
    }

    if (this.position.y < 0) {
      this.position.y = 0;
      this.velocity.y *= -0.5;
    }

  }
}
```


```js
let mover;

function setup() {
  createCanvas(640, 240);
  mover = new Mover();
}

function draw() {
  background(255);

  // gravedad suave
  let gravity = createVector(0, 0.05);
  mover.applyForce(gravity);

  mover.update();
  mover.show();
  mover.checkEdges();
}


function keyPressed() {

  // flecha derecha acelera hacia la derecha
  if (keyCode === RIGHT_ARROW) {
    let force = createVector(0.5, 0);
    mover.applyForce(force);
  }

  // flecha izquierda acelera hacia la izquierda
  if (keyCode === LEFT_ARROW) {
    let force = createVector(-0.5, 0);
    mover.applyForce(force);
  }

}
```

https://editor.p5js.org/TheWarrior710/sketches/phGIEcfar

<img width="547" height="216" alt="image" src="https://github.com/user-attachments/assets/117eba87-8588-4df6-a8b8-dd4379d3654c" />


#### Actividad 4


1. ¿Dónde está el marco Motion 101 en la simulación?

El marco Motion 101 aparece dentro de la función update() de la clase Mover.

```js
this.velocity.add(this.acceleration);
this.position.add(this.velocity);
this.acceleration.mult(0);
```

Este fragmento representa el modelo básico del movimiento usado en The Nature of Code.

El orden es el siguiente:

La aceleración modifica la velocidad
```js
this.velocity.add(this.acceleration);
```
La velocidad modifica la posición
```js
this.position.add(this.velocity);
```
La aceleración se reinicia
```js
this.acceleration.mult(0);
```
Esto permite que cada frame se calcule un nuevo movimiento del objeto.



2. ¿Qué modificación hay que hacer al Motion 101 cuando se agregan fuerzas acumulativas?

Cuando se agregan fuerzas, no se modifica directamente la velocidad. Primero las fuerzas se convierten en aceleración usando la segunda ley de Newton.

𝐹 = 𝑚 ∗ 𝑎

Entonces:

𝑎 = 𝐹 / 𝑚

Por eso aparece el método:

```js
applyForce(force)
applyForce(force) {
  let f = p5.Vector.div(force, this.mass);
  this.acceleration.add(f);
}
```
Aquí sucede algo importante: Cada fuerza se suma a la aceleración Varias fuerzas pueden actuar al mismo tiempo


Todas se acumulan antes de actualizar el movimiento

Luego en update() se aplica Motion 101.


Esto permite simular fenómenos físicos como:

-gravedad

-fricción

-resistencia del aire

-atracción gravitacional



3. ¿Dónde está el Attractor en la simulación?

El Attractor es el objeto que ejerce la fuerza gravitacional sobre los movers.

Se crea en sketch.js:
```js
let attractor;

function setup() {
  createCanvas(640, 240);
  attractor = new Attractor();
}
```
Y se dibuja en draw():
```js
attractor.display();
```
El attractor está ubicado en el centro de la pantalla:
```js
this.position = createVector(width / 2, height / 2);
```
Su función es generar una fuerza que atrae a los objetos usando la fórmula de gravedad.




4. Cambiar el color del Attractor

El color se define en display() dentro de Attractor.js.

Código original:
```js

fill(175, 200);
```

Por ejemplo puedes cambiarlo a rojo:
```js

fill(255, 80, 80);
```

O azul:
```js
fill(80,150,255);
```


5. ¿Para qué sirven dragging y rollover?

Estas variables permiten interacción con el mouse.

Variable	Función
dragging	indica si el attractor se está arrastrando
rollover	indica si el mouse está encima

Pero en el código original no están implementadas todavía.


6. ¿Cómo hacer que funcione el arrastre con el mouse?

Para esto se usan funciones de interacción de p5.js como:

mousePressed()

mouseReleased()

mouseX

mouseY

dist()



 
attractor.js

```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Attractor {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.mass = 20;
    this.G = 1;
    this.dragging = false;
    this.rollover = false; 
  }

  attract(mover) {
    // Calculate direction of force
    let force = p5.Vector.sub(this.position, mover.position);
    // Distance between objects
    let distance = force.mag();
    // Limiting the distance to eliminate "extreme" results for very close or very far objects
    distance = constrain(distance, 5, 25);

    // Calculate gravitional force magnitude
    let strength = (this.G * this.mass * mover.mass) / (distance * distance);
    // Get force vector --> magnitude * direction
    force.setMag(strength);
    return force;
  }

  // Method to display
  display() {
    ellipseMode(CENTER);
    stroke(0);
    if (this.dragging) {
      fill(50);
    } else if (this.rollover) {
      fill(100);
    } else {
      fill(175, 200);
    }
    ellipse(this.position.x, this.position.y, this.mass * 2);
  }
  
  pressed() {
  let d = dist(mouseX, mouseY, this.position.x, this.position.y);
  if (d < this.mass) {
    this.dragging = true;
  }
}

released() {
  this.dragging = false;
}

hover() {
  let d = dist(mouseX, mouseY, this.position.x, this.position.y);
  this.rollover = d < this.mass;
}

drag() {
  if (this.dragging) {
    this.position.x = mouseX;
    this.position.y = mouseY;
  }
}
  
}
```

sketch.js

```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let movers = [];
let attractor;

function setup() {
  createCanvas(640, 240);

  for (let i = 0; i < 20; i++) {
    movers.push(new Mover(random(width), random(height), random(0.1, 2)));
  }
  attractor = new Attractor();
}

function draw() {
  background(255);

  attractor.display();

  for (let i = 0; i < movers.length; i++) {
    let force = attractor.attract(movers[i]);
    movers[i].applyForce(force);

    movers[i].update();
    movers[i].show();
    
    attractor.hover();
    attractor.drag();
  }
  
  function mousePressed() {
  attractor.pressed();
}

function mouseReleased() {
  attractor.released();
}
  
  
}
```
Ahora el Attractor es interactivo:

Cuando el mouse está encima - cambia de color

Cuando haces click - lo puedes arrastrar

Los movers siguen la atracción gravitacional

Esto permite controlar el sistema de fuerzas en tiempo real.


En esta actividad entendí cómo el marco Motion 101 se combina con las fuerzas físicas para crear simulaciones más realistas. En lugar de modificar directamente la velocidad, primero se aplican fuerzas que se transforman en aceleración y luego se integran al sistema de movimiento. Además, al agregar interacción con el mouse, el usuario puede modificar la posición del attractor y observar cómo cambia el comportamiento de los objetos que son atraídos por él.

https://editor.p5js.org/TheWarrior710/sketches/CvODviHeJ


<img width="501" height="248" alt="image" src="https://github.com/user-attachments/assets/c2714612-57e3-4920-9072-c3f86eb52fdf" />

#### Actividad 5

1. ¿Cuál es la relación entre r y θ con x y y?

En esta simulación se utilizan coordenadas polares, que representan una posición usando:

r → distancia desde el origen

θ (theta) → ángulo respecto al eje horizontal

Para dibujar en la pantalla, estas coordenadas deben convertirse a coordenadas cartesianas (x, y).

La conversión matemática es:

𝑥 = 𝑟 ⋅ 𝑐 𝑜 𝑠 ( 𝜃 )

𝑦 = 𝑟 ⋅ 𝑠 𝑖 𝑛 ( 𝜃 )


En el código esto se ve así:
```js
let x = r * cos(theta);
let y = r * sin(theta);
```

Esto significa que:

r controla qué tan lejos está el punto del centro

θ controla la dirección o ángulo del punto

Por eso el círculo se mueve alrededor del centro de la pantalla formando un movimiento circular.

Además, en el código se usa:

translate(width / 2, height / 2);

Esto mueve el origen del sistema de coordenadas al centro de la pantalla para que el movimiento circular ocurra alrededor del centro.


2. Primera modificación del código
```js   
let v = p5.Vector.fromAngle(theta);
line(0, 0, x, y);
circle(v.x, v.y, 48);
```

¿Qué ocurre?

El círculo ya no gira alrededor del centro como antes, sino que se mueve cerca del origen.

¿Por qué ocurre esto?

La función:

p5.Vector.fromAngle(theta)

crea un vector con:

dirección θ

magnitud = 1

Es decir, el vector tiene longitud 1 unidad.

Por eso el punto aparece muy cerca del centro, ya que no se está usando el valor de r para definir la distancia.

En otras palabras:

el ángulo se mantiene

pero la distancia al centro se pierde




Segunda modificación del código

let v = p5.Vector.fromAngle(theta, r);
¿Qué ocurre ahora?

Ahora el círculo vuelve a moverse en una trayectoria circular alrededor del centro, similar al primer ejemplo.

¿Por qué ocurre?

Porque esta versión de la función:

p5.Vector.fromAngle(theta, r)

crea un vector que tiene:

dirección = θ

magnitud = r

Es decir, el vector ya contiene tanto el ángulo como la distancia al centro.

Internamente hace exactamente lo mismo que la fórmula polar:

𝑥 = 𝑟 ⋅ 𝑐 𝑜 𝑠 ( 𝜃 ) 𝑦 = 𝑟 ⋅ 𝑠 𝑖 𝑛 ( 𝜃 )


Por lo tanto:

v.x = r cos(θ)

v.y = r sin(θ)

4. ¿Qué ocurre en la simulación final?

En el código final aparece esta línea:

r = height * 0.5 * sin(theta);

Esto hace que r cambie constantemente usando una función seno.

El resultado es que:

la distancia al centro aumenta y disminuye

el objeto no solo gira

también se acerca y se aleja del centro

Esto produce un movimiento de oscilación radial, donde el punto parece expandirse y contraerse mientras gira.

Este comportamiento conecta directamente con el capítulo de Oscillation, donde las funciones seno y coseno se usan para generar movimientos periódicos.

Conclusión

En esta actividad aprendí cómo funcionan las coordenadas polares y cómo se pueden convertir a coordenadas cartesianas para dibujar en pantalla. También entendí que el ángulo θ define la dirección del movimiento, mientras que r define la distancia al origen. Además, al utilizar funciones como sin(), es posible modificar dinámicamente el radio y generar movimientos oscilatorios más complejos dentro de la simulación.



#### Actividad 6

Funciones sinusoides

En esta actividad exploré el comportamiento de la función sinusoide mediante una simulación en p5.js. La función seno es muy importante en simulaciones porque permite generar movimientos periódicos y oscilatorios, como los que se encuentran en fenómenos naturales como ondas, vibraciones, sonido o movimiento pendular.

En el código se utilizan dos funciones seno para controlar la posición horizontal de dos círculos que se mueven a lo largo de una línea. La posición se calcula usando la expresión:

𝑥 = 𝐴 ⋅ 𝑠 𝑖 𝑛 ( 𝜔 𝑡 + 𝜙 )


donde cada parámetro representa una característica del movimiento.

Amplitud

La amplitud representa la distancia máxima que alcanza la oscilación desde el punto central. En el código aparece en la variable:

let amplitude = 200;

Esto significa que los círculos pueden moverse hasta 200 píxeles a la izquierda o a la derecha del centro. Si la amplitud aumenta, el movimiento se vuelve más amplio; si disminuye, la oscilación se vuelve más pequeña.

Periodo

El periodo representa el tiempo que tarda la onda en completar un ciclo completo. En el código se controla con las variables:

let period1 = 120;
let period2 = 120;

Estas variables determinan cuántos frames tarda el movimiento en completar una oscilación completa. Un periodo mayor produce una oscilación más lenta, mientras que uno menor genera un movimiento más rápido.

Velocidad angular

La velocidad angular describe qué tan rápido cambia el ángulo de la función seno. En el código aparece implícitamente en esta expresión:

(TWO_PI * frameCount) / period

Aquí TWO_PI representa una vuelta completa (360°), y frameCount aumenta en cada frame, lo que hace que el valor dentro del seno cambie continuamente y produzca el movimiento oscilatorio.

Frecuencia

La frecuencia está relacionada con cuántas oscilaciones ocurren en un periodo de tiempo. Matemáticamente es el inverso del periodo:

𝑓 = 1 / 𝑇 

Por lo tanto, cuando el periodo disminuye, la frecuencia aumenta y la oscilación ocurre más rápido.

Fase

La fase representa un desplazamiento horizontal de la onda. En la simulación se controla con la variable:

let phase = 0;

y se modifica con el teclado:

phase += TWO_PI/360;

Cada vez que se presiona una tecla, la fase cambia ligeramente. Esto provoca que la segunda onda seno se desplace horizontalmente respecto a la primera, generando una diferencia en el movimiento de los dos círculos.

Este efecto permite observar cómo dos ondas pueden tener el mismo periodo y amplitud, pero estar desfasadas.

Observaciones de la simulación

Al ejecutar la simulación se pueden observar dos círculos que se mueven horizontalmente siguiendo una función seno. El primer círculo utiliza la función seno básica, mientras que el segundo incluye un desplazamiento de fase. Cuando se presionan las teclas, la fase cambia y el segundo círculo comienza a moverse adelantado o atrasado respecto al primero.

Esto demuestra cómo pequeñas modificaciones en los parámetros de una función sinusoide pueden cambiar significativamente el comportamiento del movimiento.

Conclusión

Esta actividad me permitió comprender cómo las funciones sinusoides se utilizan para simular movimientos oscilatorios en programación creativa. Los parámetros como amplitud, periodo, frecuencia, velocidad angular y fase controlan diferentes aspectos de la onda y permiten generar animaciones dinámicas y naturales. Este tipo de funciones es muy utilizado en gráficos computacionales, simulaciones físicas y animación procedural.


#### Actividad 7

En esta actividad trabajé a partir de la simulación de osciladores propuesta en The Nature of Code. En el ejemplo original, cada oscilador genera un movimiento usando funciones seno en los ejes x y y, lo que produce trayectorias oscilatorias alrededor del centro de la pantalla.

El movimiento se calcula con las siguientes ecuaciones:

𝑥 = 𝑠 𝑖 𝑛 ( 𝜃 𝑥 ) ⋅ 𝑎 𝑚 𝑝 𝑙 𝑖 𝑡 𝑢 𝑑 𝑥

	​

𝑦 = 𝑠 𝑖 𝑛 ( 𝜃 𝑦 ) ⋅ 𝑎 𝑚 𝑝 𝑙 𝑖 𝑡 𝑢 𝑑 𝑦

	​


donde el ángulo cambia continuamente mediante una velocidad angular, lo que genera el movimiento oscilatorio.

Para esta actividad modifiqué la simulación incorporando conceptos de dos unidades anteriores:

Unidad 1: aleatoriedad utilizando Perlin Noise

Unidad 3: aplicación de fuerzas

Modificación 1 — Aleatoriedad con Perlin Noise

En lugar de usar únicamente random(), utilicé la función noise() para modificar suavemente la amplitud del movimiento.

La diferencia entre random() y noise() es que:

random() genera valores completamente impredecibles

noise() produce valores suaves y continuos, lo que genera movimientos más naturales

Implementé un valor llamado noiseOffset que cambia lentamente en cada actualización.

Ejemplo conceptual:
```js
this.noiseOffset = random(1000);
```
Luego en update():
```js
let n = noise(this.noiseOffset);
this.amplitude.x = map(n, 0, 1, 50, width/2);
this.noiseOffset += 0.01;
```
Esto hace que la amplitud del oscilador cambie gradualmente en el tiempo, produciendo un movimiento más orgánico.

Modificación 2 — Aplicación de fuerzas

También agregué el concepto de fuerza, inspirado en el sistema de física visto en la unidad 3.

Creé un vector de fuerza que modifica la velocidad angular del oscilador.

Conceptualmente funciona así:


```js
applyForce(force){
  this.angleVelocity.add(force);
}
```
Luego, en el draw() del sketch, agregué una pequeña fuerza basada en el movimiento del mouse:
```js
let wind = createVector(0.001, 0);
oscillators[i].applyForce(wind);
```
Esto hace que los osciladores cambien ligeramente su velocidad angular, lo que altera el patrón de movimiento.

De esta forma el sistema ya no tiene un movimiento completamente constante, sino que responde a una fuerza externa.

Resultado de la simulación

Después de las modificaciones, la simulación presenta los siguientes comportamientos:

Los osciladores siguen moviéndose con trayectorias sinusoidales.

La amplitud cambia gradualmente debido al uso de Perlin Noise.

Las fuerzas externas modifican la velocidad angular, produciendo variaciones en el movimiento.

El resultado es una animación más dinámica y menos predecible que la versión original.

Conclusión

Esta actividad me permitió integrar conceptos de diferentes unidades en una sola simulación. El uso de Perlin Noise genera variaciones suaves en el movimiento, mientras que la incorporación de fuerzas permite modificar el comportamiento dinámico del sistema. Esto demuestra cómo pequeñas modificaciones en los parámetros físicos y matemáticos pueden producir animaciones más complejas y realistas.


#### Actividad 8

Para lograr que la onda se comporte como una ola en movimiento, fue necesario mover la lógica de dibujo a la función draw(), que se ejecuta continuamente en cada frame de la animación.

Además, se agregó una variable llamada phase que se incrementa con el tiempo. Esta variable desplaza la onda horizontalmente, creando la sensación de movimiento.

```js
let angle = 0;
let angleVelocity = 0.2;
let amplitude = 100;

let phase = 0;

function setup() {
  createCanvas(640, 240);
}

function draw() {
  background(255);

  stroke(0);
  strokeWeight(2);
  fill(127,127);

  let angle = phase;
  
for (let x = 0; x <= width; x += 24) {
    // 1) Calculate the y position according to amplitude and sine of the angle.
    let y = amplitude * sin(angle);
    // 2) Draw a circle at the (x,y) position.
    circle(x, y + height / 2, 48);
    // 3) Increment the angle according to angular velocity.
    angle += angleVelocity;
  }

  phase += 0.05; // mueve la ola
}
```



Después de realizar la modificación, la onda ya no permanece estática. Ahora los puntos que forman la onda se desplazan continuamente, generando un movimiento similar al de una ola que se propaga horizontalmente.

Este efecto ocurre porque el valor inicial del ángulo cambia en cada frame, lo que desplaza la función seno a lo largo del eje horizontal.



## Bitácora de aplicación 



## Bitácora de reflexión

























