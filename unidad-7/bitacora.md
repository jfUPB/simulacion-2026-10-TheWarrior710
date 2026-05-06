# Unidad 7

## Bitácora de proceso de aprendizaje


#### Actividad 1

1.  Análisis de ejemplos

Analicé varios ejemplos del proyecto Word as Image de Ji Lee y seleccioné los siguientes:

1. Balloon
La palabra está manipulada de forma que una de las letras se convierte en un globo, generalmente elevándose por encima del resto. Esto transforma la palabra en una imagen literal sin dejar de ser legible.

2. Drop La tipografía se modifica para que una parte de la palabra parezca caer, como una gota.  La acción de “caer” se vuelve visible dentro de la estructura de la palabra.

3. Eat Una de las letras parece “morder” a otra o desaparecer parcialmente. Se representa el acto de comer directamente en la palabra.

4. Explode Las letras están separadas o fragmentadas como si hubieran estallado. La forma visual transmite energía y ruptura.

2. -¿Cómo la manipulación tipográfica refuerza el significado?

En estos ejemplos, la tipografía deja de ser solo texto y se convierte en imagen.

Se usan deformaciones mínimas pero precisas para no perder legibilidad.
La forma de las letras se adapta al concepto de la palabra.
Se integra acción (caer, explotar, flotar) dentro de la estructura tipográfica.

Esto hace que el significado no solo se lea, sino que también se vea y se sienta, generando una comprensión más inmediata y visual.


3. Propuestas propias


Propongo las siguientes palabras:

1. “Electricidad”
Las letras podrían estar conectadas por líneas tipo corriente, con pequeñas descargas o chispas entre ellas. Algunas partes de la palabra podrían verse interrumpidas o “vibrando”, como si la energía fuera inestable.

2. “Eco”
La palabra se repetiría varias veces con menor opacidad y tamaño, simulando un rebote visual.

3. “Caos”
Las letras estarían desordenadas, rotadas o superpuestas, rompiendo la estructura lineal de la palabra.

4. Elección y justificación

Elegí electricidad porque no solo describe algo visual, sino un comportamiento: flujo, energía y variación constante. Me interesa porque representa energía, movimiento y transformación. Visualmente permite trabajar con efectos como líneas de corriente, chispas, vibración o interrupciones en la forma de las letras.




#### Actividad 02
1. Conceptos fundamentales

En esta actividad aprendí los conceptos básicos de Matter.js y cómo se integran con p5.js para simular comportamiento físico.

-Engine:
Es el motor de física. Se encarga de calcular todo lo que pasa en el sistema: gravedad, colisiones, movimiento, etc. Es como el “cerebro” que actualiza todo en cada frame.

-World:
Es el espacio donde existen todos los objetos. Ahí se agregan los cuerpos físicos para que interactúen entre sí.

-Bodies:
Son los objetos físicos, como círculos o rectángulos. Tienen propiedades como masa, posición, velocidad y pueden chocar entre ellos.

-Constraint:
Son conexiones entre cuerpos. Funcionan como cuerdas o resortes que mantienen una relación entre dos objetos.

-MouseConstraint:
Permite interactuar con los objetos usando el mouse, como si los arrastraras dentro del mundo físico.


2. Experimentos realizado

Experimento 1: Colisiones de partículas

```js
class Ball {
  constructor(x, y, r) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(2, 4));
    this.acc = createVector(0, gravity);

    this.r = r;
    this.mass = r * r;
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.bounds();
  }

  bounds() {
    if (this.pos.y + this.r > height) {
      this.pos.y = height - this.r;
      this.vel.y *= -restitution;
    }

    if (this.pos.x + this.r > width || this.pos.x - this.r < 0) {
      this.vel.x *= -restitution;
    }
  }

  collide(other) {
    let dist = p5.Vector.dist(this.pos, other.pos);
    let minDist = this.r + other.r;

    if (dist < minDist) {
      let force = p5.Vector.sub(this.pos, other.pos).normalize();
      this.vel.add(force);
      other.vel.sub(force);
    }
  }

  show() {
    fill(100, 150, 255);
    noStroke();
    ellipse(this.pos.x, this.pos.y, this.r * 2);
  }
}
```

```js
let gravity = 0.3;
let restitution = 0.6;
let balls = [];

function setup() {
  createCanvas(500, 400);

  for (let i = 0; i < 5; i++) {
    balls.push(new Ball(random(width), random(-200, 0), random(20, 50)));
  }
}

function draw() {
  background(240);

  // colisiones
  for (let i = 0; i < balls.length; i++) {
    for (let j = i + 1; j < balls.length; j++) {
      balls[i].collide(balls[j]);
    }
  }

  // update y dibujo
  for (let b of balls) {
    b.update();
    b.show();
  }
}

function mousePressed() {
  balls.push(new Ball(mouseX, mouseY, random(20, 50)));
}
```
<img width="504" height="403" alt="Captura de pantalla 2026-05-05 235102" src="https://github.com/user-attachments/assets/fe95ca95-9814-4d7a-80e8-486d86d4fd97" />

