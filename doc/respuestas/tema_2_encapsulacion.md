<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

### Respuesta
-Encapsulación: busca agrupar en una sola unidad (la clase) tanto los datos (atributos) como los comportamientos (métodos) que operan sobre esos datos. Es, en esencia, "empaquetar" la realidad que queremos modelar.
-Ocultación: es el complemento necesario de la encapsulación. Su objetivo es restringir el acceso directo a los detalles internos de un objeto, exponiendo únicamente lo que es estrictamente necesario a través de una interfaz pública. En términos simples: busca que el "qué hace" un objeto sea visible, pero el "cómo lo hace" permanezca privado.
Ventajas de encapsulación:
Modularidad: Permite segmentar el sistema en piezas independientes (clases) que son más fáciles de gestionar.

Abstracción de procesos: Facilita la comprensión del sistema al agrupar comportamientos complejos bajo una identidad única.

Reutilización: Una clase bien encapsulada funciona como un componente "plug-and-play" que puede usarse en distintos proyectos.

Organización del código: Evita que las funciones y variables estén dispersas, mejorando la legibilidad para cualquier desarrollador.

Ventajas de ocultación:

Protección de la integridad: Impide que agentes externos asignen valores inválidos o ilógicos a los atributos (por ejemplo, un sueldo negativo).

Desacoplamiento (Independencia): El código que usa el objeto no depende de cómo está construido internamente, lo que facilita realizar cambios sin romper otras partes del programa.

Seguridad: Mantiene los datos sensibles y los procesos críticos fuera del alcance de manipulaciones accidentales o malintencionadas.

Mantenibilidad simplificada: Si hay un error, sabes exactamente dónde está la lógica que maneja ese dato, y puedes corregirla sin afectar a los usuarios del objeto.

Interfaz simplificada: El usuario del objeto solo ve lo que "puede hacer", eliminando el ruido visual de los detalles técnicos internos.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta
Interfaz pública: es el conjunto de métodos, propiedades y constantes que una clase expone voluntariamente al exterior. Se define mediante el modificador de acceso public.
La interfaz pública y la ocultación de información se relacionan estableciendo una frontera de seguridad entre el exterior y el interior de un objeto. Mientras que la ocultación protege la lógica compleja y los datos sensibles bajo un acceso privado, la interfaz pública actúa como el único canal autorizado y simplificado para interactuar con ellos. Esta separación permite que el funcionamiento interno pueda modificarse libremente sin afectar al resto del sistema, garantizando que los usuarios del objeto solo vean lo necesario y no puedan corromper su estado interno.



## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta
La interfaz pública es el contrato legal de tu clase; diseñarla con cuidado es vital para mantener el encapsulamiento y evitar que otros objetos dependan de detalles internos que deberían ser secretos. Si expones demasiado, creas un sistema frágil donde cualquier cambio interno afecta a todo el programa.

Cambiarla no es nada fácil, ya que cualquier modificación rompe el código de todos los usuarios que dependen de ella, obligando a realizar refactorizaciones costosas. Mientras que el interior de la clase es flexible, la interfaz pública debe ser minimalista porque, una vez publicada, es muy difícil de retirar o alterar sin causar errores en cadena.



## 4. 
Las invariantes de clase son condiciones o reglas que siempre deben cumplirse para que un objeto de una clase sea válido, antes y después de ejecutar cualquier método público. Describen el estado correcto del objeto.
Por ejemplo, si una clase representa una cuenta bancaria, una invariante podría ser que el saldo nunca sea negativo.

La ocultación de información (encapsulamiento) nos ayuda a mantener estas invariantes porque impide que el estado interno del objeto sea modificado directamente desde fuera. Al obligar a que los cambios se realicen solo a través de métodos controlados, la clase puede verificar y preservar sus invariantes en todo momento.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta
public class Punto {

    // Atributos privados → ocultación de información
    private double x;
    private double y;

    // Constructor
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Métodos públicos (interfaz pública)
    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }

    public void setX(double x) {
        this.x = x;
    }

    public void setY(double y) {
        this.y = y;
    }

    // Método pedido
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta
Clases, atributos (variables de instancia o de clase), métodos, constructores, clases internas (inner classes).


