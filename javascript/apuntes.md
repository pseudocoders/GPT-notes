* Arrows
* Perdida del contexto de this
* Argumentos ... en funciones
* Objetos, creación literal y función constructora
  * herencia prototípica, cadena prototípica
  * EC6: class, contructor, extends, super


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
