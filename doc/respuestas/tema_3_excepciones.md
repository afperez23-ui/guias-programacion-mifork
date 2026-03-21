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

-Devolviendo un valor "especial" o pasando un parametro por referencia para devolver el error.

Ej:
```C
float raiz (float num){
    if(num < 0.0>){
        return -1.0;
    }
    return sqrt(num);
}
```
## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

- Una excepción es una situación atípica. Errores de entrada (validación), de programación o de entorno (E/S).

- Objetivos para implementar funciones: *Validación de precondiciones* (Garantizar que la función solo opere con datos válidos. Si los argumentos son incorrectos, se "lanza" (throw) la excepción.), *Separación de lógica*(mezcla código lógico con errores manuales (retornar -1)) y Delegación de errores (Permite que la función informe sobre un fallo que no le renta resolver)

    try catch

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

```java
class Calculadora{
    static float raiz(float num){
        if(num < 0.0){
            throw new IlegalArgumentException("num negativo");
        }
        return Math.sqrt(num);
    }
}
class App{
    public static void main(String[] args){
        float num = leerDato();
        try{
            float resultado = Calculadora.raiz(num);
            sout("Raiz: "+ resultado);
        }catch(IlegalArgumentException){
            sout("No se pudo calcular");
        }
    }
}
```

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

- Lanzar una excepción significa generar un error durante la ejecución del programa para indicar que algo ha salido mal.
- Controlar una excepción es manejar el error usando try-catch para que el programa no se detenga y pueda reaccionar al problema.
- Cuando un método lanza una excepción pero no la captura.
- Cuando una excepción se propaga, va subiendo por la pila de llamadas (call stack) buscando un método que la capture con try-catch.
        Si un método no captura la excepción, termina inmediatamente.
        Se elimina de la pila de llamadas.
        La excepción se pasa al método que lo llamó.
        El proceso continúa hasta encontrar un catch o llegar al main

- No. Las funciones que no controlan (capturan) la excepción no se reanudan.
Cuando una excepción se lanza y no se captura en un método, ese método termina inmediatamente y se elimina de la pila de llamadas. La ejecución continúa en el primer catch que encuentre la excepción.

```java
public class Ejemplo {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException("Número negativo");
        }
        return Math.sqrt(x);
    }

    public static void calcular() {
        double r = raiz(-4);
        System.out.println("Resultado: " + r);
    }

    public static void main(String[] args) {
        try {
            calcular();
        } catch (IllegalArgumentException e) {
            System.out.println("Error: no se puede calcular la raíz de un número negativo");
        }
    }
}
```
## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

- Código más limpio y sencillo
No es necesario comprobar errores después de cada llamada a función (como en C con códigos de error).
La excepción se propaga automáticamente.

- Separación entre lógica y manejo de errores
El código principal del programa no se llena de comprobaciones de error; el tratamiento del error se pone en los bloques catch.

- Centralización del manejo de errores
Un método superior (por ejemplo main) puede encargarse de manejar el error para muchas funciones.

- Menos riesgo de olvidar comprobar errores
En C es fácil olvidar revisar un valor de retorno. Con excepciones, el error no pasa desapercibido porque se propaga.

- Mayor claridad y mantenimiento
El flujo del programa es más claro y el código es más fácil de mantener.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

- Sí. En Java, las excepciones son objetos que pertenecen a clases que heredan de Exception o Throwable.
- Al ser objetos, las excepciones pueden encapsular información sobre el error.
    pueden contener:

    un mensaje descriptivo
    datos sobre el error
    métodos para obtener esa información

    Esto permite:

    organizar mejor los errores
    transportar información detallada del problema
    manejar errores de forma más estructurada
    
- En Java podemos crear nuestras propias clases de excepción heredando de Exception o RuntimeException.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

- 3 elementos tiene toda excepción:
    - Mensaje: 
    - Traza de llamadas:
    - Opcionalmente, otra excepción de causa: 

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

- Sí, puede haber más de un catch, estos se ejecutará uno, el primero que cumpla el try. 
- Se va comprobando por orden de declaración del catch, el primero es el que se ejecuta. Se debe ordenar de más especifico a más general.
Ej:
```java
try{

}catch(AccesDeniedException e){

}catch(IOException e2)....
```

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.
```java
try{
    ...Files.readAllBytes(fichero);
}Finally{
    //se ejecuta SIEMPRE que haya entrado en el try
}
```
## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

- Sí
- Sí, el propósito de finally es garantizar la ejecución de código de limpieza
- Sí, el bloque finally se ejecuta incluso si hay un return. Siempre se ejecuta al entrar en try, pase lo que pase


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

- Las excepciones controladas: Errores típicos de causas externas y que siempre pueden llegar a ocurrir (E/S).
- Las excepciones no controladas: Errores de programación que una cez solventads no vuelven a ocurrir.


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

- throws es una palabra clave que se usa en la cabecera de un método para indicar que ese método puede lanzar una excepción.
- En Java, las excepciones controladas (checked exceptions) deben manejarse obligatoriamente de una de estas dos formas: try-catch y throws

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.
```java
public String leerFichero(Path p) throws IOException{

    try{

    }catch(IOException e){
        return null;
    }
}
```
## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Por poder se puede, el compilador no obliga a try catch/throws en el código llamador. Sería como un throws placebo 

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

-No hay ambas excepciones en todos los lenguajes.
-Las más típica es la de no controladas

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Sí, tiene sentido
Sí, se puede relanzar el mismo objeto excepción, tras por ejemplo haber ejecutado algo en catch


```java
//Lanzo la misma excepción
try{

}catch(IOSException e){
    .
    .
    .
    throw e;
}
//Lanzo otra nueva
try{


}catch(IOSException e ){

    throw new NetflluxException("error els");
}
```

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?
```java
try{

}catch(IOException e){
throw new NetfluxException("error els", e);
}
```