## 7. 
Sí. En Programación Orientada a Objetos (POO) normalmente existen más niveles de visibilidad además de public y private. Depende del lenguaje, pero la idea general es controlar desde dónde se puede acceder a clases, atributos o métodos.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta
Los miembros de instancia privados están ocultos para: clases(a),  no están ocultos para (b) otras instancias de la misma clase.

Es decir, todas las instancias de una clase pueden acceder a los miembros privados de otras instancias de esa misma clase, siempre que el acceso se haga desde código de la propia clase.
Código: 
public class Punto {

    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    // Método pedido
    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;   // Acceso a privado de otra instancia
        double dy = this.y - otro.y;   // Acceso a privado de otra instancia
        return Math.sqrt(dx * dx + dy * dy);
    }
}



## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta
Un getter es un método que devuelve (lee) el valor de un atributo.
Un setter es un método que modifica (escribe) el valor de un atributo.
Los métodos getter y setter son métodos usados en POO para acceder y modificar los atributos de un objeto de forma controlada, en lugar de acceder directamente a las variables.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta 
No exactamente.
Cuando en POO se dice que la ocultación de información mejora la “seguridad”, normalmente no se refiere a seguridad contra hackers, sino a seguridad del diseño del programa (evitar usos incorrectos o errores).


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta
Miembro de instancia: Es una variable o método que pertenece a cada objeto creado a partir de la clase. Cada instancia tiene su propia copia.

Miembro de clase: Es una variable o método que pertenece a la clase, compartido por todas las instancias. No depende de un objeto específico.

Sí, los miembros de clase también se pueden “ocultar” usando convención de guión bajo (_miembro) o doble guión bajo (__miembro) en Python, igual que los de instancia, pero no se crea una copia por objeto, solo se “protegió” dentro de la clase.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta
Tiene sentido hacer constructores privados cuando quieres controlar o limitar la creación de objetos.

No tiene sentido si cada objeto de la clase debe crearse libremente.

Es una técnica común en Singleton, patrones Factory y clases de utilidad.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta
public class Punto {
    // Miembros de instancia
    private int x;
    private int y;

    // Miembros de clase
    private static int maxX = Integer.MIN_VALUE;
    private static int maxY = Integer.MIN_VALUE;

    // Constructor
    public Punto(int x, int y) {
        this.x = x;
        this.y = y;

        // Actualizar los valores máximos
        if (x > maxX) {
            maxX = x;
        }
        if (y > maxY) {
            maxY = y;
        }
    }

    // Métodos de instancia
    public int getX() { return x; }
    public int getY() { return y; }

    // Métodos de clase (static)
    public static int getMaxX() { return maxX; }
    public static int getMaxY() { return maxY; }
}


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta
Sí, en este caso sí usamos static, porque un método factoría no necesita una instancia existente de Punto para crear un nuevo objeto; pertenece a la clase.
public static Punto crearRedondeado(double x, double y) {
    int xRedondeado = (int) Math.round(x);
    int yRedondeado = (int) Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}



## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta
public class Punto {
    // Array interno para almacenar las coordenadas
    private double[] coordenadas = new double[2];

    // Miembros de clase para máximos
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    // Constructor
    public Punto(double x, double y) {
        this.coordenadas[0] = x;
        this.coordenadas[1] = y;

        // Actualizar máximos
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }

    // Métodos de instancia (interfaz pública)
    public double getX() {
        return coordenadas[0];
    }

    public double getY() {
        return coordenadas[1];
    }

    // Métodos de clase (static)
    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }

    // Método factoría para crear un Punto con coordenadas redondeadas
    public static Punto crearRedondeado(double x, double y) {
        int xRedondeado = (int) Math.round(x);
        int yRedondeado = (int) Math.round(y);
        return new Punto(xRedondeado, yRedondeado);
    }
}



## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta
No, no es mejor declarar un atributo público aunque tenga getter y setter.

Convención habitual: los atributos son private y se usan getters y setters públicos.

Razón: protege las invariantes de clase, es decir, reglas que siempre deben cumplirse en el objeto.

Ejemplo: una edad no puede ser negativa, un punto no puede tener coordenadas inválidas, etc.

