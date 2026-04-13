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
````java
#include <stdio.h>
#include <math.h>

// Estructura base: Punto
struct Punto {
    double x;
    double y;
};

// Estructura compuesta: Linea (tiene-dos Puntos)
struct Linea {
    struct Punto p1;
    struct Punto p2;
};

// Función para calcular la distancia entre dos puntos
// d = sqrt((x2-x1)^2 + (y2-y1)^2)
double calcularDistancia(struct Punto a, struct Punto b) {
    return sqrt(pow(b.x - a.x, 2) + pow(b.y - a.y, 2));
}

// Función para hallar la longitud de una línea
double calcularLongitudLinea(struct Linea l) {
    return calcularDistancia(l.p1, l.p2);
}

int main() {
    struct Punto puntoA = {0.0, 0.0};
    struct Punto puntoB = {3.0, 4.0};
    struct Linea miLinea = {puntoA, puntoB};

    printf("Longitud de la linea: %.2f\n", calcularLongitudLinea(miLinea));

    return 0;
}
````

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  
```java
//Para garantizar la inmutabilidad, los atributos se definen como private final y no se proporcionan métodos setter.
public final class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    // Calcula la distancia a otro objeto Punto
    public double calcularDistancia(Punto otro) {
        return Math.sqrt(Math.pow(otro.getX() - this.x, 2) + 
                         Math.pow(otro.getY() - this.y, 2));
    }
}

//Clase Linea (Composición)
//La clase Linea "tiene-dos" objetos Punto. Al ser sus campos final, la relación entre los puntos es fija desde la construcción.
public final class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public Punto getP1() { return p1; }
    public Punto getP2() { return p2; }

    // Delegación: la línea usa el método del punto para hallar su longitud
    public double calcularLongitud() {
        return p1.calcularDistancia(p2);
    }
}

public class Main {
    public static void main(String[] args) {
        Punto puntoA = new Punto(0, 0);
        Punto puntoB = new Punto(3, 4);
        
        Linea miLinea = new Linea(puntoA, puntoB);

        System.out.println("Longitud de la línea: " + miLinea.calcularLongitud());
    }
}
```
## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La multiplicidad en el contexto de la composición (y del modelado de clases en general) indica el número de instancias de una clase que pueden estar vinculadas a una instancia de otra clase. Define los límites inferiores y superiores de esta relación (por ejemplo: uno a uno, uno a muchos, etc.).

- El concepto de Composición:
En la composición, el objeto "hijo" (Motor) no tiene sentido de existencia sin el objeto "padre" (Coche). Si el coche se        destruye, el motor también.
## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

- Composición Fuerte (Composición): El objeto hijo no puede existir sin el padre. Su ciclo de vida está ligado: si el padre se destruye, el hijo también.

- Composición Débil (Agregación): El objeto hijo puede existir independientemente. Si el padre se destruye, el hijo sobrevive.


- La consecuencia principal radica en quién controla la creación y destrucción de los objetos:

    Composición (Fuerte): El objeto padre es el dueño absoluto del ciclo de vida del hijo. El hijo nace cuando el padre lo crea y muere cuando el padre es destruido. No tienen vida independiente.

    Agregación (Débil): Los objetos tienen ciclos de vida independientes. El hijo suele crearse fuera del padre y se le asigna después. Si el padre se destruye, el hijo sobrevive y puede vincularse a otros objetos.

-Asociación o Agregación: Se refiere a la composición débil.

-Composición: Se refiere estrictamente a la composición fuerte.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Es una relación de uso en lugar de pertenencia. Se diferencia de la composición porque el objeto no forma parte de la estructura permanente de la clase (no es un atributo), sino que aparece de forma temporal durante la ejecución de un método.

Composición/Agregación: "A tiene un B" (vínculo permanente).

Dependencia: "A usa un B" (vínculo transitorio).

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.
```java
public final class Persona {
    private final String nombre;
    private final Persona madre; // Composición recursiva

    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }

    @Override
    public String toString() {
        return nombre + (madre != null ? " (hijo/a de " + madre.getNombre() + ")" : " (raíz de la familia)");
    }
}
```
## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta
