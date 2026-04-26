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

### Respuesta    
Para implementar una estructura de datos capaz de almacenar cualquier tipo de elemento sin utilizar genericidad, se debe recurrir al tipo de dato más general referencial disponible en el lenguaje. En el caso de C, esto se logra mediante un arreglo de punteros genéricos (`void*`), los cuales pueden apuntar a cualquier dirección de memoria perdiendo la información del tipo original. En Java, aprovechando los conceptos de herencia y polimorfismo, se utiliza un arreglo de la clase `Object`, dado que absolutamente todas las clases en Java heredan directa o indirectamente de esta superclase raíz.

A continuación, se presenta un ejemplo en Java de una estructura básica que envuelve un arreglo primitivo de tipo `Object` para alojar y recuperar distintos tipos de datos:

```java
public class ListaUniversal {
    private Object[] elementos;
    private int indiceActual;

    // Constructor que inicializa el array con un tamaño fijo
    public ListaUniversal(int capacidad) {
        this.elementos = new Object[capacidad];
        this.indiceActual = 0;
    }

    // Método que permite agregar cualquier objeto
    public void agregar(Object elemento) {
        if (indiceActual < elementos.length) {
            elementos[indiceActual] = elemento;
            indiceActual++;
        }
    }

    // Método para recuperar un objeto por su posición
    public Object obtener(int posicion) {
        if (posicion >= 0 && posicion < indiceActual) {
            return elementos[posicion];
        }
        return null;
    }
}

// Ejemplo de uso:
// ListaUniversal miLista = new ListaUniversal(5);
// miLista.agregar("Una cadena de texto"); // Se guarda un String
// miLista.agregar(Integer.valueOf(42));   // Se guarda un Integer
```

El funcionamiento de esta estructura radica en que el método `agregar` acepta cualquier instancia de clase, realizando una conversión ascendente implícita (*upcasting*) hacia `Object`. Esto permite que en el mismo arreglo interno convivan en memoria distintos tipos de objetos. El arreglo primitivo subyacente simplemente almacena referencias polimórficas genéricas a los objetos originales. 

Sin embargo, el principal inconveniente de este enfoque se manifiesta al momento de recuperar la información. Como el método `obtener` devuelve una referencia de tipo `Object`, el compilador desconoce por completo la naturaleza original del elemento. Para poder utilizar los métodos específicos del objeto almacenado (por ejemplo, obtener la longitud del `String` guardado), es estrictamente obligatorio realizar una conversión descendente explícita (*casting*). Si por error del programador se realiza un *casting* hacia un tipo incorrecto, el compilador lo permitirá, pero se producirá una excepción en tiempo de ejecución que detendrá el programa.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

### Respuesta    
La **programación genérica** es un paradigma que consiste en desarrollar algoritmos y estructuras de datos independientes del tipo de dato específico sobre el que van a operar. En lugar de escribir múltiples versiones de una misma función o clase para manejar enteros, cadenas de texto o estructuras complejas, se define una única implementación parametrizada. De esta forma, el tipo exacto se suministra posteriormente al momento de instanciar la clase o invocar el método, lo que permite reutilizar el mismo código manteniendo una estricta verificación de tipos durante la fase de compilación.

Respecto a la segunda cuestión, el ejemplo presentado en la respuesta anterior **no se considera un verdadero caso de programación genérica**. Aunque la estructura `ListaUniversal` logra el propósito de almacenar y manejar distintas clases de información, el mecanismo que emplea se basa puramente en el **polimorfismo** (al utilizar la superclase raíz `Object` en Java) o en la evasión del sistema de tipos (al usar `void*` en C). La distinción fundamental radica en que estos métodos ocultan la identidad original del dato, lo que obliga al programador a realizar conversiones explícitas (*castings*) para recuperar la funcionalidad del objeto, exponiendo el programa a errores en tiempo de ejecución.

Por consiguiente, la auténtica programación genérica se caracteriza por no perder nunca la información del tipo de dato introducido. Al emplear el mecanismo formal de genericidad (como los parámetros de tipo `<T>` en Java introducidos en versiones posteriores), el compilador tiene el conocimiento absoluto de lo que almacena cada estructura. Esto garantiza, por ejemplo, que una lista instanciada específicamente para alojar texto rechace automáticamente cualquier intento de guardar un valor numérico, proporcionando una seguridad que el uso tradicional de `Object` o `void*` no puede ofrecer.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

