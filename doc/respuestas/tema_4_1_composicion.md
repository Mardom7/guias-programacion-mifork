<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta
#include <stdio.h>
#include <math.h>

// Definición de un punto
struct Punto {
    float x;
    float y;
};

// Definición de una línea (compuesta por dos puntos)
struct Linea {
    struct Punto p1;
    struct Punto p2;
};

// Función para calcular la distancia entre dos puntos
float distancia(struct Punto a, struct Punto b) {
    return sqrt(pow(b.x - a.x, 2) + pow(b.y - a.y, 2));
}

// Función para calcular la longitud de una línea
float longitudLinea(struct Linea l) {
    return distancia(l.p1, l.p2);
}

int main() {
    struct Punto a = {1.0, 2.0};
    struct Punto b = {4.0, 6.0};

    struct Linea linea = {a, b};

    printf("Distancia entre puntos: %.2f\n", distancia(a, b));
    printf("Longitud de la linea: %.2f\n", longitudLinea(linea));

    return 0;
}


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta
// Clase Punto (inmutable)
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método para calcular distancia a otro punto
    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

    // Getters (opcionales, solo lectura)
    public double getX() {
        return x;
    }

    public double getY() {
        return y;
    }
}


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta
La multiplicidad en una relación de composición indica cuántas instancias de una clase pueden estar asociadas con una instancia de otra clase. Es decir, define la cantidad de objetos que participan en la relación entre dos clases dentro de un diseño orientado a objetos.

En el ejemplo de las clases Linea y Punto, la relación es de composición porque una línea está formada por puntos. En este caso, la multiplicidad de Linea a Punto es 2, ya que cada línea está compuesta exactamente por dos puntos: un punto inicial y un punto final.

Por otro lado, la multiplicidad de Punto a Linea es 0..*, ya que un mismo punto podría no pertenecer a ninguna línea o podría formar parte de varias líneas diferentes.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta
La composición fuerte y la composición débil son dos tipos de relaciones entre objetos en la programación orientada a objetos que se diferencian principalmente en el grado de dependencia entre los objetos y su ciclo de vida.

La composición fuerte se produce cuando un objeto está formado por otros objetos que dependen completamente de él para existir. Esto significa que el ciclo de vida de los objetos contenidos está ligado al del objeto principal. Si el objeto principal se elimina, los objetos que lo componen también desaparecen. A esta relación es a la que normalmente se le llama composición propiamente dicha.

Por otro lado, la composición débil ocurre cuando un objeto contiene o utiliza otros objetos, pero estos pueden existir de forma independiente. En este caso, si el objeto principal desaparece, los otros objetos pueden seguir existiendo porque no dependen completamente de él. A este tipo de relación se le suele llamar asociación o agregación.


## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta
Cuando una clase utiliza a otra únicamente dentro de sus métodos, por ejemplo recibiéndola como parámetro, devolviéndola como valor de retorno, creándola con new dentro de un método o utilizándola como variable local, no hablamos de composición. En estos casos se trata de una relación de dependencia.

La dependencia se produce cuando una clase necesita utilizar temporalmente otra clase para realizar alguna operación, pero no la mantiene como parte de su estructura interna. Es decir, la clase solo usa a la otra de forma puntual dentro de un método y no existe una relación permanente entre ellas.

En cambio, en la composición un objeto contiene a otros objetos como atributos de la clase, formando parte de su estructura. En este caso la relación es más fuerte y duradera.

Por tanto, cuando una clase solo utiliza otra como parámetro, valor de retorno, variable local o la crea dentro de un método, estamos hablando de una relación de dependencia y no de composición.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta
En el ejemplo de Linea y Punto, la relación puede programarse de dos maneras dependiendo de si los puntos dependen o no del ciclo de vida de la línea.

Composición fuerte

En la composición fuerte, los objetos Punto se crean dentro de Linea y no existen fuera de ella. Por tanto, si se destruye la Linea, también desaparecen los Punto. El ciclo de vida de los puntos está ligado al de la línea.
class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}
En este caso, los puntos se crean dentro de Linea, por lo que no pueden existir independientemente.