https://editor.p5js.org/TheWarrior710/sketches/uXSC9U-HM




Experimento 2: Cadena con Matter.js

En este experimento utilicé Matter.js para crear una cadena de cuerpos conectados. Simular una estructura flexible tipo cable o corriente.
```js
const { Engine, World, Bodies, Mouse, MouseConstraint } = Matter;

let engine;
let world;

let chain;
let mouseConstraint;

function setup() {
  let canvas = createCanvas(600, 400);

  engine = Engine.create();
  world = engine.world;

  // crear cadena
  chain = new Chain(15, 150, 50);

  // mouse interactivo 
  let mouse = Mouse.create(canvas.elt);

  mouseConstraint = MouseConstraint.create(engine, {
    mouse: mouse,
    constraint: {
      stiffness: 0.2
    }
  });

  World.add(world, mouseConstraint);
}

function draw() {
  background(20);

  Engine.update(engine);

  chain.show();
}



class Chain {
  constructor(num, startX, startY) {
    this.bodies = [];
    this.constraints = [];

    //  colores halloween
    this.colors = ["#ff7b00", "#7b2cbf"]; // naranja y morado

    for (let i = 0; i < num; i++) {
      let fixed = (i === 0);

      let body = Matter.Bodies.circle(startX + i * 25, startY, 10, {
        isStatic: fixed
      });

      this.bodies.push(body);
      Matter.World.add(world, body);

      // conectar con el anterior
      if (i > 0) {
        let constraint = Matter.Constraint.create({
          bodyA: this.bodies[i - 1],
          bodyB: body,
          length: 25,
          stiffness: 0.9
        });

        this.constraints.push(constraint);
        Matter.World.add(world, constraint);
      }
    }
  }

  show() {
    // dibujar conexiones
    stroke(255);
    strokeWeight(2);

    for (let c of this.constraints) {
      line(
        c.bodyA.position.x,
        c.bodyA.position.y,
        c.bodyB.position.x,
        c.bodyB.position.y
      );
    }

    // dibujar bolitas 
    noStroke();

    for (let i = 0; i < this.bodies.length; i++) {
      let b = this.bodies[i];

      // alternar colores
      let col = this.colors[i % 2];
      fill(col);

      ellipse(b.position.x, b.position.y, 20);
    }
  }
}

```




https://editor.p5js.org/TheWarrior710/sketches/QlAaMEkbQ


<img width="361" height="377" alt="Captura de pantalla 2026-05-06 000257" src="https://github.com/user-attachments/assets/56f02fb8-25fa-4fc4-b3fb-21d20f008867" />




4. Me interesa explorar un comportamiento físico que combine tensión, descarga y energía acumulada, tomando como base la palabra “electricidad”.

La idea es que la palabra no sea estática, sino que funcione como un sistema activo. Cada letra se comportaría como un cuerpo físico que puede reaccionar a fuerzas, vibraciones o impulsos, generando una sensación de inestabilidad controlada.

En particular:

Algunas letras, especialmente la “i”, emitirían chispas o pequeñas descargas eléctricas, representando puntos de alta concentración de energía.
Estas chispas no serían constantes, sino intermitentes, como si la energía se estuviera acumulando y liberando en momentos específicos.
El sistema completo tendría una ligera vibración o movimiento, reforzando la idea de que la electricidad nunca está completamente en reposo.

Además, quiero introducir un contexto más atmosférico:

Aparecería una nube oscura en la parte superior, como si se estuviera cargando una tormenta.
Desde esa nube, se generarían rayos que atraviesan la palabra, conectando el cielo con las letras.
Estos rayos podrían activar o intensificar el comportamiento de las letras, haciendo que reaccionen con más energía en el momento del impacto.

En conjunto, el comportamiento físico no solo simula movimiento, sino que construye una metáfora visual de la electricidad como algo:

inestable,
acumulativo,
y explosivo.


### Actividad 3



🧪 EXPERIMENTO 1 — Amplitud → Núcleo que late 

 Similar a lo que hice la vez pasada  
 
```sketch.js


let song;
let amplitude;

function preload() {
  song = loadSound("chuck.mp3");
}

function setup() {
  createCanvas(400, 400);
  amplitude = new p5.Amplitude();
  song.loop();
}

function draw() {
  background(0);

  let level = amplitude.getLevel();

  // tamaño según volumen
  let size = map(level, 0, 0.3, 50, 250);

  fill(200, 100, 100);
  noStroke();
  ellipse(width / 2, height / 2, size);
}
```

