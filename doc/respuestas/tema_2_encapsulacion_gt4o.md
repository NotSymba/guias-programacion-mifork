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

### Respuesta   

### Objetivos de la Encapsulación y Ocultación

El objetivo principal de la encapsulación y la ocultación de información es proteger la integridad del estado interno de un objeto, evitando que sea manipulado de forma arbitraria o inconsistente por código externo. En C, una `struct` expone todos sus miembros; cualquier función puede alterar sus valores sin control, lo que a menudo lleva a errores difíciles de rastrear si los datos quedan en un estado inválido (por ejemplo, asignar un valor negativo a una variable que representa una longitud). La POO busca restringir este acceso directo, obligando a que cualquier interacción con los datos se realice exclusivamente a través de métodos controlados (la interfaz pública), garantizando así que el objeto sea el único responsable de gestionar su propia información.

### Ventajas de la Ocultación

Entre las ventajas más destacadas se encuentra el **mantenimiento y la flexibilidad** del código. Al ocultar la representación interna de los datos, es posible cambiar la implementación de una clase (por ejemplo, cambiar un tipo de dato `int` a `double` o cambiar la estructura de datos interna) sin que esto rompa el código de los programas que la utilizan, siempre y cuando se mantenga la misma interfaz de métodos públicos. Esto soluciona el problema habitual en C donde modificar una definición en un `.h` obliga a refactorizar múltiples archivos `.c`.

Otra ventaja fundamental es la **validación y consistencia**. Al forzar el acceso a través de métodos, se pueden introducir reglas de lógica (validaciones con `if`) antes de aceptar un cambio de estado. Esto asegura que el objeto nunca pueda quedar en un estado "roto" o ilógico. Finalmente, promueve la **abstracción**, ya que permite utilizar objetos complejos entendiendo solo qué hacen (sus métodos públicos) sin necesidad de comprender la complejidad de cómo guardan o procesan los datos internamente, reduciendo la carga cognitiva del programador.


## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

### Respuesta   

### Definición de Interfaz Pública

La **interfaz pública** se define como el conjunto de métodos (y ocasionalmente atributos constantes) que una clase expone abiertamente para que sean utilizados por otras partes del programa. Constituye el "contrato" de uso del objeto: especifica qué acciones puede realizar y qué datos puede devolver, pero no revela los detalles de cómo se llevan a cabo esas operaciones internamente.

Para un programador de C, la interfaz pública es conceptualmente equivalente al archivo de cabecera (`.h`) de una biblioteca. En dicho archivo se declaran los prototipos de las funciones disponibles para el usuario, mientras que la implementación real y las variables internas residen ocultas en el archivo fuente (`.c`) o en la librería compilada. Del mismo modo, en Java, los métodos marcados como `public` forman esta interfaz visible, mientras que todo lo marcado como `private` queda fuera de ella.

### Relación con la Ocultación de Información

La relación entre la interfaz pública y la ocultación de información es fundamental: la interfaz pública es el mecanismo que **permite** y **gestiona** la ocultación. Si se ocultaran todos los miembros de una clase (haciéndolos todos privados), el objeto sería inutilizable desde el exterior; si se mostraran todos, no habría seguridad ni abstracción.

Por tanto, la interfaz pública actúa como una frontera selectiva o un filtro. Al obligar a los objetos externos a interactuar únicamente a través de estos métodos públicos, se protege el estado interno (los datos ocultos). Esto permite desacoplar la implementación del uso: se puede cambiar radicalmente cómo se procesan o almacenan los datos internamente (el "cómo") sin afectar al código externo, siempre y cuando no se modifique la firma de los métodos de la interfaz pública (el "qué").


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

### Respuesta   

### Importancia del diseño de la interfaz pública

La interfaz pública actúa como un contrato inmutable entre el creador de la clase y el resto del sistema que la utiliza. Al igual que en C modificar el prototipo de una función en un archivo de cabecera (`.h`) muy utilizado rompería la compilación de todos los módulos dependientes, en Java, cada método público crea una dependencia directa. Si se diseña la interfaz exponiendo detalles internos innecesarios (por ejemplo, permitiendo acceso directo a una estructura de datos interna), se pierde la capacidad de optimizar o cambiar esa estructura en el futuro sin reescribir todo el código externo que depende de ella. Por ello, se debe exponer solo lo estrictamente necesario.

### Dificultad de modificación

Cambiar una interfaz pública ya establecida es, por lo general, difícil y costoso. Una vez que otros objetos han comenzado a llamar a un método público, cualquier alteración en su nombre, tipo de retorno o parámetros (firma) provocará errores de compilación en cascada en todas las partes del proyecto que lo utilicen. A diferencia de los miembros privados, que pueden refactorizarse con total libertad porque su alcance está limitado al archivo de la clase, la interfaz pública es rígida. Por esta razón, se considera una buena práctica de diseño ser conservador: es fácil hacer público algo que era privado, pero es casi imposible volver privado algo que ya se hizo público y está en uso.


## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

### Respuesta   

### Definición de Invariante

Una **invariante de clase** es una condición lógica o regla de integridad que debe cumplirse siempre para que el estado de un objeto sea considerado válido. Desde la perspectiva de C, se puede imaginar como las restricciones implícitas de una estructura de datos; por ejemplo, en una `struct Tiempo` con campos de horas y minutos, una invariante sería que "los minutos deben estar siempre entre 0 y 59". Si en algún momento una variable de esa estructura contiene un 75 en el campo de minutos, se ha violado la invariante y el dato se considera corrupto o inconsistente, lo que puede provocar fallos en funciones que dependan de esa coherencia.

### El Rol de la Ocultación

La **ocultación de información** es la herramienta fundamental para garantizar que estas invariantes nunca se rompan. Si los atributos de una clase fueran públicos (accesibles directamente como en una `struct`), cualquier parte del código externo podría asignar valores inválidos (como `tiempo.minutos = 90;`) sin restricción alguna. Al ocultar los datos (`private`) y obligar a modificarlos a través de métodos, la clase puede interceptar cualquier intento de modificación. Estos métodos incluyen lógica de validación (sentencias `if`) para rechazar datos incorrectos antes de que afecten al estado interno, asegurando así que el objeto se mantenga siempre dentro de sus límites válidos.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

### Respuesta   

```java
public class Punto {
    // 1. Ocultación de información: Atributos privados
    // Solo visibles dentro de estas llaves { ... }
    private double x;
    private double y;

    // 2. Interfaz Pública: Constructor
    // Permite inicializar el objeto desde fuera
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // 3. Interfaz Pública: Método de comportamiento
    // Ofrece un servicio sin revelar cómo se guardan los datos
    public double calcularDistanciaAOrigen() {
        return Math.sqrt((x * x) + (y * y));
    }
    
    // Métodos de acceso (Getters) para permitir lectura controlada
    public double getX() { return x; }
    public double getY() { return y; }
}

```

### La Interfaz Pública y los Modificadores

La **interfaz pública** de la clase `Punto` está constituida por todos los miembros que han sido declarados con el modificador `public`. En este caso concreto, la interfaz la conforman el **constructor**, el método **`calcularDistanciaAOrigen()`** y los métodos **`getX()`/`getY()**`. Estos elementos representan el "contrato" o el conjunto de funciones disponibles para que otros objetos interactúen con `Punto`. Cualquier código externo (como el `main`) solo "ve" y puede utilizar estos métodos, ignorando por completo la existencia de las variables `x` e `y`, las cuales podrían cambiar de nombre o tipo en el futuro sin afectar al uso de la clase.

El modificador **`private`** restringe la visibilidad del miembro exclusivamente al ámbito de la propia clase. Es comparable a declarar una variable estática global dentro de un archivo `.c`: dicha variable es invisible para otros archivos del proyecto. Al marcar `x` e `y` como privados, se prohíbe el acceso directo `punto.x = 5;`, forzando el uso del constructor o de métodos específicos. Por el contrario, **`public`** permite que el miembro sea accesible desde cualquier lugar del programa, actuando como las funciones declaradas en un archivo de cabecera `.h` en C, disponibles para cualquiera que incluya la librería.


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

### Respuesta   

### 1. Miembros de la Clase (Atributos y Métodos)

El uso más frecuente de `public` y `private` se da en los **atributos** (variables de instancia) y los **métodos** (funciones). Al aplicarlos aquí, se define quién puede acceder a los datos o invocar el comportamiento del objeto. Esto es diferente a C++, donde los modificadores abren secciones completas (`public:` seguido de varias declaraciones); en Java, el modificador debe anteponerse explícitamente a **cada** declaración individual de atributo o método (por ejemplo, `private int x; private int y;`).

### 2. La Clase misma y sus Constructores

Los modificadores también se aplican a la definición de la **clase** en sí misma. Una clase declarada como `public class` es visible y utilizable por cualquier otra clase en cualquier paquete del proyecto (similar a un `.h` globalmente accesible). Si se omite `public`, la clase solo es visible dentro de su propio paquete. Asimismo, se aplican a los **constructores**: un constructor `private` impide que se creen instancias de la clase desde el exterior usando `new`, una técnica utilizada para restringir la creación de objetos (como en el patrón *Singleton* o en clases de pura utilidad como `Math`).

### 3. Restricción en Variables Locales

