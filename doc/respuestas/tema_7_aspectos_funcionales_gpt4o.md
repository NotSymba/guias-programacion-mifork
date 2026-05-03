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

### Respuesta    
Un puntero a una función en el lenguaje C es una variable que almacena la dirección de memoria donde comienza el código ejecutable de una función. A diferencia de un puntero convencional que apunta a un dato (como un `int` o un `char`), este permite invocar una función de manera indirecta. Esta técnica es la base del polimorfismo en lenguajes de bajo nivel, permitiendo pasar comportamientos como argumentos a otras funciones, similar a cómo las interfaces funcionales operan en Java.

La declaración de un puntero a función requiere especificar el tipo de retorno y los tipos de los parámetros entre paréntesis, para que el compilador pueda verificar la firma durante la llamada. En términos de memoria, el nombre de una función actúa como una constante que apunta a su dirección de inicio, por lo que asignar una función a un puntero es una operación directa de asignación de direcciones.

A continuación, se presenta la implementación solicitada, donde se define una lógica para transformar una cadena y se utiliza un puntero local para ejecutarla:

```c
#include <stdio.h>
#include <ctype.h>

// Función que transforma la cadena a mayúsculas in-place
void transformarMayusculas(char *cadena) {
    while (*cadena) {
        *cadena = toupper((unsigned char)*cadena);
        cadena++;
    }
}

int main() {
    char texto[] = "hola mundo";

    // Declaración del puntero a función 'aMayusculas'
    // Recibe un char* y no devuelve nada (void)
    void (*aMayusculas)(char *) = transformarMayusculas;

    // Invocación de la función a través del puntero
    aMayusculas(texto);

    printf("Resultado: %s\n", texto);

    return 0;
}
```

La invocación mediante el puntero `aMayusculas(texto)` es semánticamente equivalente a llamar directamente a `transformarMayusculas(texto)`. En el contexto de los conocimientos de Java previos, este mecanismo de C es el ancestro conceptual de las expresiones lambda; mientras que en C se gestiona la dirección de memoria manualmente, en Java se encapsula ese comportamiento en objetos funcionales que garantizan la seguridad de tipos y la gestión de memoria.

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

### Respuesta    
Una **función lambda** es una función anónima, es decir, una función que se define sin un nombre identificador. Se trata de una unidad de código que puede ser tratada como una variable: puede ser asignada a una referencia, pasada como argumento a otros métodos o devuelta como resultado. Su propósito principal es permitir la escritura de código más conciso y facilitar la programación funcional, permitiendo inyectar comportamiento directamente allí donde se necesita.

A diferencia de las funciones convencionales o los punteros a función en C, las lambdas suelen tener la capacidad de "capturar" variables de su entorno léxico, un concepto conocido como clausura o *closure*. Mientras que en C se debe pasar explícitamente toda la información mediante parámetros, las lambdas en lenguajes modernos mantienen una referencia al contexto donde fueron creadas, lo que simplifica la estructura de los programas y reduce la necesidad de estados globales.

En **JavaScript**, la sintaxis de las funciones de flecha (*arrow functions*) permite definir estas funciones de forma extremadamente ligera. El siguiente ejemplo muestra cómo asignar una lambda a la variable `aMayusculas`:

```javascript
// Ejemplo en JavaScript
const aMayusculas = (cadena) => cadena.toUpperCase();

const resultado = aMayusculas("hola mundo");
console.log(resultado); // Imprime: HOLA MUNDO
```

En **Java**, para utilizar una función lambda se requiere de una interfaz funcional que defina el tipo. Como se solicita el uso de `Function<String, String>`, la lambda recibe un objeto de tipo `String` y devuelve otro, cumpliendo con la firma del método `apply` definido en dicha interfaz:

```java
import java.util.function.Function;

public class EjemploLambda {
    public static void main(String[] args) {
        // Ejemplo en Java
        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

        String resultado = aMayusculas.apply("hola mundo");
        System.out.println(resultado); // Imprime: HOLA MUNDO
    }
}
```

Es importante observar que en Java, al ser un lenguaje con tipado fuerte y basado en objetos, la variable `aMayusculas` no es un puntero directo a memoria como en C, sino una instancia de una clase generada en tiempo de ejecución que implementa la interfaz `Function`. La invocación no se realiza directamente sobre la variable, sino llamando al método `apply()`, lo que mantiene la coherencia con el modelo de objetos y polimorfismo previamente estudiado.</String,>

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

### Respuesta   
El **paradigma funcional** es un estilo de programación centrado en la evaluación de funciones matemáticas y en evitar el cambio de estado y la mutación de datos. A diferencia del paradigma imperativo (como C), donde se describe una serie de pasos para cambiar el estado del programa, en el funcional se construyen programas mediante la composición de funciones puras. Estas funciones tienen la propiedad de ser deterministas: para una misma entrada, siempre producirán la misma salida sin generar efectos secundarios, como modificar una variable global o un puntero externo.