Dato leído del audio:
Estoy utilizando la amplitud, que representa el volumen general de la canción en tiempo real.

Comportamiento activado:
La amplitud controla el tamaño de una figura central. Cuando el volumen aumenta, la forma crece; cuando baja, se contrae.

Interpretación:
Esto genera un efecto de latido o pulso, que se siente muy conectado a la idea de energía viva o electricidad en movimiento.

<img width="426" height="418" alt="image" src="https://github.com/user-attachments/assets/cad39aae-e367-4690-889d-13e7a067632b" />

https://editor.p5js.org/TheWarrior710/sketches/0fW1VQglq




EXPERIMENTO 2 — Frecuencia (bass) 






```js
let song;
let fft;
let started = false;

let sliderRate;
let sliderPan;

function preload() {
  soundFormats('mp3', 'ogg');
  song = loadSound("this-dot-kp.mp3");
}

function setup() {
  createCanvas(400, 400);
  fft = new p5.FFT();

  // sliders 🎛️
  sliderRate = createSlider(0.5, 1.5, 1, 0.01);
  sliderPan = createSlider(-1, 1, 0, 0.01);
}

function draw() {
  background(10);

  if (!started) {
    fill(255);
    textAlign(CENTER, CENTER);
    text("CLICK PARA INICIAR", width / 2, height / 2);
    return;
  }

  // aplicar sliders
  song.rate(sliderRate.value());
  song.pan(sliderPan.value());

  let bass = fft.getEnergy("bass");

  //  VISUAL BASE (para que no esté vacío)
  fill(200, 50, 100);
  noStroke();
  ellipse(width / 2, height / 2, 50 + bass);

  //  chispas
  if (random(1) < bass / 255) {
    drawSpark();
  }
}

function mousePressed() {
  if (!started) {
    userStartAudio();
    song.loop();
    started = true;
  }
}

function drawSpark() {
  stroke(255, 200, 0);
  strokeWeight(2);

  let x = random(width);
  let y = random(height / 2);

  for (let i = 0; i < 5; i++) {
    let x2 = x + random(-20, 20);
    let y2 = y + random(10, 40);

    line(x, y, x2, y2);
```



Dato leído del audio:
Estoy utilizando la energía de las frecuencias bajas (bass), que normalmente corresponden a los golpes rítmicos de la música.

Comportamiento activado:
Cada vez que la energía del bajo supera un umbral, se generan chispas visuales que aparecen de forma repentina en pantalla.

Interpretación:
Esto crea una respuesta puntual y explosiva, que se siente como descargas eléctricas activadas por el ritmo.



 ¿Qué tipo de respuesta sonora te serviría más y por qué?

Te dejo esto listo para copiar:

Para mi palabra “electricidad”, me interesa trabajar con una combinación de dos tipos de respuesta sonora:

Respuesta continua (amplitud): para representar la energía constante, como un flujo eléctrico que nunca se detiene. Esto puede controlar elementos como el pulso, el brillo o el movimiento general del sistema.
Respuesta puntual (bass o golpes): para representar descargas eléctricas, chispas o rayos. Este tipo de respuesta permite generar eventos visuales más dramáticos y expresivos.

Considero que esta combinación es importante porque la electricidad no es solo un flujo estable, sino también algo que puede acumularse y liberarse de forma repentina.

Por eso, usar ambos tipos de datos del audio me permite construir una visual más rica, donde conviven lo continuo y lo explosivo.





https://editor.p5js.org/TheWarrior710/sketches/VvkJppmve



### Actividad 4

1. Prueba inicial

La prueba consiste en una visual interactiva donde la palabra “ELECTRICITY” está construida como un sistema físico usando Matter.js.
Cada letra funciona como un cuerpo conectado mediante restricciones, lo que genera un comportamiento elástico.

Al interactuar con el mouse:

La palabra vibra (simulando electricidad)
Se generan rayos visuales
Se activa un sonido
La segunda letra “E” se transforma en un rayo
2. ¿Qué parte de la palabra construiste?

Trabajé con la palabra completa:

“ELECTRICITY”

pero con una intervención semántica específica:

La segunda “E” fue modificada visualmente para representarse como un rayo
Las demás letras se mantienen legibles para conservar la lectura

Esto permite que la palabra siga siendo reconocible, pero con una carga visual más fuerte en el concepto de electricidad.

3. ¿Qué propiedad física manipulaste?

Usé Matter.js para simular física con:

Bodies (cuerpos): cada letra es un cuerpo circular
Constraints: conectan las letras como si fueran una cadena flexible
FrictionAir: controla la resistencia del movimiento
isStatic: la primera y última letra están fijas para mantener estructura

Además:

Eliminé la gravedad (world.gravity.y = 0)
→ para que la palabra no caiga y se mantenga flotando
Apliqué fuerzas con Body.applyForce()
→ para generar vibración al hacer click

Esto crea un comportamiento de tensión eléctrica contenida, no de caída.

4. ¿Qué aspecto del audio afecta qué comportamiento?

El audio se activa con interacción (click), pero en esta prueba:

El sonido no controla parámetros continuos
Funciona como evento detonante

Relación actual:

Click → reproduce sonido
Click → activa vibración física
Click → genera rayos

Es decir:

 El audio funciona como disparo energético, no como control continuo

5. Evaluación (qué funcionó y qué no)
Lo que funcionó
La vibración transmite muy bien la idea de electricidad
La letra “E” en forma de rayo refuerza el significado
El uso de constraints crea una sensación orgánica interesante
La interacción con click se siente clara y directa
Los rayos visuales ayudan a reforzar la narrativa


https://editor.p5js.org/TheWarrior710/sketches/hrT7kiiTn


<img width="763" height="442" alt="image" src="https://github.com/user-attachments/assets/d2e6d735-4370-44a1-aad7-612203999ef6" />




## Bitácora de aplicación 


### Actividad 5

 1. Palabra con intención semántica

La palabra elegida es:

ELECTRICITY

Elegí esta palabra porque permite trabajar conceptos como:

-energía
-vibración
-descarga
-tensión
-inestabilidad

Además, tiene elementos visuales fuertes (como la letra “E”) que se pueden reinterpretar como rayos.

 2. Tipografía semántica

La palabra no se presenta como texto estático, sino como un sistema visual donde:

Las letras están conectadas  representan flujo eléctrico
La segunda “E” se transforma en un rayo  refuerza el concepto directamente
Las letras vibran transmiten inestabilidad energética

Esto mantiene la legibilidad, pero introduce una capa simbólica:

 la palabra no solo se lee, se siente como electricidad

3. Comportamiento físico con sentido

Se utilizó Matter.js para construir el comportamiento:

Cada letra = un cuerpo físico (Body)
Las letras están conectadas con Constraints
simulando una cadena conductora

Decisiones clave:

Sin gravedad  la electricidad no cae, fluye
Primera y última letra fijas  mantienen estructura (circuito cerrado)
Fuerzas aleatorias al interactuar simulan descargas

Resultado:

 La palabra se comporta como un sistema eléctrico inestable, no como objetos físicos normales.

 4. Respuesta al audio con sentido semántico

El audio no está usado como fondo, sino como detonador:

El sonido se activa con click → representa una descarga eléctrica
Cada activación genera:
vibración en las letras
aparición de rayos
alteración del sistema físico

Esto crea una relación clara:

 audio = energía → activa electricidad

No es una reacción continua, sino puntual, como una descarga real.

 5. Pantalla completa

La pieza utiliza:

createCanvas(windowWidth, windowHeight)

Esto permite que la obra:

ocupe toda la pantalla
funcione como experiencia inmersiva


6. Sin dashboards ni instrucciones

No hay:

sliders
botones
texto en pantalla

La interacción es implícita:

el usuario descubre que el sistema responde al click

Esto refuerza la experiencia como obra, no como interfaz.


 7. Interacción performativa

La pieza funciona como un instrumento visual:

Mouse
Click  activa descarga eléctrica
Genera:
vibración
rayos
sonido

Esto permite “tocar” la pieza en vivo:

puedes controlar ritmo e intensidad de las descargas

 8. Moodboard / referencias

Referencias conceptuales:

Rayos eléctricos (naturaleza)
Estética glitch / energía digital
Visuales tipo NCS (electrónica)
Líneas de energía y circuitos

Características visuales:

colores oscuros + acentos amarillos
líneas irregulares (rayos)
movimiento vibratorio

 9. Bocetos (descripción)
Boceto 1

Palabra alineada horizontalmente
 conectada como cadena
 comportamiento estable

Boceto 2

Interacción:
 click genera vibración
 rayos salen del centro

Boceto final
palabra conectada
“E” transformada en rayo
sistema vibra + descarga

 10. Mapa de decisiones



 11. Mapa de interpretación

Cómo se ejecuta la pieza:

Click repetido  ritmo de descargas
Click lento  energía contenida
Click rápido  sobrecarga eléctrica

Esto permite interpretar la pieza en vivo como:

 un instrumento de energía

 12. Uso de IA

Uso de IA en el proceso:

Apoyo en:
estructura de código
integración de p5.js + Matter.js
solución de errores

Decisiones propias:

elección de la palabra
concepto de electricidad
comportamiento vibratorio
uso del rayo en la “E”
relación audio-interacción

 La IA fue usada como herramienta técnica, no como generador conceptual.

## Bitácora de reflexión