### Respuesta    
El principal problema respecto al chequeo de tipos al emplear `void*` en C o `Object` en Java es la pérdida total de la información original del tipo de dato durante la fase de compilación. Al utilizar estas abstracciones universales, el mecanismo de verificación del compilador se anula en la práctica, ya que este asume que la estructura simplemente almacena un elemento genérico. Esto impide detectar errores de consistencia en el momento de compilar el código, permitiendo, por ejemplo, que se inserte accidentalmente un valor numérico en una estructura que lógicamente estaba destinada a contener únicamente texto.

Como consecuencia directa de esta falta de verificación previa, surge la obligación de realizar conversiones de tipo explícitas (*castings*) cada vez que se extrae un elemento de la estructura para poder utilizar sus métodos o propiedades. Si la suposición sobre el tipo de dato recuperado es incorrecta, el compilador no emitirá ninguna advertencia, trasladando el fallo directamente al tiempo de ejecución. En entornos como C, esto suele derivar en comportamientos indefinidos o errores críticos de acceso a memoria (*segmentation fault*), mientras que en Java se manifestará mediante el lanzamiento de una excepción `ClassCastException`, lo cual detendrá la ejecución del programa si no es capturada y tratada.

Adicionalmente, esta carencia de chequeo estricto de tipos perjudica de forma notable la legibilidad y el mantenimiento del código fuente. La necesidad constante de forzar conversiones al recuperar los datos ensucia la lógica del algoritmo con operaciones repetitivas y redundantes. Asimismo, obliga a mantener un control riguroso —y altamente propenso a errores humanos— sobre qué clase de información exacta se ha insertado en cada lugar, una responsabilidad que en la verdadera programación genérica es asumida de forma automática y segura por el propio lenguaje.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

### Respuesta    
Los parámetros de tipo son el mecanismo fundamental que hace posible la verdadera programación genérica en lenguajes como Java. Así como una función en C o C++ recibe variables convencionales (parámetros de valor) para operar con distintos datos sin necesidad de reescribir el algoritmo, una clase o método genérico recibe **parámetros de tipo**. Esto significa que, en lugar de pasar un número o un carácter, se suministra una definición de tipo completa (como la clase `String` o `Integer`). De este modo, la estructura abstracta se configura lógicamente para trabajar de forma exclusiva con esa clase específica.

Sintácticamente, estos parámetros se declaran inmediatamente después del nombre de la clase o método utilizando corchetes angulares, y se representan habitualmente con letras mayúsculas simples por convención (como `<T>` para "Tipo", `<E>` para "Elemento" o `<K, V>` para "Clave y Valor"). Al definir una estructura con un parámetro `<T>`, se le está indicando al compilador que `T` actuará como un identificador temporal. Cuando la clase se instancia proporcionando un tipo real, el compilador trata todas las apariciones de `T` dentro de esa instancia como si fueran el tipo definitivo proporcionado.

La introducción de los parámetros de tipo resuelve de raíz los problemas derivados del uso de `void*` u `Object`. Al sustituir la generalidad absoluta por un tipo concretado en la instanciación, el compilador recupera la capacidad de realizar una verificación estricta (*type safety*). Si se define una lista con el parámetro de tipo `<Integer>`, cualquier intento de introducir texto provocará un error inmediato en la compilación. Consecuentemente, al extraer los datos, el sistema ya sabe con absoluta certeza su naturaleza original, eliminando por completo la necesidad de realizar conversiones explícitas (*castings*).

```java
// Definición usando un parámetro de tipo <T>
public class CajaGenerica<T> {
    private T contenido; // 'T' tomará el valor del tipo especificado

    public void meter(T nuevoContenido) {
        this.contenido = nuevoContenido;
    }

    public T sacar() {
        return this.contenido;
    }
}

// Uso práctico de los parámetros de tipo
public class Programa {
    public static void main(String[] args) {
        // Se sustituye el parámetro <T> por el tipo real <String>
        CajaGenerica<String> cajaDeTexto = new CajaGenerica<>();
        
        cajaDeTexto.meter("Solo admite texto");
        // cajaDeTexto.meter(100); // <- Esto daría error de compilación
        
        // No se requiere 'casting', el compilador sabe que devuelve un String
        String mensaje = cajaDeTexto.sacar(); 
    }
}
```

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