Se afirma que lenguajes como Java 8 son **multi-paradigma** porque permiten combinar la Programación Orientada a Objetos (POO) con conceptos del mundo funcional. Históricamente, Java obligaba a que todo comportamiento residiera dentro de una clase (objetos), pero con la introducción de las lambdas y los Streams, se permite trabajar con lógica declarativa. Esto otorga al programador la flexibilidad de usar la herencia y el polimorfismo para estructurar el sistema, mientras emplea técnicas funcionales para procesar datos de forma más eficiente y legible.



La expresión **"ciudadanos de primera clase"** (*first-class citizens*) aplicada a las funciones significa que estas no están limitadas a ser meras definiciones de código, sino que reciben el mismo trato que cualquier otro valor o dato del lenguaje. En C, las funciones no son estrictamente ciudadanos de primera clase porque no se pueden crear funciones dinámicamente dentro de otras; solo se pueden manejar mediante punteros. En cambio, en un lenguaje donde las funciones son de primera clase, una función puede:
*   Ser asignada a una variable (como se hizo con `aMayusculas`).
*   Ser pasada como argumento a otra función.
*   Ser devuelta como el valor de retorno de otra función.

Este concepto es el pilar que sostiene la flexibilidad de las expresiones lambda en Java. Al tratar los comportamientos como ciudadanos de primera clase, el lenguaje permite abstraer operaciones comunes (como filtrar una lista o transformar un objeto) de una manera mucho más genérica y potente. Ya no se necesita crear una jerarquía de clases completa solo para pasar una pequeña lógica de comparación; basta con pasar la función directamente como si fuera un entero o un objeto convencional.

## 4. Explica la sintaxis básica de una función lambda en Java.

### Respuesta    
La sintaxis de una función lambda en Java está diseñada para ser minimalista, eliminando la necesidad de declarar modificadores de acceso, nombres de métodos o tipos de retorno de forma explícita. Se compone esencialmente de tres partes: una **lista de parámetros**, el operador de flecha o "token lambda" (`->`) y el **cuerpo de la función**. Esta estructura permite que el compilador de Java infiera la mayor parte de la información basándose en la interfaz funcional que la lambda está implementando.

Existen variaciones en la sintaxis dependiendo de la complejidad de la lógica. Si solo hay un parámetro, los paréntesis pueden omitirse; si el cuerpo de la función consta de una sola instrucción, las llaves y la sentencia `return` no son necesarias. No obstante, si la lógica requiere múltiples líneas, se deben utilizar llaves `{}` y especificar explícitamente el retorno si la función no es `void`. Esta flexibilidad permite pasar de una sintaxis muy compacta a una más detallada según la necesidad del código.



A continuación se presentan las variantes más comunes de esta sintaxis:

*   **Sin parámetros:** `() -> System.out.println("Hola");`
*   **Un parámetro (sin paréntesis):** `s -> s.length();`
*   **Varios parámetros (con tipos explícitos):** `(int a, int b) -> a + b;`
*   **Cuerpo con múltiples líneas:**
```java
(x, y) -> {
    int sum = x + y;
    return sum / 2;
};
```

Un aspecto técnico importante es que las lambdas en Java no definen un nuevo ámbito (*scope*) de variables, a diferencia de las clases anónimas. Esto significa que las variables definidas dentro de la lambda no pueden entrar en conflicto con las variables locales del método que las contiene. Además, para que una lambda pueda utilizar una variable local externa, dicha variable debe ser **final** o "efectivamente final" (que no cambie su valor después de la inicialización), lo que garantiza la seguridad y consistencia de los datos en entornos que podrían ser multihilo.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

### Respuesta    
Para implementar el paso de funciones como parámetros, se utiliza el concepto de **funciones de orden superior**. En este modelo, el método `transformar` no conoce la lógica específica que se aplicará al texto; simplemente define un contrato donde acepta una cadena y un comportamiento. Esto permite una gran reutilización de código, ya que el mismo método `transformar` podría usarse para convertir a mayúsculas, minúsculas o cifrar un texto, simplemente cambiando la función enviada.

En **JavaScript**, debido a su naturaleza dinámica, no es necesario definir interfaces. El parámetro `funcionTransformadora` se trata como cualquier otra variable y se invoca utilizando paréntesis. Esta flexibilidad es característica de los lenguajes donde las funciones han sido ciudadanos de primera clase desde sus versiones iniciales.

```javascript
// Ejemplo en JavaScript
const aMayusculas = (cadena) => cadena.toUpperCase();

// El método recibe la función como un parámetro más
function transformar(texto, funcionTransformadora) {
    return funcionTransformadora(texto);
}

const resultado = transformar("hola mundo", aMayusculas);
console.log(resultado); // Imprime: HOLA MUNDO
```

