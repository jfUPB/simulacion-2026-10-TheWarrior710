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



4. En este código se está realizando un paso por referencia.

Esto ocurre porque los vectores en p5.js son objetos. Cuando el vector se pasa a la función playingVector(v), no se envía una copia, sino la referencia al vector original. Por eso, cuando se cambian los valores v.x y v.y, también cambia el vector original.

Aprendí que los vectores en p5.js se pasan por referencia, lo que significa que cualquier función puede modificar directamente el vector original. Esto es importante porque permite que los objetos como posición, velocidad y aceleración se actualicen correctamente dentro de funciones o clases. También aprendí que debo tener cuidado, porque modificar un vector dentro de una función afectará el objeto original.


#### Actividad 5

1. ¿Para qué sirve el método mag()? ¿Cuál es la diferencia con magSq()? ¿Cuál es más eficiente?

El método mag() sirve para calcular la magnitud o longitud de un vector. Es decir, qué tan largo es el vector desde el origen hasta su posición. Esto permite saber la velocidad o la distancia que representa el vector. El método magSq() hace lo mismo, pero devuelve la magnitud al cuadrado, sin calcular la raíz cuadrada.

La diferencia es que:

mag() usa raíz cuadrada

magSq() no usa raíz cuadrada

magSq() es más eficiente porque calcular la raíz cuadrada consume más recursos del computador. Por eso, si solo necesito comparar magnitudes, es mejor usar magSq().


2. ¿Para qué sirve el método normalize()?

El método normalize() sirve para convertir un vector en un vector unitario, es decir, que tenga magnitud 1 pero conserve su dirección.Esto es útil cuando quiero mantener la dirección del movimiento pero controlar la velocidad por separado. Por ejemplo, en movimiento, normalize() me permite que un objeto se mueva siempre en la misma dirección sin que la velocidad dependa de la longitud original del vector.


3. ¿Para qué sirve el método dot()? (explicado en una frase)

El método dot() sirve para medir qué tan alineados están dos vectores, es decir, si apuntan en la misma dirección, en dirección opuesta o son perpendiculares.


4. ¿Cuál es la diferencia entre la versión estática y la versión de instancia de dot()?

La versión de instancia se usa desde un vector existente, por ejemplo:

v1.dot(v2);

Esto calcula el producto punto entre v1 y v2 usando v1 como referencia. La versión estática se usa desde la clase p5.Vector: p5.Vector.dot(v1, v2); La diferencia es que la versión de instancia pertenece a un objeto vector, mientras que la versión estática pertenece a la clase y recibe ambos vectores como parámetros. Ambas hacen lo mismo, pero se usan de forma diferente.


5. Interpretación geométrica del producto cruz

El producto cruz genera un nuevo vector que es perpendicular a los dos vectores originales. La orientación del vector resultante depende de la regla de la mano derecha, es decir, apunta en una dirección específica dependiendo del orden de los vectores. La magnitud del vector resultante representa el área del paralelogramo formado por los dos vectores originales. En resumen, el producto cruz crea un vector perpendicular que representa el área y la orientación entre dos vectores.

6. ¿Para qué sirve el método dist()?

El método dist() sirve para calcular la distancia entre dos vectores, es decir, la distancia entre dos puntos en el espacio.Esto es útil para detectar colisiones, medir qué tan lejos está un objeto de otro o calcular distancias en simulaciones. Por ejemplo, puedo usarlo para saber si un objeto está cerca del mouse o de otro objeto.

7. ¿Para qué sirven los métodos normalize() y limit()?

El método normalize() sirve para convertir un vector en un vector de magnitud 1 sin cambiar su dirección. El método limit() sirve para limitar la magnitud de un vector, es decir, evitar que su velocidad supere un valor máximo. Esto es importante en simulaciones de movimiento, porque permite controlar la velocidad de los objetos y evitar que se muevan demasiado rápido.


#### Actividad 6 


```js
let t = 0;
let speed = 0.01;

function setup() {
  createCanvas(300, 300);
}

function draw() {
  background(220);

  let base = createVector(150, 150);

  // Vector rojo fijo
  let v1 = createVector(60, 0);

  // Vector azul fijo
  let v2 = createVector(0,50);

  // Vector verde que conecta rojo con azul
  let greenVec = p5.Vector.sub(v2, v1);

  // Vector morado que se mueve entre rojo y azul
  let movingVec = p5.Vector.lerp(v1, v2, t);

  // Movimiento ida y vuelta
  t += speed;
  if (t > 1 || t < 0) {
    speed *= -1;
  }

  // Interpolación de color
  let redColor = color(255, 0, 0);
  let blueColor = color(0, 0, 255);

  let movingColor = lerpColor(redColor, blueColor, t);

  // Dibujar vectores
  drawArrow(base, v1, "red");
  drawArrow(base, v2, "blue");
  drawArrow(p5.Vector.add(base, v1), greenVec, "green");
  drawArrow(base, movingVec, movingColor);
}

function drawArrow(base, vec, myColor) {
  push();
  stroke(myColor);
  strokeWeight(3);
  fill(myColor);

  translate(base.x, base.y);

  line(0, 0, vec.x, vec.y);

  rotate(vec.heading());

  let arrowSize = 7;
  translate(vec.mag() - arrowSize, 0);

  triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);

  pop();
}

```

<img width="416" height="412" alt="image" src="https://github.com/user-attachments/assets/67cb7662-0ab9-45bd-a477-b4bc1a5e637d" />


El método lerp() significa linear interpolation (interpolación lineal). Sirve para encontrar un punto intermedio entre dos vectores. Esto hace que el vector morado se mueva desde el vector rojo hasta el vector azul de forma progresiva.