### Respuesta   
Aunque los mecanismos internos difieren profundamente —C++ utiliza *templates* que generan nuevo código compilado específico para cada tipo, mientras que Java emplea "borrado de tipos" (*type erasure*) manteniendo una única clase compilada—, ambos lenguajes permiten implementar estructuras de datos dinámicas con total seguridad de tipos. Al instanciar estas colecciones indicando el tipo concreto, el compilador asume el trabajo de verificación, garantizando que solo se inserten los elementos permitidos y evitando inconsistencias y conversiones manuales.

```cpp
// Ejemplo en C++ usando templates (std::vector)
#include <iostream>
#include <vector>
#include <string>

int main() {
    // Instanciación de un vector dinámico que solo admite strings
    std::vector<std::string> listaCadenas;

    listaCadenas.push_back("Primera cadena en C++");
    listaCadenas.push_back("Segunda cadena en C++");
    // listaCadenas.push_back(100); // El compilador bloquearía esta línea

    // Recorrido seguro mostrando una propiedad específica (length)
    for (size_t i = 0; i < listaCadenas.size(); i++) {
        std::string elemento = listaCadenas[i];
        std::cout << "Texto: " << elemento 
                  << " (Longitud: " << elemento.length() << ")" << std::endl;
    }
    
    return 0;
}
```

En el ejemplo de C++, se hace uso de la biblioteca estándar, donde la estructura `std::vector` está programada mediante *templates*. Al declarar explícitamente `std::vector<std::string>`, el compilador de C++ genera y valida una versión del vector diseñada para trabajar exclusivamente con texto. Al extraer los elementos durante el recorrido, el sistema sabe con absoluta certeza que lo recuperado es un `std::string`, lo que permite invocar métodos propios del tipo, como `length()`, sin riesgo alguno de fallos de memoria.

```java
// Ejemplo en Java usando generics (ArrayList)
import java.util.ArrayList;

public class EjemploGenerics {
    public static void main(String[] args) {
        // Instanciación de una lista dinámica que solo admite Strings
        ArrayList<String> listaCadenas = new ArrayList<>();

        listaCadenas.add("Primera cadena en Java");
        listaCadenas.add("Segunda cadena en Java");
        // listaCadenas.add(100); // El compilador bloquearía esta línea

        // Recorrido seguro usando un bucle for-each (sin castings)
        for (String elemento : listaCadenas) {
            System.out.println("Texto: " + elemento + 
                               " (Longitud: " + elemento.length() + ")");
        }
    }
}
```

Por su parte, en Java se utiliza la colección genérica `ArrayList<E>`, definiendo el parámetro de tipo como `String`. Al igual que ocurre en C++, cualquier intento de añadir un número u otra clase distinta provocará un error sintáctico inmediato. En el bucle de recorrido, la información se extrae directamente y de forma segura en una variable referencial de tipo `String`. Esto demuestra que el chequeo de tipos se ha realizado correctamente en tiempo de compilación, permitiendo operar con el texto sin recurrir a conversiones ascendentes o descendentes y blindando la ejecución frente a errores como el `ClassCastException`.

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

### Respuesta    
Cuando se instancia una clase genérica proporcionando un tipo de dato concreto, el compilador procesa la información para garantizar la seguridad del código, pero el mecanismo interno para lograrlo difiere radicalmente dependiendo del lenguaje. **C++ y Java no hacen lo mismo bajo el capó**. Mientras que el compilador de C++ opta por la generación de código múltiple y específico para cada tipo utilizado, el compilador de Java mantiene una única versión genérica del código para todos los tipos, aplicando transformaciones invisibles para asegurar su correcto funcionamiento. Esta decisión de diseño impacta directamente en el tamaño del programa compilado y en el comportamiento en tiempo de ejecución.

