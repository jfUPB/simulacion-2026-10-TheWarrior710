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

-apartir de la explicasion del profe se que el Walker solo se mueve 1 pixel por frame, Como draw() se ejecuta muchas veces por segundo, el movimiento es lento y detallado entonces lo que hago cuando cambio this.x++; por esto this.x += 3; pasa que en lugar de que el programa se mueva 1 píxel, se mueve 3 píxeles en un solo paso y hace que los pixeles se vean un poco mas dispersos

##### Paso 3 ejecutar

-Al ejecutar sucedio lo esperado el problema es que tambien se ve que los pixeles de ven mas espaciados entonces fue lo que mas curiosidad me dio porque no pense que eso fuera a pasar


### Actividad 03
Uniforme en terminos numericos pienso que signfica que cuando se hace un proceso lleva una secuencia especifica y no uniforme signica un grado de dispercion el cual no sigue un orden lo cual lo hace mas aleatorio

## Bitácora de aplicación 



## Bitácora de reflexión







