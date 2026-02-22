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

Sí, tiene mucho sentido y es una práctica arquitectónica muy común en el diseño de software orientado a objetos. Declarar un constructor como `private` impide que cualquier código externo a la clase pueda utilizar el operador `new` para instanciarla. Desde la perspectiva de la programación en C, esto sería equivalente a definir un tipo `struct` pero ocultar deliberadamente la función encargada de invocar a `malloc`, bloqueando así la creación de la estructura desde módulos externos.

Un uso típico de esta restricción se da en las **clases de utilidad**. Estas clases se diseñan exclusivamente para agrupar variables constantes y métodos estáticos (`static`), funcionando de manera idéntica a una librería de funciones en C (como `<math.h>` o `<string.h>`). Dado que todo su contenido es estático y no manejan un estado individual, carece de sentido lógico y técnico reservar memoria para crear un objeto. El constructor privado actúa como una barrera explícita que previene la instanciación accidental de la clase.

Otro uso fundamental es el control centralizado sobre la creación de objetos, siendo el **Patrón Singleton** su aplicación más famosa. En ciertos escenarios, se requiere garantizar que exista una, y solo una, instancia de una clase en toda la ejecución del programa (por ejemplo, un gestor de conexiones a una base de datos o un registro de configuración). Al privatizar el constructor, la propia clase asume la responsabilidad de crear internamente esa única instancia y proporciona un método público estático para devolver su referencia, asegurando que todos los componentes del sistema utilicen exactamente el mismo objeto.

A continuación se muestra un ejemplo básico de cómo se implementa este control de instanciación en Java:

```java
public class GestorConfiguracion {
    // 1. Se crea la única instancia internamente como atributo estático privado
    private static GestorConfiguracion instanciaUnica = new GestorConfiguracion();

    // 2. El constructor privado impide el uso de 'new GestorConfiguracion()' desde fuera
    private GestorConfiguracion() {
        // Código de inicialización del gestor...
    }

    // 3. Método público estático que devuelve siempre la misma instancia compartida
    // En C, sería como una función que devuelve un puntero a una variable global estática
    public static GestorConfiguracion obtenerInstancia() {
        return instanciaUnica;
    }
}

```


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta   

### Declaración de Miembros de Clase

En Java, los miembros de clase se indican utilizando la palabra reservada `static`. Cuando un atributo o método se declara con este modificador, deja de pertenecer a las instancias individuales (los objetos creados dinámicamente con `new`) y pasa a estar vinculado a la definición de la clase en sí misma. Para un programador de C, un atributo estático de Java es conceptualmente idéntico a una variable global o a una variable `static` dentro de un archivo `.c`: existe una única copia de ese dato en memoria, la cual es compartida y accesible por todas las instancias presentes y futuras de la clase.

Para resolver el problema de rastrear los valores máximos históricos de las coordenadas `x` e `y`, se requiere información que trascienda la existencia de un solo punto. Esta información representa el "estado global" de todos los puntos creados. Por consiguiente, se deben definir dos atributos estáticos para almacenar dichos máximos. Cada vez que el constructor inicializa un nuevo objeto, este comparará sus propias coordenadas de instancia con estas variables globales de la clase, actualizándolas si resulta necesario.

Es fundamental destacar la forma correcta de interactuar con estos elementos desde el exterior. Puesto que los miembros estáticos pertenecen a la plantilla general y no a los objetos físicos, se invocan utilizando el nombre de la clase directamente, seguido de un punto (por ejemplo, `Punto.getMaxX()`). No es necesario —ni recomendable, aunque el compilador lo permita— instanciar un objeto para acceder a un método o atributo estático, lo que subraya su independencia del ciclo de vida de la memoria dinámica.

### Ejemplo Práctico: Rastreo de Máximos

En el siguiente código se observa cómo se combinan los atributos de instancia (propios de cada objeto) con los atributos de clase (compartidos). Se inicializan los máximos con el valor negativo más grande posible para garantizar que el primer punto creado los sobrescriba correctamente.