En el caso de C++, el proceso se conoce como **instanciación de plantillas** (*template instantiation*). Cuando el compilador detecta el uso de un *template* con un tipo concreto (por ejemplo, una lista para almacenar enteros y luego la misma lista para cadenas de texto), actúa como si el programador hubiera copiado y pegado el código original, sustituyendo manualmente los tipos. Por consiguiente, genera una clase completamente nueva e independiente en código máquina para cada tipo de dato utilizado. Si la plantilla se instancia para cinco tipos distintos, el ejecutable contendrá cinco versiones diferentes de la misma clase. Este enfoque puede aumentar considerablemente el tamaño del programa (fenómeno conocido como *code bloat*), pero ofrece un rendimiento máximo, ya que el código resultante está optimizado nativamente para cada tipo.

Por el contrario, Java emplea un mecanismo denominado **borrado de tipos** (*type erasure*). Este sistema fue diseñado para garantizar que el código compilado con genericidad fuera totalmente compatible con versiones antiguas de la Máquina Virtual de Java (JVM) que no comprendían este concepto. En este enfoque, los parámetros de tipo (como `<T>`) existen de forma exclusiva durante la fase de compilación. El compilador los utiliza rigurosamente para verificar que no se introduzcan datos incompatibles, pero, una vez completada la validación, "borra" toda esa información genérica del código resultante. En el *bytecode* final, el tipo `<T>` es reemplazado simplemente por la superclase `Object`.

Para compensar esta pérdida de información y evitar que el programador tenga que lidiar con los problemas habituales del uso de `Object`, el compilador de Java interviene realizando el trabajo sucio. En los puntos exactos del código donde se extrae un elemento de la estructura genérica, el compilador inserta automáticamente y de forma invisible las conversiones descendentes (*castings*) necesarias. En consecuencia, en memoria solo existe una única clase compilada, lo que ahorra espacio, pero con la particularidad de que, durante la ejecución del programa, la JVM desconoce por completo con qué tipo genérico fue instanciado originalmente el objeto.


## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

### Respuesta    
Para la creación de una clase genérica con múltiples parámetros, se emplean identificadores de tipo (habitualmente letras mayúsculas como `T`, `U` o `V`) separados por comas dentro de los corchetes angulares. Estos parámetros permiten que la clase `Par` actúe como una estructura de datos "tupla", capaz de vincular dos objetos de naturalezas distintas sin perder su identidad específica. La ventaja fundamental radica en la reutilización del código: la misma definición de clase sirve para almacenar un par de números, una relación nombre-teléfono o, como en el caso solicitado, un conjunto de resultados estadísticos.

A continuación, se define la clase `Par` siguiendo los estándares de encapsulación:

```java
public class Par<T, U> {
    private final T primero;
    private final U segundo;

    // Constructor que inicializa ambos tipos
    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    // Getters para recuperar los valores con su tipo original
    public T getPrimero() {
        return primero;
    }

    public U getSegundo() {
        return segundo;
    }
}
```



El uso de esta estructura es ideal para métodos que deben retornar más de un valor, una limitación clásica en lenguajes como C o Java convencional. Al devolver un objeto `Par<Double, Double>`, se está empaquetando la media y la desviación típica en una sola entidad que mantiene la seguridad de tipos. Esto evita tener que recurrir a soluciones menos elegantes, como devolver un arreglo de `double[]` (donde se pierde el significado semántico de qué posición es cada cosa) o usar la clase `Object`.

```java
public class Analizador {
    // Función que devuelve un Par con los resultados calculados
    public static Par<Double, Double> calcularEstadisticas(double[] datos) {
        double suma = 0;
        for (double d : datos) suma += d;
        double media = suma / datos.length;

        double varianza = 0;
        for (double d : datos) varianza += Math.pow(d - media, 2);
        double desviacion = Math.sqrt(varianza / datos.length);

        // Se retorna el par de valores de forma segura
        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] valores = {10.0, 20.0, 30.0};
        Par<Double, Double> res = calcularEstadisticas(valores);

        // Acceso directo y seguro sin necesidad de casting
        System.out.println("Media: " + res.getPrimero());
        System.out.println("Desviación: " + res.getSegundo());
    }
}
```

