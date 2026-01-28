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

### Actividad 03
Uniforme en terminos numericos pienso que signfica que cuando se hace un proceso lleva una secuencia especifica y no uniforme signica un grado de dispercion el cual no sigue un orden lo cual lo hace mas aleatorio

## Bitácora de aplicación 



## Bitácora de reflexión




