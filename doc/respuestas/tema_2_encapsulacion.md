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

    - Encapsulación: Agrupa el conjunto de datos(atributos) y comportamientos(métodos) en una sola clase (Permite ocultar miebros al exterior de la clase). Añade protección a mi clase.
        -Evita que otro código dependa o acceda a partes que no se quiere
        -Garantiza que mi estado interno es válido
        -Facilita poder cambiar partes sin afectar otros


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

- Interfaz pública: Los miembros que se van desde otras clases, es decir, lo que no está oculto. (Se definen con "public")

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

- La interfaz pública: Si se cambia tiene más consecuencias que si cambio partes ocultas. Mientras que los cambios en la implementación oculta son invisibles para el resto del sistema, cualquier modificación en la interfaz pública rompe el "contrato" establecido con los objetos que la consumen, obligando a localizar y corregir cada una de las dependencias externas para evitar errores de compilación o fallos en el funcionamiento global.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Son las reglas o condiciones que se cumplen siempre para los objetos de esa clase. Refiriendonos habitualmente al estado.
Ej: 
    Cuenta Bancaria ---> Siempre debe tener saldo >= 0
    Persona ---> Debe tener >= 0 años

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?
```java
public class Punto{
    private double x;
    private double y;
        public Punto(double x, double y){
         this.x = x;
         this.y = y;
    }
    public double calcularDistanciaOrigen(){
        return Math.sqrt(Math.pow(this.x, 2) + Math.pow(this.y, 2));
    }

}
//interfaz pública: - Punto - DistanciaOrigen
//oculto:  - x e y
```
- Public: Son accesibles desde cualqueir parte del programa (cualquier zona del código independientemente de su clase)
- Private: Solo puede hcer referencia a todo aquellos miembros en su clase

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

public - clases y miembros.
private - clases internas y miembros


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

- protected -> miembros accesibles desde subclases (lo determina la jerarquia). 
  sin modificador o  "package private" -> solo accesible desde el paquete en el que está
- En Java, la visibilidad se decide combinando dos conceptos: el Paquete (la carpeta donde vive el archivo) y la Herencia (la familia de clases).
- En otros lenguajes no es igual que en java.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

```java
public class Punto{
    private double x;
    private double y;
        public Punto(double x, double y){
         this.x = x;
         this.y = y;
    }
    public double calcularDistanciaOrigen(){
        return Math.sqrt(Math.pow(this.x, 2) + Math.pow(this.y, 2));
    }
    double distanciaPuntoA(Punto otro){
        double dx = this.x - otro.x;
        double dx = this.x - otro.x;
    return Math.sqrt(dx*dx+dy*dy);
    }
}

```
## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

getter - Devuelve el valor de un objeto privado.
setter - Modifica el valor de un objeto privado.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No, nos referimos a intentar reducir errores de programacióm


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?
Miembro ( métodos o atributos)

Miembros de clase: Asociados a la clase compartidas, no existe el this.
Miembro de istancia: Accede siempre a una instancia.
Sí, los miembros de clase (aquellos declarados con la palabra clave static) también se pueden ocultar y están sujetos a las mismas reglas de visibilidad y encapsulación que los miembros de instancia.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, Quiero crear objetos solo a través de métodos factoría (Uso de Métodos Factoría: Al hacer el constructor privado, obligas a los demás programadores a usar métodos estáticos con nombres descriptivos).
La clase solo tiene miembros de clase.
Controlo el número de instancias que se crean.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

```java
public class Punto{
    private double x;
    private double y;

    private static MAX_X = Double.NEGATIVE_INFINITY;
    private static MAX_Y = Double.NEGATIVE_INFINITY;

        public staticPunto(double x, double y){
         this.x = x;
         this.y = y;
         if(x > MAX_X){
            MAX_X = Y;
         }
         if(y > MAX_Y){
            MAX_Y = y;
         }
    }
    public double calcularDistanciaOrigen(){
        return Math.sqrt(Math.pow(this.x, 2) + Math.pow(this.y, 2));
    }
    double distanciaPuntoA(Punto otro){
        double dx = this.x - otro.x;
        double dx = this.x - otro.x;
    return Math.sqrt(dx*dx+dy*dy);
    }
}
```

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

```java
public class Punto{
    private double x;
    private double y;

    private static MAX_X = Double.NEGATIVE_INFINITY;
    private static MAX_Y = Double.NEGATIVE_INFINITY;

        public staticPunto(double x, double y){
         this.x = x;
         this.y = y;
         if(x > MAX_X){
            MAX_X = Y;
         }
         if(y > MAX_Y){
            MAX_Y = y;
         }
    }
    public double calcularDistanciaOrigen(){
        return Math.sqrt(Math.pow(this.x, 2) + Math.pow(this.y, 2));
    }
    
    static Punto nuevoPuntoconRedondeo(double x, double y){
        return new Punto(Math.round(x),Math.round(y))
    }

    double distanciaPuntoA(Punto otro){
        double dx = this.x - otro.x;
        double dx = this.x - otro.x;
    return Math.sqrt(dx*dx+dy*dy);
    }
}
```

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.
```java
public class Punto{
    private double[] coordenada = new double[2]; 
    private double x;
    private double y;

    private static MAX_X = Double.NEGATIVE_INFINITY;
    private static MAX_Y = Double.NEGATIVE_INFINITY;

        public staticPunto(double x, double y){
         this.coordenada[0] = x;
         this.coordenada[1] = y;
         if(x > MAX_X){
            MAX_X = Y;
         }
         if(y > MAX_Y){
            MAX_Y = y;
         }
    }
    public double calcularDistanciaOrigen(){
        return Math.sqrt(Math.pow(this.coordenada[0], 2) + Math.pow(this.coordenada[1], 2));
    }
    
    static Punto nuevoPuntoconRedondeo(double x, double y){
        return new Punto(Math.round(x),Math.round(y))
    }

    double distanciaPuntoA(Punto otro){
        double dx = this.coordenada[0] - otro.coordenada[0];
        double dx = this.coordenada[1] - otro.coordenada[1];
    return Math.sqrt(dx*dx+dy*dy);
    }
}
```

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