```java
public class Punto {
    // 1. Miembros de Instancia: Cada objeto tiene su propia copia
    private double x;
    private double y;

    // 2. Miembros de Clase (Static): Única copia compartida en toda la ejecución
    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;

        // Actualización de los datos globales desde la instancia recién creada
        if (this.x > Punto.maxX) {
            Punto.maxX = this.x;
        }
        if (this.y > Punto.maxY) {
            Punto.maxY = this.y;
        }
    }

    // 3. Métodos de Clase (Static): Permiten leer los datos compartidos
    public static double getMaxX() {
        return Punto.maxX;
    }

    public static double getMaxY() {
        return Punto.maxY;
    }
}

```

```java
// Ejemplo de uso
public class Principal {
    public static void main(String[] args) {
        // Inicialmente, incluso sin objetos, los métodos estáticos funcionan
        System.out.println("Max X inicial: " + Punto.getMaxX()); // -Infinity

        Punto p1 = new Punto(2.0, 5.0);
        Punto p2 = new Punto(10.0, -3.0);
        Punto p3 = new Punto(7.0, 8.0);

        // Se consulta a la clase Punto, no a 'p1', 'p2' o 'p3'
        System.out.println("Max X histórico: " + Punto.getMaxX()); // 10.0
        System.out.println("Max Y histórico: " + Punto.getMaxY()); // 8.0
    }
}

```


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta   

```java
public static Punto crearPuntoRedondeado(double x, double y) {
    double xRedondeado = Math.round(x);
    double yRedondeado = Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}

```

El método implementado utiliza obligatoriamente el modificador `static`. En el paradigma orientado a objetos, un método factoría (*factory method*) tiene como propósito principal la creación y devolución de una nueva instancia de la clase. Por lo tanto, debe poder ser invocado directamente sobre la clase misma (por ejemplo, `Punto.crearPuntoRedondeado(3.4, 5.8)`), sin requerir la existencia previa de un objeto en memoria para poder llamarlo.

Desde la perspectiva de C, este comportamiento es análogo a una función global encargada de procesar unos argumentos, reservar memoria con `malloc` e inicializar una `struct`, devolviendo posteriormente el puntero al nuevo bloque. Si el método en Java no fuera estático (es decir, si fuera un miembro de instancia), se produciría un problema circular: sería necesario tener un objeto `Punto` previamente construido para poder invocar al método encargado de construir un `Punto`.

Adicionalmente, el interior del método encapsula la lógica de transformación requerida. Se hace uso de funciones matemáticas estándar para procesar los datos de entrada antes de delegar la instanciación real al constructor tradicional mediante el operador `new`. De este modo, el código externo queda liberado de la responsabilidad de realizar el redondeo manualmente antes de solicitar la creación del objeto, mejorando la legibilidad y centralizando las reglas de negocio.


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta   

Este ejercicio representa la demostración definitiva del poder de la **encapsulación** y la ocultación de información. Si en C se modificara una `struct Punto { double x; double y; }` para que pasara a ser `struct Punto { double coords[2]; }`, cualquier parte del código fuente que utilizara `punto.x` dejaría de compilar instantáneamente, obligando a reescribir y revisar el programa completo. En la Programación Orientada a Objetos, al haber ocultado la representación de los datos detrás de una interfaz pública, es posible alterar drásticamente el interior del objeto sin que el resto del sistema se vea afectado.

Para llevar a cabo esta modificación, se eliminan los atributos individuales y se sustituyen por una única referencia a un array (`private double[] coordenadas`). El aspecto crucial es que **la firma de los métodos públicos no cambia**. El constructor sigue recibiendo dos parámetros `double x` y `double y`, pero ahora su trabajo interno consiste en reservar memoria para el array (análogo a un pequeño `malloc` de dos posiciones) y almacenar esos valores en los índices `0` y `1`.

Del mismo modo, los métodos *getters* y las operaciones matemáticas se reescriben internamente para acceder a `coordenadas[0]` cuando se les pide la componente X, y a `coordenadas[1]` para la componente Y. El código externo que invoque a `punto.getX()` o a `new Punto(3.0, 4.0)` seguirá funcionando de forma transparente, ignorando por completo que el motor interno de la clase ha sido completamente rediseñado.

