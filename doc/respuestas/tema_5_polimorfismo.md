<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

### Respuesta
Polimorfismo: Es la capacidad de que un mismo método o función se comporte de distintas maneras según el objeto que lo utilice. Sirve para escribir código más flexible y reutilizable, ya que permite tratar diferentes objetos de forma uniforme.

Sobreescritura de métodos (override): Es cuando una clase hija redefine un método que ya existe en su clase padre, cambiando su comportamiento sin modificar la estructura general.


## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta
Ligadura dinámica (enlace tardío): Consiste en que la decisión de qué método ejecutar se toma en tiempo de ejecución, no en compilación. Es decir, el programa decide qué implementación concreta usar según el tipo real del objeto en ese momento.

Relación con el polimorfismo: Está directamente relacionada: la ligadura dinámica es lo que permite el polimorfismo en tiempo de ejecución. Gracias a ella, una misma llamada a un método puede ejecutar diferentes versiones según el objeto.

C++:
Por defecto, usa ligadura estática (en tiempo de compilación).
Para tener ligadura dinámica hay que indicarlo con la palabra clave virtual.
Ejemplo: métodos virtuales → permiten polimorfismo en tiempo de ejecución.
Java: 
Usa ligadura dinámica por defecto para métodos (excepto static, final y private).
No hace falta indicarlo explícitamente.
El polimorfismo dinámico es el comportamiento habitual.
Python:
También usa ligadura dinámica por defecto.
No es necesario declarar nada especial.
Todo es dinámico: el tipo real del objeto se evalúa en tiempo de ejecución.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta
// Clase base
class Soldado {
    public void saludar() {
        System.out.println("Soy un soldado.");
    }
}

// Subclase Zapador (sobrescribe completamente el método)
class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Soy un zapador, experto en explosivos.");
    }
}

// Subclase Artillero (también sobrescribe)
class Artillero extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Soy un artillero, manejo cañones.");
    }
}

// Clase principal para probar el polimorfismo
public class Main {
    public static void main(String[] args) {
        // Array de referencias de tipo Soldado
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Soldado();
        ejercito[1] = new Zapador();
        ejercito[2] = new Artillero();

        // Recorrido polimórfico
        for (Soldado s : ejercito) {
            s.saludar(); // Ligadura dinámica: se decide en tiempo de ejecución
        }
    }
}


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta
Sí, puedes invocar el método de la clase base desde la subclase y añadir comportamiento adicional.

En Java se hace usando la palabra clave super:

class Soldado {
    public void saludar() {
        System.out.println("Soy un soldado.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        // Llamada al método de la clase base
        super.saludar();
        // Comportamiento adicional
        System.out.println("¡ZAPADOR A SUS ORDENES!");
    }
}

class Artillero extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Soy un artillero, manejo cañones.");
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = {
            new Soldado(),
            new Zapador(),
            new Artillero()
        };

        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta
Al sobreescribir un método en Java, los parámetros deben ser exactamente los mismos que en la clase base. El tipo de retorno puede ser igual o un subtipo compatible, y no se puede reducir la visibilidad del método ni ampliar las excepciones que lanza.

La sobreescritura (overriding) ocurre entre una clase padre y una hija, mantiene la misma firma y se resuelve en tiempo de ejecución (polimorfismo). La sobrecarga (overloading) ocurre en la misma clase, cambia los parámetros y se resuelve en tiempo de compilación.

La anotación @Override sirve para indicar que un método está siendo sobrescrito. Es recomendable porque ayuda al compilador a detectar errores si la sobreescritura no es correcta (por ejemplo, errores de nombre o firma).



## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?
### Respuesta
Sí, en Java el polimorfismo se usa desde muy temprano, aunque al principio no siempre se explique con ese nombre.

Cuando sobrescribes métodos como toString() o equals(), sí estás utilizando polimorfismo, concretamente polimorfismo por sobreescritura (dinámico).

Ejemplo: 
Object obj = "Hola";
System.out.println(obj.toString());




## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta
Una clase abstracta es una clase que no se puede instanciar directamente y que se usa como base para otras clases. Puede contener métodos normales y métodos abstractos.
-Un método abstracto es un método que:
Se declara sin cuerpo (sin implementación)
Obliga a las subclases a implementarlo

No se pueden crear objetos de una clase abstracta directamente.

Ejemplo: 
abstract class Soldado {