Composición débil (agregación)

En la composición débil, los objetos Punto se crean fuera de Linea y luego se pasan a su constructor. Los puntos pueden existir independientemente y pueden utilizarse en varias líneas.

class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distancia(Punto otro) {
        double dx = otro.x - this.x;
        double dy = otro.y - this.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distancia(p2);
    }
}

En este caso, los Punto existen de forma independiente y simplemente se asocian a la Linea.


En resumen, en la composición fuerte los puntos se crean dentro de la línea y su existencia depende de ella, mientras que en la composición débil los puntos se crean fuera y pueden existir y utilizarse independientemente de la línea.


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta
En Java, en una relación de composición fuerte, no es necesario destruir explícitamente los objetos contenidos. Esto se debe a que Java utiliza un sistema automático de gestión de memoria llamado recolector de basura (garbage collector).

El recolector de basura se encarga de liberar la memoria de aquellos objetos que ya no tienen referencias activas en el programa. Cuando un objeto deja de ser accesible desde cualquier parte del código, el sistema lo considera innecesario y puede eliminarlo automáticamente.

En el ejemplo de Linea y Punto, la clase Linea contiene dos objetos Punto como atributos. Cuando una instancia de Linea deja de existir (por ejemplo, cuando ya no hay ninguna referencia a ella), los objetos Punto que estaban dentro de ella también quedan sin referencias. Como consecuencia, el garbage collector detecta que esos objetos ya no son accesibles y los elimina automáticamente.

Por esta razón, en Java no vemos una destrucción explícita de los objetos Punto dentro de Linea, a diferencia de lenguajes como C o C++, donde el programador debe liberar la memoria manualmente. En Java, la eliminación de los objetos ocurre de forma automática cuando dejan de ser accesibles, lo que simplifica la gestión del ciclo de vida de los objetos.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta
class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isEmpty()) {
            throw new IllegalArgumentException("El nombre del profesor no puede estar vacío");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class Departamento {
    private Profesor director;
    private final Profesor[] profesores;
    private int cantidadProfesores;
    private static final int MAX_PROFESORES = 50;

    // Constructor: el departamento siempre debe tener un director
    public Departamento(Profesor director) {
        if (director == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }
        this.director = director;
        this.profesores = new Profesor[MAX_PROFESORES];
        this.profesores[0] = director; // el director es siempre el primer profesor
        this.cantidadProfesores = 1;
    }

    // Añadir un profesor al final de la lista
    public void addProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("No se puede añadir un profesor null");
        }
        if (cantidadProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("Número máximo de profesores alcanzado");
        }
        profesores[cantidadProfesores] = p;
        cantidadProfesores++;
    }

    // Eliminar un profesor por posición
    public void removeProfesor(int index) {
        if (index < 0 || index >= cantidadProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        Profesor p = profesores[index];
        if (p == director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento");
        }
        // Mover todos los elementos posteriores una posición hacia atrás
        for (int i = index; i < cantidadProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[cantidadProfesores - 1] = null; // limpiar referencia
        cantidadProfesores--;
    }

    // Obtener número de profesores
    public int getCantidadProfesores() {
        return cantidadProfesores;
    }

    // Obtener profesor por posición
    public Profesor getProfesor(int index) {
        if (index < 0 || index >= cantidadProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida");
        }
        return profesores[index];
    }

    // Cambiar el director del departamento
    public void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }

        // Verificar que el nuevo director ya está en la lista
        boolean encontrado = false;
        for (int i = 0; i < cantidadProfesores; i++) {
            if (profesores[i] == nuevoDirector) {
                encontrado = true;
                break;
            }
        }

        if (!encontrado) {
            throw new IllegalStateException("El nuevo director debe ser un profesor del departamento");
        }

        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }
}


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta
Perfecto, podemos reescribir el ejemplo usando List<Profesor> de Java en lugar de arrays primitivos. Esto simplifica mucho el código, porque List maneja internamente el tamaño, el desplazamiento al eliminar elementos, y permite añadir o eliminar fácilmente.

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isEmpty()) {
            throw new IllegalArgumentException("El nombre del profesor no puede estar vacío");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class Departamento {
    private Profesor director;
    private final List<Profesor> profesores;
    private static final int MAX_PROFESORES = 50;

    // Constructor: siempre debe tener un director
    public Departamento(Profesor director) {
        if (director == null) {
            throw new IllegalArgumentException("El director no puede ser null");
        }
        this.director = director;
        this.profesores = new ArrayList<>();
        this.profesores.add(director);
    }

    // Añadir profesor
    public void addProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("No se puede añadir un profesor null");
        }
        if (profesores.size() >= MAX_PROFESORES) {
            throw new IllegalStateException("Número máximo de profesores alcanzado");
        }
        profesores.add(p);
    }

    // Eliminar profesor por posición
    public void removeProfesor(int index) {
        Profesor p = profesores.get(index);
        if (p == director) {
            throw new IllegalStateException("No se puede eliminar al director del departamento");
        }
        profesores.remove(index);
    }

    // Obtener número de profesores
    public int getCantidadProfesores() {
        return profesores.size();
    }

    // Obtener profesor por posición
    public Profesor getProfesor(int index) {
        return profesores.get(index);
    }

    // Cambiar director (debe estar en la lista)
    public void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null || !profesores.contains(nuevoDirector)) {
            throw new IllegalStateException("El nuevo director debe ser un profesor del departamento");
        }
        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }

    // Obtener lista de profesores (de forma segura)
    public List<Profesor> getProfesores() {
        // Devolver una copia inmutable para no exponer la lista interna
        return Collections.unmodifiableList(profesores);
    }
}


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta
class Persona {
    private final String nombre;
    private final Persona madre; // composición recursiva

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isEmpty()) {
            throw new IllegalArgumentException("El nombre no puede ser null o vacío");
        }
        this.nombre = nombre;
        this.madre = madre; // puede ser null si no se conoce la madre
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }
}