```java
public class Punto {
    // 1. Cambio en la representación interna (Oculto al exterior)
    private double[] coordenadas;

    // 2. La interfaz pública se mantiene INTACTA
    public Punto(double x, double y) {
        // El constructor adapta los parámetros a la nueva estructura
        this.coordenadas = new double[2];
        this.coordenadas[0] = x;
        this.coordenadas[1] = y;
    }

    // Los métodos públicos actúan como traductores hacia la nueva estructura interna
    public double getX() {
        return this.coordenadas[0];
    }

    public double getY() {
        return this.coordenadas[1];
    }

    public double calcularDistanciaAOrigen() {
        // La lógica interna usa el array, el exterior solo recibe el resultado final
        return Math.sqrt((this.coordenadas[0] * this.coordenadas[0]) + 
                         (this.coordenadas[1] * this.coordenadas[1]));
    }
}

```


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta    

La convención más extendida y recomendada en la Programación Orientada a Objetos es declarar siempre los atributos como `private`, incluso si se van a proporcionar métodos *getter* y *setter* públicos para todos ellos. Aunque a simple vista parezca redundante, existe una diferencia arquitectónica fundamental respecto a dejar el atributo `public` (como el comportamiento por defecto de los campos en una `struct` de C). Al exponer directamente una variable, se pierde por completo el control sobre quién y cuándo accede a ella, imposibilitando la intercepción de estas operaciones. Los métodos de acceso actúan como una frontera o capa de intermediación obligatoria entre el mundo exterior y los datos internos.

Esta práctica está íntimamente ligada a la preservación de las **invariantes de clase**. Una invariante es una regla lógica que define la validez del estado de un objeto. Si un atributo es público, cualquier código externo podría asignarle un valor que corrompa dicho estado (por ejemplo, asignar una longitud negativa o un puntero nulo no esperado). Al utilizar un *setter*, se proporciona un embudo o punto de control centralizado donde se pueden evaluar condicionales (`if`) para validar los datos antes de escribirlos en la memoria del objeto. Si el dato rompe la invariante, el *setter* puede rechazar la operación o lanzar un error, garantizando que la instancia nunca adquiera un estado inconsistente.

Además de la seguridad lógica, mantener los atributos privados mediante *getters* y *setters* proporciona una enorme flexibilidad para el mantenimiento y evolución del software. Al igual que se demostró en el ejercicio anterior al cambiar las coordenadas individuales por un array, encapsular el acceso permite modificar la estructura interna de los datos sin afectar al exterior. También permite añadir comportamiento dinámico en el futuro, como calcular un valor al vuelo en el *getter* en lugar de almacenarlo, o registrar en un archivo de *log* cada vez que se modifica una variable mediante el *setter*, todo ello sin necesidad de alterar ni una sola línea del código cliente que utiliza la clase.


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta    

Una clase es **inmutable** cuando el estado de sus objetos (el valor de sus atributos) no puede ser modificado en ningún momento tras su creación e inicialización. Desde la perspectiva de C, se asemeja a declarar una variable de tipo `struct` acompañada del calificador `const` tras haberle asignado sus valores iniciales: se permite la lectura de sus campos, pero cualquier intento de sobrescribir esa región de memoria es bloqueado. En Java, esto se logra habitualmente declarando todos los atributos como `private` y `final` (el equivalente más cercano a `const` para variables), y omitiendo de la interfaz pública cualquier método que ofrezca la posibilidad de alterar dichos datos una vez que el constructor ha terminado de ejecutarse.

Un **método modificador** (o mutador) es cualquier función perteneciente a la clase que altera el estado interno de la instancia, cambiando el valor de uno o más atributos en el *heap*. Aunque los *setters* (como `setX(double x)`) son el ejemplo más elemental de métodos modificadores, **no todos los modificadores son *setters***. Un *setter* tiene el propósito exclusivo de reemplazar directamente el valor de un único atributo. Por el contrario, un método como `desplazar(double dx, double dy)` aplicado a un `Punto` también es un modificador, ya que altera las coordenadas mediante una operación matemática combinada, sin limitarse a una simple asignación. Por definición, una clase verdaderamente inmutable carece por completo de métodos modificadores.

El diseño basado en clases inmutables proporciona ventajas estructurales y de seguridad muy notables. La principal es la eliminación de efectos colaterales imprevistos: al pasar la referencia de un objeto inmutable como argumento a otra función, existe la certeza matemática de que dicha función no corromperá ni alterará los datos originales. Esto evita la necesidad de realizar "copias defensivas" (el equivalente en C a pasar una `struct` grande por valor en lugar de por puntero solo para protegerla de cambios). Adicionalmente, la inmutabilidad es la base de la programación concurrente segura; al ser los datos de solo lectura, múltiples hilos de ejecución pueden acceder a la misma dirección de memoria simultáneamente sin requerir bloqueos de exclusión mutua (*mutex*) ni provocar condiciones de carrera (*race conditions*).

A continuación se muestra cómo se implementaría un punto inmutable. Obsérvese que, en lugar de modificar el objeto actual, las operaciones que lógicamente requerirían un cambio de estado devuelven una nueva instancia.

```java
public class PuntoInmutable {
    // Atributos privados y marcados como 'final' (constantes una vez inicializados)
    private final double x;
    private final double y;

    // El estado del objeto queda sellado al finalizar el constructor
    public PuntoInmutable(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Solo se proporcionan métodos de lectura (Getters)
    public double getX() { return this.x; }
    public double getY() { return this.y; }

    // En lugar de ser un método modificador que cambie 'this.x',
    // se crea y retorna un NUEVO bloque de memoria con el resultado.
    public PuntoInmutable desplazar(double dx, double dy) {
        return new PuntoInmutable(this.x + dx, this.y + dy);
    }
}

```


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta    

No, no es recomendable incluir métodos *setter* sistemáticamente ni por convención para todos los atributos de una clase. Hacerlo genera una falsa ilusión de encapsulación: si todo atributo privado posee un *getter* y un *setter* públicos que simplemente leen y asignan el valor sin lógica adicional, la clase se vuelve funcionalmente equivalente a una `struct` pública tradicional en C. El estado interno queda expuesto a manipulaciones arbitrarias desde cualquier parte del programa, anulando el principio fundamental de ocultar y proteger los detalles de implementación.

En el diseño riguroso orientado a objetos, es preferible exponer **comportamiento** en lugar de exponer memoria directa. En vez de proporcionar un *setter* genérico que permita sobrescribir una variable, se recomienda diseñar métodos que representen acciones o procesos reales. Por ejemplo, en una clase que represente una cuenta bancaria, proporcionar un `setSaldo(double nuevoSaldo)` es arquitectónicamente peligroso; la aproximación correcta es ofrecer métodos funcionales como `depositar(double cantidad)` o `retirar(double cantidad)`. De esta forma, el objeto mantiene el control absoluto sobre cómo evoluciona su estado interno y se garantiza el cumplimiento de las invariantes.

Además, la generación automática de *setters* atenta contra la inmutabilidad parcial de los objetos. Existen numerosos atributos, como un número de identificación (DNI), un identificador único en base de datos (ID) o una fecha de creación, que deben inicializarse exclusivamente a través del constructor y permanecer inalterables durante toda la vida de la instancia en el *heap*. Proveer un método modificador para estos campos abriría una brecha lógica, permitiendo alteraciones indebidas. Por consiguiente, la inclusión de un *setter* debe ser una decisión restrictiva, aplicable únicamente a aquellos datos que el modelo de negocio exija explícitamente modificar a lo largo del tiempo.


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta    

### La Inmutabilidad de `String`

En Java, la clase `String` es estrictamente **inmutable**. Una vez que se instancia un objeto de este tipo en la memoria, su secuencia interna de caracteres no puede ser alterada bajo ninguna circunstancia. Esta característica contrasta fuertemente con el lenguaje C, donde las cadenas de texto son simples secuencias en arreglos de caracteres (`char[]`) o punteros a memoria (`char *`) que pueden ser modificados posición por posición (por ejemplo, `cadena[0] = 'H';`). En la clase `String` de Java, el arreglo interno que almacena los caracteres está encapsulado y marcado como constante (`final`), lo que garantiza la integridad del dato frente a modificaciones accidentales o accesos concurrentes.

### El Proceso de Concatenación

Debido a esta inmutabilidad estructural, cuando se realiza una operación de concatenación entre dos cadenas (usando el operador `+` o el método `concat()`), los objetos originales no sufren ninguna modificación. En su lugar, la Máquina Virtual de Java reserva un nuevo bloque de memoria en el *Heap*, copia los caracteres de las cadenas originales en esta nueva ubicación y devuelve una referencia al nuevo objeto `String` resultante. Los objetos originales permanecen intactos; si las variables dejan de apuntar a ellos, quedarán huérfanos y la memoria que ocupan será liberada automáticamente por el Recolector de Basura (el equivalente a realizar un `free()` automático en C).

### Construcción Eficiente: `StringBuilder`

Si un algoritmo requiere construir una cadena muy larga paso a paso, como al leer un archivo grande o al concatenar texto dentro de un bucle extenso, utilizar la clase `String` con el operador `+` resulta extremadamente ineficiente. Esta práctica forzaría la creación y posterior destrucción de miles de objetos temporales en la memoria, saturando el sistema. Para resolver este problema, se debe utilizar la clase **`StringBuilder`**. Esta clase funciona como un búfer de texto mutable, operando de manera muy similar a un arreglo dinámico gestionado con `malloc` y `realloc` en C: permite añadir nuevos caracteres al final del espacio de memoria existente sin necesidad de instanciar un objeto nuevo en cada iteración.

```java
// Ejemplo de construcción eficiente de cadenas
public static String construirTextoLargo(int iteraciones) {
    // Se reserva un búfer mutable en memoria
    StringBuilder constructor = new StringBuilder();

    for (int i = 0; i < iteraciones; i++) {
        // 'append' modifica el búfer interno directamente sin crear nuevos objetos
        constructor.append("Línea ").append(i).append("\n");
    }

    // Finalmente, se convierte el búfer mutable a un String inmutable convencional
    return constructor.toString();
}

```


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta    

En la Programación Orientada a Objetos, la comparación de dos instancias puede realizarse bajo dos enfoques: por su **identidad** o por su **contenido**. La comparación por identidad verifica si dos variables apuntan exactamente a la misma ubicación de memoria en el *heap*; en C, esto es equivalente a evaluar si dos punteros almacenan la misma dirección (`puntero1 == puntero2`). Por otro lado, la comparación por contenido evalúa si los atributos internos de dos objetos ubicados en distintas posiciones de memoria poseen los mismos valores (similar a comparar manualmente campo por campo dos estructuras `struct` independientes). En Java, el operador relacional `==` se emplea exclusivamente para comparar la identidad (las referencias) cuando se aplica a objetos.

Para realizar comparaciones basadas en el contenido, Java proporciona el método **`equals()`**, el cual es heredado por todas las clases desde la superclase cósmica `Object`. Sin embargo, el comportamiento por defecto de este método base es idéntico al del operador `==`: únicamente compara las direcciones de memoria. Para que `equals()` compare verdaderamente el estado interno (por ejemplo, verificar si dos instancias distintas de `Punto` comparten las mismas coordenadas `x` e `y`), es indispensable que el desarrollador sobrescriba (redefina) este método dentro de su clase, estableciendo explícitamente la lógica de comparación campo por campo.

Este mecanismo es de vital importancia al trabajar con texto. Dado que en Java las cadenas son objetos de la clase `String`, utilizar el operador `==` para comparar dos textos constituye un error lógico grave, matemáticamente análogo a comparar dos punteros `char *` en C en lugar de invocar a la función estandarizada `strcmp()`. Dos variables `String` que contienen exactamente la misma palabra pueden residir en espacios de memoria completamente distintos. Por consiguiente, la comparación de cadenas en Java debe efectuarse **siempre** mediante el método `equals()` (por ejemplo, `cadena1.equals(cadena2)`), cuya implementación interna ya ha sido sobrescrita por el lenguaje para recorrer y validar ambos arreglos de caracteres posición por posición.

```java
public class ComparacionEjemplo {
    public static void main(String[] args) {
        // En C: char *s1 = malloc(...); strcpy(s1, "Hola");
        String texto1 = new String("Hola");
        String texto2 = new String("Hola");

        // Comparación de Identidad (Punteros)
        if (texto1 == texto2) {
            System.out.println("Misma memoria"); // NO se imprimirá
        }

        // Comparación de Contenido (Valores internos, como strcmp)
        if (texto1.equals(texto2)) {
            System.out.println("Mismo texto"); // SÍ se imprimirá
        }
    }
}

```


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta    

### El Concepto de Clase Envoltorio (Wrapper)

En los lenguajes de programación orientados a objetos que mantienen tipos de datos primitivos (como `int`, `float` o `char` en C), las **clases *wrapper*** o envolventes son clases diseñadas específicamente para encapsular uno de estos valores primitivos crudos dentro de un objeto. Desde la perspectiva de C, esto equivale a definir una `struct Entero { int valor; }` y dotarla de funciones asociadas. En Java, por cada tipo primitivo existe su clase envoltorio correspondiente, generalmente con la inicial en mayúscula (por ejemplo, `Integer` para `int`, `Double` para `double`, o `Character` para `char`). Esto permite tratar a un simple número en la memoria de la pila (*stack*) como si fuera una instancia completa con estado y comportamiento en el *heap*.

### Automatización: Autoboxing y Unboxing

El proceso de conversión entre un primitivo y su objeto envoltorio está altamente automatizado en las versiones modernas de Java mediante un mecanismo conocido como **autoboxing** (empaquetado automático) y **unboxing** (desempaquetado). El compilador detecta cuándo se requiere un objeto pero se proporciona un primitivo crudo (por ejemplo, al asignar `Integer numero = 5;`), e inyecta silenciosamente el código necesario para instanciar el objeto en memoria y guardar el valor. De manera inversa, si se opera matemáticamente con el objeto (`numero + 10`), el compilador extrae automáticamente el valor primitivo subyacente para que la CPU pueda realizar la suma aritmética tradicional.

### Ventajas y Necesidad de los Wrappers

La principal ventaja de estas clases es que permiten utilizar valores básicos en contextos arquitectónicos que exigen estrictamente el uso de objetos. Por ejemplo, las estructuras de datos dinámicas de la biblioteca estándar de Java (como `ArrayList`, que actúan como arreglos de tamaño variable gestionados automáticamente) están diseñadas para almacenar únicamente referencias a objetos; es imposible guardar un `int` nativo en ellas, pero sí se puede almacenar un objeto `Integer`. Adicionalmente, estas clases actúan como bibliotecas de utilidades, agrupando métodos estáticos relacionados con su tipo de dato, como la conversión de cadenas de texto a números (funcionalidad equivalente a la función `atoi()` o `atof()` de la librería `<stdlib.h>` en C).

### Comparación con otros lenguajes

No todos los lenguajes orientados a objetos poseen tipos primitivos ni requieren clases envoltorio. Java y C++ son lenguajes híbridos: conservaron los tipos primitivos nativos heredados de C por razones estrictas de eficiencia y rendimiento (ocupan menos memoria y operan más rápido). Sin embargo, lenguajes orientados a objetos "puros", como Ruby, Python o Smalltalk, no hacen esta distinción arquitectónica. En estos entornos, absolutamente todo es un objeto desde su concepción, incluyendo el número `1` o el valor lógico `true`, por lo que el concepto de clase envoltorio resulta innecesario al no existir una brecha entre datos procedurales y objetos.

```java
public class EjemploWrappers {
    public static void main(String[] args) {
        // Autoboxing: El compilador transforma el primitivo '100' en un objeto Integer
        Integer numeroObjeto = 100;

        // Unboxing: El compilador extrae el 'int' interno para la operación matemática
        int primitivo = numeroObjeto + 50;

        // Uso de métodos de utilidad proporcionados por la clase Wrapper estáticamente
        // Similar a utilizar atoi() en C para convertir texto a entero
        String texto = "250";
        int convertido = Integer.parseInt(texto);
    }
}

```


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta   

### Definición de Tipo Enumerado

Un **tipo de dato enumerado** (`enum`) en la Programación Orientada a Objetos es un tipo especial que representa un grupo fijo y cerrado de constantes. Se utiliza cuando una variable solo puede tomar uno de un conjunto predefinido de valores posibles (por ejemplo, los días de la semana, los palos de una baraja o los estados de un proceso: `INICIADO`, `EN_CURSO`, `FINALIZADO`).

En el lenguaje C, un `enum` es fundamentalmente una abstracción cosmética: el compilador lo trata en el fondo como simples números enteros (`int`), empezando por el 0. Esto significa que a una variable de tipo `enum DiaSemana` se le puede asignar accidentalmente el valor `99` sin que el compilador proteste, lo que rompe la lógica del programa.

### El Enum en Java: Una Clase Especial

En respuesta a tu pregunta: **sí, en Java un enumerado es una clase**. Esta es una de las diferencias más radicales respecto a C.

Cuando declaras un `enum` en Java, el compilador crea internamente una clase real que hereda automáticamente de `java.lang.Enum`. Cada uno de los valores que defines dentro del enumerado (como `LUNES`, `MARTES`, etc.) no es un simple número entero, sino que es una **instancia real (un objeto)** de esa misma clase, declarada implícitamente como `public static final`.

### Ventajas de Encapsulación

Que los enumerados sean clases completas aporta ventajas arquitectónicas masivas en términos de encapsulación y seguridad:

1. **Seguridad de Tipos (Type Safety) estricta:** A diferencia de C, es absolutamente imposible asignar un valor fuera de los definidos. Si un método espera un objeto `DiaSemana`, no puedes pasarle un número ni un string; debes pasarle estrictamente `DiaSemana.LUNES`. Esto elimina de raíz toda una categoría de errores (*bugs*).
2. **Estado Interno Oculto:** Al ser clases, los enumerados en Java pueden tener sus propios **atributos privados**, **constructores privados** y **métodos (getters)**. Esto permite asociar datos complejos a cada constante, encapsulando la información en el propio tipo de dato en lugar de usar sentencias `switch` dispersas por todo el código.

A continuación, un ejemplo de cómo un enumerado en Java encapsula información interna, algo imposible en un `enum` tradicional de C:

```java
public enum Moneda {
    // Estas son instancias de la clase Moneda. 
    // Llaman al constructor privado internamente.
    CENTIMO(1),
    NICKEL(5),
    DIME(10),
    QUARTER(25);

    // 1. Atributo privado encapsulado
    private final int valorEnCentavos;

    // 2. Constructor privado (no se puede usar 'new Moneda()' desde fuera)
    private Moneda(int valor) {
        this.valorEnCentavos = valor;
    }

    // 3. Método público para acceder a la información
    public int getValorEnCentavos() {
        return this.valorEnCentavos;
    }
}

```

```java
// Ejemplo de uso
public class UsoEnum {
    public static void main(String[] args) {
        Moneda miMoneda = Moneda.QUARTER;
        // Obtenemos el dato encapsulado directamente del objeto enumerado
        System.out.println("Valor: " + miMoneda.getValorEnCentavos() + " centavos"); 
    }
}

```


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta   

A continuación se presenta la implementación del tipo enumerado `Mes` en Java, aplicando los conceptos de encapsulación mediante atributos privados y constructores.

En Java, los enumerados son clases especiales. Por lo tanto, podemos definir atributos (`dias` y `ordinalAno`), un constructor para inicializar esos valores cuando se crea cada constante (como `ENERO` o `FEBRERO`), y métodos públicos (*getters*) para acceder a dicha información.

```java
public enum Mes {
    // 1. Declaración de las 12 instancias (objetos constantes)
    // Se invoca al constructor privado pasándole los días y el ordinal
    ENERO(31, 1),
    FEBRERO(28, 2), // Nota: Se asume un año no bisiesto por simplicidad
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    // 2. Atributos privados y finales (inmutables tras la creación)
    private final int dias;
    private final int ordinalAno;

    // 3. Constructor del enumerado
    // En Java, el constructor de un enum es siempre privado implícitamente.
    // Solo se ejecuta una vez al cargar la clase para instanciar las constantes definidas arriba.
    private Mes(int dias, int ordinalAno) {
        this.dias = dias;
        this.ordinalAno = ordinalAno;
    }

    // 4. Interfaz pública: Métodos de acceso (Getters)
    public int getDias() {
        return this.dias;
    }

    public int getOrdinalAno() {
        return this.ordinalAno;
    }
}

```

### Detalles Arquitectónicos del Ejemplo

