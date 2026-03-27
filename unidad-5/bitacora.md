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

```js
class Emitter
```
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

2. Ventaja de encapsular en Emitter

La ventaja principal es organización y escalabilidad.

-Antes:

Todo estaba en un solo lugar (menos modular)

-Ahora:

Cada emisor controla sus propias partículas

-Beneficios:

-Código más limpio
-Reutilización de lógica
-Permite múltiples sistemas independientes
-Fácil de escalar muchos emisores al mismo tiempo

Se aplica el principio de encapsulamiento, separando la lógica del sistema de partículas en una clase independiente.


3. ¿Quién crea qué?

-Los emitters se crean aquí:

```js
function mousePressed() {
  emitters.push(new Emitter(mouseX, mouseY));
}
```

-Las partículas las crea cada emitter:

```js
addParticle() {
  this.particles.push(new Particle(this.origin.x, this.origin.y));
}
```

-Entonces:

Usuario - crea emitters
Emitter - crea partículas

<img width="806" height="651" alt="image" src="https://github.com/user-attachments/assets/e7986a5a-76ca-4597-8fde-96787a6f6f9b" />


Niveles de colección:

Array de emitters
Array de partículas dentro de cada emitter

Total: 2 niveles de colección



#### Descripción abstracta del sistema



El sistema está compuesto por entidades organizadas jerárquicamente.

Existe una colección principal de emisores, donde cada emisor actúa como una entidad que genera y gestiona una colección interna de partículas.

Cada partícula es una entidad con:

un estado físico (posición, velocidad, aceleración)
un estado vital (tiempo de vida)

Durante cada ciclo de actualización:

el emisor genera nuevas partículas
cada partícula actualiza su estado en función de fuerzas
las partículas que han cumplido su ciclo de vida son eliminadas

El sistema evoluciona en el tiempo mediante iteraciones continuas, donde:

se aplican fuerzas
se actualizan estados
se gestionan dinámicamente las colecciones



##### Actividad 3


1. ¿Qué tienen en común y qué tienen diferente?


En común (herencia)

Tanto Particle como Confetti:

Tienen:

-position
-velocity
-acceleration
-lifespan

Usan:

```-applyForce()```
```-update()```
```-run()```
```-isDead()```

Esto es porque:

class Confetti extends Particle

Confetti hereda todo el comportamiento base de Particle.

Diferente

La diferencia está en:
```js
show()
```
Confetti sobrescribe (override) este método:
```js
show() {
  // dibuja un cuadrado rotando
}
```
Mientras que Particle dibuja:
```js
circle(...)
```
Entonces:

Particle - círculo
Confetti - cuadrado rotando



2. ¿Por qué el Emitter no necesita saber el tipo?

Porque ambos objetos (Particle y Confetti) comparten la misma interfaz:
```js
p.run();
p.isDead();
```
-El Emitter solo dice:
```js
let p = this.particles[i];
p.run();
```
Y no le importa si es:

Particle
Confetti

-Esto es polimorfismo.


El emisor no necesita conocer el tipo específico de partícula porque todas comparten la misma interfaz de comportamiento. Esto permite tratar diferentes objetos de manera uniforme, lo que es una aplicación directa del polimorfismo.



3. Agregar un tercer tipo de partícula

Si quiero agregar otro tipo (ej: FireParticle):

Debo hacer:
Crear una nueva clase:
```js
class FireParticle extends Particle {
  show() {
    // nueva representación visual
  }
}
```

-NO debo modificar:
```js
Emitter.run()
Particle.update()
isDead()
```
lógica de fuerzas

Solo podría agregar en addParticle():

this.particles.push(new FireParticle(...));

Esto demuestra que el sistema es extensible sin romper lo existente.



4. Comparación con Example 4.2

   
¿Cambió la lógica del Emitter?

-No cambió

Sigue:

-creando partículas
-actualizando
-eliminando

¿Cambió la lógica de muerte?

No cambió

Sigue siendo:
```js
lifespan < 0
```
¿Qué capa cambió?

Cambió:

Capa de visualización

ahora hay múltiples representaciones círculo vs cuadrado
¿Qué capas NO cambiaron?

Se mantuvieron:

comportamiento (Motion 101, fuerzas)
estructura (emitter, arrays, eliminación)

