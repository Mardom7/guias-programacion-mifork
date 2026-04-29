# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

### Respuesta

Un **puntero a función** en C es una variable que almacena la dirección de memoria de una función. Al igual que un puntero normal apunta a un dato, un puntero a función apunta a código ejecutable. Esto permite pasar funciones como argumentos a otras funciones, guardarlas en variables o invocarlas indirectamente.

La sintaxis del tipo de un puntero a función incluye el tipo de retorno y los tipos de los parámetros, lo que permite al compilador verificar que la función apuntada tiene la firma correcta. En el ejemplo, `aMayusculas` es una variable local que apunta a la función `convertirAMayusculas` y se usa para invocarla de forma indirecta.

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Función que convierte una cadena a mayúsculas (modifica la cadena in situ)
char* convertirAMayusculas(char* cadena) {
    for (int i = 0; cadena[i]; i++) {
        cadena[i] = toupper(cadena[i]);
    }
    return cadena;
}

int main() {
    // Puntero a función: devuelve char* y recibe char*
    char* (*aMayusculas)(char*) = convertirAMayusculas;

    char texto[] = "hola mundo";
    printf("%s\n", aMayusculas(texto)); // HOLA MUNDO
    return 0;
}
```

---

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta

Una **función lambda** (también llamada función anónima) es una función que se define directamente en el lugar donde se usa, sin necesidad de darle un nombre ni declararla en otro sitio. Es equivalente a un puntero a función de C, pero con una sintaxis mucho más compacta y, como se verá más adelante, con la capacidad adicional de capturar variables del entorno donde se define.

En JavaScript las funciones son ciudadanos de primera clase de forma nativa, por lo que una lambda se puede asignar directamente a una variable. En Java, desde la versión 8, también se pueden definir lambdas, pero necesitan un tipo que las respalde: una **interfaz funcional** como `Function<String, String>`.

```javascript
// JavaScript
const aMayusculas = cadena => cadena.toUpperCase();
console.log(aMayusculas("hola mundo")); // HOLA MUNDO
```

```java
// Java
import java.util.function.Function;

Function<String, String> aMayusculas = cadena -> cadena.toUpperCase();
System.out.println(aMayusculas.apply("hola mundo")); // HOLA MUNDO
```

---

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta

El **paradigma funcional** es un estilo de programación en el que el código se estructura como la composición y aplicación de funciones matemáticas puras, evitando el estado mutable y los efectos secundarios. Las funciones no modifican datos externos: dado el mismo input, siempre producen el mismo output. Este enfoque facilita el razonamiento sobre el código, su prueba y su paralelización.

Java es un lenguaje orientado a objetos en su núcleo, pero desde Java 8 incorporó elementos del paradigma funcional: lambdas, interfaces funcionales, streams y referencias a métodos. Por eso se dice que es **multi-paradigma**: permite programar tanto con objetos y clases como con funciones de primera clase y composición funcional, eligiendo el estilo más adecuado para cada problema.

Decir que las funciones son **ciudadanos de primera clase** significa que las funciones pueden tratarse como cualquier otro valor: asignarse a variables, pasarse como argumentos a otras funciones, devolverse como resultado de otras funciones y almacenarse en estructuras de datos. En C los punteros a función lo permiten de forma limitada; en Java 8 y JavaScript se consigue de manera más natural y expresiva.

---

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta

Una lambda en Java se compone de tres partes separadas por el operador flecha `->`: la lista de parámetros, el operador flecha y el cuerpo. El compilador infiere los tipos de los parámetros a partir de la interfaz funcional esperada, por lo que no es obligatorio escribirlos.

Si el cuerpo es una sola expresión, no hacen falta llaves ni `return`: el valor de la expresión se devuelve automáticamente. Si el cuerpo necesita varias sentencias, se usan llaves y `return` explícito.

```java
// Sin parámetros
Runnable r = () -> System.out.println("hola");

// Un parámetro (los paréntesis son opcionales con uno solo)
Function<String, String> f1 = s -> s.toUpperCase();

// Un parámetro con tipo explícito
Function<String, String> f2 = (String s) -> s.toUpperCase();

// Varios parámetros
BiFunction<Integer, Integer, Integer> suma = (a, b) -> a + b;

