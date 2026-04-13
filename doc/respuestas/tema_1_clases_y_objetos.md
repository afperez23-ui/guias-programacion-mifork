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

- Abstracción: Se basa en identificar un objeto ignorando caracteristicas complejas y comportamietos de este. Basicamente simplifica lo que es para mejor entendimiento.
- Encapsulamiento: Oculta el estado interno del objeto para protegerlo de cualquier acceso externo. Se basa en la capacidad de agrupar datos y métodos que actuan sobre una clase
- Herencia: Se basa en la creación de clases nuevas a partir de clases existentes.
- Polimorfismo: Se basa en la capacidad de cada objeto de distintas clases para responder de manera distinta a un mismo mensaje.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

C++, Python, JavaScript y Java.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La programación estructurada, se basa en dividir el programa en bloques lógicos y secuenciales. Se centra en algoritmos y el flujo de control.
La programación modular, es una evolución directa de la estructurada. Se basa en la división del programa en partes pequeñas llamadas módulos, estos lo hacen más manejable y pequeñas.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

- Identidad: Todo objeto tiene identidad única. (Subdirección de memoria)
- Estado: Está definido por Atributos. El Estado es el valor que en ese momento se tenga para ese atributo. El estado es el valor de sus atributos.
- Comportamiento: Se define a través de los Métodos. Funciones que pueden hacer y pueden modificar el propio estado.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

- Clase: Molde que define el estado y el comportamiento. Ej: Coche (marca, año).
- No es lo mismo que una clase.
- Objetos o instancias: Realizaciones concretas en ejecución de una clase. Ej: Mercedes 2009 (vendrían siendo 2 instancias de "Coche")
- No todos, por ejemplo en Javascript no existen.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

- Los objetos se almacenan exclusivamente en el área de memoria llamada Heap.
- No es igual, por ejemplo en C se le permite al usuario decidir entre crear un objeto en el Stack o en el Heap, mientras que en Java se crea automaticamente en el heap y no permite su creación en el Stack.
_ Ventajas del Heap:
    -La memoria es dinámica, lo se decide lo que ocupa tiempo de ejecución.
    -La vida de los objetos del heap no depende de la vida de la función que los crea
_ Problemas del Heap:
    - Hay que encargarse de liberar memoria no usada del heap-> ·Manual (dificil y propenso a bugs) ·Con recolector de basura (Mal rendimiento)

- La colección de basura se produce mediante el recolectro de basura el cual se dedica a eliminar todo objeto en el heap en desuso para liberar RAM.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

- ¿Qué es un método? Cualquiera de las funciones definidas dentro de una clase, estos métodos son los que conforman esos comportamientos de esa clase.
- La sobrecarga de métodos: Es la posibilidad de crear métodos dentro de una clase con el mismo nombre pero cambiendo el tipo y o número de parámetros.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método
```java
class Punto{
    int x;
    int y;
        double calcularDistanciaOrigen(struct Punto p){
            return sqrt(x*x + y*y);
        }
}
class Ejercicio8{
    
    public static void main(String[] args) {
        
        Punto miPunto = new Punto();
        System.out.println("Introduce los puntos x y luego el y");

        x = miPunto.nextInt();
        y = miPunto.nextInt();

        double resultado = miPunto.calcularDistanciaOrigen();
    }
}
```
## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

- El punto de entrada de un programa en Java sería el main.
- La palabra clave static significa que el método pertenece a la clase en sí, no a una instancia específica de esa clase.
- Static no solo es único de main. También se usa para, por ejemplo, variables estaticas que no interesa cambiar en otros métodos.
- El final hace que no se pueda modificar un objeto. Entonces si pones que un objeto es final static estarías creando una constante.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

- Guardamos un texto en un .java (texto legible para el humano), el javac lo traduce a Byte-code y lo guard en un .class. Después la JVM (Tiempo de ejecución) compila a lenguaje real del PC sobre la marcha.



## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

- El 'new' permite reservar un espacio de memoria para guardar un objeto.
- Inicializa los atributos de la clase para que el objeto no empiece vacío.
```java

public class Empleado{
    String DNI;
    String apellidos;
    String Nombre;
    int edad;
    public Empleados(String DNI, String apellidos, String nombre, int edad){
        this.DNI = DNI;
        this.apellidos = apellidos;  
        this.nombre = nombre;
        this.edad = edad;
  }
  public static void main(String args[]){
    System.out.println("Empleado: "+nombre+ ", " +apellidos+ ". \nEdad:"+edad+"\nDNI: "+DNI);
  }
}
```
## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

- La referencia this hace referencia a la instancia actual de la clase.
- No, dependiendo el lenguaje esta puede variar. Por ejemplo en Python se usa self en vez de this.
```java

- public class Punto(){

    int x;
    int y;

        public Punto(int x, int y){
            this.x = x;
            this.y = y;
        }
}
class Ejercicio11{
    public static void main(String args[]);

    System.out.println("Punto en: " + this.x + ", " + this.y);
}
```
## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado
```java
- public class Punto(){

    int x;
    int y;

        public Punto(int x, int y){
            this.x = x;
            this.y = y;
        }
        public double distancia(Punto punto2){
            double px = this.x-punto2.x
            double py = this.y*punto2.y
            return Math sqrt( px*px + py*py);
        }
}
class Ejercicio11{
    public static void main(String args[]);

    Punto p1 = new Punto(0,4);
    Punto p2 = new Punto(2,3);
    double di = p1.distancia(p2); 
    System.out.println("La distancia es: " +di);
}
```
## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

- En java todo se pasa por copia, cuando copias Punto a un método, java copia la referencia. Esto afecta los cambios. (Los primitivos se pasan por copia, mientras que los objetos por copia de referencia).
- Si pasas un int, lo que ocurre es un paso por copia (valor) del dato puro, y el resultado es totalmente distinto al del objeto Punto


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

- En Java, todos los objetos, sin excepción, heredan de la clase madre Object. Esta clase ya trae un método toString() por defecto. El toString() pasa una salida que daría una dirección de memoria a un valor legible y comprensible para el humano.
- No existe el toString() en si, pero si otros métodos que hacen lo mismo. 
```java

public class Punto {
    int x;
    int y;

    public Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }
    public String toString() {
        return "(" + this.x + ", " + this.y + ")";
    }
}

class Ejercicio15 {
    public static void main(String[] args) {
        Punto p = new Punto(5, 12);

            System.out.println("La ubicación actual es: " + p); 
    }
}

```

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

- En C, un struct solo puede contener datos (variables). Si quieres hacer algo con esos datos, tienes que crear una función externa y pasarle el struct como argumento.
- Comportamiento(Métodos), Encapsulamiento, Identidad, Ciclo de vida y Herencia y polimorfismo.


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?
```C
#include <stdio.h>
#include <math.h>

struct Punto {
    int x;
    int y;
};

double distanciaAlOrigen(struct Punto *p) {
    return sqrt((p->x * p->x) + (p->y * p->y));
}

int main() {
    struct Punto miPunto = {3, 4};
    double d = distanciaAlOrigen(&miPunto);
    cout << ;
    return 0;
}
```

- En C no existe e método 'this', en su lugar se usan punteros. 