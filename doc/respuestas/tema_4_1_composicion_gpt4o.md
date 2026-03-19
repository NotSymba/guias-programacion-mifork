<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

### Respuesta    

La composición en lenguajes procedimentales como C se logra anidando tipos de datos mediante la palabra clave `struct`. Al definir una estructura base y utilizarla como miembro dentro de otra estructura más compleja, se establece una relación de pertenencia explícita (el "tiene-un"). Este enfoque permite construir abstracciones mayores a partir de piezas modulares, organizando la memoria de forma contigua sin requerir los metadatos o las restricciones de acceso inherentes a las clases y objetos.

```c
#include <stdio.h>
#include <math.h>

// Estructura fundamental
typedef struct {
    double x;
    double y;
} Punto;

// Estructura compuesta mediante la inclusión de otras estructuras
typedef struct {
    Punto inicio;
    Punto fin;
} Linea;

// Función para calcular la distancia euclídea entre dos puntos
double calcular_distancia(Punto p1, Punto p2) {
    return sqrt(pow(p2.x - p1.x, 2) + pow(p2.y - p1.y, 2));
}

// Función que aprovecha la composición para hallar la longitud
double longitud_linea(Linea l) {
    return calcular_distancia(l.inicio, l.fin);
}

```

El cálculo subyacente implementado en el código corresponde a la fórmula analítica de la distancia euclídea bidimensional:

$$d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$

Esta operación matemática se resuelve mediante la biblioteca estándar `<math.h>`, extrayendo las coordenadas directamente de los miembros de las estructuras. Al pasar las estructuras por valor a las funciones, se realiza una copia en memoria de los datos; una práctica eficiente para estructuras de datos pequeñas, pero que marca una diferencia operativa con Java, donde siempre se pasan referencias a los objetos.

A diferencia del paradigma orientado a objetos, en C la relación de composición carece de encapsulación protectora. Todos los campos (`x`, `y`, `inicio`, `fin`) son de acceso público por defecto y la lógica de negocio reside en funciones globales separadas de los datos. Esto subraya la evolución histórica hacia Java, donde estas mismas estructuras se fusionarían con sus funciones operativas bajo un esquema de acceso estrictamente controlado.

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

### Respuesta    

La transición de las estructuras de C a las clases en Java introduce la capacidad de unificar el estado (datos) y el comportamiento (métodos) en una misma entidad. Para lograr la inmutabilidad solicitada, se recurre a la encapsulación mediante el uso del modificador `private`, complementado con la palabra clave `final`. Esto garantiza que los atributos solo puedan ser asignados una única vez durante la construcción del objeto, protegiendo así las coordenadas de los puntos y los extremos de la línea frente a cualquier modificación posterior a su creación.

```java
public class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // El método opera sobre el estado interno del propio objeto (this)
    public double calcularDistancia(Punto otro) {
        return Math.sqrt(Math.pow(otro.x - this.x, 2) + Math.pow(otro.y - this.y, 2));
    }
}

public class Linea {
    // Composición: La línea "tiene un" punto de inicio y un punto de fin
    private final Punto inicio;
    private final Punto fin;

    public Linea(Punto inicio, Punto fin) {
        // Al ser Punto una clase inmutable, es seguro asignar las referencias directamente
        this.inicio = inicio;
        this.fin = fin;
    }

    public double calcularLongitud() {
        // Se delega el cálculo al método interno del objeto Punto compuesto
        return this.inicio.calcularDistancia(this.fin);
    }
}

```

En este diseño orientado a objetos, la composición se establece al declarar referencias a objetos `Punto` como atributos de la clase `Linea`. A diferencia de C, donde la memoria de las estructuras anidadas es contigua y se manejan copias por valor por defecto, en Java la clase contenedora almacena referencias (conceptualmente similares a punteros, pero gestionados automáticamente) hacia los objetos contenidos, los cuales residen en el espacio de memoria dinámico. Al no proporcionar métodos de alteración de estado (conocidos comúnmente como *setters*) y forzar la inicialización en el constructor, se asegura que la topología de la línea permanezca estrictamente inalterable.