En **Java**, se debe especificar el tipo de la interfaz funcional en la firma del método. Al usar `Function<String, String>`, se indica que el método `transformar` espera cualquier objeto que cumpla con recibir un `String` y devolver otro. Dentro del método, la ejecución se realiza mediante el método `apply()`, que es el método abstracto definido en la interfaz `Function`.



```java
import java.util.function.Function;

public class EjemploParametro {
    public static void main(String[] args) {
        Function<String, String> aMayusculas = (s) -> s.toUpperCase();

        // Se pasa la variable que contiene la lambda al método
        String resultado = transformar("hola mundo", aMayusculas);
        System.out.println(resultado);
    }

    // Método que recibe la interfaz funcional como parámetro
    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }
}
```

Este enfoque en Java es una evolución natural de los conceptos de **polimorfismo** y **composición** ya conocidos. Mientras que en la POO clásica se pasaría un objeto de una clase que implementa una interfaz, aquí se pasa directamente la implementación del método. El resultado es un código más desacoplado, donde la estructura del programa (el método `transformar`) está separada de la lógica de negocio (la lambda).</String,>

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

### Respuesta   
La principal ventaja de las funciones lambda es la capacidad de definir lógica personalizada en el mismo instante en que se necesita, sin necesidad de crear una variable intermedia o una clase específica. Al pasar una lambda directamente como argumento, el código se vuelve mucho más expresivo y localizado, ya que la implementación de la transformación reside exactamente donde se invoca el método `transformar`.

En **JavaScript**, esto se logra escribiendo la función de flecha dentro de los paréntesis de la llamada. Para invertir una cadena en este lenguaje, es común convertir la cadena en un array de caracteres, invertir el array y volver a unirlo en una cadena única, todo ello en una sola línea de ejecución.

```javascript
// Invocación directa en JavaScript
const resultado = transformar("reconocer", (s) -> s.split("").reverse().join(""));

console.log(resultado); // Imprime: reconocer
```

En **Java**, el proceso es análogo. Se escribe la expresión lambda directamente en el lugar del segundo parámetro del método `transformar`. Dado que Java no tiene un método `reverse()` nativo en la clase `String` (por ser inmutable), se suele recurrir a la clase `StringBuilder`, que sí permite la manipulación eficiente de secuencias de caracteres y cuenta con el método de inversión.

```java
// Invocación directa en Java
String resultado = transformar("Java", (s) -> new StringBuilder(s).reverse().toString());

System.out.println(resultado); // Imprime: avaJ
```

Este estilo de programación reduce drásticamente el ruido sintáctico. Mientras que en C se tendría que definir una función externa con un nombre global y pasar su puntero, o en versiones antiguas de Java se tendría que crear una clase anónima interna con varias líneas de código, el paradigma funcional permite resolver la tarea con una sentencia concisa que mejora la legibilidad y el flujo del programa.

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

### Respuesta    
Un **cierre** o *closure* es una función que "recuerda" y tiene acceso al entorno o ámbito léxico en el que fue creada, incluso después de que ese entorno haya terminado su ejecución. En términos prácticos, permite que una función lambda capture y utilice variables que están definidas fuera de su propio cuerpo, manteniendo una referencia a esos valores externos. Esto crea una combinación de la lógica de la función junto con el estado de su entorno circundante.

En Java, aunque se permite esta captura de variables, existen restricciones importantes para garantizar la seguridad de la memoria y la consistencia en entornos multihilo. Una lambda solo puede acceder a variables locales del método que la contiene si estas son **final** o **efectivamente finales** (es decir, que su valor no se modifica después de ser asignadas). Si se intentara cambiar el valor de la variable capturada tanto dentro como fuera de la lambda, el compilador generaría un error, ya que Java necesita asegurar que el valor capturado sea predecible.

A continuación se muestra el ejemplo solicitado, donde la lambda utiliza una variable local llamada `prefijo` para realizar su transformación:

```java
import java.util.function.Function;

public class EjemploClosure {
    public static void main(String[] args) {
        // Variable local en el contexto del método main
        String prefijo = "Resultado: ";

        // La lambda "captura" la variable 'prefijo' de su entorno
        Function<String, String> concatenarPrefijo = (entrada) -> prefijo + entrada;

        // Se invoca a través del método transformar
        String resultado = transformar("éxito", concatenarPrefijo);
        System.out.println(resultado); // Imprime: Resultado: éxito
    }

    public static String transformar(String texto, Function<String, String> funcion) {
        return funcion.apply(texto);
    }
}
```

Este mecanismo es extremadamente útil para configurar comportamientos personalizados sin necesidad de pasar múltiples parámetros extra. Al "encerrar" la variable `prefijo` dentro de la función `concatenarPrefijo`, la lambda se convierte en una unidad de computación autónoma que lleva consigo la información necesaria para ejecutarse, simplificando la firma de los métodos y mejorando la modularidad del código.

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

