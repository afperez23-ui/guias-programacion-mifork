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
- No todos, por ejemplo en Javascript no existen 
## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

- ¿Qué es un método? Cualquiera de las funciones definidas dentro de una clase, estos métodos son los que conforman esos comportamientos de esa clase.
- La sobrecarga de métodos: Es la posibilidad de crear métodos dentro de una clase con el mismo nombre pero cambiendo el tipo y o número de parámetros.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

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

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
