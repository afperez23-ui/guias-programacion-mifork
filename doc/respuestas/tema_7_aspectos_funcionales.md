<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

- Un puntero a una función es una variable que, en lugar de almacenar el valor de un dato (como un entero o un carácter), almacena la dirección de memoria donde comienza el código ejecutable de una función.

```c
#include <stdio.h>
#include <ctype.h>

// 1. Definición de la función
// Recibe un puntero a char y lo modifica in-place
char* convertirAMayusculas(char* cadena) {
    char* inicio = cadena;
    while (*cadena) {
        *cadena = toupper((unsigned char)*cadena);
        cadena++;
    }
    return inicio;
}

int main() {
    char texto[] = "hola mundo en c";

    // 2. Declaración del puntero a la función
    // Sintaxis: tipo_retorno (*nombre_puntero)(tipo_parametros)
    char* (*aMayusculas)(char*);

    // 3. Asignación de la dirección de la función al puntero
    aMayusculas = &convertirAMayusculas;

    // 4. Invocación de la función a través del puntero
    // Se puede usar (*aMayusculas)(texto) o simplemente aMayusculas(texto)
    char* resultado = aMayusculas(texto);

    printf("Resultado: %s\n", resultado);

    return 0;
}
```

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

- Una función lambda (también llamada función anónima) es una función que se define sin un nombre identificador. Se trata de un bloque de código que puede ser tratado como una variable: puede ser asignado a una referencia, pasado como argumento o devuelto por otra función.

```java
import java.util.function.Function;

public class EjemploLambda {
    public static void main(String[] args) {
        // Variable local con el tipo de interfaz funcional
        // La sintaxis es: (parámetros) -> { cuerpo }
        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

        // Invocación usando el método funcional 'apply'
        String texto = "hola mundo en java";
        String resultado = aMayusculas.apply(texto);

        System.out.println(resultado); // Muestra: "HOLA MUNDO EN JAVA"
    }
}
```

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

- El paradigma funcional es un estilo de programación basado en la composición de funciones matemáticas puras, que prioriza la inmutabilidad de los datos y evita el uso de estados compartidos o efectos secundarios.
- Se les llama multi-paradigma porque permiten combinar la estructura clásica de la Programación Orientada a Objetos (POO) con capacidades propias de la Programación Funcional.
- Que las funciones sean "ciudadanos de primera clase" (o first-class citizens) significa que el lenguaje las trata como a cualquier otra variable o valor (como un int, un String o un objeto).

## 4. Explica la sintaxis básica de una función lambda en Java.

- La sintaxis de una función lambda en Java es muy minimalista y se basa en el operador flecha (->), que separa los parámetros de entrada del cuerpo de la función.

```java
// 1. Completa
Comparator<Integer> c1 = (Integer a, Integer b) -> { return a.compareTo(b); };

// 2. Simplificada (Inferencia y expresión única)
Comparator<Integer> c2 = (a, b) -> a.compareTo(b);

// 3. Referencia de método (Nivel experto)
Comparator<Integer> c3 = Integer::compareTo;
```
## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

- Este es el corazón de la programación funcional: las funciones de orden superior (Higher-Order Functions). Un método de orden superior es simplemente aquel que recibe otra función como argumento para delegarle parte de su lógica.

```java
import java.util.function.Function;

public class EjemploOrdenSuperior {
    
    // Método que recibe el String y la función (comportamiento)
    public static String transformar(String texto, Function<String, String> funcion) {
        // En Java, las funciones se invocan con el método .apply()
        return funcion.apply(texto);
    }

    public static void main(String[] args) {
        // Definimos la lambda
        Function<String, String> aMayusculas = s -> s.toUpperCase();

        // Invocamos el método pasando la lógica por parámetro
        String resultado = transformar("java es multiplataforma", aMayusculas);
        
        System.out.println(resultado); // "JAVA ES MULTIPLATAFORMA"
        
        // También podemos pasar la lambda directamente (en línea)
        System.out.println(transformar("hola", s -> s + "!!!")); // "hola!!!"
    }
}
```

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

