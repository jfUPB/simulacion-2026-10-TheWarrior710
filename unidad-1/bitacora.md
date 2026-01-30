# Unidad 1

## Bitácora de proceso de aprendizaje
### Actividad 01
Genera mayor probabilidad de hacer cosas asombrosas que el ser humano no tendria la capacidad facilmente de realizar y muchas de estas cosas se esta usando en en la industria del cine para hacer efectos especiales
### Actividad 02

```java
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show();
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show() {
    stroke(0);
    point(this.x, this.y);
  }

  step() {
  const choice = floor(random(4));
  if (choice == 0) {
    this.x += 3;
  } else if (choice == 1) {
    this.x -= 3;
  } else if (choice == 2) {
    this.y += 3;
  } else {
    this.y -= 3;
  }
}

}
```

-apartir de la explicasion del profe se que el `Walker` solo se mueve 1 pixel por frame, Como `draw()` se ejecuta muchas veces por segundo, el movimiento es lento y detallado entonces lo que hago cuando cambio `this.x++;` por esto `this.x += 3` pasa que en lugar de que el programa se mueva 1 píxel, se mueve 3 píxeles en un solo paso y hace que los pixeles se vean un poco mas dispersos

#### Paso 3 ejecutar

-Al ejecutar sucedio lo esperado el problema es que tambien se ve que los pixeles de ven mas espaciados entonces fue lo que mas curiosidad me dio porque no pense que eso fuera a pasar

#### Paso 4 ejecutar

-Un poo pero como te digo hubieron cambios que no esperaba por eso me toco estudiar mas el codigo y ahi si entendi porque


### Actividad 03
Uniforme en terminos numericos pienso que signfica que cuando se hace un proceso lleva una secuencia especifica y no uniforme signica un grado de dispercion el cual no sigue un orden lo cual lo hace mas aleatorio

```java
unction setup() {
  createCanvas(100, 100);

  background(200);

  describe('Three horizontal black lines are filled in randomly. The top line spans entire canvas. The middle line is very short. The bottom line spans two-thirds of the canvas.');
}

function draw() {
  // Style the circles.
  noStroke();
  fill(0, 10);

  // Distribución NO uniforme, favorece la derecha
let x = random(100) + random(30);
x = constrain(x, 0, 100);
let y = 25;
circle(x, y, 5);


  // Gaussian distribution with a mean of 50 and sd of 1.
  x = randomGaussian(50);
  y = 50;
  circle(x, y, 5);

  // Gaussian distribution with a mean of 50 and sd of 10.
  x = randomGaussian(50, 10);
  y = 75;
  circle(x, y, 5);
}
```

¿Por qué esto favorece la derecha?
1️random(100)

Genera valores entre 0 y 100

Todos tienen la misma probabilidad (uniforme)

2️ + random(30)

Suma un valor extra entre 0 y 30

Eso hace que:

Los valores pequeños sigan siendo posibles

Pero los valores grandes aparezcan más

 Ejemplo mental:

20 + 15 = 35

70 + 20 = 90

85 + 25 = 110 → luego se limita

3️ constrain(x, 0, 100)

Evita que los puntos se salgan del canvas.

### Actividad 4

```
function setup() {
  createCanvas(640, 240);
  background(255);
}

function draw() {
  // Distribución normal: media 320, desviación estándar 60
  let x = randomGaussian(320, 60);

  noStroke();
  fill(0, 10);

  // Dibujar un triángulo centrado en x
  triangle(
    x, 110,      // vértice superior
    x - 8, 130,  // vértice inferior izquierdo
    x + 8, 130   // vértice inferior derecho
  );
}
```

enlace de mi tu sketch en p5.js : https://editor.p5js.org/TheWarrior710/sketches/uZT7MIjxr

<img width="626" height="219" alt="image" src="https://github.com/user-attachments/assets/8498660e-3d5f-4276-8127-880fa7ec113b" />


### Actividad 5

Al aplicar el el comportamiento Lévy en mi  codigo anterior lo que espero es que los puntos se vean mas agrupados en el canvaa, caminos densos en zonas pequeñas, de repente pixeles a lo lejos en posiciones aleatorias y con un recorrido no uniforme

```java
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let x, y;

function setup() {
  createCanvas(640, 240);
  background(255);
  x = width / 2;
  y = height / 2;
}

function draw() {
  stroke(0, 50);

  // Ángulo aleatorio
  let angle = random(TWO_PI);

  // Paso con distribución tipo Lévy
  let stepSize = levy();

  // Movimiento
  x += cos(angle) * stepSize;
  y += sin(angle) * stepSize;

  point(x, y);

  // Evitar que se salga del canvas
  x = constrain(x, 0, width);
  y = constrain(y, 0, height);
}

// Distribución Lévy simple
function levy() {
  let r = random(1);

  // Muchos pasos pequeños, pocos grandes
  if (r < 0.01) {
    return random(50, 100); // salto largo
  } else {
    return random(1, 3); // pasos cortos
  }
}
```



En este sketch se implementa una distribución tipo Lévy flight modificando el movimiento de un walker aleatorio. La técnica consiste en generar principalmente pasos cortos y, con baja probabilidad, saltos largos. Este comportamiento permite simular patrones de exploración más realistas e impredecibles. Se espera observar trayectorias densas en ciertas zonas del canvas y desplazamientos repentinos hacia regiones lejanas, lo cual es característico del movimiento Lévy.


Enlace: https://editor.p5js.org/TheWarrior710/sketches/srfFC_LSL 



<img width="707" height="283" alt="Captura de pantalla 2026-01-29 231201" src="https://github.com/user-attachments/assets/81ec47e4-5957-44bc-83c1-14e93549d7c3" />






## Bitácora de aplicación 



## Bitácora de reflexión


