### Respuesta   
A pesar de que ambos conceptos comparten el objetivo de tratar el comportamiento como un dato, la diferencia fundamental reside en el nivel de **abstracción y seguridad**. En C, un puntero a función es una dirección de memoria "desnuda" que apunta al segmento de código; el programador es responsable de asegurar que la firma sea correcta y de gestionar el estado manualmente. En cambio, una lambda en Java es un objeto de alto nivel que encapsula no solo el código, sino también el contexto que lo rodea, garantizando la seguridad de tipos mediante el sistema de interfaces funcionales.

Otra distinción crucial es la capacidad de **clausura (closure)**. Como se ha analizado, las lambdas pueden capturar variables de su entorno léxico (variables locales "finales"), mientras que un puntero a función en C no tiene un estado interno ni acceso automático a las variables del ámbito donde fue referenciado; para lograr algo similar en C, se debe pasar manualmente una estructura de datos adicional o usar variables globales, lo que aumenta la complejidad y el riesgo de errores.

Desde el punto de vista de la **gestión de memoria**, la diferencia es notable. En C, el puntero a función simplemente apunta a una dirección que existe durante toda la ejecución del programa (el segmento de texto). En Java, la lambda puede implicar la creación de una instancia en el *heap* (montículo) que es gestionada por el Recolector de Basura (*Garbage Collector*). Esto permite que la lambda sobreviva al método que la creó, manteniendo las variables capturadas a salvo, algo que con punteros a memoria local en C provocaría un comportamiento indefinido.

Finalmente, la **sintaxis y el propósito** marcan una distancia clara. Mientras que los punteros en C se perciben como una herramienta técnica de bajo nivel para implementar *callbacks* o tablas de despacho, las lambdas en Java están integradas en una API orientada al procesamiento de datos (como los Streams). La lambda no solo busca ejecutar código de forma indirecta, sino promover un estilo de programación declarativo que sea más legible, menos propenso a errores de segmentación y más fácil de paralelizar.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

### Respuesta    
La capacidad de devolver funciones desde otros métodos es una de las características más potentes del paradigma funcional, transformando a los métodos en **fábricas de comportamiento**. En Java, esto se logra definiendo un método cuyo tipo de retorno sea una interfaz funcional, como `Function<Double, Double>`. En este escenario, el método `crearDescuento` no realiza el cálculo, sino que construye y entrega una "receta" (la lambda) que sabe cómo aplicar el porcentaje específico que se le indicó al momento de su creación.

El fenómeno de la **closure** es aquí el protagonista absoluto. Cuando el método `crearDescuento` termina su ejecución, el parámetro `porcentaje` normalmente debería desaparecer de la pila de memoria. Sin embargo, la función lambda que se devuelve "atrapa" dicho valor y lo mantiene vivo dentro de su propia estructura. De este modo, cuando la función descuento se invoca mucho más tarde, todavía tiene acceso al porcentaje con el que fue configurada, actuando como una función con memoria interna pero sin necesidad de usar variables globales o atributos de clase tradicionales.

A continuación se muestra la implementación de esta lógica y su aplicación práctica:

```java
import java.util.function.Function;

public class FabricaDescuentos {
    public static void main(String[] args) {
        // Se crean dos funciones distintas con porcentajes diferentes
        Function<Double, Double> descuentoDiez = crearDescuento(10.0);
        Function<Double, Double> descuentoCincuenta = crearDescuento(50.0);

        double precioBase = 100.0;

        // Se aplican las funciones creadas
        System.out.println("Precio con 10%: " + descuentoDiez.apply(precioBase));
        System.out.println("Precio con 50%: " + descuentoCincuenta.apply(precioBase));
    }

    // Método que devuelve una función (ciudadano de primera clase)
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // El parámetro 'porcentaje' queda atrapado en el cierre de la lambda
        return (cantidad) -> cantidad - (cantidad * (porcentaje / 100));
    }
}
```

En este ejemplo, `descuentoDiez` y `descuentoCincuenta` son técnicamente la misma lógica de código, pero poseen **entornos capturados diferentes**. Mientras que en C se necesitaría crear una estructura que almacenara el porcentaje y pasarla junto al puntero a la función en cada llamada, Java gestiona esta persistencia del contexto de forma transparente. Esto permite escribir código mucho más limpio y modular, donde se pueden generar infinitas variaciones de una misma operación simplemente capturando distintos estados del entorno.</Double,>

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

### Respuesta   
Una **interfaz funcional** en Java es una interfaz que actúa como el "tipo" de una expresión lambda o de una referencia a un método. Dado que Java es un lenguaje de tipado estático, el compilador necesita conocer la firma (parámetros y retorno) de cualquier bloque de código antes de ejecutarlo. La interfaz funcional proporciona este molde formal, permitiendo que una función anónima sea tratada como un objeto legal dentro del sistema de tipos de Java.

