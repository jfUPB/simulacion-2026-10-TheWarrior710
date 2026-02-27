# Unidad 3

## Bitácora de proceso de aprendizaje

#### Actividad 1

Basicamente estamos viviendo en un mundo el cual nos estamos volviendo autodependientes de las maquinas inclyendo de la ia ya que queremos que esta nos haga cada tipo de cosas inclyendo se esta volviendo nuestro psicologo personal y estamos sufriendo un mar de ignarancia absoluta asi como la critica que decia Robert Hodgin en la charla estamos dejando que la IA piense por nosotros

#### Actividad 2

Lo que aprendi en este ejemplo y muestra fue que ahora basado en el marco motion 101 nostros ya no tenemos el control total de la acelareacion si no que a diferencia de esta ahora dependemos de una sumatoria de fuerza el cual es la que ahora nos ayuda a aque le movimiento sea mas realista basado en la segunda ley de Newton, tambien aprendi que las fuer zas se aplican en cada frame  porque modifican su aceleracion
pero que estas se deben reiniciar despues de cambiar su velocidad y pocision ya que las fuerzas solo actuan en ese instante si no se reinician la aceleracion se acomularia indefinidiamente.
También comprendí la importancia del paso por referencia en JavaScript. Al modificar directamente el vector fuerza, se altera el objeto original, lo cual puede generar errores. Por eso es necesario crear una copia del vector antes de dividirlo por la masa. En conclusión, ahora el movimiento no es simplemente una suma de vectores, sino un sistema basado en principios físicos donde la aceleración surge de fuerzas aplicadas en cada frame.



## Bitácora de aplicación 


#### Actividad 3


1. Fricción





<img width="694" height="364" alt="image" src="https://github.com/user-attachments/assets/c8d9d4c8-8575-4d1b-8fa8-37e6be78e52b" />



2. Resistencia al aire y fluidos


Liquid.js

```js
class Liquid {
  constructor(x, y, w, h, c) {
    this.x = x;
    this.y = y;
    this.w = w;
    this.h = h;
    this.c = c;
  }

  contains(mover) {
    return (
      mover.position.y > this.y
    );
  }

  calculateDrag(mover) {
    let speed = mover.velocity.mag();
    let dragMagnitude = this.c * speed * speed;

    let drag = mover.velocity.copy();
    drag.normalize();
    drag.mult(-dragMagnitude);

    return drag;
  }

  show() {
    fill(0,100,200);
    rect(this.x, this.y, this.w, this.h);
  }
}
```

mover.js

```js
class Mover {
  constructor(x, y, m) {
    this.mass = m;
    this.startY = y;
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
  }

  resetX() {
    this.position.x = random(width);
    this.position.y = this.startY;
    this.velocity.mult(0);
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    fill(100,150,255);
    noStroke();
    circle(this.position.x, this.position.y, this.mass * 16);
  }
}
```

sketch.js

```js
let mover;
let liquid;
let splashes = [];

function setup() {
  createCanvas(640, 400);
  mover = new Mover(width/2, 40, 3);
  liquid = new Liquid(0, height/2, width, height/2, 0.1);
}

function draw() {
  background(230);

  liquid.show();

  let gravity = createVector(0, 0.2 * mover.mass);
  mover.applyForce(gravity);

  if (liquid.contains(mover)) {
    let drag = liquid.calculateDrag(mover);
    mover.applyForce(drag);

    // generar salpicadura
    splashes.push(createVector(mover.position.x, liquid.y));
  }

  mover.update();
  mover.show();

  // dibujar salpicaduras
  for (let s of splashes) {
    fill(255);
    circle(s.x + random(-5,5), s.y + random(-5,5), 5);
  }
}

function mousePressed() {
  mover.resetX();
}
```


<img width="741" height="444" alt="image" src="https://github.com/user-attachments/assets/bbabd7ff-f3b6-454e-9c0d-03b82dd550b7" />










3.ATRACCIÓN GRAVITACIONAL

```js
let attractor;

function setup() {
  createCanvas(640, 240);
  mover = new Mover();
  attractor = createVector(width/2, height/2);
}

function draw() {
  background(255);

  let G = 1;
  let force = p5.Vector.sub(attractor, mover.position);

  let distance = constrain(force.mag(), 5, 25);
  force.normalize();

  let strength = (G * mover.mass * 20) / (distance * distance);
  force.mult(strength);

  mover.applyForce(force);

  mover.update();
  mover.show();

  fill(0);
  ellipse(attractor.x, attractor.y, 16);
}
```

<img width="333" height="252" alt="image" src="https://github.com/user-attachments/assets/36219fb5-3cc6-4918-9dd3-a599a3aa9039" />










## Bitácora de reflexión