Si el atributo fuera público, cualquier código podría cambiarlo directamente y romper estas invariantes, perdiendo control sobre el estado del objeto.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta
Una clase es inmutable si sus objetos no pueden cambiar su estado después de ser creados.
Modificador: Es un método que modifica el estado interno del objeto. No siempre es un setter . Un setter es un tipo de método modificador que asigna directamente un valor a un atributo, pero un método modificador puede cambiar el estado de forma más compleja (ej. incrementarSaldo()).
Ventajas de que una clase sea inmutable:

Seguridad en programación concurrente (hilos).

Objetos predecibles, sin cambios inesperados.

Pueden usarse como claves en mapas o sets.

Menos errores por efectos secundarios.



## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta
No siempre conviene incluir setters porque rompen la encapsulación y permiten cambiar atributos sin control.
Solo deben usarse si es necesario y se pueden mantener las invariantes de la clase.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta
La clase String en Java es inmutable.

Al concatenar dos cadenas (s1 + s2), se crea un nuevo objeto String; las cadenas originales no cambian.

Si se van a concatenar muchas veces, conviene usar StringBuilder (mutable) para construir la cadena paso a paso de manera eficiente y evitar crear muchos objetos intermedios.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta
En POO:Por defecto, los objetos se comparan por identidad, es decir, si son exactamente el mismo objeto en memoria. Para comparar su contenido, es necesario implementar un método que lo haga explícitamente. Inicialmente, la comparación se hace por identidad. Comparar por contenido requiere un método específico que examine los atributos relevantes del objeto.
En Java: equals es un método de la clase Object que sirve para determinar si un objeto es “igual” a otro. Permite definir criterios de igualdad más allá de la identidad.or defecto, equals compara referencias, es decir, verifica si los dos objetos son el mismo en memoria, igual que el operador ==.Para cadenas, se debe usar el método equals, porque permite comparar el contenido de los objetos String, no su referencia en memoria.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta
Wrapper: Son clases que “envuelven” tipos de datos primitivos (como int, double, boolean) para que puedan comportarse como objetos. Permiten usar operaciones de objetos sobre datos que originalmente no lo son.

Ejemplo en Java: Integer envuelve a int, Double envuelve a double.

Se puede hacer de dos formas: manual (Integer x = new Integer(5);) o automático (Integer y = 5;  // autoboxing  int z = y;      // unboxing)

Ventajas de las clases wrapper:

Permiten usar primitivos como objetos (por ejemplo, en colecciones como ArrayList<Integer>).

Permiten acceder a métodos asociados al tipo (ej. Integer.parseInt("123")).

Facilitan la compatibilidad con APIs orientadas a objetos.

No  todos los lenguajes necesariamente tienen primitivos y wrappers:

Lenguajes como Java tienen primitivos y necesitan wrappers para usar objetos.

Lenguajes como Python, Ruby o JavaScript no tienen distinción estricta entre primitivos y objetos, por lo que no necesitan wrappers explícitos.


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta
Un tipo enumerado es un tipo de dato que define un conjunto limitado y fijo de valores posibles, cada uno con un nombre simbólico. Se usa para representar opciones discretas y claras, por ejemplo, días de la semana o colores.
En Java, cada enum es una clase especial, lo que significa que los valores enumerados son objetos de esa clase y pueden tener métodos, atributos y comportamientos.
Ventajas: 
Los enumerados ocultan la implementación interna y limitan los posibles valores a los definidos, protegiendo así la integridad del dato.

Se evita el uso de constantes sueltas (int o String) que podrían asignarse de forma incorrecta, aumentando la seguridad y la legibilidad del código.

Al ser objetos, pueden incluir métodos y atributos propios, manteniendo el comportamiento encapsulado dentro del enumerado.


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
public enum Mes {
    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    // Atributos privados
    private final int dias;
    private final int ordinal;

    // Constructor del enum
    private Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    // Métodos públicos
    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }

    // Métodos de estación según hemisferio
    public boolean esDePrimavera(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == MARZO || this == ABRIL || this == MAYO;
        } else {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        }
    }

    public boolean esDeVerano(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        } else {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        }
    }

    public boolean esDeOtoño(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE;
        } else {
            return this == MARZO || this == ABRIL || this == MAYO;
        }
    }

    public boolean esDeInvierno(boolean esHemisferioNorte) {
        if (esHemisferioNorte) {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO;
        } else {
            return this == JUNIO || this == JULIO || this == AGOSTO;
        }
    }
}