- Esta es una de las mayores ventajas de las lambdas: la capacidad de definir comportamiento "al vuelo" (on-the-fly) sin necesidad de declarar una variable previa.
```java
public class EjemploInversion {
    public static void main(String[] args) {
        
        // Invocamos transformar pasando la lógica de inversión "en línea"
        String resultado = transformar("Españita", s -> new StringBuilder(s).reverse().toString());

        System.out.println(resultado); // "atiñapsE"
    }

    public static String transformar(String texto, java.util.function.Function<String, String> funcion) {
        return funcion.apply(texto);
    }
}
```

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

- Un cierre o closure es la capacidad de una función (generalmente una lambda) de "recordar" y acceder al entorno (variables, constantes) en el que fue creada, incluso si ese entorno ya ha terminado su ejecución. La función lambda "envuelve" o captura las variables externas que necesita para funcionar, llevándoselas consigo.

```java
import java.util.function.Function;

public class EjemploClosure {
    public static void main(String[] args) {
        // 1. Variable local definida FUERA de la lambda
        String prefijo = "Resultado: "; 
        
        // 2. Definición de la lambda que "captura" la variable prefijo
        // Esto es un closure: la lambda depende de una variable externa
        Function<String, String> concatenarPrefijo = (texto) -> prefijo + texto;

        // 3. Uso del método transformar
        String mensaje = "La operación ha sido un éxito";
        String resultadoFinal = transformar(mensaje, concatenarPrefijo);

        System.out.println(resultadoFinal); 
        // Imprime: "Resultado: La operación ha sido un éxito"
    }

    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }
}
```
## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

- Esta es una excelente pregunta de fondo, porque aunque ambos sirven para "pasar comportamiento" como si fuera un dato, pertenecen a eras y filosofías de computación totalmente distintas.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

```java
import java.util.function.Function;

public class GeneradorDescuentos {

    // Método que fabrica y devuelve una función personalizada
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // La lambda "captura" la variable 'porcentaje'
        return (precioOriginal) -> precioOriginal * (1 - porcentaje / 100);
    }

    public static void main(String[] args) {
        // Creamos dos funciones de descuento distintas
        Function<Double, Double> descuentoBlackFriday = crearDescuento(50.0);
        Function<Double, Double> descuentoIVA = crearDescuento(21.0); // En este caso, resta el 21%

        double precioProducto = 100.0;

        // Aplicamos las funciones creadas
        System.out.println("Precio con 50%: " + descuentoBlackFriday.apply(precioProducto)); // 50.0
        System.out.println("Precio menos 21%: " + descuentoIVA.apply(precioProducto));      // 79.0
    }
}
```
## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

- En Java, una interfaz funcional es el "molde" o tipo que define la firma de una función lambda. Dado que Java es un lenguaje orientado a objetos, no puede tener funciones "sueltas" flotando por el código; por ello, cada lambda se asocia automáticamente a una interfaz que describa su estructura.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

- Aunque no pongas la anotación @FunctionalInterface, si la interfaz solo tiene un método abstracto, Java la tratará como funcional. Sin embargo, ponla siempre: es lo que evita que un compañero de equipo (o tú mismo en el futuro) añada un segundo método por error y rompa todas las lambdas que dependían de ella.

```java
public class EjemploTransformadorManual {

    // Método que acepta nuestra interfaz personalizada
    public static String ejecutarCambio(String texto, Transformador t) {
        return t.transformar(texto);
    }

    public static void main(String[] args) {
        // Implementación 1: A mayúsculas
        Transformador aMayusculas = s -> s.toUpperCase();

        // Implementación 2: Inversión (definida en línea)
        String resultado = ejecutarCambio("Java", s -> new StringBuilder(s).reverse().toString());

        System.out.println("Mayúsculas: " + aMayusculas.transformar("hola")); // HOLA
        System.out.println("Invertido: " + resultado); // avaJ
    }
}
```

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.
```java
public class Main {
    public static void main(String[] args) {
        
        // Definimos el transformador: Recibe Double (T) y devuelve Integer (R)
        Transformador<Double, Integer> redondeador = (valor) -> (int) Math.round(valor);

        // Uso del transformador
        Double miDecimal = 9.75;
        Integer resultado = redondeador.transformar(miDecimal);

        System.out.println("Valor original: " + miDecimal); // 9.75
        System.out.println("Valor redondeado: " + resultado); // 10
    }
}
```
## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

