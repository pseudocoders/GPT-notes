
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

Solución:

```javascript
//pendeinte para casa
```