Adicionalmente, se evidencia una clara ventaja en la distribución de responsabilidades y la ocultación de información. Las funciones globales del entorno estructurado se transforman en métodos propios de las entidades a las que pertenecen. El método `calcularLongitud` de la línea no realiza cálculos matemáticos directamente, sino que delega esa operación al objeto `Punto` interno, demostrando cómo la composición en la programación orientada a objetos fomenta la colaboración segura entre entidades mientras se esconden los detalles concretos de implementación.

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

### Respuesta    

Respuesta de clase:  Multiplicidad de A y B (p.ej: entre Linea y Punto). Con cuantos objetos esta relacionado un objeto como minimo y cuantos se relacionan con el objeto como máximo. En el caso de la linea y el punto la multiplicidad seria: 1 Linea se relaciona como mínimo con 2 Puntos y como máximo con 2 Puntos. Y 1 Punto se relaciona como mínimo con 0 Lineas y como máximo con muchas Lineas.

La **multiplicidad** en la composición define la cantidad exacta de instancias de una clase que están vinculadas a una única instancia de otra clase dentro de una relación estructural.  En el diseño orientado a objetos, este concepto cuantifica la relación de pertenencia, especificando si un objeto contenedor aloja exactamente un elemento, un número fijo estricto o una cantidad variable de sub-objetos (de forma análoga a cómo en C++ se puede tener un único puntero o un arreglo dinámico de estructuras). Esencialmente, dicta las reglas de cardinalidad que rigen la conexión entre el "todo" y sus "partes".

En el ejemplo de código desarrollado, la multiplicidad dirigida desde la clase `Linea` hacia la clase `Punto` es exactamente **2**. Al inspeccionar la estructura de `Linea`, se hace evidente que cada instancia requiere de manera obligatoria un punto para el atributo `inicio` y otro para el atributo `fin`. Una línea, bajo este diseño específico, no puede ser instanciada con un único punto ni puede contener una lista infinita de ellos; la encapsulación y el constructor imponen una cardinalidad rígida donde un todo (la línea) se compone de exactamente dos partes (los puntos).

Si se analiza la relación en la dirección inversa, es decir, desde la clase `Punto` hacia la clase `Linea`, la multiplicidad en la implementación actual es **0**. La clase `Punto` es una entidad completamente independiente y no contiene ninguna referencia, puntero o atributo que la vincule de vuelta con la línea que la contiene. El objeto `Punto` ignora por completo si está siendo utilizado por una línea, por un triángulo o si existe de manera aislada. Aunque teóricamente un punto en el espacio podría pertenecer a múltiples líneas (una multiplicidad de **0..***), a nivel de esta implementación bidireccional en Java, la composición es estrictamente unidireccional: el contenedor conoce a sus componentes, pero los componentes no conocen a su contenedor.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

### Respuesta    

Respuesta de clase: Composicion fuerte vs débil. La fuerte -> El contenedor (p.ej Linea) es el que crea los objetos que contiene (p.ej Punto) y estos no vienen más allá del contenedor. El ciclo de vida del contenido está vinculado al contenedor. La débil -> El contenedor y contenido tienen ciclos de vida independientes(p.ej Los objetos Punto pueden vivir sin estar en objetos Linea).

En el diseño orientado a objetos, la diferencia entre una relación fuerte y una débil radica en el grado de dependencia lógica e independencia existencial entre el objeto contenedor (el "todo") y los objetos contenidos (las "partes").  Una relación débil implica que los componentes tienen significado y utilidad por sí mismos, pudiendo existir de forma autónoma en el sistema independientemente del contenedor. Por el contrario, una relación fuerte establece que las partes son intrínsecas e inseparables del todo, careciendo de sentido o de vida útil si el objeto principal desaparece.

Esta distinción tiene una consecuencia directa y fundamental sobre el ciclo de vida de los objetos gestionados en la memoria. En una relación fuerte, el contenedor es el único responsable de la creación y destrucción de sus partes; en términos de Java, cuando el objeto principal deja de ser referenciado y es eliminado por el recolector de basura (equivalente a liberar la memoria dinámicamente en C), sus sub-objetos exclusivos también son destruidos simultáneamente porque nadie más tiene acceso a ellos. En una relación débil, las instancias contenidas suelen crearse en el exterior y se pasan al contenedor (por ejemplo, inyectando las referencias a través del constructor), por lo que la desaparición del contenedor no provoca la destrucción de sus partes, permitiendo que estas sigan siendo utilizadas por otras secciones del programa.