```java
import java.util.function.*;

public class Tienda {
    public static void main(String[] args) {
        // 1. Predicado: ¿El producto es caro?
        Predicate<Double> esCaro = precio -> precio > 50.0;

        // 2. Función: Aplicar impuesto (Transformación)
        Function<Double, Double> aplicarIVA = precio -> precio * 1.21;

        // 3. Consumidor: Mostrar ticket (Acción)
        Consumer<Double> imprimirTicket = p -> System.out.println("Total con IVA: " + p);

        // 4. Proveedor: Código de descuento (Generación)
        Supplier<String> codigoPromo = () -> "PROMO_2026";

        // Uso lógico
        double miCompra = 60.0;
        if (esCaro.test(miCompra)) {
            double finalPrice = aplicarIVA.apply(miCompra);
            imprimirTicket.accept(finalPrice);
            System.out.println("Usa este código: " + codigoPromo.get());
        }
    }
}
```

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

```java
import java.util.Arrays;
import java.util.List;

public class EjemploForEach {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-5, 10, 0, 25, -2);

        // Uso de forEach con una expresión lambda
        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("El número " + n + " es positivo.");
            }
        });
    }
}
```
## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

- PECS significa Producer Extends, Consumer Super. Es la regla para usar comodines (wildcards) en genéricos:
Producer Extends (? extends T): Si el objeto produce datos (extraes información de él), se usa extends. Permite leer T o sus hijos.

Consumer Super (? super T): Si el objeto consume datos (le pasas información para que la procese), se usa super. Permite que el consumidor sea de un tipo más genérico (padre) que el dato que le envías.

- Se usa Consumer<? super T> por flexibilidad: permite que un consumidor de un tipo superior (ej. Number) procese elementos de un tipo inferior (ej. Integer).

-
## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

```java
class Persona {
    String nombre;
    Persona(String n) { this.nombre = n; }
    void saludar() { System.out.println("Hola, soy " + nombre); }
}

public class Main {
    public static void main(String[] args) {
        Persona p = new Persona("Ana");
        // Referencia al método de la instancia 'p'
        Runnable ref = p::saludar; 
        ref.run(); // Invoca el método
    }
}```

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

Método estático: Clase::metodoEstatico
   - Ejemplo: Double::sum
Constructor: Clase::new
   - Ejemplo: ArrayList::new
Instancia de un objeto concreto: instancia::metodoInstancia
   - Ejemplo: System.out::println (donde out es la instancia).
Instancia de un objeto arbitrario de un tipo dado: Clase::metodoInstancia
   - Ejemplo: String::toUpperCase (se aplica sobre el elemento que reciba).

```java
import java.util.*;
import java.util.function.*;

public class EjemploReferencias {
    public static void main(String[] args) {
        
        // 1. Método estático (Math.abs)
        Function<Integer, Integer> valorAbsoluto = Math::abs;

        // 2. Constructor (ArrayList::new)
        Supplier<List<String>> creadorLista = ArrayList::new;

        // 3. Instancia de un objeto concreto (System.out::println)
        Consumer<String> impresor = System.out::println;

        // 4. Instancia de un objeto arbitrario de un tipo dado (String::isEmpty)
        Predicate<String> esVacia = String::isEmpty;

        // Pruebas
        impresor.accept("Resultado Raíz: " + valorAbsoluto.apply(-10)); // Usa el 1 y el 3
        System.out.println("¿Texto vacío?: " + esVacia.test(""));       // Usa el 4
    }
}
```
## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.
```java
- class Persona {
    String nombre;
    int edad;

    Persona(String nombre, int edad) {
        this.nombre = nombre;
        this.edad = edad;
    }
    @Override
    public String toString() { return nombre + " (" + edad + ")"; }
}
// Leído: "Compara por edad y luego por nombre"
Collections.sort(personas, Comparator
    .comparingInt((Persona p) -> p.edad)
    .thenComparing(p -> p.nombre)
);
```