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


### Actividad 6

concepto : a diferecia de random() una caracterisitca que tiene este es que se evidencia cambios drasticos en cambio el ruido perlin() Los valores cambian suavemente

Hay continuidad en el tiempo, El movimiento parece natural ideal para movimiento orgánico


Qué resultados se esperan?

al ejecutar s el sketch: Se supone que el círculo se mueve de izquierda a derecha, No hay saltos bruscos, El movimiento es continuo y fluido, Parece mas “natural”, no robótico, Si cambio noise(t) por random(width): El círculo saltaría de un lado a otro y Se perdería la continuidad.

```java
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let t = 0;

function setup() {
  createCanvas(640, 240);
  background(255);
}

function draw() {
  background(255);

  // Ruido Perlin devuelve valores suaves entre 0 y 1
  let x = noise(t) * width;

  noStroke();
  fill(0);
  circle(x, height / 2, 20);

  // Avanza suavemente en el tiempo
  t += 0.01;
}
```

https://editor.p5js.org/TheWarrior710/sketches/jDM1vz_gM

<img width="660" height="405" alt="image" src="https://github.com/user-attachments/assets/167d50cd-081c-4026-bf78-c135fac9492e" />




## Bitácora de aplicación 

### Actividad 7

Una obra generativa son pixeles que crean formas aleatorias los cuales dependiendo del programador puede implementar cosas que ni el artista se imagina ya sea asignandole un patron, color, escala a un pixel determinado y eso hace que tengan resultado unicos y naturlas hechos principalmente por una maquina guiada por el hombre y eso fue lo que quise implementar

```java
// Obra generativa interactiva - Actividad 07
// The Nature of Code
// Nicolas Sedano

let x, y;
let t = 0;

let shapeType = 0;
let currentColor;
let baseSize = 20;

function setup() {
  createCanvas(640, 400);
  background(255);
  x = width / 2;
  y = height / 2;

  // Color inicial
  currentColor = color(0, 0, 0, 20);
}

function draw() {
  noStroke();

  // --- Ruido Perlin para dirección ---
  let angle = noise(t) * TWO_PI * 2;

  // --- Lévy flight ---
  let stepSize;
  if (random(1) < 0.02) {
    stepSize = random(30, map(mouseX, 0, width, 40, 120));
  } else {
    stepSize = random(1, 4);
  }

  // Movimiento
  x += cos(angle) * stepSize;
  y += sin(angle) * stepSize;

  // --- Tamaño basado en distribución normal ---
  let size = randomGaussian(baseSize, baseSize * 0.3);

  fill(currentColor);

  // --- Dibujar forma ---
  if (shapeType === 0) {
    ellipse(x, y, size, size);
  } 
  else if (shapeType === 1) {
    rectMode(CENTER);
    rect(x, y, size, size);
  } 
  else if (shapeType === 2) {
    triangle(
      x, y - size / 2,
      x - size / 2, y + size / 2,
      x + size / 2, y + size / 2
    );
  }

  // Limitar canvas
  x = constrain(x, 0, width);
  y = constrain(y, 0, height);

  t += 0.01;
}

// --- Interacción con teclado ---
function keyPressed() {
  // Cambiar forma
  shapeType = (shapeType + 1) % 3;

  // Cambiar color (drástico)
  currentColor = color(
    random(255),
    random(255),
    random(255),
    25
  );

  // Cambiar tamaño base ALEATORIAMENTE
  let choice = int(random(3));
  if (choice === 0) {
    baseSize = random(8, 15);   // pequeño
  } else if (choice === 1) {
    baseSize = random(18, 30);  // normal
  } else {
    baseSize = random(40, 70);  // grande
  }
}
```

https://editor.p5js.org/TheWarrior710/sketches/xsHHBFytx


<img width="695" height="438" alt="Captura de pantalla 2026-01-30 000306" src="https://github.com/user-attachments/assets/32b68b7f-c89b-4433-ab0d-cc8a32b7cd9f" />

La obra generativa combina ruido Perlin, Lévy flight y distribución normal para producir una composición visual en tiempo real. La interacción con el teclado activa cambios aleatorios en la forma, el color y el tamaño de los elementos, introduciendo variabilidad controlada. El tamaño base se modifica de manera aleatoria en cada interacción, lo que genera contrastes visuales impredecibles. El usuario no define el resultado final, sino que influye en el sistema que lo genera.


## Bitácora de reflexión

### Actividad 8



-La aleatoridad generada por random() hace cambios mas drasticos los cuales ayudan a crear saltos aleatorios que nos sirven para cierto tipo de obras generativas y Ruido Perlin (noise()) genera una apariencia de aleatoriedad, pero en realidad los valores están relacionados entre sí de manera continua Hay continuidad en el tiempo, El movimiento parece natural ideal para movimiento orgánico


Usaría random() cuando quiero resultados caóticos o impredecibles, como destellos, explosiones o cambios abruptos. y Usaría ruido Perlin cuando busco movimiento orgánico, fluido o natural, como trayectorias suaves, paisajes o animaciones continuas.


-Una distribución de probabilidad describe qué tan probable es que ocurra un determinado valor dentro de un conjunto de posibles resultados.

En una caminata aleatoria con distribución uniforme, todos los valores tienen la misma probabilidad de ocurrir. Visualmente esto genera movimientos más dispersos y desordenados, donde los pasos pueden variar mucho sin un centro claro.

En una caminata con distribución normal, la mayoría de los valores se concentran alrededor de un valor promedio, y los valores extremos ocurren con menor frecuencia. Visualmente esto produce trayectorias más agrupadas, con variaciones suaves y ocasionales desviaciones más grandes.

-La aleatoriedad cumple un papel fundamental en el arte generativo porque permite que el sistema produzca resultados únicos y no completamente predecibles.

Una de sus funciones es introducir variación, evitando que la obra sea repetitiva o mecánica. Otra función importante es generar emergencia, donde comportamientos complejos surgen a partir de reglas simples, creando resultados que ni el artista puede anticipar completamente.


-Concepto de aleatoriedad usado en mi obra final (Actividad 07)

En mi obra final utilicé una caminata aleatoria con características de Lévy flight, donde la mayoría de los movimientos son pequeños, pero ocasionalmente ocurren saltos largos. Esta elección fue adecuada porque permitió que la figura explorara el espacio de forma impredecible, creando zonas densas y otras más dispersas.

Este comportamiento generó una composición visual más dinámica y orgánica, reforzando la idea de exploración y crecimiento no lineal dentro del espacio del canvas.

-Una caminata, en el contexto de la simulación, es un proceso donde un objeto se mueve paso a paso siguiendo reglas que incluyen elementos de aleatoriedad. Cada nueva posición depende de la anterior y de una decisión aleatoria.

Una caminata de tipo Lévy flight se caracteriza porque combina muchos pasos pequeños con saltos largos ocasionales. Esta característica permite una exploración más eficiente del espacio y produce patrones visuales menos uniformes y más expresivos que una caminata aleatoria tradicional.






















