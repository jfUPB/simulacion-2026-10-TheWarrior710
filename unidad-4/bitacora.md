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















## Bitácora de aplicación 



## Bitácora de reflexión















