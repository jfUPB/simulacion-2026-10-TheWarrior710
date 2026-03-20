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


4. ¿Quién crea las partículas?

Las partículas se crean en el draw():

``particles.push(new Particle(width / 2, 20));``

Se crea una nueva partícula en cada frame
 El punto de creación actúa como un emisor

5. ¿Quién decide cuándo eliminarlas?

El mismo 

```js
draw():

if (particle.isDead()) {
  particles.splice(i, 1);
}
```

 El sistema no la partícula controla su eliminación

6. ¿Por qué recorrer el array al revés?

   
``for (let i = particles.length - 1; i >= 0; i--)``

Porque se están eliminando elementos del array

Si lo hiciera normal:

``for (let i = 0; i < particles.length; i++)``

Problema:


Se saltan partículas

Se desordenan los índices

 Recorrer al revés evita errores al hacer ``splice()``

7. ¿Qué pasa si no se eliminan?

Si pasa esto:

``particles.splice(i, 1);``

-El array crece indefinidamente
-Se acumulan partículas “muertas”
-Baja el rendimiento FPS
-Puede colapsar la simulación

Esto demuestra que la gestión de memoria es parte del sistema


8. Representación visual

Cada partícula se dibuja con:
```js
circle(this.position.x, this.position.y, 8);
```
Y además:

stroke()  contorno

fill()  color

9. Relación vida  apariencia

```js
fill(127, this.lifespan);
stroke(0, this.lifespan);
```

El lifespan controla la transparencia (alpha)

Resultado:

Vida alta - visible

Vida baja - transparente

Vida 0 - desaparece

Esto conecta comportamiento con visualización


10. Cambiar la representación visual

    

Si quisiera usar líneas en vez de círculos:

 SOLO cambio esto:
 
```js
circle(...)
```
por ejemplo:

```js
line(this.position.x, this.position.y, this.position.x + 10, this.position.y + 10);
```

NO cambio:
```js
física (velocity, acceleration)

ciclo de vida (lifespan)
```

lógica del sistema

Esto demuestra separación entre:

comportamiento

estructura

visualización


#### Actividad 2

1. ¿Qué responsabilidades pasaron de draw() a Emitter?
Antes (Example 4.2), en draw() se hacía todo:

•	Crear partículas
•	Actualizarlas
•	Dibujarlas
•	Eliminarlas

-Ahora, en Example 4.4, esas responsabilidades están dentro de:


class Emitter

-Específicamente en:

```js
run()
addParticle()
```
-Entonces el draw() ahora solo:
```js
for (let emitter of emitters) {
  emitter.run();
  emitter.addParticle();
}
```
 -Es decir:
 
``draw()`` ya no gestiona partículas directamente
Ahora delega esa responsabilidad al emisor














## Bitácora de aplicación 


## Bitácora de reflexión