Al observar el ejemplo de uso, se aprecia que el compilador de Java infiere los tipos automáticamente. Cuando se invoca `res.getPrimero()`, el sistema ya sabe que el retorno es de tipo `Double`, permitiendo operar con él inmediatamente. Esta capacidad de "auto-documentación" del código mejora la legibilidad, ya que cualquier programador que vea la firma del método comprende al instante que se están devolviendo dos valores numéricos vinculados a un mismo proceso de cálculo.

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

### Respuesta    
En Java, la genericidad no se limita exclusivamente a la definición de clases o interfaces completas; también es posible declarar parámetros de tipo a nivel de un método individual. Esta capacidad resulta especialmente útil para diseñar funciones de utilidad estáticas, o cuando una operación específica requiere flexibilidad de tipos dentro de una clase que, por lo demás, no es genérica en su totalidad. Sintácticamente, el parámetro de tipo (como `<T>`) se debe colocar en la firma del método justo antes del tipo de retorno.

```java
public class SelectorAleatorio {

    // Versión 1: Uso de Object (enfoque clásico/polimórfico)
    public static Object seleccionaUnoObject(Object a, Object b) {
        return Math.random() < 0.5 ? a : b;
    }

    // Versión 2: Uso de un método genérico
    public static <T> T seleccionaUnoGenerico(T a, T b) {
        return Math.random() < 0.5 ? a : b;
    }

    public static void main(String[] args) {
        String texto1 = "Cara";
        String texto2 = "Cruz";

        // (i) Evitar downcasting
        // Con Object: El compilador exige un casting explícito para recuperar el String
        String resultado1 = (String) seleccionaUnoObject(texto1, texto2); 
        
        // Con Generics: El compilador infiere el tipo y no requiere casting
        String resultado2 = seleccionaUnoGenerico(texto1, texto2);

        // (ii) Forzar que ambos objetos sean del mismo tipo
        // Con Object: Se permite mezclar tipos absurdos silenciosamente
        Object resultadoMixto = seleccionaUnoObject("Texto", 42); 

        // Con Generics: Forzar tipos distintos genera un error de compilación
        // String error = seleccionaUnoGenerico("Texto", 42); // El compilador lo bloquea
    }
}
```

Al analizar el primer punto, la diferencia fundamental radica en la **evitación del downcasting explícito**. Cuando se emplea el enfoque basado en `Object`, el método pierde la noción del tipo original de los argumentos; para el compilador, lo que se devuelve es un objeto genérico abstracto. Por consiguiente, para asignar ese resultado a una variable concreta (como un `String`), es estrictamente obligatorio realizar una conversión descendente. En contraste, al invocar el método genérico `<T>`, el compilador infiere dinámicamente que si se introdujeron dos cadenas de texto, el retorno será indudablemente una cadena, permitiendo la asignación directa y eliminando el riesgo de sufrir una excepción `ClassCastException` en tiempo de ejecución.

Respecto al segundo punto, el uso de un único parámetro de tipo `<T>` en la firma del método permite **forzar la consistencia estricta de los argumentos**. Si la lógica dicta que la función debe elegir entre dos alternativas equivalentes, la versión con `Object` es incapaz de imponer esta restricción, dado que acepta cualquier combinación de clases (como mezclar un texto y un número entero). Sin embargo, al definir ambos parámetros como `T a` y `T b`, se le indica al compilador que ambas variables deben compartir el mismo tipo. Si se intenta compilar una invocación con tipos incompatibles, el sistema detectará la incongruencia inmediatamente y detendrá el proceso de construcción del programa.

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

### Respuesta   
Sí, es posible establecer restricciones sobre los parámetros de tipo, un concepto conocido en Java como "límites superiores" (*upper bounds*). Al definir un tipo genérico, se puede exigir que el tipo suministrado herede de una clase o implemente una interfaz específica utilizando la palabra reservada `extends`. Por ejemplo, al declarar `<T extends Number>`, se instruye al compilador para que solo admita clases numéricas (como `Integer`, `Double` o `Float`), rechazando de inmediato cadenas de texto u otros objetos incompatibles. Esta característica es vital porque permite utilizar, dentro del código genérico, los métodos definidos en la clase límite (como `doubleValue()`), ya que el compilador tiene la certeza absoluta de que el tipo instanciado los poseerá.

