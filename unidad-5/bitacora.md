# Unidad 5


## Bitácora de proceso de aprendizaje

#### Actividad 1


1. Propiedades de cada partícula

Cada partícula tiene las siguientes propiedades:

-position

-velocity

-acceleration

-lifespan


Clasificación:

Estado físico:

position : posición en el espacio

velocity : velocidad de movimiento

acceleration : cambios en la velocidad


Estado vital:

lifespan : representa el tiempo de vida de la partícula

2. ¿Cuándo muere una partícula?

La partícula muere cuando:

``this.lifespan < 0``

 Es una muerte gradual, porque el lifespan va disminuyendo poco a poco:

``this.lifespan -= 2;``

No desaparece de inmediato, sino que se desvanece progresivamente.


3. Actualización por frame (Motion 101)

El patrón Motion 101 se ve aquí:

```js
this.velocity.add(this.acceleration);
this.position.add(this.velocity);
this.acceleration.mult(0);
```

Flujo:

 fuerza - aceleración - velocidad - posición

Esto simula física básica en cada frame.


## Bitácora de aplicación 


## Bitácora de reflexión