    public void saludar() {
        System.out.println("Soy un soldado.");
    }

    public abstract void atacar();
}

class Zapador extends Soldado {

    @Override
    public void atacar() {
        System.out.println("El zapador coloca explosivos.");
    }

    @Override
    public void saludar() {
        System.out.println("Soy un zapador.");
    }
}

class Artillero extends Soldado {

    @Override
    public void atacar() {
        System.out.println("El artillero dispara el cañón.");
    }

    @Override
    public void saludar() {
        System.out.println("Soy un artillero.");
    }
}

public class Main {
    public static void main(String[] args) {

        Soldado[] ejercito = {
            new Zapador(),
            new Artillero()
        };

        for (Soldado s : ejercito) {
            s.saludar();
            s.atacar();
        }
    }
}

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta
La palabra clave final en Java impide que un método pueda ser sobrescrito o que una clase pueda ser heredada. Si se aplica a un método, este no puede ser redefinido en una subclase; si se aplica a una clase, no se pueden crear subclases a partir de ella.

Esto se relaciona directamente con el polimorfismo, ya que limita el polimorfismo por herencia: si un método es final, no puede haber sobreescritura y, por tanto, no hay polimorfismo dinámico en ese caso. Si una clase es final, tampoco puede haber polimorfismo mediante subtipos.

Un ejemplo de clase final en la API de Java es String, que no puede ser extendida, lo que garantiza su comportamiento inmutable y seguro.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta
En Java, una interfaz es un tipo especial que define un conjunto de métodos que una clase debe implementar, pero sin proporcionar su implementación (o solo de forma muy limitada en versiones modernas). Sirve para definir un “contrato” de comportamiento.

Se parecen a las clases abstractas porque ambas pueden definir métodos sin implementar, pero son diferentes: una clase abstracta puede tener atributos, constructores y métodos con código, mientras que una interfaz se centra en definir qué debe hacerse, no cómo. Además, una clase solo puede heredar de una clase abstracta, pero puede implementar varias interfaces.

Sí, una clase en Java puede implementar varias interfaces, lo que permite simular herencia múltiple de comportamiento, algo que no se permite con clases.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}

// Punto 2D
class Punto2D extends Punto {
    double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p = (Punto2D) otro; // downcasting
            return Math.sqrt(Math.pow(p.x - this.x, 2) + Math.pow(p.y - this.y, 2));
        }
        throw new IllegalArgumentException("Punto incompatible: no es 2D");
    }
}

// Punto 3D
class Punto3D extends Punto {
    double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p = (Punto3D) otro; // downcasting
            return Math.sqrt(
                Math.pow(p.x - this.x, 2) +
                Math.pow(p.y - this.y, 2) +
                Math.pow(p.z - this.z, 2)
            );
        }
        throw new IllegalArgumentException("Punto incompatible: no es 3D");
    }
}

// Clase Linea que trabaja con cualquier tipo de Punto
class Linea {
    private Punto a;
    private Punto b;

    public Linea(Punto a, Punto b) {
        this.a = a;
        this.b = b;
    }

    public double longitud() {
        return a.calcularDistanciaA(b);
    }
}

// Clase principal
public class Main {
    public static void main(String[] args) {

        Punto2D p1 = new Punto2D(0, 0);
        Punto2D p2 = new Punto2D(3, 4);

        Linea l1 = new Linea(p1, p2);
        System.out.println("Longitud 2D: " + l1.longitud());

        Punto3D p3 = new Punto3D(0, 0, 0);
        Punto3D p4 = new Punto3D(1, 2, 2);

        Linea l2 = new Linea(p3, p4);
        System.out.println("Longitud 3D: " + l2.longitud());
    }
}


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
La herencia de interfaces en Java consiste en que una interfaz puede extender otra interfaz, heredando sus métodos y pudiendo añadir nuevos.

Sí, en Java existe la herencia múltiple de interfaces, es decir, una interfaz puede extender varias interfaces a la vez, algo que no se permite con clases.

Ejemplo: 

interface Fichero {
    String leer();
}

interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}
