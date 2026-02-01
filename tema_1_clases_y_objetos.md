<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta:
Las características son: 
Abstracción: Consiste en identificar las características y comportamientos esenciales de un objeto del mundo real, ignorando los detalles irrelevantes, para crear un modelo (clase) en el programa.
Encapsulamiento: Es el principio de ocultar los detalles internos de un objeto y restringir el acceso directo a sus datos. Los atributos suelen ser privados y se accede a ellos mediante métodos públicos (getters/setters).
Herencia: Permite crear nuevas clases (subclases) a partir de clases existentes (superclases), reutilizando y extendiendo su funcionalidad.
Polimorfismo: Significa "muchas formas" y permite que objetos de diferentes clases respondan de manera específica a un mismo mensaje o método.




## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta: 
Java, Python,C++ y TypeScript.



## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta 
Programación estructurada: Es un paradigma que organiza el código usando tres estructuras de control fundamentales para eliminar el "código espagueti" y hacer el flujo del programa más predecible y legible.
Programación modular: Es un paradigma que descompone un programa en módulos o componentes independientes (como funciones, bibliotecas o archivos) que realizan tareas específicas y se comunican entre sí mediante interfaces bien definidas. Su objetivo principal es la separación de responsabilidades y la reutilización de código.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta:
Se definen por lo siguiente:
Identidad: Es lo que hace único a un objeto, diferenciándolo de cualquier otro, incluso si sus atributos son idénticos.
Estado: Conjunto de valores o características que describen la situación actual del objeto en un momento dado.
Comportamiento: Conjunto de operaciones que el objeto puede realizar, definiendo cómo interactúa y cómo puede modificar su estado.


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta
Clase: Una clase es una plantilla, molde o plano que define la estructura (atributos) y el comportamiento (métodos) que tendrán los objetos creados a partir de ella. Es una definición abstracta.
Una clase no es lo mismo que un objeto , ya que este último es una entidad concreta ejecutable, es decir, una materialización en memoria con estado actual y capacidad operativa real, que existe durante el tiempo de ejecución del programa.
Instancia: Una instancia es un objeto concreto creado a partir de una clase. El proceso de crear una instancia se llama instanciación.
No todos los lenjuajes manejan el concepto de clase, hay dos enfoques principales:

POO basada en clases (la mayoría):

Java, C++, Python, C#

Usan clases como plantillas explícitas

POO basada en prototipos:

JavaScript (aunque tiene azúcar sintáctico con class)

Lua

Los objetos se crean clonando otros objetos existentes (prototipos), no instanciando clases


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta
Los objetos se almacenan en el heap (montón), que es una región de memoria dinámica donde se reserva espacio en tiempo de ejecución según se necesitan los objetos.

No, no es igual en todos los lenguajes. La gestión de memoria de objetos varía significativamente:

Lenguajes con recolección automática (Java, C#, Python, JS): Los objetos van casi siempre al heap y se gestionan automáticamente.

Lenguajes con gestión manual (C++, Rust): El programador decide si el objeto va al stack (creación automática) o al heap (con new/malloc), y debe liberar la memoria explícitamente.

Sistemas híbridos (Swift, D): Combina recolección automática con control manual o conteo de referencias.

Recolección de basura: Es un mecanismo automático de gestión de memoria que identifica y libera objetos que ya no son accesibles por el programa.



## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta
Método:Un método es una función o procedimiento definido dentro de una clase que describe un comportamiento que los objetos de esa clase pueden realizar. Es la implementación concreta de una operación que puede modificar el estado del objeto o interactuar con él.


Sobrecarga de métodos: es la capacidad de definir múltiples métodos con el mismo nombre dentro de una clase, pero con diferentes parámetros (tipo, número o orden). Su propósito es ofrecer distintas formas de realizar una operación similar, adaptándose a los datos proporcionados.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta
// Clase Punto
class Punto {
    // Atributos con visibilidad por defecto (package-private)
    double x;
    double y;
    
    // Constructor
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    // Método que calcula la distancia al origen (0,0)
    public double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

// Ejemplo de uso
public class Main {
    public static void main(String[] args) {
        // Crear una instancia de Punto
        Punto miPunto = new Punto(3.0, 4.0);
        
        // Usar el método para calcular la distancia al origen
        double distancia = miPunto.calculaDistanciaAOrigen();
        
        // Mostrar resultados
        System.out.println("Punto en (" + miPunto.x + ", " + miPunto.y + ")");
        System.out.println("Distancia al origen: " + distancia);
        
        // Verificación matemática: √(3² + 4²) = √(9 + 16) = √25 = 5.0
        System.out.println("(Comprobación: 3² + 4² = 25, √25 = 5.0)");
    }
}


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta
Punto de entrada de Java: Es el primer código que ejecuta la JVM al iniciar el programa.
Static: Modificador que indica que un miembro (método o variable) pertenece a la clase, no a instancias individuales.
El 'main' no solo se emplea para este método, sino que tambien: 

Métodos de utilidad (ej: Math.sqrt())

Variables compartidas entre instancias

Bloques de inicialización

Constantes (static final)

Patrones como Singleton

Se combina con 'final' para crear constantes de clase, es decir, valores inmutables compartidos por todos los objetos.




## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta
Para compilarlo, se aplica en siguiente comando: javac HolaMundo.java y para ejecutarlo : java HolaMundo
Java es compildo, pero de forma híbrida: Se compila a bytecode (.class) y luego se interpreta/compila por la JVM. Es compilado e interpretado
Máquina virtual: es el motor que se encarga de ejecutar bytecode Java, proporcionar independencia de plataforma, gestionar memoria automáticamente y optimizar código en timepo de ejecución.
Byte code: Código intermedio entre Java y máquina nativa. Es independiente de plataforma y lo ejecuta la JVM.
Class: Archivos que contienen bytecode compilado. Un .class por cada clase Java, con formato estructurado para la JVM.




## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta
New: es el operador de instanciación que se encagar de reservar memoria , llamar al constructor de la clase para inicializarlo y devolver una referencia a la memoria asignada.
Constructor: Método especial con el mismo nombre que la clase que se ejecuta al crear un objeto para inicializar sus atributos.
Ejemplo:
public class Empleado {
    private String dni;
    private String nombre;
    private String apellidos;
    
    // Constructor
    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta
This:es una referencia automática al objeto actual dentro del cual se está ejecutando el código. Es una palabra clave que apunta a la instancia misma. No se llama igual en todos los leguajes.

Ejemplo:
public class Punto {
    private double x;
    private double y;
    
    // Constructor usando 'this' para diferenciar atributos de parámetros
    public Punto(double x, double y) {
        this.x = x;  // this.x = atributo, x = parámetro
        this.y = y;  // this.y = atributo, y = parámetro
    }
    
    // Método que usa 'this' para llamar a otro constructor (sobrecarga)
    public Punto(double valor) {
        this(valor, valor);  // Llama al constructor principal
        // Equivale a: new Punto(valor, valor)
    }
    
    // Método que devuelve 'this' para permitir encadenamiento (method chaining)
    public Punto mover(double dx, double dy) {
        this.x += dx;
        this.y += dy;
        return this;  // Devuelve la instancia actual
    }
    
    // Método que compara con otro punto usando 'this'
    public boolean esIgual(Punto otro) {
        return this.x == otro.x && this.y == otro.y;
    }
    
    // Método estático que crea un punto desde 'this' (NO se puede)
    // public static void algo() {
    //     this.x = 5;  // ERROR: 'this' no existe en métodos estáticos
    // }
    
    // Getter usando 'this' explícitamente (aunque no es necesario)
    public double getX() {
        return this.x;  // Podría ser solo 'return x;'
    }
    
    // Setter típico con 'this'
    public void setX(double x) {
        this.x = x;  // Obligatorio para diferenciar
    }
    
    // Método que muestra las coordenadas usando 'this' implícitamente
    public void mostrar() {
        // Aquí 'x' y 'y' se refieren implícitamente a 'this.x' y 'this.y'
        System.out.println("Punto(" + x + ", " + y + ")");
    }
    
    // Ejemplo de uso encadenado
    public static void main(String[] args) {
        Punto p = new Punto(0, 0);
        
        // Encadenamiento gracias a que 'mover' devuelve 'this'
        p.mover(5, 3).mover(2, 1).mostrar();
        // Resultado: Punto(7.0, 4.0)
        
        // Uso del constructor con un solo parámetro
        Punto p2 = new Punto(10);  // x=10, y=10
        p2.mostrar();  // Punto(10.0, 10.0)
        
        // Comparación usando 'this' internamente
        Punto p3 = new Punto(7, 4);
        System.out.println("¿p igual a p3? " + p.esIgual(p3));  // true
    }
}



## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta
public class Punto {
    private double x;
    private double y;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    // Método existente: distancia al origen (0,0)
    public double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
    
    // NUEVO MÉTODO: distancia a otro punto
    public double distanciaA(Punto otro) {
        // Fórmula: √[(x2 - x1)² + (y2 - y1)²]
        double diferenciaX = this.x - otro.x;
        double diferenciaY = this.y - otro.y;
        return Math.sqrt(diferenciaX * diferenciaX + diferenciaY * diferenciaY);
    }
    
    // Getter para x (útil para pruebas)
    public double getX() {
        return x;
    }
    
    // Getter para y (útil para pruebas)
    public double getY() {
        return y;
    }
    
    // Método toString para representación
    @Override
    public String toString() {
        return "(" + x + ", " + y + ")";
    }
    
    // Ejemplo de uso
    public static void main(String[] args) {
        // Crear dos puntos
        Punto p1 = new Punto(3, 4);
        Punto p2 = new Punto(6, 8);
        Punto origen = new Punto(0, 0);
        
        // Calcular distancias
        System.out.println("Punto p1: " + p1);
        System.out.println("Punto p2: " + p2);
        System.out.println();
        
        System.out.println("Distancia de p1 al origen: " + p1.calculaDistanciaAOrigen());
        System.out.println("Distancia de p2 al origen: " + p2.calculaDistanciaAOrigen());
        System.out.println();
        
        System.out.println("Distancia de p1 a p2: " + p1.distanciaA(p2));
        System.out.println("Distancia de p2 a p1: " + p2.distanciaA(p1) + " (debe ser igual)");
        
        // Caso especial: distancia de un punto a sí mismo
        System.out.println("Distancia de p1 a sí mismo: " + p1.distanciaA(p1) + " (debe ser 0)");
        
        // Verificación matemática: √[(6-3)² + (8-4)²] = √[3² + 4²] = √[9+16] = √25 = 5
        System.out.println("\nVerificación: √[(6-3)² + (8-4)²] = √[3² + 4²] = √25 = 5.0");
    }
}


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta
Cuando se pasa un objeto (como una instancia de Punto), se pasa por valor de referencia. Esto significa que dentro del método se recibe una copia de la referencia al objeto, no una copia del objeto completo. Por lo tanto, si se modifican los atributos del objeto dentro del método (por ejemplo, cambiando sus coordenadas x o y), esos cambios sí se reflejarán en el objeto original fuera del método. Sin embargo, si se intenta reasignar la referencia a un nuevo objeto dentro del método, esa reasignación no afecta a la referencia original en el código llamante.

En cambio, cuando se pasa un tipo primitivo (como un int), se pasa por valor. Esto significa que se crea una copia completa del valor numérico. Cualquier modificación que se haga a ese parámetro dentro del método solo afecta a la copia local y no tiene ningún efecto sobre la variable original que se usó como argumento.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta
toString: es un método definido en la clase Object (raíz de todas las clases en Java) que devuelve una representación en String del objeto. Su propósito es proporcionar una descripción legible y útil del estado del objeto. Si que existe en otros lenguajes pero con diferente nombre.
 Ejemplo:
 public class Punto {
    private double x;
    private double y;
    
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }
    
    // SOBRESCRITURA de toString()
    @Override
    public String toString() {
        // Formato: "Punto(x, y)"
        return String.format("Punto(%.2f, %.2f)", x, y);
    }
    
    // Versión alternativa más simple:
    // @Override
    // public String toString() {
    //     return "Punto[" + x + ", " + y + "]";
    // }
    
    // Versión con JSON-style:
    // @Override
    // public String toString() {
    //     return "{\"x\": " + x + ", \"y\": " + y + "}";
    // }
    
    // Ejemplo de uso
    public static void main(String[] args) {
        Punto p1 = new Punto(3.14159, 2.71828);
        Punto p2 = new Punto(5, -3.5);
        
        // Llamada explícita
        System.out.println(p1.toString());  // Punto(3.14, 2.72)
        
        // Llamada implícita (automática)
        System.out.println("Primer punto: " + p1);  // Concatenación automática
        System.out.println(p2);  // println llama automáticamente a toString()
        
        // En colecciones
        java.util.ArrayList<Punto> puntos = new java.util.ArrayList<>();
        puntos.add(p1);
        puntos.add(p2);
        System.out.println("Lista: " + puntos);
        // Muestra: Lista: [Punto(3.14, 2.72), Punto(5.00, -3.50)]
        
        // Comparación con el toString() por defecto (sin sobrescribir)
        System.out.println("\nSin toString() personalizado:");
        System.out.println("Por defecto sería: " + p1.getClass().getName() + "@" + Integer.toHexString(p1.hashCode()));
        // Ejemplo: Punto@15db9742 (poco informativo)
    }
}


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta
Un struct en C y una clase en programación orientada a objetos comparten la característica de agrupar datos bajo un mismo tipo, pero difieren fundamentalmente en su propósito y capacidades.

Mientras que un struct es esencialmente una estructura de datos pasiva que define únicamente cómo se organiza la información en memoria, una clase es una unidad activa que encapsula tanto datos como el comportamiento que opera sobre ellos.

Para que un struct se asemejara a una clase y sus variables fueran verdaderas instancias, necesitaría incorporar: encapsulamiento (control de acceso a los datos), métodos (comportamiento asociado), constructores y destructores (gestión del ciclo de vida), polimorfismo (capacidad de responder de diferentes formas a un mismo mensaje), y una referencia implícita al objeto actual (como this). Además, requeriría un sistema de gestión de memoria automático que liberara al programador de la administración manual de recursos.

En esencia, el struct representa un contenedor de datos, mientras que la clase representa una entidad con identidad, estado y comportamiento propios.



## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?
### Respuesta:
En C, podemos emular una clase Punto utilizando un struct para agrupar los datos (coordenadas x e y) y definiendo funciones separadas que actúen como métodos. La clave es que cada función que opera sobre un punto debe recibir explícitamente un puntero a ese struct como primer parámetro, que hace las veces del this implícito de los lenguajes orientados a objetos.

En este enfoque procedural, this deja de ser una referencia implícita y automática dentro de los métodos para convertirse en un parámetro explícito que el programador debe pasar manualmente a cada función. Donde en Java escribiríamos p1.distanciaA(p2) y el compilador automáticamente pasa p1 como this, en C debemos escribir distanciaA(&p1, &p2), pasando explícitamente la dirección de p1 como el "objeto actual".

Esta emulación logra agrupar datos y operaciones relacionadas bajo un mismo concepto, pero carece de las características esenciales de la POO: el encapsulamiento (todos los campos del struct son públicos), la herencia, el polimorfismo y la gestión automática del ciclo de vida. Es, en esencia, una forma organizada de programación procedural que imita superficialmente la estructura de la orientación a objetos.