Es crucial destacar dónde **no** se pueden usar: las **variables locales**. A diferencia de los atributos que viven en el *heap* dentro del objeto, las variables declaradas dentro de un método (en el *stack*) tienen su visibilidad naturalmente limitada al bloque de ejecución de la función, tal y como ocurre en C. Por tanto, es un error de compilación intentar declarar `private int contador` dentro de un método `main` o cualquier otra función; el control de acceso de Java no opera a nivel de variables temporales de la pila.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

### Respuesta    

Sí, la visibilidad no es un concepto binario; existe un espectro de control de acceso que permite refinar quién puede ver y modificar los componentes de una clase. Además de los extremos `public` (acceso total) y `private` (acceso restringido a la propia clase), la mayoría de los lenguajes orientados a objetos incluyen el modificador **`protected`**. Este nivel es crucial para la **herencia**: permite que las subclases (las "hijas") accedan a los miembros de la clase padre, manteniéndolos ocultos para el resto del mundo externo. Es un equilibrio entre la seguridad de `private` y la apertura de `public`.

En el caso específico de **Java**, existen cuatro niveles de visibilidad. Aparte de `public`, `private` y `protected`, existe la **visibilidad por defecto** (o *package-private*), que se aplica cuando no se escribe ningún modificador. Este nivel permite el acceso a cualquier clase que resida en el mismo **paquete** (directorio lógico). Para un programador de C, esto es comparable al alcance de las variables globales estáticas dentro de un mismo archivo fuente o módulo: son visibles para todas las funciones de ese archivo, pero invisibles para otros archivos del proyecto.

En otros lenguajes, la filosofía varía. **C++** utiliza `public`, `private` y `protected` con una semántica similar a Java respecto a la herencia, pero introduce el concepto de `friend` (clases o funciones amigas), que permite a una clase externa específica acceder a los miembros privados de otra, rompiendo el encapsulamiento de forma controlada. Por otro lado, lenguajes como **Python** no tienen modificadores de acceso estrictos forzados por el compilador; se basan en convenciones (como poner un guion bajo `_` antes del nombre) para indicar que un atributo "debería" tratarse como privado, confiando en la responsabilidad del programador.


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta   

La respuesta correcta es la opción **(a): están ocultos para otras clases**. En la mayoría de los lenguajes orientados a objetos, incluido Java (y C++), los modificadores de acceso se aplican a nivel de **Clase** (el tipo de dato o molde), no a nivel de **Instancia** (el objeto individual). Esto significa que un objeto de la clase `Punto` tiene permiso total para acceder a los miembros privados de *cualquier* otro objeto que también sea de la clase `Punto`. La barrera de protección `private` se levanta contra el código externo (otras clases), pero no existe entre objetos "hermanos" del mismo tipo.

Esta característica es fundamental para la implementación de operaciones binarias (operaciones que involucran dos objetos del mismo tipo). Si la privacidad fuera estricta por instancia, sería imposible o muy ineficiente escribir métodos que interactúen con otro objeto de la misma clase, ya que `this` no podría ver los datos de `otro`. Desde la perspectiva de C, esto es equivalente a escribir una función dentro de una librería que recibe dos punteros a la misma estructura opaca (`struct Punto *a`, `struct Punto *b`); dicha función tiene acceso a los campos internos de ambas estructuras porque "conoce" la definición del tipo, independientemente de qué instancia sea.

A continuación se muestra el ejemplo solicitado. Nótese cómo dentro del método `calcularDistanciaAPunto`, se accede directamente a `otro.x` y `otro.y`. A pesar de que `x` e `y` son `private`, el compilador lo permite porque el código reside dentro de la definición de la clase `Punto`.

```java
public class Punto {
    // Atributos privados: Ocultos para el mundo exterior (Main, otras clases...)
    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método que recibe OTRO objeto de la misma clase
    public double calcularDistanciaAPunto(Punto otro) {
        // ACCESO PERMITIDO:
        // Aunque 'otro.x' es privado, podemos acceder a él directamente
        // porque estamos dentro de la clase 'Punto'.
        double cateto1 = this.x - otro.x; 
        double cateto2 = this.y - otro.y;

        return Math.sqrt((cateto1 * cateto1) + (cateto2 * cateto2));
    }
}

```


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta   

### Definición y Función como Intermediarios

Los métodos "getter" (accesores) y "setter" (mutadores) son funciones públicas diseñadas para leer y modificar, respectivamente, los valores de los atributos privados de una clase. En C, lo habitual es acceder directamente a los campos de una estructura (`variable.campo`). En la orientación a objetos, estos métodos actúan como una capa de intermediación obligatoria: el *getter* recupera el dato y lo devuelve al llamante, mientras que el *setter* recibe un nuevo valor y lo asigna al atributo interno.

### Control y Validación de Datos

