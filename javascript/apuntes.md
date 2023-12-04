* diferencias entre let const y var
* Arrows
* Repaso de las funciones map filter y reduce con arrows
* Perdida del contexto de this
* Argumentos ... en funciones (rest operator) (spread operator)
* Creación literal de objetos
* Creación de objetos con función constructora
* new y this
* Herencia prototípica (prototype)
* Orientación a objetos ES6 con class, constructor, extends y super
* asincronismo y callbacks (ejemplo de los dos bucles)
* interpolación de strings
* aplicación parcial y currificación
* modo estricto
* parametros por defecto en javascript
* patron factory en javascript
* patron singleton
* pte: async/await + promises
* pte: repaso de map filter y reduce




# Pérdida del contexto de this

En JavaScript clásico (ES5), el valor de `this` puede cambiar dependiendo del contexto de ejecución. Un caso común en el que `this` pierde su contexto es cuando se utiliza dentro de una función anidada o callback. Veamos un ejemplo:

```javascript
function Persona(nombre) {
  this.nombre = nombre;
  
  this.saludar = function() {
    console.log("Hola, soy " + this.nombre);
    
    // Función anidada o callback
    setTimeout(function() {
      console.log("Hola después de 1 segundo, soy " + this.nombre);
    }, 1000);
  };
}

var persona = new Persona("Juan");
persona.saludar();
```

En este ejemplo, `this` dentro de la función anidada que se pasa a `setTimeout` ya no se refiere a la instancia de la persona. En lugar de eso, `this` apunta al objeto global (`window` en un navegador o `global` en Node.js), o es `undefined` en modo estricto.

Para solucionar este problema, se puede guardar una referencia al contexto deseado antes de entrar en la función anidada. Esto se hace comúnmente asignando `this` a una variable llamada `self` o `that`:

```javascript
function Persona(nombre) {
  var self = this; // Guardar referencia al contexto actual
  
  this.nombre = nombre;
  
  this.saludar = function() {
    console.log("Hola, soy " + this.nombre);
    
    // Función anidada o callback
    setTimeout(function() {
      console.log("Hola después de 1 segundo, soy " + self.nombre);
    }, 1000);
  };
}

var persona = new Persona("Juan");
persona.saludar();
```

Al hacer esto, `self` sigue apuntando al contexto deseado incluso dentro de la función anidada dentro de `setTimeout`. Este patrón era comúnmente utilizado en el pasado antes de que se introdujeran las arrow functions en ES6, ya que las arrow functions no crean su propio `this` y toman el valor del `this` del contexto circundante.

## Variante de Rafa

```javascript
function rafa(that){console.log("hola despues de 1 sec soy " + that.nombre); }
function Persona3(nombre) {
  this.nombre = nombre;
  
  this.saludar = function() {
    console.log("Hola, soy " + this.nombre);
    
    // Función anidada o callback
    setTimeout(rafa(this), 1000);
  };
}
var persona3 = new Persona3("Ana");
persona3.saludar();
```
## ejemplo de clase herencia
```javascript
function Animal(name, age) {
  this.name = name;
  this.age = age;
}

function Dog(name, age, breed) {
  Animal.call(this, name, age);
  this.breed = breed;
}

Dog.prototype=Animal.prototype;

Animal.prototype.emitirSonido=function(){console.log("aaaaaaah")};

Dog.emitirSonido() // ERROR VM750:1 Uncaught TypeError: Dog.emitirSonido is not a function
    
Animal.emitirSonido() // ERROR VM801:1 Uncaught TypeError: Animal.emitirSonido is not a function
    
tobi = new Dog("tobi",23,"ok");
tobi.emitirSonido()
VM705:1 aaaaaaah


pol=new Animal("pol",5);
pol.emitirSonido();
VM705:1 aaaaaaah

function Servivo(nombre){ 
   this.name=nombre;
}

Animal.prototype = Servivo.pro

Animal.prototype = Servivo.prototype;

Servivo.prototype.decirHola=function(){console.log("hola... ")};

geranio=new Servivo("geranio");

geranio.decirHola()
VM1491:1 hola... 

pol.decirHola() //ERROR: VM1620:1 Uncaught TypeError: pol.decirHola is not a function
    
pol2=new Animal("pol2",12);
pol2.decirHola()
VM1491:1 hola... 

pol.decirHola() //ERROR: VM1756:1 Uncaught TypeError: pol.decirHola is not a function
    
Object.prototype.saludar = function (){console.log("saluditos...")}

pol.saludar()
VM1950:1 saluditos...

tobi.saludar()
VM1950:1 saluditos...

```