Para que una interfaz sea considerada funcional, el requisito técnico indispensable es que debe tener **exactamente un método abstracto**. Este método único, conocido como el *método funcional*, es el que define la firma que la lambda debe cumplir. Si una interfaz tuviera dos o más métodos abstractos, el compilador no sabría qué método se está intentando implementar con la expresión lambda, lo que resultaría en un error de ambigüedad.

A pesar de esta restricción de "un solo método", una interfaz funcional puede contener otros elementos sin perder su naturaleza:
*   **Métodos predeterminados (`default`):** Al tener implementación, no cuentan como abstractos.
*   **Métodos estáticos:** Pertenecen a la interfaz y no requieren una instancia.
*   **Métodos de la clase `Object`:** Si una interfaz redefine un método público de `Object` (como `equals` o `toString`) de forma abstracta, este tampoco cuenta para el límite de uno.

Es una práctica recomendada marcar estas interfaces con la anotación **`@FunctionalInterface`**. Aunque no es obligatoria para que el código funcione, actúa como un contrato de seguridad: si un programador intenta añadir un segundo método abstracto por error, el compilador generará un aviso inmediato. Esta estructura permite que Java mantenga su rigurosidad de tipos clásica mientras se beneficia de la flexibilidad del paradigma funcional.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

### Respuesta    
Para definir una interfaz funcional manualmente, se debe declarar una interfaz estándar de Java que contenga un único método abstracto. En este caso, la interfaz `Transformador` actuará como el contrato formal que especifica que cualquier implementación (ya sea mediante una clase, una clase anónima o una lambda) debe recibir un objeto `String` y devolver otro `String`. Esta estructura es la que permite al compilador de Java realizar la comprobación de tipos durante la asignación de una expresión lambda.

Aunque el compilador reconoce cualquier interfaz con un solo método abstracto como funcional, se utiliza la anotación `@FunctionalInterface` para expresar la intención de diseño de forma explícita. Esto evita que, en el futuro, otros desarrolladores añadan accidentalmente más métodos abstractos, lo que rompería todas las expresiones lambda que dependan de esta interfaz. Es, en esencia, una salvaguarda de arquitectura.

A continuación se muestra la definición de la interfaz y su aplicación práctica:

```java
@FunctionalInterface
public interface Transformador {
    // El único método abstracto que define la firma de la lambda
    String transformar(String entrada);
}
```

Para utilizar esta interfaz en lugar de la genérica `Function<String, String>`, simplemente se cambia el tipo de la referencia. El funcionamiento interno sigue siendo el mismo, pero el código gana en semántica y claridad al usar nombres de dominio específicos:

```java
public class EjemploInterfazPropia {
    public static void main(String[] args) {
        // Uso de nuestra propia interfaz funcional
        Transformador conversor = (s) -> s.toLowerCase();
        
        String resultado = ejecutarTransformacion("HOLA", conversor);
        System.out.println(resultado); // Imprime: hola
    }

    public static String ejecutarTransformacion(String t, Transformador tform) {
        return tform.transformar(t);
    }
}
```

Esta capacidad de crear interfaces propias es fundamental cuando se desea que el código sea más descriptivo. Mientras que `Function` es muy genérico, una interfaz llamada `Transformador`, `Validador` o `FiltroDeSeguridad` comunica mucho mejor la intención del método que la recibe, mejorando la legibilidad del sistema sin perder la potencia del paradigma funcional.</String,>

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

### Respuesta    
Para que una interfaz sea verdaderamente flexible, se deben emplear **parámetros de tipo (generics)**. En lugar de limitar la interfaz a trabajar exclusivamente con `String`, se definen tipos genéricos (comúnmente representados como `<T, R>`) que actúan como marcadores de posición. Esto permite que el mismo molde estructural sirva para cualquier tipo de transformación: de cadena a entero, de doble a cadena, o incluso entre objetos de clases personalizadas, manteniendo la seguridad de tipos en tiempo de compilación.

La sintaxis genérica traslada la responsabilidad de definir los tipos concretos al momento de la declaración de la variable. El primer parámetro de tipo suele representar la **entrada** ($T$) y el segundo el **resultado** ($R$). Al utilizar esta aproximación, la interfaz funcional se vuelve universal y se asemeja al comportamiento de `Function<T, R>` de la biblioteca estándar de Java, pero bajo un nombre que puede ser más significativo para el dominio del problema que se esté resolviendo.

A continuación se muestra la interfaz genérica y su aplicación para transformar un `Double` en un `Integer`:

```java
@FunctionalInterface
public interface TransformadorGenerico<T, R> {
    // Recibe un tipo T y devuelve un tipo R
    R transformar(T entrada);
}

public class EjemploGenericos {
    public static void main(String[] args) {
        // Se define un transformador que recibe Double y devuelve Integer
        TransformadorGenerico<Double, Integer> redondear = (d) -> (int) Math.round(d);

        Integer resultado = redondear.transformar(15.75);
        System.out.println("Resultado del redondeo: " + resultado); // Imprime: 16
    }
}
```

En este ejemplo, el uso de genéricos permite que el compilador verifique que el valor devuelto por la lambda (`int`) sea compatible con el tipo esperado (`Integer`) mediante el proceso de *autoboxing*. Si se intentara asignar una lambda que devuelve un `String` a esta referencia, el código no compilaría. Esta es la principal ventaja sobre el lenguaje C: se obtiene la flexibilidad de los punteros a función pero con un control estricto sobre qué datos entran y salen, evitando errores de interpretación de memoria en tiempo de ejecución.</T,></T,>

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

### Respuesta    
Efectivamente, la interfaz `TransformadorGenerico<T, R>` es funcionalmente idéntica a `java.util.function.Function<T, R>`. Java 8 introdujo un robusto paquete de interfaces predefinidas para cubrir los casos de uso más comunes, evitando que el desarrollador tenga que definir sus propias interfaces para tareas genéricas. Estas se clasifican principalmente por la cantidad de argumentos que reciben y si devuelven o no un resultado.

Las cuatro interfaces funcionales base que todo desarrollador de Java debe conocer son:

*   **`Function<T, R>`**: Representa una función que acepta un argumento de tipo $T$ y produce un resultado de tipo $R$. Se utiliza para transformaciones.
*   **`Predicate<T>`**: Acepta un argumento de tipo $T$ y devuelve un valor booleano (`boolean`). Es ideal para filtros o validaciones.
*   **`Consumer<T>`**: Acepta un argumento de tipo $T$ y no devuelve nada (`void`). Se emplea para realizar acciones o efectos colaterales, como imprimir en consola.
*   **`Supplier<T>`**: No recibe argumentos y devuelve un resultado de tipo $T$. Es útil para la creación de objetos o el suministro de datos bajo demanda.

Además de estas cuatro básicas, existen variantes para casos específicos. Por ejemplo, las versiones **Bi** (como `BiFunction<T, U, R>`) permiten manejar dos argumentos de entrada. También existen versiones especializadas para tipos primitivos, como `IntPredicate` o `DoubleFunction`, que evitan el coste de rendimiento del *autoboxing* al trabajar directamente con tipos básicos de datos en lugar de sus clases envoltorias (`Integer`, `Double`, etc.).

| Interfaz | Firma del método | Propósito |
| :--- | :--- | :--- |
| `Predicate<T>` | `boolean test(T t)` | Evaluar una condición |
| `Function<T, R>` | `R apply(T t)` | Transformar un dato |
| `Consumer<T>` | `void accept(T t)` | Consumir/Procesar un dato |
| `Supplier<T>` | `T get()` | Proveer un dato |
| `UnaryOperator<T>` | `T apply(T t)` | Transformación donde entrada y salida son del mismo tipo |

Esta estandarización permite que las librerías de Java (como la API de Streams) sean altamente interoperables. Al usar estas interfaces predefinidas en lugar de crear las nuestras, nos aseguramos de que cualquier otra parte del ecosistema de Java pueda entender y utilizar nuestras funciones lambda sin necesidad de adaptadores adicionales.

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

### Respuesta    
El método `forEach` es una de las adiciones más significativas de Java 8 a la interfaz `Iterable`. Representa el paso de un bucle **externo** (donde el programador controla manualmente el iterador o el índice, como en los bucles `for` de C) a un bucle **interno**. En este modelo, el programador simplemente le entrega a la colección una acción que debe ejecutar para cada elemento, y es la propia lista la que se encarga de la gestión de la iteración.

Desde el punto de vista técnico, `forEach` recibe como parámetro una interfaz funcional de tipo `Consumer<T>`. Como se analizó anteriormente, un `Consumer` es una función que acepta un valor y no devuelve ningún resultado, lo que encaja perfectamente con la tarea de realizar efectos secundarios, como imprimir en la consola o modificar el estado de un objeto externo.

A continuación, se muestra cómo utilizar `forEach` para filtrar visualmente los números positivos de una lista de enteros:

```java
import java.util.Arrays;
import java.util.List;

public class EjemploForEach {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-5, 10, 0, 25, -2, 8);

        // Se emplea forEach con una lambda que actúa como Consumer
        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("El número " + n + " es positivo.");
            }
        });
    }
}
```

Esta forma de iterar es mucho más limpia y menos propensa a los típicos errores de "fuera de rango" (*out of bounds*) que suelen ocurrir en C al manejar índices. Además, al delegar la iteración en la propia colección, Java tiene la libertad de optimizar el proceso internamente. Es el primer paso para pensar de forma declarativa: ya no se escribe "recorre esta lista desde 0 hasta N e imprime", sino "para cada elemento de esta lista, si cumple la condición, imprímelo".

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