public class Main {
    public static void main(String[] args) {
        // Crear la abuela (sin madre conocida)
        Persona abuela = new Persona("Marta", null);

        // Crear la madre, con referencia a la abuela
        Persona madre = new Persona("Ana", abuela);

        // Crear el nieto, con referencia a la madre
        Persona nieto = new Persona("Lucas", madre);

        // Mostrar la línea materna
        Persona p = nieto;
        while (p != null) {
            System.out.println(p.getNombre());
            p = p.getMadre();
        }
    }
}
## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta

class Profesor {
    private final String nombre;
    private Departamento departamento; // referencia bidireccional

    public Profesor(String nombre) {
        if (nombre == null || nombre.isEmpty())
            throw new IllegalArgumentException("Nombre no puede estar vacío");
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    public Departamento getDepartamento() {
        return departamento;
    }

    // Método interno para asignar departamento (package-private o privado)
    void setDepartamento(Departamento depto) {
        this.departamento = depto;
    }
}

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Departamento {
    private Profesor director;
    private final List<Profesor> profesores = new ArrayList<>();
    private static final int MAX_PROFESORES = 50;

    public Departamento(Profesor director) {
        if (director == null) throw new IllegalArgumentException("El director no puede ser null");
        this.director = director;
        this.profesores.add(director);
        director.setDepartamento(this); // sincronización bidireccional
    }

    public void addProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("Profesor null");
        if (profesores.size() >= MAX_PROFESORES)
            throw new IllegalStateException("Máximo de profesores alcanzado");
        profesores.add(p);
        p.setDepartamento(this); // sincronización bidireccional
    }

    public void removeProfesor(int index) {
        Profesor p = profesores.get(index);
        if (p == director) throw new IllegalStateException("No se puede eliminar al director");
        profesores.remove(index);
        p.setDepartamento(null); // rompe la relación bidireccional
    }

    public void setDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null || !profesores.contains(nuevoDirector))
            throw new IllegalStateException("El director debe estar en la lista");
        this.director = nuevoDirector;
    }

    public Profesor getDirector() {
        return director;
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
    }
}