-Se implementa herencia para reutilizar comportamiento base y polimorfismo para permitir múltiples representaciones visuales sin modificar la lógica del sistema. Esto mantiene desacopladas las capas de comportamiento y visualización.


Herencia - reutilizo código
Polimorfismo - diferentes comportamientos con misma interfaz
Emitter - no sabe qué tipo de partícula hay
Solo cambió la visualización



#### Actividad 4



Fuerzas globales vs. locales

1. En Example 4.6

   
La gravedad se define en draw():
```js
let gravity = createVector(0, 0.1);
```

Se aplica así:
```js
emitter.applyForce(gravity);
```

-Conclusión:

La gravedad es una fuerza global

-Porque:

No depende de cada partícula
Se aplica igual a todas
Viene “desde afuera” del sistema


La gravedad es una fuerza externa global aplicada uniformemente a todas las partículas del sistema.


2. En Example 4.7

Aquí aparece el repeller.

La fuerza no está en draw(), sino en:
```js
repeller.repel(particle)
```

Y depende de:

```js
let distance = force.mag();
```

Conclusión:

Es una fuerza local

Porque:

Depende de la distancia, Cada partícula recibe una fuerza distinta

El repeller genera una fuerza local dependiente de la distancia, lo que introduce comportamiento emergente en el sistema.



3. Principio físico modelado

Esto:
```js
strength = (-1 * this.power) / (distance * distance);
```
Representa:

Ley del inverso del cuadrado

(igual que gravedad o electrostática)


Se modela una interacción basada en una ley de inverso del cuadrado, donde la intensidad de la fuerza disminuye con la distancia.



4. ¿Cambió la clase Particle?

No porque:

Particle sigue igual, No sabe que existe el repeller

-Conclusión:

Las fuerzas están desacopladas del comportamiento interno de la partícula.



Existe una clara separación entre la lógica interna de la entidad y las fuerzas externas aplicadas.

<img width="661" height="653" alt="image" src="https://github.com/user-attachments/assets/8f304ea9-f010-417e-bc46-30f74058bb84" />


yo elijo :

 (b) Cambiar fuerzas SIN cambiar estructura ni visual

 Cambie en draw():
 ```js
let wind = createVector(0.05, 0);
emitter.applyForce(wind);
```
lo que hice fue agregar una nueva fuerza horizontal constante osea viento.

2. ¿Qué clases modifiqué?

Ninguna

3. ¿Qué NO modifiqué?
   
-Particle 
-Emitter 
-Repeller 
-Visualización 


4. ¿Por qué fue posible?

Porque el sistema está bien diseñado:


Las fuerzas están separadas, Se aplican desde afuera, El sistema es modular.

Gracias a la separación entre comportamiento, estructura y fuerzas externas, fue posible introducir una nueva dinámica sin alterar la arquitectura del sistema.






## Bitácora de aplicación 




#### Actividad 5


1. Mi obra representa el ciclo de vida humano como una serie de escalones. Cada escalón simboliza una etapa importante de crecimiento personal. La partícula representa a una persona que avanza con cada decisión "click del usuario", creciendo en tamaño y experiencia. Al llegar al último escalón, la partícula “rebota” como símbolo de logro y felicidad, pero finalmente desaparece, representando que la vida no termina al cumplir metas, sino que todo ciclo eventualmente llega a su fin.

2. Bocetos

<img width="1151" height="735" alt="image" src="https://github.com/user-attachments/assets/51419bd0-a03e-447d-a48f-e1e27b45fd66" />

<img width="358" height="191" alt="image" src="https://github.com/user-attachments/assets/98be9910-9492-4c15-90c1-c7348c21b2c3" />





3. Mapa de decisiones:

Partícula principal

Representa: persona / vida
Crece - experiencia
Se mueve por escalones - progreso
- Escalones
Representan: etapas de la vida
Son estáticos - el mundo / estructura de la vida
- Click del usuario

  
No es random
Es una decisión consciente

La interacción del usuario representa decisiones que impulsan el progreso del ciclo de vida.

Fuerzas
Gravedad - realidad / peso de la vida
Rebote - emoción / logro
- Muerte
No desaparece de golpe, Se desvanece


La muerte se representa como una disolución progresiva, no como un corte abrupto.


