# 3D-visual-tracking-trainer
Repository for a web trainer to help develop visual awareness

## Resumen
El objetivo de la aplicación es el entrenamiento de rastreo visual en un espacio 3D, libre de contexto, con el propósito de mejorar el reconocimiento visual de patrones en practicantes de diversos deportes.

Para esto se utilizó el lenguaje de programación Javascript y la librería three.js, la cual nos permitió generar un espacio 3D y los objetos a rastrear, así como el color y la animación de movimiento.

## Introducción
En deportes dinámicos (soccer, football americano, basketball, etc.) la capacidad de "leer el juego" distingue a los atletas hábiles, sin embargo, esta habilidad no está ligada a la capacidad visual del atleta. En lugar de esto, los científicos han identificado habilidades ligadas a la anticipación de movimientos y toma de decisiones, las cuales representan la habilidad del cerebro de extraer contexto de la escena visual.

El objetivo de la aplicación desarrollada es tener un sistema de entrenamiento y evaluación de dicha capacidad en un jugador. Existe evidencia de que el entrenamiento de rastreo de objetos en movimiento en 3D puede mejorar las habilidades cognitivas al mejorar la atención y la velocidad de procesamiento de información visual así como la memoria.

## Método
Se utilizó la libreria three.js que tiene funcionalidades de renderizado y animación de objetos 3D, incluyendo mallas, texturas, materiales, luces y sombras. Esta libreria corresponde al lenguaje Javascript que, junto con una página simple de html, será usado para crear nuestra aplicación, cuyo objetivo es precisamente el entrenamiento de rastreo visual de objetos 3D, además se utilizó la libreria de física cannon.js para manejar las colisiones de los objetos.

## Requerimentos
Para alcanzar el objetivo fijado se diseñó e implementó una aplicación que:


1.   Presenta objetos 3D en un espacio.
2.   Dichos objetos deben estar libres de contexto, es decir, deben ser figuras simples sin características excepcionales que podrían facilitar su identificación.
3.   Realiza un movimiento de estos objetos.
4.   Realiza una evaluación de qué tan bien fueron rastreados.
5.   Es infinitamente reutilizable, los patrones no están prediseñados, se generan.

## Diseño
Para cumplir con los requerimentos anteriores se diseño una aplicación con las siguientes características:


*   El programa genera 8 esferas en posiciones al azar en un espacio.
*   4 de estas esferas, seleccionadas al azar, se pintan brevemente de rojo.
*   A las 8 esferas se les asigna una dirección y velocidad al azar representados como un vector 3D.
*   Una vez terminado el movimiento el usuario debe selecionar las esferas que fueron marcadas en rojo.
*   El programa otorga una evaluación de que tan bueno fue el rastreo, dependiendo de los aciertos del usuario

## Resultados
En este apartado se muestra el programa resultante así como partes importantes del código que se utilizó.
Comenzamos con 8 esferas colocadas al azar que posteriormente se pintan de rojo, despues de un tiempo cada esfera vuelve a pintarse de amarillo y comienzan a moverse en direcciones aleatorias.
![texto alternativo](https://i.imgur.com/t3LbWCw.png)
Cuando este movimiento termina se le pide al usuario que indique las esferas que se habian pintado originalmente.
![texto alternativo](https://i.imgur.com/q0pAkO7.png)
Una vez que el usuario ha elegido 4 esferas, el programa indica cuántas de las selecciones fueron acertadas.![texto alternativo](https://i.imgur.com/SiYPsKW.png)

El comportamiento de los objetos 3D usa la librería cannon.js
Aqui se definen parámetros como gravedad y los objetos físicos que van a estar unidos a los objetos visuales de three.js, los cuerpos de las esferas comienzan en posiciones generadas al azar.
```
function initCannon() {
            //se crea el mundo de cannon
            world = new CANNON.World();
            world.gravity.set(0, 0, 0);
            world.broadphase = new CANNON.NaiveBroadphase();
            world.solver.iterations = 100;
            //se crean las esferas y a cada una se le da una posicion y velocidad
            shape = new CANNON.Sphere(3);
            mass = .1;
            j = 0;
            k = 0;
            while (j < 8) {
                shape = new CANNON.Sphere(sphradius);
                Bodys[j] = new CANNON.Body({
                    mass: 1
                });
                Bodys[j].addShape(shape);
                Bodys[j].angularDamping = 0.5;
                rand.randomizePosition();
                Bodys[j].position.set(rand.x, rand.y, rand.z);
                //Bodys[j].quaternion.set(0, 0, 0);
                Bodys[j].velocity.set(1, 1, 1)
                world.addBody(Bodys[j]);
                j++;
            }
        }
```
Se pintan 4 esferas aleatorias de rojo. Como podemos ver, la variable t es un contador de cuántas veces se ha llamado a la función de animación, lo cual nos sirve como medición del tiempo que ha transcurrido.
```
if (t > 75 && done == 0) {
                for (j = 0; j < 4; j++) {
                    rnd = Math.round(Math.random() * (7 - j - (0)) + (0));
                    it = randoms[rnd];
                    correct.push(it);
                    randoms.splice(rnd, 1);
                    spheres[it].material = redMaterial;
                }
                done = 1;
Se asigna una velocidad (y en consecuencia dirección) aleatoria a cada cuerpo.
```
```
if (t == 150) {
                Bodys.forEach(element => {
                    element.velocity.set(rands[i].x * .5, rands[i].y * .5, rands[i].z * .5)
                    i++;
                });
            }
```
Aqui se compara la respuesta del usuario con la respuesta correcta.
```
function checkAnswer() { //compara la respuesta del usuario con la correcta
            cnt = 0;
            correct.sort();
            answers.sort();
            correct.forEach(a => {
                answers.forEach(b => {
                    if (a === b) {
                        cnt++;
                    }
                });
            });
            return cnt;
        }
```
Tras lo cual se muestra su puntuación
```
 if (t > 800) {
                txt[0].textContent = "Seleccione las esferas que se pintaron en rojo";
                complete = 1;
                if (answers.length > 3) { //se indica n/4 esferas correctas
                    txt[0].textContent = checkAnswer().toString() + "/4 correctas!";
                    complete = 0;
                }
            }
```

Estos son algunos ejemplos del código que se utilizó para crear la aplicación web.