// Cuerpo con varias sentencias
Function<String, String> f3 = s -> {
    String resultado = s.trim();
    return resultado.toUpperCase();
};
```

---

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta

Pasar una función como parámetro es la base de la programación de orden superior. El método `transformar` no sabe qué transformación concreta se va a aplicar: solo sabe que recibirá una cadena y una función, y que debe invocar esa función pasándole la cadena. La lógica concreta queda en manos de quien llama al método.

En JavaScript, al ser las funciones ciudadanos de primera clase de forma nativa, el parámetro se declara como cualquier otro y se invoca directamente. En Java, el parámetro debe tener el tipo de la interfaz funcional correspondiente y se invoca con el método `apply`.

```javascript
// JavaScript
const aMayusculas = cadena => cadena.toUpperCase();

function transformar(texto, funcion) {
    return funcion(texto);
}

console.log(transformar("hola mundo", aMayusculas)); // HOLA MUNDO
```

```java
// Java
import java.util.function.Function;

public static String transformar(String texto, Function<String, String> funcion) {
    return funcion.apply(texto);
}

// Uso:
Function<String, String> aMayusculas = s -> s.toUpperCase();
System.out.println(transformar("hola mundo", aMayusculas)); // HOLA MUNDO
```

---

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta

Una de las ventajas de las lambdas es que no es necesario asignarlas a una variable previamente: se pueden definir directamente en el punto de la llamada, como un valor literal. Esto hace el código más compacto cuando la función solo se usa en un lugar.

En el ejemplo, la lambda que invierte la cadena se escribe directamente como argumento de `transformar`, sin necesidad de crear una variable `aInvertida` aparte.

```javascript
// JavaScript: lambda definida directamente en la llamada
console.log(
    transformar("hola mundo", cadena => cadena.split("").reverse().join(""))
); // odnum aloh
```

```java
// Java: lambda definida directamente en la llamada
System.out.println(
    transformar("hola mundo", s -> new StringBuilder(s).reverse().toString())
); // odnum aloh
```

---

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta

Un **closure** (cierre) es la capacidad de una función lambda de "recordar" y acceder a las variables del entorno donde fue definida, incluso cuando se ejecuta en un contexto diferente. La lambda no solo captura su lógica, sino también el estado de las variables externas que necesita en el momento de su creación.

En Java, una lambda puede capturar variables locales del método donde se define, pero con una restricción: esas variables deben ser **efectivamente finales** (`effectively final`), es decir, no pueden ser modificadas después de su asignación. Esto garantiza que el comportamiento de la lambda sea predecible y seguro.

En el ejemplo, la lambda captura la variable `sufijo` del método exterior. Aunque `sufijo` no es un parámetro de la lambda, esta la "recuerda" y la usa cuando se invoca más tarde.

```java
import java.util.function.Function;

public static String transformar(String texto, Function<String, String> funcion) {
    return funcion.apply(texto);
}

public static void main(String[] args) {
    String sufijo = " [procesado]";  // variable local del contexto exterior

    // La lambda captura 'sufijo' aunque no sea su parámetro
    Function<String, String> concatenar = s -> s + sufijo;

    System.out.println(transformar("hola mundo", concatenar));
    // hola mundo [procesado]
}
```

---

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta

La diferencia fundamental es que un **puntero a función en C** solo almacena la dirección de una función ya definida en el código. No puede capturar ninguna variable del contexto donde se crea: únicamente sabe dónde está el código de la función, nada más.

Una **lambda**, en cambio, es un **closure**: empaqueta junto con la función el entorno de variables que necesita. Puede "llevarse" consigo variables locales del ámbito donde fue creada y usarlas cuando se ejecute más tarde, aunque ese ámbito ya no exista. Esto las hace mucho más expresivas y potentes.

| | Puntero a función (C) | Lambda / Closure |
|---|---|---|
| ¿Apunta a código? | ✅ Sí | ✅ Sí |
| ¿Captura variables del entorno? | ❌ No | ✅ Sí |
| ¿Se puede definir en línea? | ❌ No (necesita nombre) | ✅ Sí |
| ¿Tiene estado propio? | ❌ No | ✅ Sí (variables capturadas) |

---

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta

`crearDescuento` es una **función de orden superior** que devuelve otra función. Cada vez que se llama, produce una nueva función de descuento personalizada con el porcentaje indicado. Esto permite crear múltiples descuentos reutilizables a partir de una sola definición.

La **closure** está en que la lambda devuelta captura la variable `porcentaje` del método `crearDescuento`. Cuando `crearDescuento` termina de ejecutarse, su variable local `porcentaje` normalmente desaparecería de la pila, pero la lambda la ha capturado y la conserva. Cada función de descuento creada recuerda su propio porcentaje de forma independiente.

```java
import java.util.function.Function;