### Respuesta   
El uso de `<? super T>` en la firma de `forEach` responde al principio de **contravarianza**. En Java, cuando trabajas con genéricos, una `List<String>` no es una subclase de `List<Object>`. Sin embargo, si tienes una lista de cadenas, debería ser perfectamente legal pasarle un `Consumer` que sepa manejar objetos genéricos (`Object`), ya que cualquier cosa que se haga con un `Object` se puede hacer de forma segura con un `String`. Al usar `? super T`, Java permite que el consumidor sea de un tipo más general que los elementos de la lista, dotando al código de una flexibilidad superior.

El acrónimo **PECS** significa **Producer Extends, Consumer Super**. Es una regla mnemotécnica para recordar cuándo usar cada comodín (*wildcard*): si una estructura "produce" datos para que tú los uses, emplea `<? extends T>` (covarianza); si la estructura "consume" datos que tú le envías, emplea `<? super T>` (contravarianza). En el caso de `forEach`, la interfaz `Consumer` está "consumiendo" elementos de la lista para procesarlos, por lo que se aplica la parte **CS** (Consumer Super) de la regla.

Si aplicamos este concepto para mejorar el método `transformar` que definimos anteriormente, la firma debería evolucionar para ser lo más flexible posible. En una transformación, la función **consume** una entrada de la lista y **produce** un resultado. Por lo tanto, para que el método acepte una función que trabaje con tipos más genéricos en la entrada o tipos más específicos en la salida, la firma ideal sería:

```java
public static <T, R> R transformar(T entrada, Function<? super T, ? extends R> funcion) {
    return funcion.apply(entrada);
}
```

Esta mejora permite, por ejemplo, que si tienes un `Integer`, puedas pasarle una función que acepte `Number` (el supertipo de Integer). Gracias a `? super T`, la función es capaz de procesar la entrada porque un `Integer` *es un* `Number`. Por otro lado, `? extends R` asegura que el resultado de la función sea compatible con el tipo de retorno esperado. En resumen, **PECS** es la herramienta que permite que el polimorfismo que ya conocías de la herencia clásica funcione correctamente dentro de las estructuras genéricas y funcionales de Java.</Object></String>

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

### Respuesta    
Las **referencias a métodos** son una forma simplificada de escribir expresiones lambda que ya existen en un método definido. En lugar de encapsular una llamada a un método dentro de una lambda (como `x -> objeto.metodo(x)`), el paradigma funcional permite apuntar directamente al método por su nombre. Esto no solo hace que el código sea más legible, sino que refuerza la idea de que los métodos son comportamientos que pueden ser transportados y almacenados.

En **JavaScript**, obtener una referencia a un método es muy directo, ya que las funciones son objetos. Sin embargo, hay un detalle técnico crítico: el contexto de `this`. Si se extrae un método de un objeto para guardarlo en una variable local, este pierde su enlace con la instancia original. Para evitar que el nombre de la persona sea `undefined`, es necesario utilizar el método `bind`, que "ancla" permanentemente la función a su objeto.

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }
    saludar() {
        console.log(`Hola, soy ${this.nombre}`);
    }
}

const p = new Persona("Carlos");
// Obtenemos la referencia vinculándola al objeto 'p'
const saludarReferencia = p.saludar.bind(p);

