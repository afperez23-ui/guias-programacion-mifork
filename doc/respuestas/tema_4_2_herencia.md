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
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

- La herencia es un mecanismo que permite crear una clase nueva a partir de una existente. La relación "A es-un B" significa que la subclase (A) es una versión especializada de la superclase (B), conservando todas sus características básicas.

- Compatibilidad de tipos (Sustitución): Gracias a la relación "es-un", un objeto de una subclase puede ser tratado legalmente como un objeto de su superclase. Esto permite, por ejemplo, guardar un Artillero en una variable de tipo Soldado.

- Herencia de estado y comportamiento: La subclase adquiere automáticamente los atributos (estado) y los métodos (comportamiento) definidos en la superclase. Esto permite reutilizar código y asegurar que todas las subclases compartan una base común sin tener que reescribirla.

```java
// Clase base
class Soldado {
    private String nombre;

    public Soldado(String nombre) { this.nombre = nombre; }
    
    public void saludar() {
        System.out.println("Hola, soy el soldado " + nombre);
    }
}

// Subclase 1
class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }
    public int getCohetes() { return cohetes; }
}

// Subclase 2
class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }
    public int getMinas() { return minas; }
}

// Uso de compatibilidad de tipos
public class Main {
    public static void main(String[] args) {
        Soldado[] peloton = {
            new Artillero("Ramiro", 5),
            new Zapador("Erik", 10),
            new Artillero("Berta", 3)
        };

        for (Soldado s : peloton) {
            s.saludar(); // Todos heredan y usan este método
        }
    }
}
```
## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

- Se ejecutan dos constructores por cada objeto y el orden es de la superclase a la subclase: Primero se ejecuta el constructor de Soldado y después se ejecuta el constructor de una clase concreta.
- El término super dentro de un constructor se utiliza para invocar directamente al constructor de la superclase.
- Sí, es obligatorio. Si la clase base carece de un constructor sin parámetros (o no es accesible), el compilador no puede insertar la llamada automática a super().

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

- Sí, forman parte del objeto en memoria.
- No, el hecho de que estén en memoria no significa que sean accesibles directamente.

En el ejemplo de Soldado:

El atributo nombre es privado.
Aunque un Artillero tiene ese nombre guardado en su espacio de memoria, no puede leerlo ni modificarlo directamente (ej. this.nombre daría error de compilación).

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

- Que tu código es abierto a la extensión pero cerrado a la modificación.
```java
// Nuevo tipo de Soldado
class Medico extends Soldado {
    public Medico(String nombre) {
        super(nombre);
    }
    
    public void curar() {
        System.out.println("Curando heridas...");
    }
}

// El código de ejecución no cambia su lógica:
public class Main {
    public static void main(String[] args) {
        Soldado[] peloton = {
            new Artillero("Ramiro", 5),
            new Zapador("Erik", 10),
            new Medico("Elena") // <--- Nuevo tipo añadido sin problemas
        };

        // Este bloque es exactamente el mismo; no se modifica para admitir al Medico
        for (Soldado s : peloton) {
            s.saludar(); 
        }
    }
}
```
## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

- Sí, es una de las características clave de la herencia en Java.
- No, el compilador de Java solo te permite invocar los métodos que estén definidos en el tipo de la referencia, no en el tipo del objeto real al que apunta.
- Upcasting: Es convertir una referencia de una subclase a una superclase (subir en la jerarquía). Es automático y seguro porque un Artillero siempre "es un" Soldado.

Ejemplo: Soldado s = new Artillero("Ramiro", 5);

- Downcasting: Es convertir una referencia de una superclase a una subclase (bajar en la jerarquía). No es automático y requiere un moldeado explícito, ya que no todos los soldados son artilleros.

Ejemplo: Artillero a = (Artillero) s;

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

- El modificador protected significa que el miembro (atributo o método) es accesible para la propia clase, para sus subclases y para cualquier otra clase dentro del mismo paquete.
```java
class Soldado {
    // Accesible por esta clase y sus hijos (subclases)
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerMina() {
        // Acceso directo a 'nombre' porque es protected
        System.out.println(this.nombre + " está colocando una mina. Quedan: " + --minas);
    }
}
```
## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

La hay en algunos; En Java: Object, En C++ No hay. Implicación de java: Todos los objetos en Java (incluidos tus Soldado, Artillero, etc.) poseen métodos heredados de Object, como toString(), equals() y hashCode().

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

- Que una clase como Soldado puede heredar de más de un padre. En Java no hay. (solo permite el extends).

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 
```java
public class UsuarioNoEncontradoException extends RuntimeException {
    private final String usuario; // El usuario que dio el problema

    // Constructor simple
    public UsuarioNoEncontradoException(String mensaje, String usuario) {
        super(mensaje);
        this.usuario = usuario;
    }

    // Constructor sobrecargado con causa (Throwable cause)
    public UsuarioNoEncontradoException(String mensaje, String usuario, Throwable causa) {
        super(mensaje, causa);
        this.usuario = usuario;
    }

    public String getUsuario() {
        return usuario;
    }
}
```
## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

-Acoplamiento fuerte: La subclase queda encadenada a las decisiones del padre; si el padre cambia, el hijo puede romperse.

-Relación incorrecta: La herencia debe usarse para "ser algo" (un Gato es un Animal), no para "tener algo" (un Coche tiene un Motor).

-Rigidez: En Java solo puedes heredar de una clase. Si heredas para reutilizar código, agotas tu única oportunidad de herencia.

-Encapsulamiento: La herencia expone detalles internos del padre a los hijos, mientras que la composición mantiene cada clase como una "caja negra".

-Conclusión: La composición es más flexible y fácil de mantener. Solo usa herencia cuando exista una relación jerárquica clara.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

-Se debe a que la herencia es más rígida y peligrosa a largo plazo. Aquí los motivos principales:

-Acoplamiento fuerte: La herencia crea una dependencia total. Si cambias la superclase, puedes romper todas las subclases accidentalmente (problema de la clase base frágil).

-Relación semántica: La herencia debe representar que algo "es un" (un Gato es un Animal). La composición representa que algo "tiene un" (un Coche tiene un Motor), lo cual es mucho más común y natural.

-Flexibilidad en tiempo de ejecución: La herencia es estática (se define al compilar). Con la composición, puedes cambiar el comportamiento de un objeto en pleno funcionamiento intercambiando sus componentes.

-Limitación de Java: Como solo puedes heredar de una clase, usar la herencia solo para "copiar" código te impide heredar de una clase más apropiada después.

En resumen: la composición mantiene las clases independientes y fáciles de modificar; la herencia las encadena para siempre.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

- La herencia rompe la encapsulación porque el diseño de una subclase está tan ligado a la implementación interna de su padre que cualquier cambio en los detalles "ocultos" de la superclase puede alterar o romper el comportamiento del hijo de forma impredecible.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

```java
class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
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
```