En la terminología formal de diseño (como en el Lenguaje Unificado de Modelado o UML), a la relación débil se le conoce comúnmente como **agregación** (o asociación, en un sentido conceptual más amplio), describiendo una semántica donde el contenedor simplemente "agrupa" elementos independientes. A la relación fuerte se le denomina estrictamente **composición**, representando la conexión pura donde un objeto "está hecho de" otros. Mientras que en la agregación los objetos pueden compartir referencias de manera segura, en la composición propiamente dicha se garantiza mediante la encapsulación que las partes no sean accesibles ni compartidas con ningún otro objeto del sistema.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

### Respuesta    

Respuesta de clase: La dependencia p.ej en punto depende de String y StringBuilder. Aparece en un nivel más bajo que la composición.

Cuando una clase interactúa con otra de forma temporal —ya sea recibiéndola como parámetro, devolviéndola como resultado, instanciándola con `new` dentro de un método o empleándola como variable local—, se establece una relación que se define estrictamente como **dependencia** (a menudo denominada relación de "uso"), y no como composición. La composición exige que la conexión sea estructural y permanente, lo que en Java se traduce en declarar la referencia a la otra clase como un atributo a nivel de la clase principal, definiendo así su estado a largo plazo.

Para establecer un paralelismo con el lenguaje C, la dependencia es equivalente a declarar una variable `struct` local dentro de una función o pasar una estructura como argumento para un cálculo puntual. En ningún momento esa estructura temporal pasa a formar parte de la definición organizativa de otra estructura mayor. En la orientación a objetos, esto implica que la relación es efímera: existe únicamente durante el tiempo de ejecución de ese método en particular y, una vez que la ejecución del bloque de código finaliza, las referencias locales desaparecen de la pila de llamadas (stack).

Desde la perspectiva del diseño y la encapsulación, la dependencia representa el nivel más débil de acoplamiento entre dos clases. Al no almacenar la referencia del objeto externo como un campo propio, la clase principal no se hace responsable del ciclo de vida de ese objeto ni lo incorpora como parte de su identidad central. Se trata simplemente de una colaboración transitoria donde una entidad solicita los servicios de otra, o delega en ella una tarea específica, como podría ser procesar un cálculo aritmético o lanzar una excepción validando un dato de entrada.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

### Respuesta    

Respuesta de clase: 

Para implementar estas dos vertientes arquitectónicas en Java, la diferencia fundamental radica en el lugar donde se instancian los objetos mediante la palabra reservada `new`. En una composición fuerte, la clase contenedora asume la responsabilidad total de crear sus componentes internos, garantizando que ninguna otra parte del sistema posea una referencia a ellos. Por el contrario, en una composición débil (frecuentemente llamada agregación), la clase contenedora recibe referencias a objetos que ya han sido creados previamente en el exterior, limitándose a almacenar dichas direcciones de memoria compartidas.

A continuación, se presentan ambas implementaciones, asumiendo la existencia previa de la clase `Punto`:

```java
// Ejemplo 1: Composición Fuerte
public class LineaFuerte {
    private final Punto inicio;
    private final Punto fin;

    // El constructor recibe los datos primitivos y crea los objetos internamente
    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.inicio = new Punto(x1, y1);
        this.fin = new Punto(x2, y2);
    }

    public double calcularLongitud() {
        return this.inicio.calcularDistancia(this.fin);
    }
}

// Ejemplo 2: Composición Débil (Agregación)
public class LineaDebil {
    private final Punto inicio;
    private final Punto fin;

    // El constructor recibe referencias a objetos que ya existen fuera de la línea
    public LineaDebil(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double calcularLongitud() {
        return this.inicio.calcularDistancia(this.fin);
    }
}

```

En el modelo de composición fuerte, que se asemeja al comportamiento de las estructuras anidadas por valor en C, el ciclo de vida está estrictamente acoplado. Al solicitar las coordenadas primitivas y ejecutar la instanciación dentro del constructor de `LineaFuerte`, se asegura que los puntos nazcan y mueran con la línea. Gracias a la estricta encapsulación, al no existir métodos que devuelvan esas referencias hacia el exterior, cuando el objeto `LineaFuerte` sea recolectado por el gestor de memoria de Java (Garbage Collector), sus componentes internos también serán destruidos automáticamente.

