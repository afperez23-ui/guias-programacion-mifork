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

- El polimorfismo es la capacidad que tiene una misma referencia de adoptar múltiples formas según el objeto real al que apunte.
- La sobreescritura (o overriding) es la capacidad de una subclase para proporcionar una implementación específica de un método que ya está definido en su superclase.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

- La ligadura dinámica (o dynamic binding) es el mecanismo por el cual el lenguaje decide qué implementación de un método ejecutar durante el tiempo de ejecución, en lugar de hacerlo durante la compilación.
- La ligadura dinámica es, técnicamente, el motor que hace posible el polimorfismo. Sin ella, el polimorfismo no funcionaría.
- Java
En Java, la ligadura dinámica es el comportamiento por defecto. No tienes que indicar nada explícitamente.

Comportamiento: Todos los métodos de instancia son "virtuales" por defecto.

Excepción: Si quieres evitar la ligadura dinámica por razones de seguridad o rendimiento, usas la palabra clave final. Un método final usa ligadura estática.

C++
En C++, la ligadura dinámica debe indicarse explícitamente.

Comportamiento: Por defecto, C++ utiliza ligadura estática (enlace en compilación) para ser más eficiente.

Palabra clave virtual: Para que un método tenga ligadura dinámica y permita polimorfismo, debes declararlo como virtual en la clase base. Si no lo haces, aunque el hijo sobreescriba el método, se ejecutará el del padre si usas un puntero de la clase base.

Python
En Python, al ser un lenguaje extremadamente dinámico, la ligadura dinámica es intrínseca y obligatoria.

Comportamiento: No existe la ligadura estática en el sentido tradicional. Todo se resuelve en tiempo de ejecución buscando el atributo o método en el objeto.

Explícito: No hay palabras clave como virtual o final. El polimorfismo es natural gracias al Duck Typing (si camina como pato y grazna como pato, es un pato).

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.
```java
// Clase base
class Soldado {
    public void saludar() {
        System.out.println("Soldado presentándose.");
    }
}

// Subclase que sobreescribe el comportamiento
class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador listo: ¡Cuidado con las minas!");
    }
}

// Subclase que hereda el comportamiento base
class Artillero extends Soldado {
    // No sobreescribe, usa el saludo estándar
}

public class Main {
    public static void main(String[] args) {
        // Array de la superclase conteniendo objetos de las subclases (Polimorfismo)
        Soldado[] peloton = {
            new Zapador(),
            new Artillero()
        };

        // Recorrido usando referencias de tipo Soldado
        for (Soldado s : peloton) {
            s.saludar(); 
        }
    }
}
```

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?
- La palabra clave
- La palabra clave que he usado es super.
super.saludar(): Le indica a Java que busque el método saludar en la jerarquía inmediatamente superior (la superclase) y lo ejecute.

- Utilidad: Es fundamental para evitar la duplicación de código. Si la clase Persona ya sabe imprimir el nombre y el DNI, el Estudiante solo tiene que llamar a super para esa parte y luego ocuparse de sus datos específicos (como el número de legajo).

```java
class Zapador extends Soldado {
    @Override
    public void saludar() {
        super.saludar(); // Invoca el comportamiento de la clase base (Soldado)
        System.out.println("ZAPADOR A SUS ORDENES"); // Añade funcionalidad extra
    }
}
```

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

1. Restricciones de tipos
Parámetros: Deben ser idénticos. Si cambias uno solo, dejas de sobreescribir.

Retorno: Debe ser igual o un subtipo (retorno covariante). Por ejemplo, si el padre devuelve Animal, el hijo puede devolver Perro.

Acceso: No puedes ser más restrictivo (si el padre es public, el hijo no puede ser private).

2. Sobreescritura vs. Sobrecarga
Sobreescritura (Overriding): Redefines en el hijo un método del padre con la misma firma para cambiar su lógica. (Ligadura dinámica).

Sobrecarga (Overloading): Creas en la misma clase varios métodos con el mismo nombre pero distintos parámetros. (Ligadura estática).

3. La anotación @Override
¿Para qué sirve?: Es un seguro. Obliga al compilador a verificar que el método realmente existe en el padre.

¿Por qué usarla?:
Evita errores: Si tecleas mal el nombre (ej. saludando() por saludar()), el compilador te avisa.
Documentación: Indica a otros programadores que ese método viene de una jerarquía superior.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

- Sí, el polimorfismo se emplea desde el principio porque en Java toda clase hereda de Object, lo que permite que métodos universales como System.out.println() funcionen con cualquier objeto mediante la ligadura dinámica del método toString().
- Sí, al sobreescribir toString() o equals() ya estás usando polimorfismo, porque permites que métodos generales de Java (como los de impresión o comparación de colecciones) ejecuten tu lógica específica en lugar de la implementación básica de la clase Object.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

- Es una clase incompleta que no permite crear objetos y sirve como molde base para que sus hijos hereden atributos y definan obligatoriamente ciertos comportamientos.
- Un método abstracto es una declaración de un método sin implementación (sin cuerpo) que obliga a las subclases a definir su propio código para poder existir.
- No, no puedes crear instancias de una clase abstracta porque se considera una definición incompleta que solo existe para ser heredada.

```java
abstract class Soldado { // 1. Clase abstracta
    public void saludar() { System.out.println("Hola"); }
    
    public abstract void atacar(); // 2. Método abstracto (sin llaves)
}
```
## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

- Prohíbe la herencia, impide que el método sea sobrescrito (overridden) por subclases. Resultado: Las clases hijas heredan el método pero no pueden cambiar su implementación.
- Mientras que el polimorfismo busca flexibilidad permitiendo múltiples formas de un mismo método o clase, final busca rigidez asegurando una única forma inmutable.
- Si estas clases no lo fueran, un programador malintencionado podría crear una subclase que parezca un String normal pero que envíe datos a un servidor externo, rompiendo la confianza total en el lenguaje.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

- Las interfaces definen comportamientos comunes entre clases que pueden no tener ninguna relación jerárquica entre sí.
- Sí, una clase en Java puede implementar múltiples interfaces.
## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

```java
class Punto2D extends Punto {
    double x, y;

    Punto2D(double x, double y) { this.x = x; this.y = y; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p2 = (Punto2D) otro; // Downcasting
            return Math.sqrt(Math.pow(p2.x - this.x, 2) + Math.pow(p2.y - this.y, 2));
        }
        throw new IllegalArgumentException("El punto debe ser 2D");
    }
}

class Punto3D extends Punto {
    double x, y, z;

    Punto3D(double x, double y, double z) { this.x = x; this.y = y; this.z = z; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p3 = (Punto3D) otro; // Downcasting
            return Math.sqrt(Math.pow(p3.x - this.x, 2) + Math.pow(p3.y - this.y, 2) + Math.pow(p3.z - this.z, 2));
        }
        throw new IllegalArgumentException("El punto debe ser 3D");
    }
}
```

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

- La herencia de interfaces ocurre cuando una interfaz extiende a otra interfaz utilizando la palabra clave extends. A diferencia de las clases, donde solo puedes heredar de una, una interfaz puede extender múltiples interfaces a la vez.
- Sí, existe. En Java, mientras que las clases tienen prohibida la herencia múltiple, las interfaces pueden extender múltiples interfaces simultáneamente.
```java
interface Encriptable {
    void cifrar();
}

// Una interfaz puede extender VARIAS interfaces a la vez
interface FicheroSeguro extends FicheroEscribible, Encriptable {
    boolean verificarIntegridad();
}
```