- La convención casi universal en la Programación Orientada a Objetos es que los atributos deben ser privados y la interacción con ellos debe realizarse exclusivamente a través de métodos públicos. A este principio se le conoce como encapsulamiento u ocultación de la información.
- Privados siempre (como mencioné antes la POO dice q sean privados).
- Sí, las invariantes de clase son reglas que aseguran que el estado de un objeto sea válido. Al declarar los atributos como privados, se evita que el código externo los modifique libremente y rompa estas reglas.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

- Inmutable cuando su estado no puede cambiar una vez que el objeto ha sido creado.
- Un método modificador es aquel que modifica el estado interno de un objeto.
- No siempre es un setter, cualquier método que modifique atributos es un modificador
- Sí, al no poder cambiar, varios hilos pueden acceder al objeto simultáneamente sin riesgo de corrupción de datos o condiciones de carrera. Por no decir que son más simples y más fáciles para el debugging.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

- No, ya que rompen el encapsulamiento, dificultan la inmutabilidad y podría provocar que el objeto tenga valores invalidos.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

- La clase String en java es inmutable, por ende su contenido no puede ser modificado.
- Basicamente se reserva espacio para el objeto String, ahí se copia el contenido de la cadena A y el de la cadena B "jutandolas" con el operador + y el garbage collector se encarga de eliminar todo lo que no se usa.
- Concatenar demasiados objetos temporales en memoria es muy ineficaz. Por eso se suele usar mejor el StringBuilder ya que evita realizar todas esas copias en memoria.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

- Por identidad usando el operador == o por contenido usando el operador .equals().
- Es un método que todas las clases heredan de la clase base Object. Su propósito es permitir que el programador defina qué significa que dos objetos sean "iguales".
- La implementación original en la clase Object compara por identidad. Esto significa que, si no se sobrescribe, el método equals se comporta igual que el operador ==, devolviendo true solo si ambas referencias apuntan al mismo objeto físico en memoria.
- Los String siempre se deben comparar utilizando el método .equals(). Esto asegura que se compare el texto carácter por carácter y no la ubicación en memoria, ya que == podría devolver false.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

- En lenguajes como Java existen 2 tipos: Primitivos y los Wrapper, las cuales son objetos que encapsulan un tipo primitivo para que este pueda ser tratado como un objeto (Integer).
- Las estructuras de datos como ArrayList o HashMap solo pueden guardar objetos. No puedes hacer un ArrayList<int>, debes usar ArrayList<Integer>. También presenta el valor nulo como parte de ellos, a diferencia de los int que siempre que son creados se les asigna el 0 por defecto.
- No. Esta es una distinción de diseño (depende si es un lenguaje Híbrido o uno Puro).

Ventajas: Tienen métodos útiles, Pueden ser usados en contextos donde se necesiten objetos: Ej: List<Integer>

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

- Es un tipo cuyos valores son finitos y conocidos de antemano. Pero también es una clase.
- El uso de enums en Java potencia la encapsulación y la robustez del diseño de software, cuyas instancias son constantes predefinidas, estáticas y finales, permitiendo así incluir atributos, constructores y métodos.
- Los enumerados en Java potencian la encapsulación al permitir que las constantes actúen como objetos completos que agrupan su propio estado y comportamiento, restringiendo la creación de nuevas instancias mediante constructores privados y garantizando la seguridad de tipos en tiempo de compilación.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.
```java
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private int dias;
    private int ordinal;

    // Constructor del tipo enumerado
    Mes (int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }
}

```

## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`
```java
public enum Mes {
    ENERO(31, 1), FEBRERO(28, 2), MARZO(31, 3), ABRIL(30, 4),
    MAYO(31, 5), JUNIO(30, 6), JULIO(31, 7), AGOSTO(31, 8),
    SEPTIEMBRE(30, 9), OCTUBRE(31, 10), NOVIEMBRE(30, 11), DICIEMBRE(31, 12);

    private int dias;
    private int ordinal;

    Mes(int dias, int ordinal) {
        this.dias = dias;
        this.ordinal = ordinal;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinal() {
        return ordinal;
    }

    public boolean esDeInvierno(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO || this == MARZO;
        } else {
            return this == JUNIO || this == JULIO || this == AGOSTO || this == SEPTIEMBRE;
        }
    }

    public boolean esDePrimavera(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == MARZO || this == ABRIL || this == MAYO || this == JUNIO;
        } else {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE || this == DICIEMBRE;
        }
    }

    public boolean esDeVerano(boolean enHemisferioNorte) {
        // El verano en el norte coincide con el invierno en el sur
        return esDeInvierno(!enHemisferioNorte);
    }

    public boolean esDeOtoño(boolean enHemisferioNorte) {
        return esDePrimavera(!enHemisferioNorte);
    }
}
```