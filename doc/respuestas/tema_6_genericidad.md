<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

### Respuesta
Una estructura de datos que permite almacenar cualquier tipo de dato puede construirse usando un array genérico. En C, se utiliza un array de void*, donde cada posición guarda la dirección de memoria de distintos tipos de datos, como enteros o cadenas. En Java, se emplea un array de Object, por ejemplo Object[] datos = new Object[5];, capaz de almacenar Integer, String, Double u otros objetos. Así, un mismo array puede contener elementos de diferentes tipos.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta
La programación genérica consiste en crear clases, métodos o estructuras de datos reutilizables que puedan trabajar con distintos tipos de datos sin depender de uno concreto. Su objetivo es escribir código más flexible y reutilizable.

Sí, el ejemplo anterior es un ejemplo básico de programación genérica, ya que el array puede almacenar diferentes tipos de datos mediante void* en C o Object en Java.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta
El principal problema de usar void* en C o Object en Java es que se pierde el chequeo de tipos en tiempo de compilación. La estructura puede almacenar cualquier dato, pero el compilador no puede verificar si el tipo usado al recuperar el elemento es correcto. Esto obliga a realizar conversiones de tipo (casting), lo que puede producir errores en ejecución si se interpreta un dato como otro tipo distinto. Además, el código resulta menos seguro y más difícil de mantener.


## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta
Los parámetros de tipo son símbolos que se usan en clases, métodos o estructuras de datos para representar tipos de datos de forma genérica. Permiten definir código reutilizable indicando el tipo concreto al utilizarlo. Por ejemplo, en Java class Caja<T> usa T como parámetro de tipo, de modo que después puede crearse Caja<Integer> o Caja<String>. Así se mantiene la flexibilidad y además el chequeo de tipos en compilación.


## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta
En Java y C++ la programación genérica permite definir estructuras de datos indicando el tipo concreto que almacenarán. Si se instancia una lista o vector dinámico de String, solo se podrán insertar cadenas de texto, manteniendo seguridad de tipos.

En Java, con generics, se puede usar una lista de tipo String:

ArrayList<String> lista = new ArrayList<>();

lista.add("Hola");
lista.add("Mundo");

for (String elemento : lista) {
    System.out.println(elemento);
}


En este caso, cada elemento recuperado es directamente de tipo String, sin necesidad de conversiones.

En C++, con templates, se puede usar un vector de string:

#include <vector>
#include <string>
#include <iostream>
using namespace std;

vector<string> lista;

lista.push_back("Hola");
lista.push_back("Mundo");

for (string elemento : lista) {
    cout << elemento << endl;
}


## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta
Cuando se instancia una clase con parámetros de tipo, el compilador adapta el código genérico al tipo concreto indicado. Sin embargo, Java y C++ no lo hacen igual.

En Java, los generics usan type erasure (borrado de tipos). Esto significa que, al compilar, la información del tipo genérico (String, Integer, etc.) se elimina y se sustituye normalmente por Object (o por su límite superior). Así, en tiempo de ejecución no existen tipos genéricos reales, solo una única versión de la clase.

En C++, las templates funcionan mediante instanciación de plantillas. El compilador genera una versión real e independiente de la clase o función para cada tipo usado. Por ejemplo, vector<string> y vector<int> producen implementaciones distintas.

Por tanto, no hacen lo mismo: Java reutiliza una sola versión del código borrando tipos, mientras que C++ crea versiones específicas para cada tipo en compilación.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta
En Java se puede definir una clase genérica Par con dos parámetros de tipo distintos, para almacenar dos valores diferentes.

class Par<T, U> {
    private T primero;
    private U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
}

Un ejemplo de uso sería una función que devuelve la media y la desviación típica de un array de double:

public static Par<Double, Double> calcular(double[] datos) {
    double suma = 0;

    for (double x : datos)
        suma += x;

    double media = suma / datos.length;

    double sumaCuadrados = 0;
    for (double x : datos)
        sumaCuadrados += Math.pow(x - media, 2);

    double desviacion = Math.sqrt(sumaCuadrados / datos.length);

    return new Par<>(media, desviacion);
}

Uso:

double[] valores = {2, 4, 6, 8};

Par<Double, Double> resultado = calcular(valores);

System.out.println("Media: " + resultado.getPrimero());
System.out.println("Desviación: " + resultado.getSegundo());

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta
En Java, los parámetros de tipo también pueden declararse a nivel de método mediante métodos genéricos. Un ejemplo es el método seleccionaUno, que recibe dos objetos del mismo tipo y devuelve uno de ellos aleatoriamente.

public class EjemploGenericos {

    // Método SIN genéricos: usa Object
    public static Object seleccionaUnoObject(Object a, Object b) {
        // Devuelve uno de los dos objetos aleatoriamente
        return Math.random() < 0.5 ? a : b;
    }

    // Método CON genéricos: usa parámetro de tipo T
    public static <T> T seleccionaUno(T a, T b) {
        // El tipo T se determina en tiempo de compilación
        return Math.random() < 0.5 ? a : b;
    }

    public static void main(String[] args) {

        // ===== EJEMPLO SIN GENÉRICOS =====
        Object resultado1 = seleccionaUnoObject("hola", "adiós");

        // (i) Downcasting necesario para recuperar el tipo real
        String r1 = (String) resultado1;

        // (ii) No hay control de tipos: esto compila pero es inseguro
        Object resultado2 = seleccionaUnoObject("hola", 5);

        // ===== EJEMPLO CON GENÉRICOS =====
        String resultado3 = seleccionaUno("hola", "adiós");

        // (i) No hace falta downcasting, el tipo ya es String
        String r2 = resultado3;

        // (ii) Error de compilación si los tipos no coinciden:
        // seleccionaUno("hola", 5); //  NO compila

    }
}


## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta
Sí, en Java se pueden establecer restricciones (bounds) sobre los parámetros de tipo usando extends. Por ejemplo, se puede exigir que T sea un tipo numérico: T extends Number.

Solución 1: usando Number directamente (sin generics reales). Esta solución es errónea:

class Punto {
    private Number x;
    private Number y;

    public Punto(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() {
        return x;
    }

    public Number getY() {
        return y;
    }

    // Calcula distancia a otro punto
    public double calcularDistanciaA(Punto otro) {
        double dx = x.doubleValue() - otro.getX().doubleValue();
        double dy = y.doubleValue() - otro.getY().doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}


Solución 2: con genéricos acotados (<T extends Number>)

class Punto<T extends Number> {
    private T x;
    private T y;

    public Punto(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() {
        return x;
    }

    public T getY() {
        return y;
    }

    // Método genérico: puede aceptar otro Punto con cualquier Number
    public double calcularDistanciaA(Punto<? extends Number> otro) {
        double dx = x.doubleValue() - otro.getX().doubleValue();
        double dy = y.doubleValue() - otro.getY().doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}


IMPORTANT: El type erasure (borrado de tipos) en Java es el proceso por el cual los parámetros genéricos se eliminan en tiempo de compilación.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta
Ninguna de las dos soluciones permite que un punto tenga una coordenada de tipo entero y la otra de tipo real de forma independiente, ya que en ambos casos el tipo está unificado para x e y.

En la solución sin genéricos, ambas coordenadas se declaran como Number, por lo que pueden almacenar cualquier subtipo numérico, pero sin distinguir el tipo concreto de cada coordenada. Esto implica que no hay control fino del tipo real almacenado.

En la solución con genéricos, el parámetro de tipo <T extends Number> también se aplica a toda la clase, por lo que x e y deben ser del mismo tipo T. Es decir, no se puede mezclar Integer y Double dentro del mismo punto.

Respecto a los métodos getX:

-Sin genéricos, getX() devuelve un Number, lo que obliga a realizar conversiones si se quiere un tipo específico.
-Con genéricos, getX() devuelve directamente el tipo T definido al instanciar la clase (por ejemplo Integer o Double), evitando conversiones y mejorando la seguridad de tipos.


## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta


public interface Punto<T> {
    double distanciaA(T p);
}

public class Punto2D implements Punto<Punto2D> {
    private final double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double distanciaA(Punto2D p) {
        return Math.sqrt(
                Math.pow(x - p.x, 2) +
                Math.pow(y - p.y, 2)
        );
    }
}

public class Punto3D implements Punto<Punto3D> {
    private final double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double distanciaA(Punto3D p) {
        return Math.sqrt(
                Math.pow(x - p.x, 2) +
                Math.pow(y - p.y, 2) +
                Math.pow(z - p.z, 2)
        );
    }
}

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta
No, no es correcto.

Aunque String es subtipo de Object, en Java List<String> no es subtipo de List<Object>. Esto se debe a que los genéricos en Java son invariantes, es decir, el tipo del contenedor no hereda la relación de subtipos del tipo que contiene. Si esto se permitiera, se perdería la seguridad de tipos, ya que se podría añadir un Integer a una lista de String, provocando errores en tiempo de ejecución.

En cambio, con los arrays sí ocurre que String[] es subtipo de Object[], porque los arrays son covariantes. Sin embargo, esto puede causar problemas en tiempo de ejecución, ya que es posible asignar un elemento de un tipo incorrecto a través de una referencia más general, lo que provoca una excepción como ArrayStoreException.

A partir de estos casos, se definen tres conceptos:

-Un tipo es covariante cuando conserva la relación de subtipos (si A es subtipo de B, entonces F(A) es subtipo de F(B)), como ocurre con los arrays.
-Es contravariante cuando invierte la relación de subtipos (si A es subtipo de B, entonces F(B) es subtipo de F(A)), lo que en Java se expresa con ? super T.
-Es invariante cuando no existe relación de subtipo entre las versiones parametrizadas, como ocurre con los genéricos (List<String> no es subtipo de List<Object>).


## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta

Un wildcard (?) en Java es un comodín que se utiliza en tipos genéricos para representar un tipo desconocido y así flexibilizar el uso de colecciones sin perder seguridad de tipos.

La diferencia entre List<? extends T> y List<? super T> está en el uso que se le da a la colección.

List<? extends T> se utiliza cuando queremos leer datos de una lista. Indica que la lista contiene elementos de un tipo T o de un subtipo de T. Permite recorrer y obtener valores como T, pero no permite añadir elementos (excepto null), ya que no se conoce el tipo exacto de la lista.

Ejemplo de suma de números:

public static double suma(List<? extends Number> lista) {
    double suma = 0;
    for (Number n : lista) {
        suma += n.doubleValue();
    }
    return suma;
}


Por otro lado, List<? super T> se utiliza cuando queremos escribir datos en una lista. Indica que la lista puede contener objetos de tipo T o de cualquier supertipo de T. Permite añadir elementos del tipo T, pero al leer solo se obtiene información de tipo Object.

Ejemplo de añadir enteros:

public static void añadirEnteros(List<? super Integer> lista) {
    lista.add(1);
    lista.add(2);
    lista.add(3);
}


En resumen, ? extends T se usa para lectura (covarianza) y ? super T se usa para escritura (contravarianza), manteniendo la seguridad de tipos en ambos casos.

