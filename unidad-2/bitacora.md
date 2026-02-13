# Unidad 2

## Bitácora de proceso de aprendizaje


#### Actividad 2 


Lo que entendi de las explicaciones del profe en clase en p5.js los vectores permiten representar posición, velocidad y aceleración de forma unificada. En lugar de manejar valores separados, el vector encapsula dirección y magnitud, lo que facilita simular movimiento natural y comportamientos físicos, como los que propone Nature of Code


La expresión position = position + velocity no funciona porque position y velocity son objetos p5.Vector. En JavaScript no se pueden sumar objetos directamente, por lo que p5.js provee métodos como add() para realizar operaciones vectoriales correctamente

#### Actividad 3

lo que tuve que hacer para la conversión  propuesta fue tomar uno de los ejemplos de caminantes de Capítulo 0 y convertilro para usar vectores. 

Antes:

this.x += 3;
this.y -= 3;


Ahora:

this.position.add(step);

Convertí las variables X y Y en un objeto p5.Vector llamado position para representar la ubicación como una entidad matemática completa. En lugar de modificar coordenadas individuales, ahora sumo un vector ddesplazamiento usando el método add(), lo que permite escalar fácilmente el sistema hacia modelos físicos más complejos.


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

##### Actividad 4

1. Espero que al inicio el vector tenga el valor (6, 9) porque así se creó con createVector(6,9). Después, cuando la función playingVector(position) cambie los valores a (20, 30), espero que el vector original también cambie a esos nuevos valores. Por lo tanto, espero ver en la consola primero el vector (6, 9) y luego el vector (20, 30).

2. 

El resultado que obtuve fue exactamente ese:

```less
p5.Vector Object : [6, 9, 0]
p5.Vector Object : [20, 30, 0]
```

lo que vi al ejecutar el codigo en pantalla fue Un canvas gris de 400 × 400 color gris porque ``background(220);``


3. El paso por valor significa que se envía una copia del dato a la función. Esto quiere decir que cualquier cambio dentro de la función no afecta la variable original. El paso por referencia significa que se envía la referencia al objeto original, no una copia. Por lo tanto, cualquier cambio dentro de la función modifica directamente el objeto original.



4.En este código se está realizando un paso por referencia.

Esto ocurre porque los vectores en p5.js son objetos. Cuando el vector se pasa a la función playingVector(v), no se envía una copia, sino la referencia al vector original. Por eso, cuando se cambian los valores v.x y v.y, también cambia el vector original.

Aprendí que los vectores en p5.js se pasan por referencia, lo que significa que cualquier función puede modificar directamente el vector original. Esto es importante porque permite que los objetos como posición, velocidad y aceleración se actualicen correctamente dentro de funciones o clases. También aprendí que debo tener cuidado, porque modificar un vector dentro de una función afectará el objeto original.





## Bitácora de aplicación 



#### Actividad 7 

El concepto del marco motion 101 se usa mucho ya que ha medida en que se le suma la posicion actual de un objeto ya sea un circulo + la velocidad del vector da como resultado  la segunda pocision del objeto, esto se usa demasiado para hacer arte generativo


## Bitácora de reflexión











