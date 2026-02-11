# Unidad 2

## Bitácora de proceso de aprendizaje


#### Actividad 2 


Lo que entendi de las explicaciones del profe en clase en p5.js los vectores permiten representar posición, velocidad y aceleración de forma unificada. En lugar de manejar valores separados, el vector encapsula dirección y magnitud, lo que facilita simular movimiento natural y comportamientos físicos, como los que propone Nature of Code


La expresión position = position + velocity no funciona porque position y velocity son objetos p5.Vector. En JavaScript no se pueden sumar objetos directamente, por lo que p5.js provee métodos como add() para realizar operaciones vectoriales correctamente

#### Actividad 3

lo que tuve que hacer para la conversacion propuesta fue tomar uno de los ejemplos de caminantes de Capítulo 0 y convertilro para usar vectores.



 este fue el codigo que edite para aplicar la tematica de vectores
 ```java
// The Nature of Code
// Walker usando VECTORES

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
    // Ahora usamos un vector
    this.position = createVector(width / 2, height / 2);
  }

  show() {
    stroke(0);
    point(this.position.x, this.position.y);
  }

  step() {
    const choice = floor(random(4));

    // Creamos un vector de movimiento
    let step;

    if (choice == 0) {
      step = createVector(3, 0);
    } else if (choice == 1) {
      step = createVector(-3, 0);
    } else if (choice == 2) {
      step = createVector(0, 3);
    } else {
      step = createVector(0, -3);
    }

    // Sumamos el vector paso a la posición
    this.position.add(step);
  }
}
 ```

<img width="639" height="256" alt="image" src="https://github.com/user-attachments/assets/dbf6df4f-b7c1-451d-b6bd-390b99354dd4" />






## Bitácora de aplicación 



#### Actividad 7 

El concepto del marco motion 101 se usa mucho ya que ha medida en que se le suma la posicion actual de un objeto ya sea un circulo + la velocidad del vector da como resultado  la segunda pocision del objeto, esto se usa demasiado para hacer arte generativo


## Bitácora de reflexión