Por otro lado, la composición débil imita la lógica de almacenar punteros a estructuras externas en un entorno de C/C++. En `LineaDebil`, el constructor acepta referencias a objetos `Punto` inyectados desde fuera. Esto significa que el módulo que invocó al constructor retiene sus propias referencias hacia esos mismos puntos. En consecuencia, si el objeto `LineaDebil` deja de ser útil y su memoria es liberada, los objetos `Punto` originales continuarán existiendo intactos y disponibles para ser usados por otras líneas o figuras, asegurando su independencia existencial.

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

### Respuesta    

Respuesta de clase: En java, la vida de Punto termina cuando es inaccesible, y en el ejemplo, ocurre cuando Linea deja de serlo a su vez. Por tanto, cuando Linea "es basura, tambien lo seran sus puntos y seran eliminados de memoria por el recolector de basura"

En Java, a diferencia de lenguajes como C o C++ donde se exige la liberación manual de la memoria (mediante funciones como `free` o el operador `delete`), la gestión del ciclo de vida de los objetos está automatizada. Este proceso se lleva a cabo mediante un componente de la Máquina Virtual de Java (JVM) llamado Recolector de Basura (*Garbage Collector*). Por este motivo, en el código de la clase `Linea` no se observa ninguna instrucción explícita para destruir los objetos `Punto`, ya que en Java no existen los destructores convencionales y la memoria no se gestiona de forma manual.

La mecánica de la composición fuerte en Java se apoya íntegramente en el control de las referencias hacia los objetos en la memoria dinámica (*heap*).  Cuando se instancia una `Linea` utilizando composición fuerte, esta encapsula y retiene las únicas referencias válidas hacia sus objetos `Punto` internos. El Recolector de Basura funciona monitoreando continuamente qué objetos siguen siendo accesibles (o "alcanzables") desde el flujo de ejecución activo del programa. Si la instancia de `Linea` deja de utilizarse (por ejemplo, cuando la variable local que la apuntaba sale de su ámbito de ejecución), el recolector marca dicha línea como elegible para ser eliminada.

El efecto de esta recolección es una destrucción en cascada. Al eliminar la `Linea`, las referencias internas hacia los objetos `Punto` desaparecen. Puesto que la composición fuerte garantiza (gracias a la encapsulación con `private`) que ningún otro componente del sistema tiene referencias hacia esos puntos específicos, estos también se vuelven inalcanzables en ese mismo instante. Consecuentemente, el entorno de ejecución de Java libera la memoria del contenedor y de sus componentes asociados en el mismo ciclo, cumpliendo el principio de que "las partes mueren con el todo" de forma completamente transparente y sin riesgo de fugas de memoria.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

### Respuesta    

Respuesta de clase: Hay 2 composiciones débiles. No se exopne el array al exterior(imposible garantizar la invariante de clase). En los métodos que gestionan el departamento se controla que no se viole la invariante de clase.

La implementación de una composición débil (agregación) requiere diseñar la clase contenedora de manera que gestione referencias a objetos preexistentes, protegiendo al mismo tiempo sus reglas de negocio. En este escenario, el `Departamento` actúa como gestor de un conjunto de objetos `Profesor`. A diferencia de C, donde un arreglo de punteros a estructuras a menudo queda expuesto para su manipulación directa, aquí se utiliza un arreglo primitivo estrictamente privado. La ocultación de información se logra ofreciendo únicamente métodos controlados para interactuar con el arreglo, previniendo desbordamientos de búfer y accesos a memoria no inicializada.

Para mantener la invariante de clase que exige la presencia continua de un director que a su vez sea profesor del departamento, se recurre al uso de constructores y excepciones. El constructor obliga a recibir un profesor inicial que asume ambos roles simultáneamente. Durante las operaciones de modificación, como la eliminación de un docente o el relevo en la dirección, se lanzan excepciones (`IllegalArgumentException` o `IllegalStateException`) si la acción amenaza con dejar al departamento sin director o si se intenta nombrar a un profesor externo. Esta validación activa reemplaza los controles manuales de errores y códigos de retorno tradicionales de C.