public static Function<Double, Double> crearDescuento(double porcentaje) {
    // La lambda captura 'porcentaje' del contexto de crearDescuento
    return precio -> precio * (1 - porcentaje / 100);
}

public static void main(String[] args) {
    Function<Double, Double> descuento10 = crearDescuento(10); // captura 10.0
    Function<Double, Double> descuento25 = crearDescuento(25); // captura 25.0

    double precio = 200.0;
    System.out.println(descuento10.apply(precio)); // 180.0
    System.out.println(descuento25.apply(precio)); // 150.0
}
```

---

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta

Una **interfaz funcional** es una interfaz que tiene exactamente **un único método abstracto**. Ese método abstracto es el que define la "forma" de la función: cuántos parámetros recibe, de qué tipos, y qué devuelve. Cuando se asigna una lambda a una variable, el compilador comprueba que la firma de la lambda coincide con la del método abstracto de la interfaz funcional esperada.

La anotación `@FunctionalInterface` es opcional pero recomendable: le indica al compilador que debe verificar que la interfaz cumple el requisito de tener un solo método abstracto, y produce un error si se intenta añadir un segundo. La interfaz puede tener métodos `default` o `static` sin problema, ya que estos no son abstractos y no cuentan.

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String entrada); // único método abstracto

    // Métodos default o static están permitidos:
    default Transformador andThen(Transformador siguiente) {
        return s -> siguiente.transformar(this.transformar(s));
    }
}

// Una lambda satisface la interfaz si su firma coincide:
Transformador aMayusculas = s -> s.toUpperCase(); // ✅
```

---

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta

Al definir `Transformador` a mano se tiene control total sobre el nombre del método y se puede hacer el código más expresivo y legible que usando `Function<String, String>`. Además, la anotación `@FunctionalInterface` hace explícita la intención del diseño.

La interfaz solo necesita declarar el método abstracto con la firma deseada. El resto (implementación) lo proporciona quien crea la lambda.

```java
@FunctionalInterface
public interface Transformador {
    String transformar(String entrada);
}
```

```java
// Uso: exactamente igual que antes, pero con nombre más expresivo
Transformador aMayusculas = s -> s.toUpperCase();
Transformador aInvertida  = s -> new StringBuilder(s).reverse().toString();

public static String transformar(String texto, Transformador t) {
    return t.transformar(texto);
}

System.out.println(transformar("hola", aMayusculas)); // HOLA
System.out.println(transformar("hola", aInvertida));  // aloh
```

---

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta

Al añadir parámetros de tipo `<E, S>` (entrada y salida), la interfaz `Transformador` se vuelve reutilizable para cualquier par de tipos, no solo `String → String`. El compilador seguirá comprobando que los tipos son correctos en cada uso concreto.

En el ejemplo, `Transformador<Double, Integer>` representa una función que recibe un `Double` y devuelve un `Integer`. La lambda que redondea satisface exactamente esa firma.

```java
@FunctionalInterface
public interface Transformador<E, S> {
    S transformar(E entrada);
}
```

```java
// Transformador que redondea un Double en un Integer
Transformador<Double, Integer> redondear = d -> (int) Math.round(d);

System.out.println(redondear.transformar(3.7)); // 4
System.out.println(redondear.transformar(2.2)); // 2

// El tipo de E y S se infiere automáticamente:
Transformador<String, Integer> longitud = s -> s.length();
System.out.println(longitud.transformar("hola")); // 4
```

---

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta

Java incluye en el paquete `java.util.function` un conjunto de interfaces funcionales predefinidas que cubren los casos de uso más habituales. Usarlas evita definir interfaces propias redundantes y hace el código más estándar e interoperable con la API de Java (streams, colecciones, etc.).

Las más importantes se agrupan según si consumen datos, los producen, los transforman o los evalúan:

| Interfaz | Firma del método | Descripción |
|---|---|---|
| `Function<T, R>` | `R apply(T t)` | Transforma un valor de tipo T en uno de tipo R |
| `BiFunction<T, U, R>` | `R apply(T t, U u)` | Transforma dos valores en uno |
| `Consumer<T>` | `void accept(T t)` | Consume un valor sin devolver nada |
| `BiConsumer<T, U>` | `void accept(T t, U u)` | Consume dos valores |
| `Supplier<T>` | `T get()` | Produce un valor sin recibir nada |
| `Predicate<T>` | `boolean test(T t)` | Evalúa una condición sobre un valor |
| `BiPredicate<T, U>` | `boolean test(T t, U u)` | Evalúa una condición sobre dos valores |
| `UnaryOperator<T>` | `T apply(T t)` | Transforma un T en otro T (caso especial de Function) |
| `BinaryOperator<T>` | `T apply(T t1, T t2)` | Combina dos T en otro T |
| `Runnable` | `void run()` | Sin parámetros ni retorno |

```java
// Ejemplos de uso:
Function<String, Integer>  longitud  = String::length;
Consumer<String>           imprimir  = System.out::println;
Supplier<String>           saludo    = () -> "Hola";
Predicate<Integer>         esPositivo = n -> n > 0;
UnaryOperator<String>      mayusculas = String::toUpperCase;
BinaryOperator<Integer>    suma       = (a, b) -> a + b;
```

---

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta

`List.forEach` recibe un `Consumer<T>` y lo aplica a cada elemento de la lista. Es el equivalente funcional del bucle `for-each` clásico, pero expresa la intención de forma más directa: "para cada elemento, haz esto". No devuelve nada ni transforma la lista; solo consume sus elementos.

La lambda que se pasa a `forEach` se ejecuta una vez por cada elemento. En el ejemplo, la lógica del mensaje positivo queda encapsulada en la propia lambda, sin necesidad de declarar variables auxiliares ni escribir la estructura del bucle.

```java
import java.util.List;

List<Integer> numeros = List.of(-3, 0, 5, -1, 8, 2);

// Con for-each clásico:
for (Integer n : numeros) {
    if (n > 0) System.out.println(n + " es positivo");
}

// Con forEach funcional:
numeros.forEach(n -> {
    if (n > 0) System.out.println(n + " es positivo");
});
```

---

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta

La firma de `forEach` es `void forEach(Consumer<? super T> action)`. Se usa `? super T` porque `forEach` solo va a **escribir** (pasar) elementos de tipo `T` al consumer, nunca a leer de él. Esto permite pasar un `Consumer<Number>` donde se espera un `Consumer<Integer>`: si el consumer sabe tratar `Number`, también sabe tratar `Integer`. Sin el wildcard, un `Consumer<Number>` no sería aceptado aunque fuera perfectamente válido.

**PECS** (Producer Extends, Consumer Super) es la regla para elegir el wildcard correcto. Si la estructura **produce** datos que se van a leer, se usa `? extends T`. Si la estructura **consume** datos que se le van a pasar, se usa `? super T`. `forEach` pasa elementos al consumer (lo usa como consumidor), así que aplica `? super T`.

Aplicado al método `transformar`, si se cambia el parámetro de `Function<String, String>` a `Function<? super String, ? extends String>`, se gana flexibilidad: se puede pasar una función que reciba `Object` (supertipo de `String`) y devuelva `String` o un subtipo.

```java
// Firma mejorada con PECS:
public static String transformar(
        String texto,
        Function<? super String, ? extends String> funcion) {
    return funcion.apply(texto);
}

// Ahora acepta, por ejemplo, una Function<Object, String>:
Function<Object, String> f = obj -> obj.toString().toUpperCase();
System.out.println(transformar("hola", f)); // HOLA
```

---

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta

Una **referencia a método** es una forma abreviada de escribir una lambda que simplemente llama a un método ya existente. En lugar de `s -> s.toUpperCase()` se puede escribir `String::toUpperCase`. Es sintácticamente más limpio cuando la lambda no hace nada más que delegar en un método.

