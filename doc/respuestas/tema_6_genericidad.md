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

```java
public class ContenedorUniversal {
    private Object[] elementos;
    private int capacidad;

    public ContenedorUniversal(int capacidad) {
        this.elementos = new Object[capacidad];
    }

    public void insertar(int posicion, Object dato) {
        elementos[posicion] = dato;
    }

    public Object obtener(int posicion) {
        return elementos[posicion];
    }
}
```
## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

- La programación genérica es una técnica que permite crear clases, interfaces y métodos utilizando parámetros de tipo (como <T>), actuando como una "plantilla" reutilizable.
- No exactamente. Aunque el ejemplo de Object o void* busca el mismo objetivo (reutilización), se considera polimorfismo de inclusión o "genericidad mediante herencia", no programación genérica real.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

- El problema es que el control de errores pasa de ser estático (detectado por el ordenador mientras programas) a dinámico (detectado por el usuario cuando el programa falla).

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

- Los parámetros de tipo son marcadores de posición (habitualmente representados por una letra mayúscula como <T>) que se utilizan al definir una clase, interfaz o método para indicar que el tipo de dato real se especificará más tarde.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.
```java
import java.util.ArrayList;

public class EjemploJava {
    public static void main(String[] args) {
        // Instanciación con parámetro de tipo String
        ArrayList<String> lista = new ArrayList<>();

        lista.add("Java");
        lista.add("Generics");

        // Recorrido seguro: el compilador sabe que 's' es String
        for (String s : lista) {
            System.out.println("Elemento (String): " + s.toUpperCase());
        }
    }
}
```

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

- Java "limpia" los tipos para que todo parezca un Object internamente. - C++ "fabrica" una clase nueva y específica para cada tipo que utilices.
- No, Java prioriza la compatibilidad y el espacio, mientras que C++ prioriza el rendimiento y la especialización del hardware.
- Java finge que tiene tipos específicos para protegerte mientras programas, pero luego los olvida; C++ se toma el trabajo de fabricar una herramienta a medida para cada tipo que decidas usar.
## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

```java
public class Estadisticas {

    public static Par<Double, Double> calcularMetricas(double[] datos) {
        if (datos == null || datos.length == 0) return new Par<>(0.0, 0.0);

        // Calcular Media
        double suma = 0;
        for (double d : datos) suma += d;
        double media = suma / datos.length;

        // Calcular Desviación Típica
        double sumaCuadrados = 0;
        for (double d : datos) {
            sumaCuadrados += Math.pow(d - media, 2);
        }
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);

        // Devolvemos ambos valores en un solo contenedor genérico
        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] misDatos = {10.5, 12.0, 9.8, 14.2};

        // Instanciamos el Par especificando que ambos son Double
        Par<Double, Double> resultado = calcularMetricas(misDatos);

        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación: " + resultado.getSegundo());
    }
}
```


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 
```java
-public Object seleccionaUnoObject(Object o1, Object o2) {
    return (Math.random() > 0.5) ? o1 : o2;
}
//parametro
public <T> T seleccionaUnoGenerico(T o1, T o2) {
    return (Math.random() > 0.5) ? o1 : o2;
}
```
## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

- Sí, se pueden establecer restricciones mediante lo que se conoce como Bounded Type Parameters (Parámetros de tipo acotados). En Java, esto se logra con la palabra clave extends.
```java
- public class PuntoNumber {
    private Number x, y;

    public PuntoNumber(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(PuntoNumber otro) {
        // Debemos convertir a double manualmente para operar
        double dx = this.x.doubleValue() - otro.getX().doubleValue();
        double dy = this.y.doubleValue() - otro.getY().doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

- Mientras que la solución con Number es un "cubo" donde cabe cualquier cifra, la solución genérica es un contrato de precisión. Con genéricos, el programador define exactamente con qué nivel de detalle numérico quiere trabajar y el compilador se encarga de que se cumpla en todo el programa.

- Sin generics: Devuelve el tipo Number.
- Con generics: Devuelve el tipo específico T (que será el tipo concreto, como Integer o Double, usado al instanciar la clase).

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
public class Punto3D implements Punto<Punto3D> { 
    private final double x, y, z; 

    public Punto3D(double x, double y, double z) { 
        this.x = x; this.y = y; this.z = z; 
    } 

    @Override 
    public double distanciaA(Punto3D p) { 
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2) + Math.pow(z - p.z, 2)); 
    } 
}    ...
    public class Punto2D implements Punto<Punto2D> { 
    private final double x, y; 

    public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    // El compilador ahora exige recibir un Punto2D obligatoriamente
    public double distanciaA(Punto2D p) { 
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2)); 
    } 
}

```
## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

- No, aunque un String sea un Object, una List<String> no es un subtipo de List<Object>. Si lo fuera, se rompería la seguridad de tipos por la que existen los genéricos.
- Los arrays se diseñaron en los inicios de Java (antes de los genéricos) para permitir cierta flexibilidad polimórfica (por ejemplo, para que un método sort(Object[] a) pudiera ordenar cualquier cosa).
- Los genéricos, al ser más modernos, se diseñaron para ser invariantes por defecto, priorizando la seguridad total y detectando todos los errores de tipo durante la compilación, evitando que el programa llegue siquiera a ejecutarse si hay un riesgo de mezcla de datos.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

- Wildcard: Es un comodín que representa un tipo desconocido. Se usa para relajar la restricción de invariancia de los genéricos y permitir el polimorfismo de forma segura.

```java
// (i) Lectura segura (extends): Acepta List<Integer>, List<Double>...
public double sumar(List<? extends Number> lista) {
    double s = 0;
    for (Number n : lista) s += n.doubleValue();
    return s;
}

// (ii) Escritura segura (super): Acepta List<Integer>, List<Number>, List<Object>
public void añadir(List<? super Integer> lista) {
    lista.add(10); 
    lista.add(20);
}
```