```java
public class Profesor {
    // Implementación omitida: contiene los datos básicos del docente
}

public class Departamento {
    private final Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Se requiere un director inicial.");
        }
        this.profesores = new Profesor[50];
        this.numProfesores = 0;
        
        // Se asegura la invariante desde la creación
        this.profesores[this.numProfesores++] = directorInicial;
        this.director = directorInicial;
    }

    public void anadirProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("El profesor no puede ser nulo.");
        }
        if (this.numProfesores >= 50) {
            throw new IllegalStateException("Se ha alcanzado la capacidad máxima (50).");
        }
        this.profesores[this.numProfesores++] = p;
    }

    public void eliminarProfesor(int indice) {
        if (indice < 0 || indice >= this.numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida.");
        }
        if (this.profesores[indice] == this.director) {
            throw new IllegalStateException("No se puede eliminar al director actual. Cambie la dirección primero.");
        }
        
        // Desplazamiento de elementos para evitar huecos en el arreglo (similar a C)
        for (int i = indice; i < this.numProfesores - 1; i++) {
            this.profesores[i] = this.profesores[i + 1];
        }
        this.profesores[--this.numProfesores] = null; // Se elimina la referencia sobrante
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El nuevo director no puede ser nulo.");
        }
        boolean pertenece = false;
        for (int i = 0; i < this.numProfesores; i++) {
            if (this.profesores[i] == nuevoDirector) {
                pertenece = true;
                break;
            }
        }
        if (!pertenece) {
            throw new IllegalArgumentException("El director debe ser un profesor del departamento.");
        }
        this.director = nuevoDirector;
    }

    public int getCantidadProfesores() {
        return this.numProfesores;
    }

    public Profesor getProfesor(int indice) {
        if (indice < 0 || indice >= this.numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida.");
        }
        return this.profesores[indice];
    }
}

```

Es importante observar cómo el método `eliminarProfesor` gestiona la memoria y la estructura de datos interna. Al igual que se haría en C al eliminar un elemento del medio de un arreglo estático, se desplazan los elementos subsecuentes una posición hacia la izquierda para mantener la contigüidad. Sin embargo, en Java es fundamental asignar `null` a la última posición que queda duplicada tras el desplazamiento; de lo contrario, el Recolector de Basura no podría liberar ese objeto `Profesor` si dejara de usarse en otras partes del programa, provocando una retención innecesaria de memoria.

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

### Respuesta    

Respuesta de clase: Con List<Profesor>: 1 No cambia la interfaz publica. 2 Es más facil implementar algunos métodos, delega en métodos de List. 3 Si se devuelve, hay que devolver una copia, para proteger la invariante de clase.

La transición de arreglos primitivos a la interfaz `List` (típicamente implementada mediante `ArrayList`) en Java abstrae la gestión manual de la memoria y el tamaño de la colección. Al emplear esta estructura, la clase contenedora delega las operaciones de inserción, eliminación y redimensionamiento, simplificando drásticamente la lógica interna necesaria para mantener la integridad de los datos, tal y como se observa en la siguiente implementación:

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Collections;

public class Departamento {
    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("Se requiere un director inicial.");
        }
        this.profesores = new ArrayList<>();
        this.profesores.add(directorInicial);
        this.director = directorInicial;
    }

    public void anadirProfesor(Profesor p) {
        if (p == null) {
            throw new IllegalArgumentException("El profesor no puede ser nulo.");
        }
        if (this.profesores.size() >= 50) {
            throw new IllegalStateException("Se ha alcanzado la capacidad máxima (50).");
        }
        this.profesores.add(p);
    }

    public void eliminarProfesor(int indice) {
        if (indice < 0 || indice >= this.profesores.size()) {
            throw new IndexOutOfBoundsException("Posición inválida.");
        }
        if (this.profesores.get(indice) == this.director) {
            throw new IllegalStateException("No se puede eliminar al director actual.");
        }
        this.profesores.remove(indice);
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null || !this.profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException("El nuevo director debe ser un profesor válido del departamento.");
        }
        this.director = nuevoDirector;
    }

    public int getCantidadProfesores() {
        return this.profesores.size();
    }

    public Profesor getProfesor(int indice) {
        if (indice < 0 || indice >= this.profesores.size()) {
            throw new IndexOutOfBoundsException("Posición inválida.");
        }
        return this.profesores.get(indice);
    }
}

