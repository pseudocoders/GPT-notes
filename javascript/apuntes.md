* Arrows
* Perdida del contexto de this
* Argumentos ... en funciones
* Objetos, creación literal y función constructora
  * herencia prototípica, cadena prototípica
  * EC6: class, contructor, extends, super
* asincronismo y callbacks


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

undefined
function Dog(name, age, breed) {
  Animal.call(this, name, age);
  this.breed = breed;
}
undefined
Dog.prototype=Animal.prototype;
{constructor: ƒ}
Animal.prototype.emitirSonido=function(){console.log("aaaaaaah")};
ƒ (){console.log("aaaaaaah")}
Dog.emitirSonido()
VM750:1 Uncaught TypeError: Dog.emitirSonido is not a function
    at <anonymous>:1:5
(anonymous) @ VM750:1
Animal.emitirSonido()
VM801:1 Uncaught TypeError: Animal.emitirSonido is not a function
    at <anonymous>:1:8
(anonymous) @ VM801:1
tobi = new Dog("tobi",23,"ok");
Dog {name: 'tobi', age: 23, breed: 'ok'}
tobi.emitirSonido()
VM705:1 aaaaaaah
undefined
pol=new Animal("pol",5);
Animal {name: 'pol', age: 5}
pol.emitirSonido();
VM705:1 aaaaaaah
undefined
function Servivo(nombre){ 
   this.name=nombre;
}
undefined
Animal.pro
undefined
Animal.prototype = Servivo.pro
undefined
Animal.prototype = Servivo.prototype;
{constructor: ƒ}
Servivo.prototype.decirHola=function(){console.log("hola... ")};
ƒ (){console.log("hola... ")}
geranio=new Servivo("geranio");
Servivo {name: 'geranio'}
geranio.decirHola()
VM1491:1 hola... 
undefined
pol.decirHola()
VM1620:1 Uncaught TypeError: pol.decirHola is not a function
    at <anonymous>:1:5
(anonymous) @ VM1620:1
pol2=new Animal("pol2",12);
Animal {name: 'pol2', age: 12}
pol2.decirHola()
VM1491:1 hola... 
undefined
pol.decirHola()
VM1756:1 Uncaught TypeError: pol.decirHola is not a function
    at <anonymous>:1:5
(anonymous) @ VM1756:1
Object.prototype.saludar = function (){console.log("saluditos...")}
ƒ (){console.log("saluditos...")}
pol.saludar()
VM1950:1 saluditos...
undefined
tobi.saludar()
VM1950:1 saluditos...
undefined
pol.decirHola()****
```
