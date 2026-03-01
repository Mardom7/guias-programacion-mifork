<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

### Respuesta
Existen las siguientes opciones: 
Opción 1 (Devolver un valor especial y usar errno): La función devuelve un valor indicador de error (por ejemplo -1.0) y establece errno para que el código externo pueda comprobar qué ocurrió.
#include <stdio.h>
#include <math.h>
#include <errno.h>

double raiz(double x) {
    if (x < 0) {
        errno = EDOM;   // Error de dominio
        return -1.0;    // Valor indicador de error
    }
    return sqrt(x);
}

int main() {
    double num = -4.0;
    double resultado = raiz(num);

    if (errno == EDOM) {
        printf("Error: no se puede calcular la raiz de un numero negativo.\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }

    return 0;
}
Ventaja: simple y compatible con el estilo clásico de C.
Desventaja: depende de valores especiales que podrían coincidir con resultados válidos en otros contextos.

Opción 2 (Devolver código de error y usar parámetro por referencia): La función devuelve un código de estado (0 = éxito, 1 = error) y el resultado se pasa mediante un puntero.
#include <stdio.h>
#include <math.h>

int raiz(double x, double *resultado) {
    if (x < 0) {
        return 1;  // Código de error
    }
    *resultado = sqrt(x);
    return 0;      // Éxito
}

int main() {
    double num = -4.0;
    double resultado;

    if (raiz(num, &resultado) != 0) {
        printf("Error: no se puede calcular la raiz de un numero negativo.\n");
    } else {
        printf("Resultado: %f\n", resultado);
    }

    return 0;
}
Ventaja: separa claramente el resultado del estado de error.
Desventaja: la interfaz es un poco más compleja.




## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta
Una excepción es un mecanismo de control de errores que permite señalar que ha ocurrido una situación anómala durante la ejecución de un programa, interrumpiendo el flujo normal para que dicho problema pueda ser gestionado en otro punto del código.

Para controlarla existe varios modos: 

-Al implementar funciones: para indicar que no pueden completar su tarea correctamente (por ejemplo, recibir datos inválidos, fallos de lectura, divisiones por cero, etc.) sin mezclar la lógica principal con el manejo de errores.

-Al llamar funciones: para capturar y tratar esos errores de forma controlada, evitando que el programa falle abruptamente y permitiendo mostrar mensajes adecuados, registrar el problema o tomar acciones alternativas.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta
class Calculadora {

    public static double raizCuadrada(double numero) {
        if (numero < 0) {
            throw new IllegalArgumentException("No se puede calcular la raíz de un número negativo.");
        }
        return Math.sqrt(numero);
    }
}

public class Main {
    public static void main(String[] args) {

        double valor = -9;

        try {
            double resultado = Calculadora.raizCuadrada(valor);
            System.out.println("La raíz cuadrada es: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }

        System.out.println("El programa continúa ejecutándose...");
    }
}





## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta
Excepción: es un mecanismo que permite indicar que durante la ejecución del programa ha ocurrido una situación anómala que impide continuar con normalidad.

Lanzar una excepción: significa generar esa situación de error usando throw, interrumpiendo inmediatamente el flujo normal del método. En el ejemplo de la raíz en Java:

public static double raiz(double x) {
    if (x < 0) {
        throw new IllegalArgumentException("Numero negativo no permitido");
    }
    return Math.sqrt(x);
}

Capturar o controlar una excepción: significa interceptarla con un bloque try-catch para evitar que el programa termine y poder reaccionar adecuadamente.Ejemplo: 
try {
    double r = raiz(-4);
} catch (IllegalArgumentException e) {
    System.out.println("Error: " + e.getMessage());
}
Propagarse una excepción: ocurre cuando un método lanza una excepción y no la captura; entonces pasa automáticamente al método que lo llamó, y así sucesivamente por la pila de llamadas hasta que alguien la capture o el programa termine.

Qué ocurre en la pila de llamadas: cuando se lanza la excepción, el método donde ocurre se detiene inmediatamente. Si el método que lo llamó no la captura, también finaliza de forma abrupta. Se van “desapilando” los métodos uno a uno hasta encontrar un catch adecuado.

Si las funciones que no la controlan se reanudan: no. Una vez que la excepción atraviesa un método sin ser capturada, ese método termina y no continúa su ejecución después de la llamada que produjo el error. El flujo del programa continúa únicamente desde el punto donde la excepción es capturada.



## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta
Frente a C, donde los errores se propagan manualmente mediante códigos de retorno, la propagación natural de excepciones (como en Java) tiene estas ventajas:

-Permite separar la lógica normal del manejo de errores, haciendo el código más limpio.
-Evita olvidos al comprobar errores, ya que si no se capturan, se siguen propagando.
-No obliga a cada función intermedia a reenviar el error manualmente.
-Facilita centralizar el tratamiento de errores en niveles superiores.

En conjunto, hace el programa más claro y robusto.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta
Sí. En programación orientada a objetos, como en Java, las excepciones son objetos (instancias de clases que heredan de Exception o Throwable).

Esto aporta ventajas claras de encapsulación: la excepción puede contener información interna sobre el error (mensaje, código, datos adicionales), y ofrecer métodos para consultarla. Así, el error no es solo un código numérico, sino un objeto que agrupa estado y comportamiento, manteniendo organizada la información y evitando variables globales o convenciones externas.

Sí, se pueden crear excepciones personalizadas definiendo nuevas clases que hereden de Exception (o de alguna subclase). Por ejemplo:
public class NumeroNegativoException extends Exception {
    public NumeroNegativoException(String mensaje) {
        super(mensaje);
    }
}
Esto permite adaptar el sistema de errores al dominio del problema, haciendo el código más claro, expresivo y mantenible.


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta
En C normalmente solo se dispone de un código de error. En Java, un objeto excepción incluye información mucho más útil:

-El tipo de excepción (qué clase de error es).
-Un mensaje descriptivo.
-La traza de la pila (dónde ocurrió y cómo se llegó ahí).

Esto permite diagnosticar el error con mucha más precisión desde el manejador.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta
Sí, en Java se pueden tener varios bloques catch tras un mismo try, para manejar distintos tipos de excepciones.

De todos los bloques catch, solo se ejecuta uno: el primero cuyo tipo coincida con la excepción lanzada.


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta
En Java, para garantizar que siempre se ejecute un bloque de código necesario (como cerrar ficheros o liberar recursos) se usa finally. Este bloque se ejecuta siempre, tanto si la excepción se captura con catch como si no se captura y sigue propagándose.

Ejemplo con catch:
try {
    double r = raiz(-4);  // puede lanzar excepción
    System.out.println("Resultado: " + r);
} catch (IllegalArgumentException e) {
    System.out.println("Error capturado: " + e.getMessage());
} finally {
    System.out.println("Se ejecuta siempre: liberando recursos");
}
Ejemplo sin catch (propagación):
try {
    double r = raiz(-4);  // puede lanzar excepción
    System.out.println("Resultado: " + r);
} finally {
    System.out.println("Se ejecuta siempre: cerrando fichero");
}

En ambos casos, el código dentro de finally se ejecuta antes de que el flujo continúe o la excepción siga subiendo por la pila, asegurando que los recursos se liberen correctamente.


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta
Sí. En Java un bloque finally puede ir sin catch.

Siempre se ejecuta, tanto si ocurre una excepción como si no ocurre, garantizando la liberación de recursos.

Si hay un return dentro del try, el bloque finally se ejecuta antes de que se complete el retorno, incluso si la función va a salir en ese momento. Por ejemplo:
public static int ejemplo() {
    try {
        return 1;  // intención de retornar
    } finally {
        System.out.println("Finalmente se ejecuta antes del retorno");
    }
}
Aquí, aunque return 1 está en el try, el mensaje del finally se imprime antes de que el valor se devuelva.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta
En Java:

Excepciones controladas (checked): son aquellas que el compilador obliga a manejar con try-catch o declarar con throws. Se usan para errores que pueden ocurrir de forma previsible y recuperable.

Excepciones no controladas (unchecked): heredan de RuntimeException. El compilador no obliga a capturarlas; suelen representar errores de programación que no se espera manejar en tiempo normal, como violaciones de contrato. RuntimeException es la clase base de todas las excepciones no controladas.

Ejemplos: 
Excepción controlada personalizada:
public class NumeroNegativoException extends Exception {
    public NumeroNegativoException(String msg) { super(msg); }
}
Excepción no controlada personalizada:
public class DivisionPorCeroException extends RuntimeException {
    public DivisionPorCeroException(String msg) { super(msg); }
}


Situaciones donde se prefiere excepción no controlada

-División entre cero (ArithmeticException).

-Acceso a índice fuera de rango (IndexOutOfBoundsException).

-Referencia nula (NullPointerException).

-Violación de contrato interno, como pasar parámetros inválidos en métodos (IllegalArgumentException).



## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta
En Java, throws se usa en la declaración de un método para indicar que ese método puede generar una o varias excepciones controladas y que no las va a capturar dentro.

Es una alternativa a try-catch porque traslada la responsabilidad de manejar la excepción al llamador del método, en lugar de gestionarla inmediatamente.

Ejemplo:
public double raiz(double x) throws NumeroNegativoException {
    if (x < 0) {
        throw new NumeroNegativoException("Número negativo no permitido");
    }
    return Math.sqrt(x);
}

Aquí, cualquier código que llame a raiz debe decidir si:

-Captura la excepción con try-catch, o

-Propaga a su vez la excepción usando throws.


Esto permite mantener limpio el método y centralizar la gestión de errores en un nivel superior si se desea.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

public class EjemploFichero {

    // El método declara que puede lanzar FileNotFoundException
    public static void abrirFichero(String nombreFichero) throws FileNotFoundException {
        Scanner sc = null;
        try {
            sc = new Scanner(new File(nombreFichero));
            System.out.println("Fichero abierto correctamente");
            // Aquí se leería el fichero...
        } finally {
            if (sc != null) {
                sc.close();
                System.out.println("Scanner cerrado en finally");
            }
        }
    }

    public static void main(String[] args) {
        try {
            abrirFichero("datos.txt");
        } catch (FileNotFoundException e) {
            System.out.println("Error: el fichero no existe");
        }
    }
}


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta
Sí, en Java.

No es obligatorio. A diferencia de las excepciones controladas (checked), como IOException, las excepciones no controladas no activan la verificación del compilador. El código compilará perfectamente aunque el llamador ignore el throws.

Si no es obligatorio ni para el que escribe el método ni para el que lo usa, ¿para qué sirve? Principalmente por tres razones:

-Documentación clara: Es una forma de decirle al desarrollador que usará tu código: "Oye, ten cuidado, aunque no te obligue a capturarla, es muy probable que este método lance esta excepción si no me pasas los datos correctos".

-Contratos de API: En librerías profesionales, se usa para explicitar el comportamiento. Ayuda a que herramientas como Javadoc generen documentación precisa.

-Auto-documentación en el IDE: Cuando otro programador empiece a escribir la llamada al método, el IDE le mostrará la excepción en la ayuda contextual, permitiéndole decidir si quiere gestionarla o no.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta
Se recomienda usar:

Excepciones controladas (checked) como IOException cuando el error es previsible y recuperable, y se espera que el llamador pueda tomar alguna acción concreta para solucionarlo (por ejemplo, un fichero que no existe o una conexión de red fallida).

Excepciones no controladas (unchecked) como IllegalArgumentException cuando el error es debido a mala lógica o violación de contrato, y normalmente no se puede o no se espera que el llamador lo maneje (por ejemplo, pasar un índice negativo a un método).

No todos los lenguajes tienen ambas opciones.

En lenguajes como C, solo existen mecanismos manuales de retorno de error, equivalentes a “no controladas” porque no hay obligación de verificarlas.

En muchos lenguajes modernos como Python o JavaScript, todas las excepciones son efectivamente “no controladas” (unchecked); se usan try-catch solo si se quiere manejar el error, pero el compilador no obliga.

Por eso, en lenguajes sin distinción, la opción más habitual es usar excepciones que se propaguen y manejar solo donde sea necesario, similar a las unchecked de Java.


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta
Sí, tiene sentido lanzar excepciones dentro de un catch. Hay dos casos principales:

Lanzar una nueva excepción diferente dentro de catch:
Se hace cuando quieres transformar un error en otro más significativo para el llamador, o cuando quieres añadir contexto.Ejemplo: 
try {
    double r = raiz(-4);
} catch (IllegalArgumentException e) {
    // Convertimos en una excepción personalizada
    throw new NumeroNegativoException("Error en cálculo de raíz", e);
}

Relanzar la misma excepción capturada (rethrow):
Se hace cuando quieres hacer algo adicional (como registrar o liberar recursos) y luego dejar que la excepción siga propagándose. Ejemplo:
try {
    double r = raiz(-4);
} catch (IllegalArgumentException e) {
    System.out.println("Registrando error: " + e.getMessage());
    throw e;  // relanzamos la misma excepción
}

Cuándo tiene sentido relanzar:

Cuando necesitas hacer tareas adicionales (logging, limpieza, auditoría) sin evitar que el llamador vea la excepción.

Cuando quieres propagar la excepción a un nivel superior que tenga más contexto para manejarla adecuadamente.




## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta
En Java, que una excepción sea la “causa” de otra significa que estamos encapsulando un error original dentro de otra excepción más significativa, normalmente de nivel superior o más abstracto, para que el llamador tenga contexto adicional.

Al imprimir la excepción con printStackTrace(), la causa se muestra automáticamente, indicando el origen del error.

Ejemplo:
import java.io.File;
import java.io.FileNotFoundException;
import java.util.Scanner;

// Excepción personalizada de alto nivel
class ErrorLecturaDatos extends Exception {
    public ErrorLecturaDatos(String msg, Throwable cause) {
        super(msg, cause);
    }
}

public class EjemploCausa {
    public static void leerFichero(String nombre) throws ErrorLecturaDatos {
        try {
            Scanner sc = new Scanner(new File(nombre)); // puede lanzar FileNotFoundException
            // leer datos...
        } catch (FileNotFoundException e) {
            // Encapsulamos la excepción de bajo nivel en otra de alto nivel
            throw new ErrorLecturaDatos("No se pudo leer el fichero de datos", e);
        }
    }

    public static void main(String[] args) {
        try {
            leerFichero("datos.txt");
        } catch (ErrorLecturaDatos e) {
            e.printStackTrace();
        }
    }
}
FileNotFoundException es la causa de ErrorLecturaDatos.

ErrorLecturaDatos proporciona un mensaje de alto nivel adecuado para el llamador.

Al imprimir e.printStackTrace(), se ve primero la excepción de alto nivel y luego la causa con su propio stack trace, mostrando claramente qué error original provocó el fallo.
Esto permite mantener la abstracción y al mismo tiempo no perder información sobre el error real.