En JavaScript se obtiene con `.bind()` para preservar el contexto `this`. En Java la sintaxis es `instancia::método`, y el compilador infiere que la firma del método encaja con la interfaz funcional esperada.

```javascript
// JavaScript
class Persona {
    constructor(nombre) { this.nombre = nombre; }
    saludar() { console.log("Hola, soy " + this.nombre); }
}

const persona = new Persona("Ana");
const saludar = persona.saludar.bind(persona); // referencia al método
saludar(); // Hola, soy Ana
```

```java
// Java
public class Persona {
    private final String nombre;
    public Persona(String nombre) { this.nombre = nombre; }
    public void saludar() { System.out.println("Hola, soy " + nombre); }
}

// En el main:
Persona persona = new Persona("Ana");
Runnable saludar = persona::saludar; // referencia al método de instancia
saludar.run(); // Hola, soy Ana
```

---

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta

Java permite cuatro tipos de referencias a método, cada una con su propia sintaxis. Todas son equivalentes a una lambda que simplemente llama a ese método, pero resultan más concisas y expresivas.

```java
import java.util.function.*;

// 1. Referencia a método ESTÁTICO: Clase::metodoEstatico
//    Equivale a: x -> Integer.parseInt(x)
Function<String, Integer> parsear = Integer::parseInt;
System.out.println(parsear.apply("42")); // 42

// 2. Referencia a CONSTRUCTOR: Clase::new
//    Equivale a: nombre -> new Persona(nombre)
Function<String, Persona> crear = Persona::new;
Persona p = crear.apply("Luis");

// 3. Referencia a método de instancia de una instancia CONCRETA: instancia::metodo
//    Equivale a: () -> persona.saludar()
Persona ana = new Persona("Ana");
Runnable saludarAna = ana::saludar;
saludarAna.run(); // Hola, soy Ana

// 4. Referencia a método de instancia sobre CUALQUIER instancia: Clase::metodo
//    Equivale a: s -> s.toUpperCase()
//    El primer parámetro de la lambda es la instancia sobre la que se llama
Function<String, String> mayusculas = String::toUpperCase;
System.out.println(mayusculas.apply("hola")); // HOLA
```

| Tipo | Sintaxis | Equivalente lambda |
|---|---|---|
| Método estático | `Clase::metodo` | `x -> Clase.metodo(x)` |
| Constructor | `Clase::new` | `x -> new Clase(x)` |
| Instancia concreta | `objeto::metodo` | `() -> objeto.metodo()` |
| Cualquier instancia | `Clase::metodo` | `obj -> obj.metodo()` |

---

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta

`Collections.sort` acepta un `Comparator<T>`, que es una interfaz funcional con un único método abstracto `compare(T o1, T o2)` que devuelve un entero negativo, cero o positivo según el orden. Al pasar una lambda, se evita crear una clase anónima entera para algo tan puntual.

La versión manual con lambda escribe explícitamente la lógica de comparación. La versión con `Comparator` usa los métodos de fábrica `Comparator.comparingInt` y `thenComparing`, que permiten encadenar criterios de forma muy legible, expresando directamente la intención: "ordenar por edad, y en caso de empate, por nombre".

```java
import java.util.*;

public class Persona {
    private final String nombre;
    private final int edad;

    public Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    public String getNombre() { return nombre; }
    public int getEdad()      { return edad; }

    @Override
    public String toString() { return nombre + "(" + edad + ")"; }
}
```

```java
List<Persona> personas = new ArrayList<>(List.of(
    new Persona("Carlos", 30),
    new Persona("Ana",    25),
    new Persona("Luis",   30),
    new Persona("Marta",  25)
));

// Versión 1: lambda con comparación manual
Collections.sort(personas, (p1, p2) -> {
    if (p1.getEdad() != p2.getEdad()) {
        return Integer.compare(p1.getEdad(), p2.getEdad());
    }
    return p1.getNombre().compareTo(p2.getNombre());
});
System.out.println(personas); // [Ana(25), Marta(25), Carlos(30), Luis(30)]

// Versión 2: usando Comparator encadenado
Collections.sort(personas,
    Comparator.comparingInt(Persona::getEdad)
              .thenComparing(Persona::getNombre)
);
System.out.println(personas); // [Ana(25), Marta(25), Carlos(30), Luis(30)]
```