## Asincronismo: Ejemplo de los dos for 

```javascript
for (var i=1;i<=10; i++){
    for (var j=1;j<=10;j++){
        setTimeout(function(){console.log(i,j)},1000)
    }
}
```

La functión anterior después de esperar 1 segundo imprime 100 parejas de (11,11)

Encontrar la solución para que al pasar un segundo imprima (1,1)(1,2)...(2,1)(2,2)...(10,10)

### solucion let

```javascript
for (let i=1;i<=10; i++){
    for (let j=1;j<=10;j++){
        setTimeout(function(){console.log(i,j)},1000)
    }
}
```

El comportamiento que estás observando está relacionado con el ámbito de las variables `var` y `let` en JavaScript, así como con el concepto de clausura (closure) en el manejo del tiempo de ejecución.

Cuando usas `var` en JavaScript, la variable se eleva (hoisting) al inicio del ámbito de la función más cercana, pero no respeta el ámbito de bloque. En tu caso, la variable `i` está siendo elevada al ámbito de la función que contiene tu bucle `for`, y se está utilizando en la función `setTimeout`. Sin embargo, debido a que las funciones de `setTimeout` se ejecutan después de que el bucle ha terminado, todas las funciones de `setTimeout` comparten la misma variable `i`, y al final del bucle, `i` tiene el valor de 11.

Al cambiar `var` por `let`, introduces el ámbito de bloque. Con `let`, la variable `i` está limitada al bloque `for`, y cada iteración del bucle crea una nueva variable `i` con su propio ámbito. Como resultado, cada función de `setTimeout` dentro del bucle captura el valor correcto de `i` al momento de la iteración.

En resumen, con `var`, todas las funciones de `setTimeout` comparten la misma variable `i`, mientras que con `let`, cada función de `setTimeout` tiene su propia variable `i` con el valor correcto. Esto lleva a la diferencia en el resultado observado.

### solucion con una función

```javascript
for (var i=1;i<=10; i++){
    for (var j=1;j<=10;j++){
        (function(i,j){
        setTimeout(function(){console.log(i,j)},1000)
        })(i,j)
    }
}
```
Justificación: se crea un ámbito local para i y j mediante la ejecución de una función.

Entro enfoque:

```javascript
function escribe(i,j){
  setTimeout(function(){console.log(i,j)},1000)
}
for (var i=1;i<=10; i++){
    for (var j=1;j<=10;j++){
      escribe(i,j);
    }
}
```



## Clase del miercoles

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ejemplos</title>
</head>

