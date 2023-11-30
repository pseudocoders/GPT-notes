
* Arrows
* Repaso de las funciones map filter y reduce con arrows
* Perdida del contexto de this
* Argumentos ... en funciones
* Creación literal de objetos
* Creación de objetos con función constructora
* new y this
* Herencia prototípica (prototype)
* Orientación a objetos ES6 con class, constructor, extends y super
* asincronismo y callbacks (ejemplo de los dos bucles)




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

### solucion let

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
            function outerFunction(x) {
                return function (y) {
                    return function (z) {
                        return x + y + z;
                    }
                }
            }
            var f1 = outerFunction(1)(2)(3);
            o.innerHTML += "f1 = " + f1 + "<br>";
            var f1 = outerFunction(1)(2);
            o.innerHTML += "f1 = " + f1(3) + "<br>";
            var f1 = outerFunction(1);
            o.innerHTML += "f1 = " + f1(2)(3) + "<br>";
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
            //------------------------------------
            o.innerHTML += "------------- <br>";
            function apply(func, arg) {
                return func(arg);
            }
            var add10 = apply(partialAdd, 10);
            o.innerHTML += "add10 = " + apply(add10, 5) + "<br>";
            //------------------------------------

        })
    </script>
</body>

</html>
```

```javascript
//pendeinte para casa
```