* **Inmutabilidad garantizada:** Al declarar los atributos `dias` y `ordinalAno` como `private final`, nos aseguramos de que el estado de cada mes no pueda ser modificado accidentalmente durante la ejecución del programa. Sería imposible hacer algo como `Mes.ENERO.dias = 50;`.
* **Encapsulación de la lógica:** En un lenguaje como C, para saber los días de un mes probablemente tendrías que escribir una función separada con un enorme bloque `switch` que evalúe el `enum`. En Java, el propio objeto `Mes` "sabe" cuántos días tiene. Si necesitas obtener los días de un mes, simplemente llamas a `miMes.getDias()`, delegando la responsabilidad al propio objeto.
* **El método implícito `ordinal()` vs atributo personalizado:** Es importante destacar que todos los enumerados en Java heredan un método llamado `ordinal()` que devuelve su posición (empezando por 0). Sin embargo, crear un atributo propio `ordinalAno` (1-12) como se solicita, es una excelente práctica para desvincular la lógica de negocio del orden en que se han escrito las variables en el código, evitando errores si alguien decide reordenar las líneas en el futuro.


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta   

Para resolver este problema de forma elegante, debemos tener en cuenta dos factores astronómicos reales: primero, los meses de transición (marzo, junio, septiembre y diciembre) contienen días de **dos** estaciones diferentes (por ejemplo, marzo tiene días de invierno y días de primavera); segundo, las estaciones en el hemisferio sur son exactamente las opuestas a las del hemisferio norte.

Para mantener el código limpio y seguir el principio **DRY** (*Don't Repeat Yourself* - No te repitas), en lugar de escribir toda la lógica dos veces, calcularemos las estaciones para el hemisferio norte y, si el parámetro `esHemisferioNorte` es falso, simplemente delegaremos la respuesta a la estación opuesta.

A continuación tienes la clase `Mes` actualizada con esta lógica:

```java
public enum Mes {
    ENERO(31, 1),
    FEBRERO(28, 2),
    MARZO(31, 3),
    ABRIL(30, 4),
    MAYO(31, 5),
    JUNIO(30, 6),
    JULIO(31, 7),
    AGOSTO(31, 8),
    SEPTIEMBRE(30, 9),
    OCTUBRE(31, 10),
    NOVIEMBRE(30, 11),
    DICIEMBRE(31, 12);

    private final int dias;
    private final int ordinalAno;

    private Mes(int dias, int ordinalAno) {
        this.dias = dias;
        this.ordinalAno = ordinalAno;
    }

    public int getDias() { return this.dias; }
    public int getOrdinalAno() { return this.ordinalAno; }

    // --- Nuevos métodos de estaciones ---

    public boolean esDePrimavera(boolean esHemisferioNorte) {
        if (!esHemisferioNorte) return esDeOtoño(true); // En el sur, la primavera es el otoño del norte
        
        // Norte: del 20 de marzo al 21 de junio
        return this == MARZO || this == ABRIL || this == MAYO || this == JUNIO;
    }

    public boolean esDeVerano(boolean esHemisferioNorte) {
        if (!esHemisferioNorte) return esDeInvierno(true); // En el sur, el verano es el invierno del norte
        
        // Norte: del 21 de junio al 22 de septiembre
        return this == JUNIO || this == JULIO || this == AGOSTO || this == SEPTIEMBRE;
    }

    public boolean esDeOtoño(boolean esHemisferioNorte) {
        if (!esHemisferioNorte) return esDePrimavera(true); // En el sur, el otoño es la primavera del norte
        
        // Norte: del 22 de septiembre al 21 de diciembre
        return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE || this == DICIEMBRE;
    }

    public boolean esDeInvierno(boolean esHemisferioNorte) {
        if (!esHemisferioNorte) return esDeVerano(true); // En el sur, el invierno es el verano del norte
        
        // Norte: del 21 de diciembre al 20 de marzo
        return this == DICIEMBRE || this == ENERO || this == FEBRERO || this == MARZO;
    }
}

```

### Por qué este diseño es ventajoso

Al encapsular esta lógica dentro del propio enumerado, cualquier parte de tu programa puede hacer consultas semánticas y legibles como `if (Mes.MARZO.esDePrimavera(true)) { ... }` sin necesidad de conocer las fechas de los equinoccios o solsticios. El objeto `Mes` actúa como un experto en su propia información.

