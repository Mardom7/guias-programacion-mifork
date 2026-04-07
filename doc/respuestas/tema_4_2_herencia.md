<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta
En programación orientada a objetos, la herencia es un mecanismo mediante el cual una clase (subclase o clase hija) puede reutilizar y extender las propiedades y comportamientos de otra clase (superclase o clase padre). Se suele expresar con la relación “A es-un B” (por ejemplo, Artillero es-un Soldado), lo que implica que todo objeto de la subclase también puede considerarse un objeto de la superclase.

Esta relación tiene dos implicaciones principales:

1) Compatibilidad de tipos

Si decimos que Artillero es-un Soldado, entonces un objeto de tipo Artillero puede usarse donde se espera un Soldado. Esto permite, por ejemplo, almacenar distintos tipos de soldados en una misma colección (como un array de Soldado) y tratarlos de forma uniforme.

2) Herencia de estado y comportamiento

La subclase hereda los atributos (estado) y métodos (comportamiento) de la superclase. Además, puede añadir nuevos atributos o métodos propios, o incluso redefinir (sobrescribir) comportamientos heredados.

Ejemplo: 
// Superclase
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

// Subclase Artillero
class Artillero extends Soldado {
    private int numeroCohetes;

    public Artillero(String nombre, int numeroCohetes) {
        super(nombre); // Llama al constructor de Soldado
        this.numeroCohetes = numeroCohetes;
    }

    public int getNumeroCohetes() {
        return numeroCohetes;
    }

    public void dispararCohetes() {
        System.out.println(getNombre() + " dispara un cohete!");
    }
}

// Subclase Zapador
class Zapador extends Soldado {
    private int numeroMinas;

    public Zapador(String nombre, int numeroMinas) {
        super(nombre);
        this.numeroMinas = numeroMinas;
    }

    public int getNumeroMinas() {
        return numeroMinas;
    }

    public void ponerMina() {
        System.out.println(getNombre() + " coloca una mina!");
    }
}

