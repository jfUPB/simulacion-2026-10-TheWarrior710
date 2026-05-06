# Unidad 7

## Bitácora de proceso de aprendizaje


#### Actividad 1

1.  Análisis de ejemplos

Analicé varios ejemplos del proyecto Word as Image de Ji Lee y seleccioné los siguientes:

1. Balloon
La palabra está manipulada de forma que una de las letras se convierte en un globo, generalmente elevándose por encima del resto. Esto transforma la palabra en una imagen literal sin dejar de ser legible.

2. Drop La tipografía se modifica para que una parte de la palabra parezca caer, como una gota.  La acción de “caer” se vuelve visible dentro de la estructura de la palabra.

3. Eat Una de las letras parece “morder” a otra o desaparecer parcialmente. Se representa el acto de comer directamente en la palabra.

4. Explode Las letras están separadas o fragmentadas como si hubieran estallado. La forma visual transmite energía y ruptura.

2. -¿Cómo la manipulación tipográfica refuerza el significado?

En estos ejemplos, la tipografía deja de ser solo texto y se convierte en imagen.

Se usan deformaciones mínimas pero precisas para no perder legibilidad.
La forma de las letras se adapta al concepto de la palabra.
Se integra acción (caer, explotar, flotar) dentro de la estructura tipográfica.

Esto hace que el significado no solo se lea, sino que también se vea y se sienta, generando una comprensión más inmediata y visual.


3. Propuestas propias


Propongo las siguientes palabras:

1. “Electricidad”
Las letras podrían estar conectadas por líneas tipo corriente, con pequeñas descargas o chispas entre ellas. Algunas partes de la palabra podrían verse interrumpidas o “vibrando”, como si la energía fuera inestable.

2. “Eco”
La palabra se repetiría varias veces con menor opacidad y tamaño, simulando un rebote visual.

3. “Caos”
Las letras estarían desordenadas, rotadas o superpuestas, rompiendo la estructura lineal de la palabra.

4. Elección y justificación

Elegí electricidad porque no solo describe algo visual, sino un comportamiento: flujo, energía y variación constante. Me interesa porque representa energía, movimiento y transformación. Visualmente permite trabajar con efectos como líneas de corriente, chispas, vibración o interrupciones en la forma de las letras.




#### Actividad 02
1. Conceptos fundamentales

En esta actividad aprendí los conceptos básicos de Matter.js y cómo se integran con p5.js para simular comportamiento físico.

-Engine:
Es el motor de física. Se encarga de calcular todo lo que pasa en el sistema: gravedad, colisiones, movimiento, etc. Es como el “cerebro” que actualiza todo en cada frame.

-World:
Es el espacio donde existen todos los objetos. Ahí se agregan los cuerpos físicos para que interactúen entre sí.

-Bodies:
Son los objetos físicos, como círculos o rectángulos. Tienen propiedades como masa, posición, velocidad y pueden chocar entre ellos.

-Constraint:
Son conexiones entre cuerpos. Funcionan como cuerdas o resortes que mantienen una relación entre dos objetos.

-MouseConstraint:
Permite interactuar con los objetos usando el mouse, como si los arrastraras dentro del mundo físico.


2. Experimentos realizado

Experimento 1: Colisiones de partículas

```js
class Ball {
  constructor(x, y, r) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(2, 4));
    this.acc = createVector(0, gravity);

    this.r = r;
    this.mass = r * r;
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.bounds();
  }

  bounds() {
    if (this.pos.y + this.r > height) {
      this.pos.y = height - this.r;
      this.vel.y *= -restitution;
    }

    if (this.pos.x + this.r > width || this.pos.x - this.r < 0) {
      this.vel.x *= -restitution;
    }
  }

  collide(other) {
    let dist = p5.Vector.dist(this.pos, other.pos);
    let minDist = this.r + other.r;

    if (dist < minDist) {
      let force = p5.Vector.sub(this.pos, other.pos).normalize();
      this.vel.add(force);
      other.vel.sub(force);
    }
  }

  show() {
    fill(100, 150, 255);
    noStroke();
    ellipse(this.pos.x, this.pos.y, this.r * 2);
  }
}
```

```js
let gravity = 0.3;
let restitution = 0.6;
let balls = [];

function setup() {
  createCanvas(500, 400);

  for (let i = 0; i < 5; i++) {
    balls.push(new Ball(random(width), random(-200, 0), random(20, 50)));
  }
}

function draw() {
  background(240);

  // colisiones
  for (let i = 0; i < balls.length; i++) {
    for (let j = i + 1; j < balls.length; j++) {
      balls[i].collide(balls[j]);
    }
  }

  // update y dibujo
  for (let b of balls) {
    b.update();
    b.show();
  }
}

function mousePressed() {
  balls.push(new Ball(mouseX, mouseY, random(20, 50)));
}
```
<img width="504" height="403" alt="Captura de pantalla 2026-05-05 235102" src="https://github.com/user-attachments/assets/fe95ca95-9814-4d7a-80e8-486d86d4fd97" />

https://editor.p5js.org/TheWarrior710/sketches/uXSC9U-HM




Experimento 2: Cadena con Matter.js

En este experimento utilicé Matter.js para crear una cadena de cuerpos conectados. Simular una estructura flexible tipo cable o corriente.
```js
const { Engine, World, Bodies, Mouse, MouseConstraint } = Matter;

let engine;
let world;

let chain;
let mouseConstraint;

function setup() {
  let canvas = createCanvas(600, 400);

  engine = Engine.create();
  world = engine.world;

  // crear cadena
  chain = new Chain(15, 150, 50);

  // mouse interactivo 
  let mouse = Mouse.create(canvas.elt);

  mouseConstraint = MouseConstraint.create(engine, {
    mouse: mouse,
    constraint: {
      stiffness: 0.2
    }
  });

  World.add(world, mouseConstraint);
}

function draw() {
  background(20);

  Engine.update(engine);

  chain.show();
}



class Chain {
  constructor(num, startX, startY) {
    this.bodies = [];
    this.constraints = [];

    //  colores halloween
    this.colors = ["#ff7b00", "#7b2cbf"]; // naranja y morado

    for (let i = 0; i < num; i++) {
      let fixed = (i === 0);

      let body = Matter.Bodies.circle(startX + i * 25, startY, 10, {
        isStatic: fixed
      });

      this.bodies.push(body);
      Matter.World.add(world, body);

      // conectar con el anterior
      if (i > 0) {
        let constraint = Matter.Constraint.create({
          bodyA: this.bodies[i - 1],
          bodyB: body,
          length: 25,
          stiffness: 0.9
        });

        this.constraints.push(constraint);
        Matter.World.add(world, constraint);
      }
    }
  }

  show() {
    // dibujar conexiones
    stroke(255);
    strokeWeight(2);

    for (let c of this.constraints) {
      line(
        c.bodyA.position.x,
        c.bodyA.position.y,
        c.bodyB.position.x,
        c.bodyB.position.y
      );
    }

    // dibujar bolitas 
    noStroke();

    for (let i = 0; i < this.bodies.length; i++) {
      let b = this.bodies[i];

      // alternar colores
      let col = this.colors[i % 2];
      fill(col);

      ellipse(b.position.x, b.position.y, 20);
    }
  }
}

```




https://editor.p5js.org/TheWarrior710/sketches/QlAaMEkbQ


<img width="361" height="377" alt="Captura de pantalla 2026-05-06 000257" src="https://github.com/user-attachments/assets/56f02fb8-25fa-4fc4-b3fb-21d20f008867" />



## Bitácora de aplicación 


## Bitácora de reflexión