La utilidad crítica de estos métodos, especialmente del *setter*, reside en la capacidad de ejecutar lógica de validación antes de alterar el estado del objeto. A diferencia de la asignación directa de memoria en una `struct`, donde se puede escribir cualquier valor permitido por el tipo de dato (por ejemplo, asignar una edad negativa a un entero), un *setter* permite incluir sentencias de control (`if`) para verificar que el dato cumple con las reglas del negocio (invariantes). Si el dato es inválido, el método puede ignorar la solicitud o notificar un error, protegiendo así la integridad de la memoria del objeto.

### Abstracción y Flexibilidad de Acceso

Además, el uso de *getters* y *setters* permite desacoplar la representación interna de los datos de su uso externo. Por ejemplo, una clase podría almacenar internamente una temperatura en grados Kelvin (tipo `double`), pero ofrecer métodos *getter* y *setter* que trabajen en grados Celsius, realizando la conversión matemática al vuelo. Asimismo, permiten definir propiedades de "solo lectura": si se proporciona un *getter* pero no se implementa el *setter* correspondiente, el atributo se vuelve inmutable para el mundo exterior, algo que en una estructura de C requeriría el uso complejo de punteros a constantes (`const`).


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta   

No, el término "seguridad" en el contexto de la encapsulación no se refiere a la protección contra ataques informáticos, hackers o robo de datos (ciberseguridad). Se refiere a la **seguridad lógica** y a la **integridad** del funcionamiento del programa. En C, es común cometer errores donde se asignan valores incoherentes a una estructura (por ejemplo, un denominador igual a cero o un puntero nulo no verificado), lo que provoca comportamientos indefinidos o fallos de segmentación (*segmentation faults*) durante la ejecución.

La encapsulación protege al código de su propio desarrollador y de otros programadores que utilicen la clase. Al ocultar los datos, se evita que se modifiquen accidentalmente de manera incorrecta, garantizando que el objeto siempre se mantenga en un estado válido. Es un mecanismo de defensa contra el uso inadecuado de la memoria y la lógica del programa, asegurando que las "invariantes" (las reglas que definen qué datos son válidos) nunca se rompan, reduciendo drásticamente la cantidad de *bugs* producidos por efectos colaterales imprevistos.

Sin embargo, es importante destacar que los modificadores `private` no detendrán a un atacante intencionado. En lenguajes como Java (mediante *Reflection*) o C++ (mediante aritmética de punteros y *casting*), es posible saltarse estas restricciones y acceder a la memoria oculta si se tiene la intención deliberada de hacerlo. Por tanto, la ocultación de información debe verse como un cinturón de seguridad para evitar accidentes de tráfico (errores de programación), no como un blindaje contra misiles (ataques maliciosos).


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta   

### Diferencia entre Miembro de Instancia y de Clase

Un **miembro de instancia** pertenece a un objeto concreto creado con el operador `new`. Cada vez que se crea una instancia, el sistema reserva un espacio de memoria nuevo y exclusivo para sus atributos no estáticos. Si se modifica un atributo de instancia en un objeto, este cambio no afecta en absoluto a los demás objetos de la misma clase. En C, esto es exactamente lo que ocurre con los campos de una `struct`: cada variable de tipo estructura tiene sus propios valores independientes; modificar `puntoA.x` no altera `puntoB.x`.

Por el contrario, un **miembro de clase** (identificado con la palabra clave `static`) existe una única vez en la memoria durante toda la ejecución del programa, independientemente de cuántos objetos se creen (incluso si no se crea ninguno). Este miembro es compartido por todas las instancias de la clase. Se comporta de manera muy similar a una **variable global** en C, pero con la ventaja de estar organizada lógicamente dentro del espacio de nombres de la clase. Si un objeto modifica un atributo estático, el cambio es visible inmediatamente para todos los demás objetos, ya que todos "apuntan" a la misma dirección de memoria para ese dato específico.

### Ocultación de Miembros de Clase

Respecto a la segunda cuestión, **sí, los miembros de clase también pueden ocultarse**. Los modificadores de acceso (`public`, `private`, `protected`) funcionan exactamente igual con miembros estáticos (`static`) que con miembros de instancia. Declarar un atributo como `private static` es una práctica muy común para manejar datos internos compartidos que no deben ser expuestos.

Desde la perspectiva de C, un miembro `private static` es funcionalmente equivalente a una variable global declarada como `static` dentro de un archivo `.c`. Dicha variable es accesible por todas las funciones (métodos) definidas en ese archivo, pero es invisible para el enlazador (*linker*) y otros archivos del proyecto. Esto permite mantener un estado compartido protegido (como un contador de objetos creados o una caché interna) sin contaminar el espacio global y sin riesgo de manipulación externa.


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