<body>
    <h1>Ejemplos</h1>
    <pre id="output"></pre>
    <script>
        window.addEventListener('DOMContentLoaded', () => {
            var o = document.getElementById("output");
            var f = function () {
                var a = 1;
                return (function () {
                    return a;
                })();
            }
            var b = f();
            o.innerHTML += "b = " + b + "<br>";
            var o = document.getElementById("output");
            var f = function () {
                var a = 1;
                return function () {
                    return a;
                };
            }
            var b = f()();
            o.innerHTML += "b = " + b + "<br>";
            //---------------------------------------
            o.innerHTML += "------------- <br>";
            function contador1() {
                var c = 0;
                return function () {
                    return ++c;
                }
            }
            // o.innerHTML += "quiero acceder a c " + c + "<br>"; ERROR!!! no se puede
            var c1 = contador1();
            o.innerHTML += "c1 = " + c1() + "<br>";
            //------------------------------------
            o.innerHTML += "------------- <br>";
            function contador2() {
                var c = 0;
                return function () {
                    return ++c;
                }
            }
            // o.innerHTML += "quiero acceder a c " + c + "<br>"; ERROR!!! no se puede
            var c2 = contador2();
            c2();
            c2();
            c2();
            o.innerHTML += "c2 = " + c2() + "<br>";
            //------------------------------------
            function contador3() {
                var c = 0;
                return {
                    incrementar: function () {
                        return ++c;
                    },
                    decrementar: function () {
                        return --c;
                    },
                    reset: function () {
                        c = 0;
                    },
                    consultar: function () {
                        return c;
                    }
                }
            }
            // o.innerHTML += "quiero acceder a c " + c + "<br>"; ERROR!!! no se puede
            o.innerHTML += "------------- <br>";
            var c3 = contador3();
            c3.incrementar();
            c3.incrementar();
            c3.decrementar();
            o.innerHTML += "c3 = " + c3.consultar() + "<br>";
            c3.incrementar();
            c3.incrementar();
            o.innerHTML += "c3 = " + c3.consultar() + "<br>";
            c3.reset();
            o.innerHTML += "c3 = " + c3.consultar() + "<br>";
            //------------------------------------
            o.innerHTML += "------------- <br>";
            function suma(x) {
                return function (y) {
                    return function (z) {
                        return x + y + z;
                    }
                }
            }
            var f1 = suma(1)(2)(3);
            o.innerHTML += "resultado1 = " + f1 + "<br>";
            var add3 = suma(1)(2);
            o.innerHTML += "resultado2 = " + add3(3) + "<br>";
            var add1 = suma(1);
            o.innerHTML += "resultado3 = " + add1(2)(3) + "<br>";            
            //------------------------------------
            o.innerHTML += "------------- <br>";
            function add(x, y) {
                return x + y;
            }
            function partialAdd(x) {
                return function (y) {
                    return add(x, y);
                };
            }
            var add5 = partialAdd(5);
            o.innerHTML += "add5 = " + add5(3) + "<br>";
            function ejecuta(func, arg) {
                return func(arg);
            }
            var add10 = ejecuta(partialAdd, 10);
            o.innerHTML += "add10 = " + ejecuta(add10, 5) + "<br>";
            //------------------------------------
            o.innerHTML += "------------- <br>";
            // función curry genérica en javascript clásico
            function curry(func) {
                var args = Array.prototype.slice.call(arguments, 1); //args es todo menos la funcion
                return function () {
                    return func.apply(this, args.concat( //se ejecuta func con los argumentos
                        Array.prototype.slice.call(arguments, 0)
                    ));
                };
            }
            var add = function (x, y) {
                return x + y;
            };
            var add10 = curry(add, 10);
            o.innerHTML += "add10 = " + add10(5) + "<br>";
            //ejemplo más complejo
            function add(x, y, z) {
                return x + y + z;
            }
            var add10 = curry(add, 10);
            var add10y20 = curry(add10, 20);
            o.innerHTML += "add10y20 = " + add10y20(30) + "<br>";
            //------------------------------------
            // función curry genérica en javascript Ecmascript 6
            function curryES6(func, ...args) {
                return function (...args2) {
                    return func(...args, ...args2);
                };
            }
            var add = function (x, y) {
                return x + y;
            };
            var add10 = curryES6(add, 10);
            o.innerHTML += "add10 = " + add10(5) + "<br>";
            //ejemplo más complejo
            function add(x, y, z) {
                return x + y + z;
            }
            var add10 = curryES6(add, 10);
            var add10y20 = curryES6(add10, 20);
            o.innerHTML += "add10y20 = " + add10y20(30) + "<br>";
        })
    </script>
</body>