lerpColor() funciona igual que lerp(), pero con colores. Esto hace que el vector cambie progresivamente de rojo → morado → azul mientras se mueve. Esto crea una transición visual suave.

El método drawArrow usa varias operaciones vectoriales y transformaciones.

El método lerp() permite interpolar posiciones entre dos vectores de forma suave, mientras que lerpColor() permite interpolar colores progresivamente. Esto permite crear animaciones fluidas tanto en movimiento como en apariencia visual.

El método drawArrow() utiliza transformaciones como translate(), rotate() y funciones vectoriales como mag() y heading() para dibujar una flecha correctamente orientada y posicionada.

Esto demuestra cómo los vectores pueden usarse para representar dirección, magnitud, movimiento e interpolación en arte generativo.



#### Actividad 7 

El concepto del marco motion 101 se usa mucho ya que ha medida en que se le suma la posicion actual de un objeto ya sea un circulo + la velocidad del vector da como resultado  la segunda pocision del objeto, esto se usa demasiado para hacer arte generativo



El marco Motion 101 establece que el movimiento se produce sumando un vector velocidad a un vector posición en cada frame. Geométricamente, esto representa la suma de vectores en el espacio, donde la velocidad actúa como un desplazamiento que mueve el objeto continuamente. Este modelo es la base fundamental para crear simulaciones físicas, animaciones y sistemas de movimiento en arte generativo y programación creativa.


#### Actividad 8 


Lo que observé

Cuando usé una aceleración constante, el objeto empezó moviéndose lento, pero cada vez se movía más rápido en la misma dirección. No se movía con velocidad constante, sino que su velocidad aumentaba progresivamente.

El movimiento se veía así:

primero lento

luego más rápido

luego muy rápido

Visualmente parecía que el objeto estaba siendo empujado continuamente en una dirección.


Lo que observé:

El objeto se movía de forma impredecible, cambiando constantemente de dirección y velocidad.A veces se movía lento, a veces rápido, a veces cambiaba de dirección bruscamente. Visualmente parecía:


-errático

-caótico

-como una partícula viva o borracha

-No seguía una trayectoria recta.



Aceleración hacia el mouse
Lo que observé

El objeto siempre se movía hacia el mouse, sin importar dónde estuviera. Si movía el mouse, el objeto lo perseguía.

Visualmente parecía:

-un objeto inteligente

-o un objeto con gravedad hacia el mouse

-o un seguidor

-El movimiento era suave y natural.


Dependiendo del tipo de aceleración:


aceleración constante → movimiento recto y cada vez más rápido

aceleración aleatoria → movimiento caótico y natural

aceleración hacia el mouse → movimiento dirigido e interactivo



Esto confirma la idea del libro de que el movimiento depende principalmente de cómo se calcula la aceleración.


## Bitácora de aplicación 

#### Actividad 9

En esta actividad lo que hice fue que hubiera unas particulas que al unirse creen unos vectores que se asemejen a las de una telaraña, esta telaraña es dinamica lo cual hace que de la impresion de que se ve de tal forma como en la vida real, el cual al oprimir la tecla "j" hace que se dispersen y cuando le doy click se juntan 

```js
let movers = [];

let mode = "web"; // "web" o "explode"

let webColor;
let explodeColor;

function setup() {

  createCanvas(800, 600);

  webColor = color(200, 200, 255);
  explodeColor = color(255, 100, 100);

  for (let i = 0; i < 70; i++) {

    movers.push(new Mover());
  }
}

function draw() {

  background(10, 10, 20);

  for (let mover of movers) {

    mover.update();

    mover.show();
  }

  drawWeb();
}

function keyPressed() {

  if (key === 'j' || key === 'J') {

    mode = "explode";

    for (let mover of movers) {

      mover.explode();
    }
  }
}

function mousePressed() {

  if (mouseButton === LEFT) {

    mode = "web";
  }
}

class Mover {

  constructor() {

    this.position = createVector(random(width), random(height));

    this.velocity = createVector(0, 0);

    this.acceleration = createVector(0, 0);

    this.maxSpeed = 6;

    this.color = webColor;
  }

  explode() {

    this.velocity = p5.Vector.random2D();

    this.velocity.mult(random(5, 10));

    this.color = explodeColor;
  }

  update() {

    if (mode === "web") {

      let center = createVector(width/2, height/2);

      this.acceleration = p5.Vector.sub(center, this.position);

      this.acceleration.normalize();

      this.acceleration.mult(0.2);

      this.color = lerpColor(this.color, webColor, 0.05);

    }

    if (mode === "explode") {

      this.acceleration = p5.Vector.random2D();

      this.acceleration.mult(0.2);

      this.color = lerpColor(this.color, explodeColor, 0.05);
    }

    this.velocity.add(this.acceleration);

    this.velocity.limit(this.maxSpeed);

    this.position.add(this.velocity);
  }

  show() {

    noStroke();

    fill(this.color);

    circle(this.position.x, this.position.y, 6);
  }
}

function drawWeb() {

  for (let i = 0; i < movers.length; i++) {

    for (let j = i + 1; j < movers.length; j++) {

      let d = dist(
        movers[i].position.x,
        movers[i].position.y,
        movers[j].position.x,
        movers[j].position.y
      );

      if (d < 120) {

        let c = lerpColor(movers[i].color, movers[j].color, 0.5);

        stroke(c);

        strokeWeight(map(d, 0, 120, 2, 0.1));

        line(
          movers[i].position.x,
          movers[i].position.y,
          movers[j].position.x,
          movers[j].position.y
        );
      }
    }
  }
}
```
https://editor.p5js.org/TheWarrior710/sketches/yCPcx_lUg


<img width="747" height="569" alt="image" src="https://github.com/user-attachments/assets/02efa291-41b8-4ca6-9ea6-36a9082b7d9b" />


## Bitácora de reflexión





