// Clase principal para probar
public class Main {
    public static void main(String[] args) {

        // Array de Soldado (compatibilidad de tipos)
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Artillero("Carlos", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Ana", 2);

        // Todos pueden saludar (herencia de comportamiento)
        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}



## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta
Al crear un soldado concreto (por ejemplo, un Artillero o un Zapador), se ejecuta un constructor por cada clase en la jerarquía. Primero se ejecuta el constructor de la superclase (Soldado) y después el de la subclase correspondiente. Esto ocurre porque Java necesita inicializar primero la parte común heredada antes de construir la parte específica.

La palabra clave super dentro de un constructor sirve para invocar el constructor de la superclase. Debe colocarse siempre como primera instrucción y permite inicializar los atributos heredados con los valores adecuados.

Si la clase base no tiene un constructor sin parámetros accesible, entonces es obligatorio llamar a super(...) de forma explícita desde la subclase, pasando los argumentos necesarios. En caso contrario, el código no compilará, ya que Java no podrá insertar automáticamente una llamada a super().

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta
Sí, los atributos privados de la superclase forman parte de la instancia de la subclase en memoria. Es decir, cuando creas un Artillero o un Zapador, el objeto contiene tanto los atributos propios de la subclase como los heredados de Soldado (incluido nombre, aunque sea privado).

Sin embargo, esto no implica que se puedan usar directamente desde el código de la subclase. El modificador private impide el acceso directo desde cualquier otra clase, incluidas las subclases.

En memoria, el Artillero sí tiene el atributo nombre, porque forma parte de su estructura heredada. Pero como es private, no puede acceder directamente a él, y debe hacerlo a través de métodos públicos o protegidos de la superclase, como getNombre().


Ejemplo: 
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class Artillero extends Soldado {
    private int numeroCohetes;

    public Artillero(String nombre, int numeroCohetes) {
        super(nombre);
        this.numeroCohetes = numeroCohetes;
    }

    public void mostrar() {
        // System.out.println(nombre);  ERROR (no accesible)
        System.out.println(getNombre()); //  Correcto
    }
}



## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta
La compatibilidad de tipos (que un Artillero, Zapador, etc. sean “un Soldado”) implica que el código es más extensible: se pueden añadir nuevas subclases sin tener que modificar el código que ya trabaja con la superclase. Esto sigue el principio de abierto a extensión, cerrado a modificación.

Es decir, cualquier nuevo tipo de Soldado podrá usarse donde se espere un Soldado, sin cambiar el código existente.

Ejemplo: 

class Medico extends Soldado {
    private int numeroBotiquines;

    public Medico(String nombre, int numeroBotiquines) {
        super(nombre);
        this.numeroBotiquines = numeroBotiquines;
    }

    public int getNumeroBotiquines() {
        return numeroBotiquines;
    }

    public void curar() {
        System.out.println(getNombre() + " está curando a un compañero.");
    }
}

public class Main {
    public static void main(String[] args) {

        Soldado[] ejercito = new Soldado[4];

        ejercito[0] = new Artillero("Carlos", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Ana", 2);
        ejercito[3] = new Medico("Marta", 4); // nuevo tipo

        // Este código NO se modifica
        for (Soldado s : ejercito) {
            s.saludar();
        }
    }
}




## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta

Sí, en Java puedes tener una referencia del supertipo (Soldado) que apunte a objetos reales de un subtipo (Artillero, Zapador, etc.). Esto es precisamente la base del polimorfismo.

Sin embargo, con una referencia del supertipo solo puedes invocar los métodos que estén definidos en el supertipo, aunque el objeto real sea más específico. No puedes acceder directamente a métodos propios del subtipo sin hacer una conversión.

El upcasting consiste en tratar un objeto de una subclase como si fuera de la superclase. Es automático y seguro:
Soldado s = new Artillero("Carlos", 5); // upcasting implícito

El downcasting es el proceso inverso: convertir una referencia del supertipo a un subtipo. No es automático y puede fallar en tiempo de ejecución si el objeto no es realmente de ese subtipo:
Artillero a = (Artillero) s; // downcasting

El operador instanceof sirve para comprobar en tiempo de ejecución si un objeto es instancia de una clase concreta (o de sus subclases), evitando errores al hacer downcasting.

Ejemplo: 
class Main {
    public static void main(String[] args) {

        Soldado[] ejercito = new Soldado[3];
        ejercito[0] = new Artillero("Carlos", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Ana", 2);

        for (Soldado s : ejercito) {
            s.saludar();

            // Comprobamos el tipo real del objeto
            if (s instanceof Artillero) {
                Artillero a = (Artillero) s; // downcasting
                System.out.println("Cohetes: " + a.getNumeroCohetes());
            }
        }
    }
}

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta
El acceso protegido (protected) significa que un atributo o método es accesible:

Desde la propia clase
Desde subclases (aunque estén en otros paquetes)
Y también desde clases del mismo paquete

Se utiliza cuando queremos ocultar parcialmente la información, pero permitir que las subclases puedan usarla directamente.

En Java se implementa usando la palabra clave protected.

class Soldado {
    protected String nombre; // ahora es protegido

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Zapador extends Soldado {
    private int numeroMinas;

    public Zapador(String nombre, int numeroMinas) {
        super(nombre);
        this.numeroMinas = numeroMinas;
    }

    public void ponerMina() {
        // Acceso directo porque 'nombre' es protected
        System.out.println(nombre + " coloca una mina.");
    }
}

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
En los lenguajes orientados a objetos, no todos tienen una clase base común para todos los objetos, aunque muchos sí la incluyen como parte de su diseño.

En algunos lenguajes, todos los objetos heredan de una clase raíz que proporciona métodos básicos comunes. En otros, especialmente en lenguajes más flexibles o de bajo nivel, esto no ocurre y los tipos pueden no compartir una jerarquía única.

En el caso de Java, sí existe una clase base para todos los objetos: la clase java.lang.Object. Todas las clases en Java heredan directa o indirectamente de ella.

Esto implica que:

Cualquier objeto en Java puede tratarse como un Object.
Se heredan métodos básicos como toString(), equals(), hashCode(), entre otros.
Se permite una mayor generalización y polimorfismo, ya que cualquier objeto puede referenciarse como Object.

En resumen, aunque no es obligatorio en todos los lenguajes orientados a objetos, en Java sí existe una clase base universal (Object) de la que derivan todas las clases.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta
La herencia múltiple es la capacidad de una clase de heredar de más de una clase base al mismo tiempo, es decir, poder tener varios “padres” que aporten atributos y comportamientos.

En algunos lenguajes esto permite combinar funcionalidades de varias clases en una sola, pero también puede generar problemas como el problema del diamante (conflictos al heredar métodos o atributos con el mismo nombre).

En Java, no existe herencia múltiple de clases. Es decir, una clase solo puede extender de una única clase padre.



## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta
En Java, las excepciones son objetos y por tanto pueden definirse clases propias para modelar errores específicos. A continuación se muestra una excepción personalizada no controlada (hereda de RuntimeException) llamada UsuarioNoEncontradoException, que incluye un objeto Usuario y permite indicar la causa mediante sobrecarga de constructores.

class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class UsuarioNoEncontradoException extends RuntimeException {

    private Usuario usuario;

    // Constructor básico
    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    // Constructor con causa (sobrecargado)
    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta
No se recomienda usar herencia solo para reutilizar código porque la herencia impone una relación “es-un” entre clases, lo que puede generar un acoplamiento fuerte y una jerarquía rígida y poco flexible.

Cuando usas herencia, la subclase queda ligada a la implementación de la superclase. Esto significa que:

Cambios en la clase base pueden afectar a todas las subclases.
Se heredan también detalles que quizás no deberían formar parte del diseño.
Se pierde flexibilidad para cambiar el comportamiento en tiempo de ejecución.

Además, la herencia puede romper principios de diseño como el principio de sustitución de Liskov, si no se modela correctamente la relación “es-un”.

Por eso, en muchos casos se recomienda preferir la composición (“tiene-un”), que consiste en construir objetos a partir de otros objetos, en lugar de heredar de ellos.

La composición:

Reduce el acoplamiento.
Es más flexible.
Permite cambiar comportamientos fácilmente en tiempo de ejecución.
Evita jerarquías complejas e innecesarias.

En resumen, no se debe usar herencia solo para reutilizar código porque puede llevar a diseños rígidos, difíciles de mantener y poco escalables, mientras que la composición ofrece una solución más flexible y desacoplada.


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta
Se prefiere composición frente a herencia porque:

La herencia crea una relación fuerte (“es-un”), generando alto acoplamiento y menor flexibilidad.
La composición usa una relación (“tiene-un”), lo que permite cambiar comportamientos fácilmente y reutilizar código de forma más flexible.
La composición facilita mantenimiento, extensión y reutilización, evitando jerarquías rígidas y problemas de diseño.

En resumen: la composición es más flexible y desacoplada que la herencia.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta
Se dice que la herencia rompe la encapsulación porque una subclase puede acceder y depender de detalles internos de la clase base (como atributos protected o el comportamiento de sus métodos), lo que expone partes de la implementación que deberían estar ocultas.

Esto hace que:

La subclase quede dependiente de cómo está implementada la superclase, no solo de su interfaz pública.
Cambios internos en la clase base puedan afectar directamente a las subclases.
Se reduzca el aislamiento y la protección de los datos.

En cambio, con una buena encapsulación, solo se debería depender de la interfaz pública. La herencia, al permitir reutilizar e incluso modificar comportamiento interno, puede romper esa idea de ocultación de información.


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
Solución con herencia: 
class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}

Solución con composición: 
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatos() {
        return datos;
    }
}

class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }

    public DatosPersonales getDatos() {
        return datos;
    }
}

Diferencia clave
Herencia: Estudiante y Trabajador son una Persona.
Composición: Estudiante y Trabajador tienen unos DatosPersonales.

La composición permite reutilizar los datos sin acoplar las clases a una jerarquía fija.