</html>
```
## Ejemplos de clase (jueves)

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ejemplos</title>
</head>

<body>
    <h1>Ejemplos</h1>
    <pre id="output"></pre>
    <script>
        window.addEventListener('DOMContentLoaded', () => {
            var o = document.getElementById("output");
            //---------------------------------------
            o.innerHTML += "<h1>function factory </h1>";

            function creaMusico(nombre) {
                return function (instrumento) {
                    o.innerHTML += nombre + " toca " + instrumento + "<br>";
                }
            }
            let musico1 = creaMusico("Juan");
            let musico2 = creaMusico("Pedro");
            musico1("guitarra");
            musico2("bajo");
            musico1("batería");

            //---------------------------------------
            o.innerHTML += "<h1>function factory</h1>";

            // Definición de la interfaz (puede ser una clase abstracta o una interfaz en otros lenguajes)
            class Product {
                constructor(name) {
                    this.name = name;
                }

                display() {
                    o.innerHTML += `Product: ${this.name}`;
                }
            }

            // Implementación de productos específicos
            class ConcreteProductA extends Product {
                constructor(name) {
                    super(name);
                }

                display() {
                    o.innerHTML += `ConcreteProductA: ${this.name} <br>`;
                }
            }

            class ConcreteProductB extends Product {
                constructor(name) {
                    super(name);
                }

                display() {
                    o.innerHTML += `ConcreteProductB: ${this.name} <br>`;
                }
            }

            // Factory que crea objetos Product
            class ProductFactory {
                createProduct(type, name) {
                    switch (type) {
                        case 'A':
                            return new ConcreteProductA(name);
                        case 'B':
                            return new ConcreteProductB(name);
                        default:
                            throw new Error('Invalid product type');
                    }
                }
            }

            // Uso del Factory
            const factory = new ProductFactory();

            const productA = factory.createProduct('A', 'ProductA');
            const productB = factory.createProduct('B', 'ProductB');

            productA.display(); // Debería imprimir "ConcreteProductA: ProductA"
            productB.display(); // Debería imprimir "ConcreteProductB: ProductB"






            o.innerHTML += "<h1>patron singleton</h1>";

            const Singleton = (function () {
                let instance;


                function createInstance(nombre) {
                    return {
                        name: nombre,
                        saluda: function () {
                            o.innerHTML += "hola soy " + this.name + "<br>";
                        },
                    };
                }

                return {
                    getInstance: function (nombre) {
                        if (!instance) {
                            instance = createInstance(nombre);
                        }
                        return instance;
                    },
                };
            })();

            // Uso del Singleton
            const instance1 = Singleton.getInstance("Juan");
            const instance2 = Singleton.getInstance("Pedro");
            const instance3 = Singleton.getInstance("Ana");

            o.innerHTML += "son la misma instancia? " + (instance1 === instance2) + "<br>"; // Debería imprimir true
            o.innerHTML += "son la misma instancia? " + (instance1 === instance3) + "<br>"; // Debería imprimir true
            o.innerHTML += "Soy " + instance1.name + "<br>";
            instance1.saluda();
            instance2.saluda();
            instance3.saluda();



        })
    </script>
</body>

</html>
```
## Promesas: ejemplo de clase

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ejemplos</title>
</head>

<body>
    <h1>Ejemplos</h1>
    <pre id="output"></pre>
    <script>
        window.addEventListener('DOMContentLoaded', () => {
            var o = document.getElementById("output");
            //---------------------------------------
            o.innerHTML += "<h1>promises</h1>";

            var oPromise = new Promise(function (resolve, reject) {
                setTimeout(function () {
                    //random resolution
                    if (Math.random() > 0.4) {
                        resolve('ven a por tu hamburguesa');
                    } else {
                        reject('vete a otro sitio');
                    }
                }, Math.random() * 5000);
            });


            //HACER OTRAS COSAS

            oPromise.then(function (value) {
                o.innerHTML += "then: " + value + "<br>";
                // expected output: "foo"
            }).catch((error) => {
                o.innerHTML += "then: ERROR " + error + "<br>";
            });

            o.innerHTML += "--> ya tengo la promesa estoy charlando con mis amigos y amigas<br>";
            // expected output: [object Promise]

        })
    </script>
</body>

</html>
```