```

Al utilizar `List`, se ha eliminado la necesidad de gestionar manualmente una variable contador (como `numProfesores`) para rastrear la ocupación del arreglo. En el método de eliminación, se ha ahorrado por completo el bucle `for` responsable de desplazar los elementos adyacentes para cubrir el hueco dejado por el elemento eliminado, así como la asignación explícita de `null` en la última posición. Estas tareas de manipulación de memoria en bloque son resueltas internamente por el método `remove`. Igualmente, la validación en `cambiarDirector` se reduce a una sola línea gracias al método `contains`.

Si existiera un método que devolviera directamente la referencia a `this.profesores`, se cometería una grave violación de la encapsulación conocida como fuga de referencias. Al entregar el objeto `List` interno, el código que invoca al método recibe acceso directo y total a la estructura subyacente. Esto permitiría a cualquier clase externa utilizar métodos como `add` o `remove` sobre esa lista, saltándose por completo las reglas de validación establecidas en la clase (como el límite máximo de 50 o la prohibición de eliminar al director), lo que corrompería irrevocablemente el estado de la composición.

Para resolver este problema y exponer la colección sin comprometer la seguridad de los datos, nunca se debe retornar la referencia original. La solución reside en devolver una "copia defensiva" instanciando una nueva lista con los mismos elementos (ej. `return new ArrayList<>(this.profesores);`), o mejor aún, devolver una vista inmutable utilizando las herramientas de Java, como `return Collections.unmodifiableList(this.profesores);`. Así, cualquier intento externo de alterar la lista devuelta lanzará una excepción sin afectar los datos privados del departamento.

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

### Respuesta   

La composición recursiva se produce cuando una clase define como atributo una referencia a un objeto de su propio tipo. Este patrón es conceptualmente idéntico a las estructuras auto-referenciadas en C, que utilizan punteros internos para apuntar a otra estructura de la misma definición (como en el caso de las listas). En el ecosistema de Java, de la misma manera que una excepción puede encapsular dentro de su estado interno a otra excepción que originó el error (conocida como su causa), una entidad puede estar compuesta por instancias de sí misma. Esto permite crear cadenas lógicas de dependencias o jerarquías con una profundidad teóricamente ilimitada, estructurando los datos de forma natural y cohesiva.

A continuación, se presenta la implementación solicitada, asegurando la inmutabilidad de los objetos y modelando una cadena generacional:

```java
public class Persona {
    private final String nombre;
    private final Persona madre; // Composición recursiva

    // Constructor para una persona cuya madre es conocida
    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    // Constructor sobrecargado para una persona cuya madre se desconoce
    public Persona(String nombre) {
        this.nombre = nombre;
        this.madre = null; // Condición de parada para la recursividad
    }

    public String getNombre() {
        return this.nombre;
    }

    public Persona getMadre() {
        return this.madre;
    }

    public static void main(String[] args) {
        // La creación debe ser "de abajo hacia arriba" (ascendente) debido a la inmutabilidad
        Persona abuela = new Persona("Carmen");
        Persona madre = new Persona("María", abuela);
        Persona nieto = new Persona("Carlos", madre);

        System.out.println("El nieto es: " + nieto.getNombre());
        
        // Navegación por la composición recursiva
        if (nieto.getMadre() != null) {
            System.out.println("Su madre es: " + nieto.getMadre().getNombre());
            
            if (nieto.getMadre().getMadre() != null) {
                System.out.println("Su abuela es: " + nieto.getMadre().getMadre().getNombre());
            }
        }
    }
}