4.

lifeParticle.js

```js
class LifeParticle extends Particle {
  constructor(x, y) {
    super(x, y);
    this.size = 20;

    this.target = createVector(x, y);
    this.history = []; // rastro

    this.stepCount = 0;
    this.maxSteps = 5;

    this.reachedTop = false;
  }

  moveTo(x, y) {
    this.target = createVector(x, y);
    this.stepCount++;

    // crecer en cada escalón
    this.size += 6;
  }

  update() {
    // dirección hacia el escalón
    let dir = p5.Vector.sub(this.target, this.position);
    dir.mult(0.08); // suavidad (no perfecto)
    this.applyForce(dir);

    // pequeña variación natural (unidad 1: aleatoriedad)
    let noiseForce = createVector(random(-0.1, 0.1), random(-0.05, 0.05));
    this.applyForce(noiseForce);

    super.update();

    // guardar rastro
    this.history.push(this.position.copy());
    if (this.history.length > 30) {
      this.history.shift();
    }

    // detectar llegada al escalón
    if (dist(this.position.x, this.position.y, this.target.x, this.target.y) < 10) {
      
      // rebote pequeño en cada escalón
      this.velocity.y = -3;

      // último escalón
      if (this.stepCount === this.maxSteps - 1 && !this.reachedTop) {
        this.velocity.y = -8; // gran rebote final
        this.reachedTop = true;
      }
    }

    // SOLO muere en el último escalón
   if (this.reachedTop) {
  this.deathTimer = (this.deathTimer || 0) + 1;

  if (this.deathTimer > 60) { // espera 1 segundo aprox
    this.lifespan -= 1;
  }
}
  }

  show() {
    //  RASTRO
    noFill();
    stroke(100, 100);
    beginShape();
    for (let v of this.history) {
      vertex(v.x, v.y);
    }
    endShape();

    //  PARTÍCULA
    fill(100, this.lifespan);
    stroke(0, this.lifespan);
    circle(this.position.x, this.position.y, this.size);
  }

  isDead() {
    return this.lifespan < 0;
  }

  //  REINICIO
  reset(x, y) {
    this.position = createVector(x, y);
    this.velocity.mult(0);
    this.acceleration.mult(0);

    this.size = 20;
    this.lifespan = 255;

    this.history = [];
    this.stepCount = 0;
    this.reachedTop = false;

    this.target = createVector(x, y);
  }
}
```

particle.js


```js
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.lifespan = 255;
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan -= 1.5;
  }

  isDead() {
    return this.lifespan < 0;
  }
}
```

sketch.js

```js
let particle;
let steps = [];
let currentStep = 0;

function setup() {
  createCanvas(600, 400);

  // Crear escalones
  let stepHeight = 60;
  for (let i = 0; i < 5; i++) {
    steps.push({
      x: 100 + i * 80,
      y: height - (i + 1) * stepHeight
    });
  }

  particle = new LifeParticle(steps[0].x, steps[0].y);
}

function draw() {
  background(255);

  // Dibujar escalones
  fill(200);
  for (let s of steps) {
    rect(s.x - 30, s.y, 60, 10);
  }

  particle.applyForce(createVector(0, 0.2)); // gravedad
  particle.update();
  particle.show();
}

function mousePressed() {
  if (currentStep < steps.length - 1 && !particle.finished) {
    currentStep++;
    particle.moveTo(steps[currentStep].x, steps[currentStep].y);
    
    
    
  }
  
  if (particle.isDead()) {
  currentStep = 0;
  particle.reset(steps[0].x, steps[0].y);
  }
  
}
```

https://editor.p5js.org/TheWarrior710/sketches/pU4ub1oDb



<img width="538" height="340" alt="Captura de pantalla 2026-03-27 004008" src="https://github.com/user-attachments/assets/bac31b4d-0ae5-43e2-b23f-0a906083e117" />

<img width="585" height="367" alt="Captura de pantalla 2026-03-27 004022" src="https://github.com/user-attachments/assets/95154d29-2030-47cc-a424-d254c644d2ea" />

<img width="551" height="370" alt="Captura de pantalla 2026-03-27 004033" src="https://github.com/user-attachments/assets/491305be-676a-4ff3-8637-2c461255a901" />


## Bitácora de reflexión