saludarReferencia(); // Imprime: Hola, soy Carlos
```

En **Java**, se utiliza una sintaxis especial con el operador de doble dos puntos (`::`). A diferencia de JavaScript, Java gestiona internamente el contexto de la instancia cuando se crea la referencia. Para almacenar esta referencia, se debe utilizar una interfaz funcional que coincida con la firma del método; en este caso, como `saludar` no recibe parámetros ni devuelve nada, la interfaz ideal es `Runnable`.

```java
public class Persona {
    private String nombre;
    public Persona(String nombre) { this.nombre = nombre; }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }

    public static void main(String[] args) {
        Persona p = new Persona("Elena");

        // Referencia al método de una instancia específica
        Runnable saludarReferencia = p::saludar;

        // Invocación a través de la referencia
        saludarReferencia.run(); // Imprime: Hola, soy Elena
    }
}
```

Esta técnica es especialmente útil en el desarrollo moderno de Java. Permite pasar comportamientos existentes a métodos de orden superior (como `forEach` o `map`) de una forma extremadamente limpia. Mientras que en C usarías un puntero a función apuntando a una dirección estática, en Java las referencias a métodos permiten capturar el comportamiento de una **instancia particular**, manteniendo toda la potencia de la orientación a objetos y la seguridad del tipado.

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

### Respuesta   
En Java, las referencias a métodos proporcionan una forma aún más compacta que las lambdas para invocar comportamientos ya definidos. Se pueden clasificar en cuatro categorías principales, dependiendo de si el método pertenece a una clase (estático), a un objeto ya creado (instancia concreta), a un objeto que se recibirá como parámetro (instancia arbitraria) o si se trata de la creación de un nuevo objeto (constructor).

Estas referencias utilizan el operador `::` y son interpretadas por el compilador como una implementación de una interfaz funcional compatible. Es una técnica que acerca a Java a la elegancia de los lenguajes funcionales puros, eliminando el ruido visual de los parámetros cuando estos solo se pasan de un lado a otro sin transformación.

A continuación se presentan ejemplos de cada tipo:

---

### 1. Referencia a método estático
Se apunta a un método que pertenece a la clase y no requiere una instancia.
*   **Sintaxis:** `Clase::metodoEstatico`
*   **Ejemplo:** `Function<Double, Double> raiz = Math::sqrt;`
    *(Equivale a `d -> Math.sqrt(d)`)

### 2. Referencia a método de instancia de un objeto concreto
Se vincula el comportamiento a un objeto que ya existe en el código.
*   **Sintaxis:** `instancia::metodoDeInstancia`
*   **Ejemplo:**
    ```java
    String prefijo = "Hola: ";
    Consumer<String> imprimidor = System.out::println;
    // Se usa el objeto 'out' de la clase System
    ```

### 3. Referencia a método de instancia de un objeto arbitrario de un tipo
Aquí el método no está vinculado a un objeto previo, sino que el primer parámetro de la interfaz funcional se convierte en el "receptor" del método.
*   **Sintaxis:** `Clase::metodoDeInstancia`
*   **Ejemplo:** `Function<String, Integer> obtenerLongitud = String::length;`
    *(Equivale a `(String s) -> s.length()`. El método se ejecuta sobre el objeto que reciba la función).*

### 4. Referencia a un constructor
Permite utilizar la creación de objetos como si fuera una fábrica de datos.
*   **Sintaxis:** `Clase::new`
*   **Ejemplo:** `Supplier<List<String>> creadorDeListas = ArrayList::new;`
    *(Equivale a `() -> new ArrayList<>()`)*.

---

Esta clasificación es vital para entender la API de Streams. Por ejemplo, es muy común ver `lista.stream().map(String::toUpperCase).collect(Collectors.toList());`, donde se combinan una referencia a método de instancia arbitraria (`toUpperCase`) con una referencia a método estático (`toList`).

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

### Respuesta    
La ordenación de colecciones en Java ha evolucionado desde la implementación de interfaces pesadas hacia un modelo fluido y funcional. En el fondo, cualquier ordenación requiere un `Comparator`, una interfaz funcional cuyo método `compare(T o1, T o2)` devuelve un entero negativo, cero o positivo según el orden relativo de los objetos. Este mecanismo es conceptualmente idéntico a las funciones de comparación que se pasan a `qsort` en C, pero con la ventaja de la seguridad de tipos de los genéricos.

En la primera versión, se escribe la lógica de comparación manualmente dentro de una expresión lambda. Este enfoque es directo y muestra claramente el proceso de decisión: primero se comparan las edades y, solo si estas son iguales, se procede a comparar los nombres utilizando el método `compareTo` de la clase `String`. Es una estructura de control anidada que resuelve la jerarquía de ordenación de forma explícita.

```java
// Versión 1: Comparación manual con lambda
Collections.sort(personas, (p1, p2) -> {
    int res = Integer.compare(p1.getEdad(), p2.getEdad());
    if (res == 0) {
        res = p1.getNombre().compareTo(p2.getNombre());
    }
    return res;
});
```

La segunda versión aprovecha los métodos de fábrica de la interfaz `Comparator`. Este estilo es mucho más declarativo y legible, ya que se "construye" el comparador encadenando reglas. Se utiliza `Comparator.comparingInt` para la clave primaria (la edad) y se encadena con `thenComparing` para la clave secundaria (el nombre). Para maximizar la expresividad, se emplean **referencias a métodos**, lo que permite leer el código casi como lenguaje natural: "comparar por edad y luego comparar por nombre".

```java
// Versión 2: Empleando métodos estáticos de Comparator y referencias a métodos
personas.sort(Comparator.comparingInt(Persona::getEdad)
                        .thenComparing(Persona::getNombre));
```

Esta segunda forma no solo es más breve, sino que es menos propensa a errores lógicos. Al usar los métodos de `Comparator`, se delega la gestión de las comparaciones individuales y los posibles valores nulos a la API estándar. Además, fíjate que en la segunda versión se utiliza `personas.sort()` directamente en lugar de `Collections.sort()`, un método incorporado en la interfaz `List` desde Java 8 que acepta el comparador directamente, siguiendo la tendencia de mover la lógica hacia las propias colecciones.