```

En el diseño expuesto, la inmutabilidad se blinda declarando todos los atributos con el modificador `final` y omitiendo cualquier método de modificación (*setter*). A causa de esta severa restricción de diseño, la construcción de la cadena de objetos debe ejecutarse invariablemente en orden inverso a la descendencia. Resulta obligatorio instanciar en primer lugar a la figura ancestral superior (la abuela) para poder inyectar su referencia durante la creación del siguiente eslabón (la madre), y así sucesivamente. Al tolerar que la referencia de la madre sea un valor nulo (`null`), se establece un límite funcional para la cadena, imitando la terminación matemática o el clásico puntero `NULL` de C que marca el final de una estructura dinámica.

Fuera de la representación genealógica o el encadenamiento de excepciones, existen abundantes escenarios clásicos en la ingeniería de software donde la composición recursiva constituye la base arquitectónica fundamental. Algunos de los casos más paradigmáticos son:

* **Estructuras de datos jerárquicas:** Los nodos individuales en árboles binarios o grafos, donde un nodo "Padre" se compone de varios nodos "Hijos" exactamente de la misma clase.
* **Sistemas de archivos (File Systems):** Un directorio o carpeta que actúa como contenedor de una lista que alberga en su interior referencias a otras carpetas (además de archivos planos).
* **Interfaces gráficas y documentos:** Componentes contenedores en ventanas (como paneles) que pueden agrupar en su interior otros sub-paneles, o los elementos anidados de un documento XML/HTML donde una etiqueta contiene otras etiquetas idénticas estructuralmente.

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

### Respuesta    

Las relaciones de composición o asociación bidireccionales ocurren cuando ambos extremos de la relación mantienen referencias mutuas.  En un diseño unidireccional, como el implementado en los ejemplos previos, el `Departamento` conoce a sus profesores, pero el `Profesor` ignora a qué departamento pertenece. En la bidireccionalidad, el flujo de conocimiento viaja en ambos sentidos. Haciendo un paralelismo con el lenguaje C, esto equivale a tener dos estructuras que contienen punteros la una hacia la otra (lo que habitualmente requiere declaraciones adelantadas o *forward declarations*), permitiendo navegar desde el "todo" hacia las "partes" y desde una "parte" de vuelta hacia el "todo".

Para implementar este paradigma en el escenario propuesto, resulta imprescindible modificar la estructura de ambas entidades. La clase `Profesor` debe incorporar un nuevo atributo de tipo `Departamento` que almacene la referencia a su contenedor actual, junto con métodos para actualizar dicho estado de forma segura. Por su parte, la clase contenedora `Departamento` asume la responsabilidad arquitectónica de mantener la consistencia de los datos en ambos extremos simultáneamente durante las operaciones de inserción y eliminación.

A continuación se ilustra la sincronización requerida en la implementación de esta relación:

```java
public class Profesor {
    private String nombre;
    private Departamento departamento; // Referencia de vuelta al contenedor

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    // El método tiene visibilidad de paquete o está protegido para evitar
    // que se asigne desde fuera sin pasar por la lógica del Departamento.
    void setDepartamento(Departamento departamento) {
        this.departamento = departamento;
    }

    public Departamento getDepartamento() {
        return this.departamento;
    }
}

public class Departamento {
    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) throw new IllegalArgumentException("...");
        this.profesores = new ArrayList<>();
        
        // Sincronización inicial bidireccional
        this.profesores.add(directorInicial);
        this.director = directorInicial;
        directorInicial.setDepartamento(this); 
    }

    public void anadirProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("...");
        
        // Sincronización al añadir: el departamento registra al profesor...
        this.profesores.add(p);
        // ...y el profesor registra al departamento.
        p.setDepartamento(this);
    }

    public void eliminarProfesor(int indice) {
        // ... (Validaciones de límites y del director omitidas por brevedad) ...
        
        Profesor p = this.profesores.get(indice);
        this.profesores.remove(indice);
        
        // Sincronización al eliminar: se rompe el enlace desde la parte hacia el todo
        p.setDepartamento(null);
    }
}

```

El mayor desafío técnico al establecer relaciones bidireccionales orientadas a objetos reside en mantener esta sincronización referencial sin corromper la encapsulación. Si la actualización recíproca no se diseña cuidadosamente encapsulando la lógica en un solo punto de control (habitualmente en la clase "todo"), se corre el riesgo de generar inconsistencias estructurales severas. Por ejemplo, un docente podría tener asignado un departamento en su atributo interno, mientras que la lista de ese mismo departamento ya no lo incluye, rompiendo la coherencia de la información en memoria.