```java
// Solución 1: Uso de polimorfismo tradicional sin generics
public class PuntoPolimorfico {
    private Number x;
    private Number y;

    public PuntoPolimorfico(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(PuntoPolimorfico otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

La primera solución emplea el polimorfismo convencional al definir las coordenadas directamente como referencias a la superclase abstracta `Number`. Aunque este diseño permite instanciar el punto con cualquier valor numérico y realizar el cálculo matemático correctamente, presenta el inconveniente habitual de la programación sin genericidad: se pierde la identidad del tipo concreto. Si se construye un punto exclusivamente con enteros, el método `getX()` devolverá una referencia de tipo general `Number`, obligando al programador a realizar una conversión explícita (*casting*) si posteriormente necesita utilizar esos datos como `Integer`.

```java
// Solución 2: Uso de genericidad con restricción (límite superior)
public class PuntoGenerico<T extends Number> {
    private T x;
    private T y;

    public PuntoGenerico(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    // Usamos PuntoGenerico<?> para permitir calcular la distancia 
    // contra otro punto numérico de distinto tipo (ej. Double vs Integer)
    public double calcularDistanciaA(PuntoGenerico<?> otro) {
        double dx = this.x.doubleValue() - otro.x.doubleValue();
        double dy = this.y.doubleValue() - otro.y.doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

En la segunda solución, la introducción del límite superior `<T extends Number>` solventa esta carencia al mantener la información de tipos. El compilador garantiza que al llamar a `getX()`, se devuelva exactamente la clase original proporcionada durante la instanciación (por ejemplo, `Double`), eliminando la necesidad de conversiones posteriores. Respecto al mecanismo de borrado de tipos (*type erasure*), el comportamiento se adapta a la restricción impuesta. Durante la compilación, en lugar de sustituir el parámetro `T` por la superclase raíz `Object`, el compilador lo reemplaza por su límite más estricto conocido. Por lo tanto, el tipo final en el código máquina (*bytecode*) de la clase `PuntoGenerico` es **`Number`**, generando un código interno prácticamente idéntico al de la primera solución, pero envolviéndolo con una capa inquebrantable de seguridad estática y validación automática.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

### Respuesta    
Al reflexionar sobre el refuerzo del chequeo de tipos, se hace evidente una diferencia crucial en cuanto a la flexibilidad y rigidez de ambas implementaciones al mezclar tipos de datos. En la solución sin genericidad (basada puramente en el polimorfismo con la superclase `Number`), las variables operan de forma completamente independiente; por lo tanto, es perfectamente posible instanciar un punto donde la coordenada horizontal sea un número entero (`Integer`) y la vertical sea un número real (`Double`), ya que ambas cumplen individualmente con ser descendientes de `Number`. Por el contrario, en la solución genérica que emplea un único parámetro `<T extends Number>` para ambas coordenadas, el compilador impone una homogeneidad estricta. Una vez que se define explícitamente que el parámetro `T` corresponde a un tipo concreto, ambas coordenadas deben pertenecer obligatoriamente a ese mismo tipo, rechazando cualquier intento de mezclar enteros y reales bajo una misma instancia (a menos que se declarasen dos parámetros distintos, como `<T, U>`).

Esta rigidez de la solución genérica se traduce en una ventaja analítica y de seguridad fundamental a la hora de recuperar la información. En el caso de la solución polimórfica tradicional, el método `getX()` devuelve invariablemente una referencia de tipo `Number`. Esto significa que el programa pierde la noción exacta de lo que almacenó, obligando a realizar una conversión descendente (*downcasting*) explícita si se requiere utilizar ese valor estrictamente como un `Integer` en operaciones posteriores. Si dicha conversión se hace erróneamente asumiendo un tipo que no era el original, el programa fallará durante la ejecución.

En contraste, con la solución basada en genericidad, el método `getX()` devuelve exactamente el tipo original `T` con el que fue instanciada la clase. Si el punto se creó parametrizado para `Double`, la firma del método `getX()` garantiza retornar un `Double` de forma directa y segura. El compilador asume el control total sobre la coherencia de los datos, permitiendo su uso inmediato en el código receptor sin necesidad de conversiones manuales y garantizando que las operaciones matemáticas subsiguientes se realicen sobre el tipo de dato preciso que el programador determinó en un inicio.

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
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

### Respuesta    
Para resolver este problema mediante el uso de genericidad avanzada, se debe emplear una técnica conocida como **Tipos Genéricos Recursivos**. En lugar de que la interfaz `Punto` acepte cualquier objeto que implemente la misma interfaz (lo cual obligaría al uso de `instanceof` para distinguir entre dimensiones), se define la interfaz con un parámetro de tipo que hace referencia a la propia clase que la implementa. Esto permite que el método `distanciaA` sea estrictamente tipado desde su definición original.

A continuación, se muestra la implementación refactorizada:

```java
// La interfaz recibe un parámetro T que debe ser, a su vez, un Punto
public interface Punto<T extends Punto<T>> { 
    public double distanciaA(T p); 
} 

public class Punto2D implements Punto<Punto2D> { 
    private final double x, y; 

    public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto2D p) { 
        // Ya no se necesita instanceof ni casting. 
        // El compilador garantiza que 'p' es Punto2D.
        return Math.sqrt(Math.pow(x - p.x, 2) + Math.pow(y - p.y, 2)); 
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
}
```
La clave de esta solución reside en la declaración `Punto2D implements Punto<Punto2D>`. Al hacer esto, el parámetro `T` de la interfaz se vincula específicamente a la clase `Punto2D`. En consecuencia, cuando se sobrescribe el método `distanciaA`, el argumento ya no es de tipo la interfaz general `Punto`, sino del tipo concreto `Punto2D`. Esto elimina por completo la necesidad de validar el tipo en tiempo de ejecución o lanzar excepciones por incompatibilidad, ya que el sistema de tipos de Java impedirá, en tiempo de compilación, que se intente calcular la distancia entre un `Punto2D` y un `Punto3D`.

Desde una perspectiva de diseño, este enfoque refuerza el contrato de la interfaz de una manera mucho más robusta que el polimorfismo tradicional. Mientras que en la versión original el programador era responsable de gestionar el error si se mezclaban dimensiones, con la genericidad es el compilador quien asume esa responsabilidad. Si se intentara ejecutar un código como `punto2d.distanciaA(punto3d)`, el IDE marcaría un error de tipos inmediatamente, garantizando que el código sea más limpio, seguro y eficiente al evitar comprobaciones lógicas innecesarias dentro de los métodos.

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

### Respuesta   
A pesar de la relación de herencia entre `String` y `Object`, el comportamiento de sus colecciones difiere radicalmente debido a decisiones de diseño del lenguaje. En Java, los **arrays son covariantes**, lo que significa que si `String` extiende de `Object`, entonces `String[]` es legalmente un subtipo de `Object[]`. Sin embargo, los **genéricos son invariantes**: una `List<String>` no es subtipo de `List<Object>`. Esta distinción es fundamental para garantizar la seguridad de tipos, ya que tratar las listas como los arrays permitiría errores lógicos que el compilador no podría detectar.

El problema principal de la covarianza en los arrays es que permite "engañar" al sistema de tipos en tiempo de ejecución. Dado que un `String[]` puede ser referenciado por un `Object[]`, el lenguaje permite intentar guardar un `Integer` en un array de cadenas a través de la referencia genérica. Aunque el código compilaría sin problemas, la Máquina Virtual de Java lanzará una excepción `ArrayStoreException` al detectar que se intenta meter un número en un espacio de memoria reservado para texto. Los genéricos evitan este problema de raíz; al ser invariantes, el compilador prohíbe la asignación inicial, obligando a que la lista sea exactamente del tipo esperado y eliminando fallos en ejecución.

Para formalizar estos comportamientos, se definen tres conceptos clave sobre la relación entre el contenedor y su contenido:

* **Invariante:** Significa que no existe relación de herencia entre los contenedores, independientemente de la relación entre sus tipos. Es el comportamiento por defecto de los genéricos en Java (`List<String>` no tiene relación con `List<Object>`).
* **Covariante:** Significa que la relación de herencia se mantiene en la misma dirección. Si $A$ es subtipo de $B$, entonces $Contenedor\langle A \rangle$ es subtipo de $Contenedor\langle B \rangle$. Los arrays en Java son covariantes.
* **Contravariante:** Es la relación inversa. Si $A$ es subtipo de $B$, entonces $Contenedor\langle B \rangle$ se considera subtipo de $Contenedor\langle A \rangle$. En Java, esto se logra mediante comodines (*wildcards*) como `<? super T>`, permitiendo que una estructura acepte tipos más generales que el suyo.

En resumen, la invarianza de los genéricos en Java es una medida de protección. Al impedir que una `List<String>` se comporte como una `List<Object>`, Java asegura que nadie pueda insertar accidentalmente un objeto de otro tipo en una lista que fue diseñada para ser estrictamente de cadenas, algo que los arrays no pueden garantizar por sí solos.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

### Respuesta    
Un **wildcard** o comodín (representado por el símbolo `?`) es un parámetro de tipo especial en Java que representa un tipo desconocido. A diferencia de un parámetro de tipo estándar como `<T>`, que fija un tipo concreto para toda la clase o método, el comodín permite flexibilizar las restricciones de invarianza. Se utiliza para definir qué rango de tipos puede aceptar una estructura genérica, permitiendo que un método sea mucho más versátil al aceptar familias de tipos en lugar de una única clase exacta.

La diferencia fundamental entre los dos tipos de wildcards radica en la dirección de la jerarquía que permiten recorrer:
* **`List<? extends T>` (Límite Superior):** Permite aceptar `T` o cualquier subtipo de `T`. Se utiliza principalmente para **lectura**. Como sabemos que todo lo que hay en la lista es, al menos, un `T`, podemos extraer elementos de forma segura. Sin embargo, no permite añadir elementos (excepto `null`), ya que el compilador no sabe si la lista real es de `T`, de `Subtipo1` o de `Subtipo2`.
* **`List<? super T>` (Límite Inferior):** Permite aceptar `T` o cualquier superclase de `T` (hasta llegar a `Object`). Se utiliza principalmente para **escritura**. Como sabemos que la lista acepta, como mínimo, objetos de tipo `T`, es seguro insertar elementos de tipo `T`. La lectura, en cambio, es limitada, ya que el compilador solo puede garantizar que lo que devuelve es un `Object`.

A continuación se presentan los ejemplos solicitados para ilustrar esta dualidad (conocida habitualmente como el principio **PECS**: *Producer Extends, Consumer Super*):

```java
import java.util.List;
import java.util.ArrayList;

public class EjemploWildcards {

    // (i) COVARIANZA: El método "produce" o lee datos de la lista.
    // Acepta List<Integer>, List<Double>, etc., tratándolos como Number.
    public static double sumarLista(List<? extends Number> lista) {
        double suma = 0.0;
        for (Number n : lista) {
            suma += n.doubleValue();
        }
        return suma;
    }

    // (ii) CONTRAVARIANZA: El método "consume" o añade datos a la lista.
    // Acepta List<Integer>, List<Number> o List<Object>.
    public static void añadirEnteros(List<? super Integer> lista) {
        lista.add(10);
        lista.add(20);
        lista.add(30);
        // No podríamos leer de aquí fácilmente como Integer, 
        // porque la lista podría ser de Objects.
    }

    public static void main(String[] args) {
        List<Double> decimales = List.of(1.5, 2.5, 3.0);
        System.out.println("Suma: " + sumarLista(decimales));

        List<Number> misNumeros = new ArrayList<>();
        añadirEnteros(misNumeros); 
        System.out.println("Lista tras añadir: " + misNumeros);
    }
}
```
En el primer ejemplo, el uso de `? extends Number` es vital porque permite que el método sea reutilizable para cualquier colección numérica. Si hubiéramos definido el método con `List<Number>`, el compilador rechazaría una `List<Double>` debido a la invarianza. En el segundo ejemplo, `? super Integer` garantiza que la lista proporcionada tenga la "capacidad" de almacenar enteros, ya sea porque es una lista de enteros, de números en general o de objetos cualesquiera, protegiendo la integridad de la estructura de datos.
