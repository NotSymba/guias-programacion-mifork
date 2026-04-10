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
## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

### Respuesta    
La herencia es un mecanismo de la programación orientada a objetos que permite crear nuevas clases basándose en clases ya existentes, estableciendo una relación semántica y estructural estricta conocida como "A es-un B". A diferencia del uso de estructuras anidadas en C o de la composición ("tiene-un") ya estudiada en Java, la herencia indica que una subclase (por ejemplo, un tipo específico de militar) es una extensión directa de su superclase. La clase derivada mantiene la esencia fundamental de la clase base, especializándola con características adicionales sin necesidad de duplicar el código común.

Esta técnica presenta dos implicaciones arquitectónicas fundamentales. En primer lugar, la **herencia de estado y comportamiento** significa que la subclase adquiere automáticamente los atributos y métodos de la superclase. Es imperativo recordar que, debido a las reglas de encapsulación, los atributos marcados como privados en la clase base no pueden accederse ni modificarse directamente desde la derivada; su inicialización debe delegarse mediante la invocación del constructor de la clase padre utilizando la palabra reservada `super`. 

En segundo lugar, se establece la **compatibilidad de tipos** (conocida técnicamente como polimorfismo de subtipado). Dado que, por definición lógica, "un Artillero es un Soldado", el sistema de tipos de Java asimila esta relación. Por consiguiente, cualquier arreglo, parámetro o variable declarada para operar con objetos de tipo `Soldado` aceptará de forma completamente transparente y segura referencias a objetos de cualquiera de sus subclases. El compilador permite esto porque tiene la garantía absoluta de que los subtipos contendrán, como mínimo, todos los métodos públicos definidos en la clase base.

A continuación, se presenta la implementación solicitada que materializa estos conceptos:

```java
// Superclase: Define el estado y comportamiento común
class Soldado {
    private String nombre; // Encapsulado, no visible directamente por las subclases

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("¡Se presenta el soldado " + nombre + "!");
    }
}

// Subclase 1: Hereda de Soldado y añade especialización
class Artillero extends Soldado {
    private int numCohetes;

    public Artillero(String nombre, int numCohetes) {
        super(nombre); // Se invoca al constructor de la superclase para inicializar 'nombre'
        this.numCohetes = numCohetes;
    }

    public int getNumCohetes() {
        return numCohetes;
    }

    public void dispararCohete() {
        if (numCohetes > 0) {
            System.out.println("¡Fiuuuu! PUM.");
            numCohetes--;
        }
    }
}

// Subclase 2: Hereda de Soldado y añade otra especialización diferente
class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre); // Se delega la inicialización del estado base
        this.numMinas = numMinas;
    }

    public int getNumMinas() {
        return numMinas;
    }

    public void ponerMina() {
        if (numMinas > 0) {
            System.out.println("Mina armada y oculta.");
            numMinas--;
        }
    }
}

// Clase principal para la ejecución
public class Main {
    public static void main(String[] args) {
        // Demostración de COMPATIBILIDAD DE TIPOS
        // Un arreglo de tipo 'Soldado' puede contener 'Artilleros' y 'Zapadores'
        Soldado[] peloton = new Soldado[3];
        peloton[0] = new Artillero("Pérez", 5);
        peloton[1] = new Zapador("Gómez", 10);
        peloton[2] = new Artillero("Ruiz", 2);

        // Demostración de HERENCIA DE COMPORTAMIENTO
        // Se recorre el arreglo y se invoca un método de la superclase.
        // Todos los objetos saben "saludar" porque heredan esta capacidad.
        for (int i = 0; i < peloton.length; i++) {
            peloton[i].saludar();
        }
        
        // Nota: Dentro del bucle, peloton[i] es tratado como un 'Soldado'.
        // No se podría llamar a peloton[0].dispararCohete() directamente sin hacer 
        // previamente un cambio de tipo explícito (casting), ya que el compilador 
        // en ese punto solo "ve" un Soldado genérico.
    }
}
```

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta    
Al instanciar un objeto de una clase derivada, como un `Artillero`, se produce una ejecución encadenada de constructores. Aunque se invoca directamente al constructor de la subclase, este no puede inicializar el objeto por completo sin preparar antes la parte correspondiente a la superclase. Por lo tanto, el orden de ejecución de los bloques de código se realiza de "arriba hacia abajo" en la jerarquía: primero se ejecuta y finaliza el constructor de la clase padre (`Soldado`) para establecer el estado base, y posteriormente concluye el constructor de la clase hija (`Artillero` o `Zapador`) para aplicar su especialización. En este escenario concreto, se ejecutan exactamente dos constructores por cada objeto creado.

La palabra reservada `super`, cuando se utiliza como método dentro del ámbito de un constructor, actúa como una llamada explícita al constructor de la superclase inmediata. Su propósito principal es transmitir los argumentos necesarios para que la clase padre pueda inicializar correctamente sus propios atributos (como el atributo privado `nombre`), garantizando así que las reglas de encapsulación se respeten, ya que la subclase no tiene permisos para alterar esa memoria directamente. Existe una regla sintáctica estricta en Java: si se escribe de forma explícita, la llamada a `super(...)` debe ser obligatoriamente la primera instrucción ejecutable dentro del constructor de la clase derivada.

Respecto a la obligatoriedad de su uso, la respuesta depende de cómo esté diseñada la clase padre. Por defecto, si el programador no escribe explícitamente `super(...)`, el compilador de Java inserta automáticamente una llamada oculta a `super()` (sin parámetros) en la primera línea. Sin embargo, si la clase base solo define constructores con parámetros (como ocurre con `Soldado`, que exige un `String`), no existirá un constructor vacío disponible. Al intentar el compilador hacer esa llamada automática sin parámetros, se producirá un error de compilación. En consecuencia, si la clase base no tiene un constructor predeterminado visible, es estrictamente obligatorio escribir la llamada a `super(...)` proporcionando los argumentos adecuados.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta    
Físicamente, cuando se instancia un objeto de una subclase, los atributos privados de la superclase sí forman parte integral de la estructura de dicho objeto en memoria. Al ejecutar `new Artillero(...)`, la máquina virtual de Java reserva un bloque de memoria contiguo que consolida tanto los atributos propios de la clase derivada (como `numCohetes`) como todos los atributos definidos en su jerarquía ascendente, incluyendo el campo privado `nombre` de la clase `Soldado`. Haciendo una analogía con el lenguaje C, el objeto resultante en la memoria se comporta de manera similar a una estructura (`struct`) que contiene internamente otra estructura anidada con los datos de la clase padre, garantizando así que el estado completo requerido para ser un "Soldado" exista físicamente dentro de cada "Artillero".

Sin embargo, la presencia física en memoria no otorga automáticamente privilegios de uso en el código. Las reglas de encapsulación de Java operan a nivel del compilador, creando una barrera estricta e infranqueable. El hecho de que el atributo `nombre` resida dentro del objeto `Artillero` no anula ni invalida su declaración original como `private` en la clase `Soldado`. Por consiguiente, es completamente imposible acceder, leer o modificar dicho atributo de forma directa desde el código interno de la subclase; intentar escribir una instrucción como `this.nombre = "Juan"` dentro de cualquier método de la clase `Artillero` o `Zapador` desencadenará un error de compilación inmediato por violación de acceso.

Para que la subclase pueda interactuar con ese estado encapsulado que posee en su propia memoria, está obligada a recurrir a los mecanismos públicos o protegidos provistos por la superclase. En el ejemplo abordado, la única vía para establecer el valor del `nombre` de un `Zapador` es mediante la delegación en el momento de la construcción, utilizando `super(nombre)`. Asimismo, aunque el zapador no puede leer su propio nombre directamente, puede utilizarlo de manera indirecta invocando el método `saludar()` heredado; dado que el código de dicho método fue compilado en el contexto de la clase `Soldado`, sí cuenta con los permisos necesarios para acceder al atributo privado en memoria y mostrarlo por pantalla.

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta    
En el contexto de la orientación a objetos, la extensibilidad se refiere a la capacidad de un sistema para incorporar nuevas funcionalidades o tipos de datos con el mínimo impacto en la base de código preexistente. La compatibilidad de tipos es el motor fundamental de esta característica. En paradigmas estructurados como C o C++, la introducción de una nueva variante de una entidad (por ejemplo, un nuevo tipo de tropa) habitualmente obliga al programador a rastrear y modificar múltiples sentencias `switch` o bloques `if-else` a lo largo de todo el programa para gestionar el nuevo caso. En Java, gracias a la herencia, el código que opera con la referencia de la superclase se vuelve completamente agnóstico e independiente respecto a las subclases específicas que procesa.

Esta dinámica establece un principio de diseño altamente eficiente: el sistema queda abierto para su extensión, pero cerrado para su modificación. Cuando el entorno de ejecución de Java evalúa una instrucción genérica sobre un arreglo polimórfico, emplea un mecanismo de enlace dinámico. Esto significa que la máquina virtual identifica en tiempo de ejecución cuál es el objeto real alojado en la memoria y delega el comportamiento adecuadamente, sin importar si dicha clase fue creada meses o años después de haber escrito el bucle principal. 

Para ilustrar este concepto, a continuación se define un nuevo subtipo `Medico` y se integra en el flujo del programa anterior. Se observará que el bucle encargado de procesar la iteración y la invocación del comportamiento común permanece absolutamente inalterado:

```java
// Se introduce una nueva subclase en el sistema sin tocar el código antiguo
class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public void curar() {
        if (botiquines > 0) {
            System.out.println("Aplicando primeros auxilios. Soldado estabilizado.");
            botiquines--;
        }
    }
}

public class Main {
    public static void main(String[] args) {
        // Se amplía la capacidad del pelotón para albergar al nuevo integrante
        Soldado[] peloton = new Soldado[4];
        
        peloton[0] = new Artillero("Pérez", 5);
        peloton[1] = new Zapador("Gómez", 10);
        peloton[2] = new Artillero("Ruiz", 2);
        
        // EXTENSIBILIDAD: Se instancia el nuevo tipo y se almacena bajo 
        // la misma referencia genérica por compatibilidad de tipos.
        peloton[3] = new Medico("López", 3); 

        // El bloque lógico de procesamiento permanece INTACTO.
        // No es necesario añadir sentencias de control para comprobar 
        // si el índice actual contiene un Artillero, Zapador o Médico.
        System.out.println("--- Pase de lista del pelotón ---");
        for (int i = 0; i < peloton.length; i++) {
            peloton[i].saludar();
        }
    }
}
```

Como se evidencia en la implementación, incorporar un elemento completamente nuevo a la simulación militar no requirió reestructurar la lógica de control principal. Esta cualidad estructural reduce drásticamente el esfuerzo de mantenimiento en el desarrollo de software a gran escala, previniendo la introducción de errores en rutinas que ya han sido probadas y estabilizadas previamente.

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta   
En Java, es completamente lícito y habitual que una variable de referencia declarada con el tipo de la superclase apunte a un objeto instanciado de cualquiera de sus subclases. Esta operación, denominada **upcasting** (conversión ascendente), se realiza de forma automática e implícita, ya que el sistema garantiza que, estructuralmente, el subtipo cumple con todos los requisitos del supertipo. Sin embargo, la respuesta a si se pueden invocar métodos específicos del subtipo utilizando la referencia genérica es negativa. El compilador de Java realiza una verificación estricta de tipos basándose exclusivamente en la declaración de la variable de referencia, ignorando el objeto real que reside en la memoria. Por lo tanto, si la referencia es de tipo `Soldado`, solo se permitirá la llamada a métodos definidos explícitamente en dicha clase base, ocultando temporalmente capacidades específicas.

Para poder acceder a la funcionalidad exclusiva y especializada de la subclase, es estrictamente necesario realizar un **downcasting** (conversión descendente). Este proceso consiste en indicar al compilador, mediante una conversión explícita de tipos `(Tipo)`, que trate la referencia genérica como una referencia del subtipo. A diferencia de C o C++, donde un "cast" entre punteros de distintas estructuras asume ciegamente que la memoria está alineada según lo solicitado, Java verifica la validez del *downcasting* en tiempo de ejecución. Si se intenta forzar la conversión de un `Zapador` a una referencia de `Artillero`, la máquina virtual interceptará el error de incompatibilidad y lanzará una excepción `ClassCastException`, abortando la ejecución.

Para prevenir este fallo crítico en tiempo de ejecución, el lenguaje provee el operador relacional `instanceof`. Este operador evalúa de forma segura si el objeto apuntado en memoria pertenece realmente a una clase específica (o a sus derivadas), devolviendo un valor booleano. Actúa como un mecanismo de inspección de tipos que permite condicionar el *downcasting* únicamente a los casos donde se tiene la certeza matemática de que la conversión será exitosa.

A continuación, se ilustra la aplicación práctica de estos tres conceptos mediante la inspección del pelotón:

```java
public class Main {
    public static void main(String[] args) {
        // UPCASTING IMPLÍCITO: Arreglo de tipo base apuntando a subtipos
        Soldado[] peloton = new Soldado[3];
        peloton[0] = new Artillero("Pérez", 5);
        peloton[1] = new Zapador("Gómez", 10);
        peloton[2] = new Artillero("Ruiz", 2);

        System.out.println("--- Inspección de armamento antitanque ---");
        
        for (int i = 0; i < peloton.length; i++) {
            // USO DE INSTANCEOF: Se verifica la verdadera identidad del objeto en memoria
            if (peloton[i] instanceof Artillero) {
                
                // DOWNCASTING SEGURO: El compilador permite tratarlo como Artillero
                // Se almacena temporalmente en una nueva referencia especializada
                Artillero especialista = (Artillero) peloton[i];
                
                // Ahora es posible invocar los métodos exclusivos del subtipo
                System.out.println("Artillero detectado. Dispone de " + especialista.getNumCohetes() + " cohetes.");
                
            } else {
                System.out.println("Este soldado no porta cohetes.");
            }
        }
    }
}
```

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta   
El encapsulamiento es un principio fundamental que protege el estado interno de un objeto. Hasta el momento, la experiencia previa contempla el uso de los modificadores `public` (acceso universal) y `private` (acceso restringido exclusivamente a la propia clase). El nivel de acceso **protegido** establece una visibilidad intermedia diseñada específicamente para interactuar de forma sinérgica con la herencia. Cuando un atributo o método se declara con este nivel, se concede permiso de acceso y manipulación directa a la propia clase, a cualquier otra clase que resida dentro del mismo paquete y, lo más relevante en este contexto, a todas sus subclases, independientemente del paquete en el que estas últimas se encuentren.

En el lenguaje Java, la implementación de este mecanismo se materializa utilizando la palabra reservada `protected` al inicio de la declaración del miembro correspondiente. Al realizar esto, se otorga a las clases derivadas la confianza y la responsabilidad de interactuar directamente con ese fragmento del estado base. A diferencia de un atributo privado, cuyo valor en la memoria de la subclase solo podría leerse o alterarse mediante métodos *getters* o *setters* proveídos por la superclase, el atributo protegido se comporta dentro del ámbito de la clase hija prácticamente como si hubiera sido declarado localmente. No obstante, frente a clases externas y clientes ajenos a la jerarquía, el atributo sigue estando bloqueado, preservando el aislamiento del objeto hacia el exterior.

Para ilustrar este comportamiento práctico, se somete el diseño inicial a una refactorización. Al degradar la restricción del atributo `nombre` en la clase `Soldado` de privado a protegido, la subclase `Zapador` adquiere el privilegio de lectura directa sobre esa porción de memoria, habilitando su uso transparente en métodos especializados:

```java
// Superclase con atributo protegido
class Soldado {
    protected String nombre; // Accesible directamente por cualquier subclase

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("¡Se presenta el soldado " + nombre + "!");
    }
}

// Subclase que hace uso del acceso protegido
class Zapador extends Soldado {
    private int numMinas;

    public Zapador(String nombre, int numMinas) {
        super(nombre); // Se sigue recomendando usar el constructor base
        this.numMinas = numMinas;
    }

    public void ponerMina() {
        if (numMinas > 0) {
            // ACCESO DIRECTO: Se utiliza 'nombre' sin invocar métodos intermedios.
            // Esta instrucción compila exitosamente gracias al modificador protected.
            System.out.println("El zapador " + nombre + " ha colocado un explosivo.");
            numMinas--;
        } else {
            System.out.println(nombre + " informa: Inventario de minas agotado.");
        }
    }
}
```

Como se evidencia en la estructura del método `ponerMina()`, el identificador `nombre` se emplea de forma natural y directa en la concatenación de la cadena de texto. A nivel de arquitectura de software, recurrir al modificador `protected` conlleva asumir un acoplamiento estructural más rígido entre la clase padre y sus descendientes. Si bien flexibiliza la escritura del código al suprimir la necesidad de funciones intermediarias, debe emplearse con prudencia analítica: cualquier alteración futura en el tipo o propósito de un atributo protegido en la superclase impactará de manera inmediata y exigirá la revisión del código interno de todas las subclases que dependan de él.

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta
En el ecosistema de los lenguajes de programación orientados a objetos, la existencia de una clase base universal, de la cual derivan implícita o explícitamente todas las demás clases, no es una regla universal estandarizada, sino una decisión de diseño arquitectónico que varía según el lenguaje. Por ejemplo, en lenguajes como C++ (con el que se tienen conocimientos de su versión procedural), no existe una única clase raíz que englobe todo el sistema de tipos; el programador puede crear jerarquías de herencia completamente aisladas e independientes entre sí.

Sin embargo, en el diseño de Java, los creadores optaron por un modelo de jerarquía unificada. En Java, absolutamente todas las clases (ya sean nativas del lenguaje o creadas por el usuario, como `Soldado`, `Artillero` o `Zapador`) heredan en última instancia de una única superclase primordial denominada `java.lang.Object`. Esta herencia se establece de manera automática: si en la declaración de una clase no se utiliza explícitamente la palabra reservada `extends` para especificar una superclase directa (como es el caso de `class Soldado { ... }`), el compilador asume y añade de forma invisible la instrucción `extends Object`. 

La consecuencia directa de esta arquitectura centralizada es que todo objeto instanciado en Java es, por definición de compatibilidad de tipos, un `Object`. Esta clase raíz proporciona un comportamiento fundamental y común que todas las instancias deben poseer para funcionar correctamente dentro del entorno de la máquina virtual (JVM). Al heredar de `Object`, cualquier clase adquiere automáticamente la capacidad de, por ejemplo, ser comparada para verificar la igualdad de referencias (mediante el método `equals()`), obtener un código numérico para estructuras de datos (con `hashCode()`), o ser representada como una cadena de texto (a través del método `toString()`), métodos que el programador puede sobrescribir para adaptar a la lógica específica de su aplicación.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

### Respuesta    
La **herencia múltiple** es un paradigma estructural presente en determinados lenguajes de programación orientados a objetos (como C++) que permite a una subclase derivar directamente de dos o más superclases simultáneamente. En este escenario, la clase hija adquiere de forma combinada el estado (atributos) y el comportamiento (métodos) de todas sus clases base. Conceptualmente, esta característica resulta útil para modelar entidades que pertenecen a múltiples categorías lógicas al mismo tiempo; por ejemplo, un hipotético `VehiculoAnfibio` podría heredar de forma simultánea de las clases `Coche` y `Barco`, adquiriendo las capacidades mecánicas de ambas.

A pesar de su aparente flexibilidad de diseño, la herencia múltiple introduce una severa complicación arquitectónica conocida teóricamente como el "problema del diamante" o colisión de nombres. El conflicto surge cuando dos o más superclases definen un atributo o un método con exactamente el mismo identificador o firma (por ejemplo, si tanto `Coche` como `Barco` poseen un método `iniciarMarcha()`). Al instanciar la clase derivada y realizar la llamada a dicho método, el compilador se enfrenta a una ambigüedad insalvable, ya que no existe un criterio evidente para determinar cuál de las dos implementaciones heredadas debe prevalecer o ejecutarse. 

Para evitar estas ambigüedades lógicas y garantizar la simplicidad, seguridad y legibilidad del código, **en Java no existe la herencia múltiple de clases**. Una decisión fundamental en el diseño de la máquina virtual y del compilador de Java fue establecer un modelo de herencia estrictamente simple, obligando a que la palabra reservada `extends` apunte hacia una, y solo una, superclase directa. Esta restricción elimina de raíz cualquier posibilidad de colisión de herencia en el estado interno o en las implementaciones de los métodos.

Cuando en Java se presenta la necesidad de que una clase combine características de diferentes dominios, se recomienda emplear la composición, un concepto ya explorado donde el objeto instancia internamente a otros objetos delegándoles las tareas específicas. Alternativamente, el lenguaje ofrece una estructura llamada "interfaz" (un concepto avanzado que se estudiará posteriormente), la cual permite heredar múltiples firmas de métodos sin aportar estado ni implementación, evadiendo de forma segura las trampas estructurales del problema del diamante.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta   
En el sistema de manejo de errores de Java, las excepciones no son meros códigos numéricos de fallo como ocurre tradicionalmente en C, sino objetos completos e independientes. Toda anomalía durante la ejecución se representa mediante la instanciación de una clase que pertenece a una extensa jerarquía cuya raíz es `java.lang.Throwable`. Para crear excepciones propias y semánticamente adaptadas al dominio de un problema, basta con aplicar el mecanismo de herencia. Si el objetivo es diseñar una excepción *no controlada* (aquella que el compilador no obliga a declarar en la firma del método ni a capturar forzosamente con bloques `try-catch`), la nueva clase debe heredar de forma directa o indirecta de `java.lang.RuntimeException`.

Al ser la excepción una clase estándar, disfruta de todas las capacidades de la orientación a objetos, incluyendo la composición. Esto implica que una excepción personalizada puede declarar atributos adicionales para capturar y transportar el contexto exacto del sistema en el instante en que se produjo el error. En lugar de limitarse a transmitir un simple mensaje de texto plano, la excepción puede albergar en su interior referencias a los propios objetos involucrados en el fallo (como un objeto `Usuario`). Esta práctica facilita enormemente la resolución de problemas, ya que la rutina encargada de interceptar el error dispondrá de acceso estructurado a los datos originales para su análisis o registro.

Adicionalmente, el diseño de excepciones robustas contempla el encadenamiento de errores para no perder la traza de la causa original (por ejemplo, si el usuario no se encontró debido a una previa desconexión de la base de datos). Para lograr esto, se recurre a la sobrecarga de constructores. Se define un constructor primario para el escenario habitual y otro sobrecargado que acepta un parámetro extra de tipo `Throwable`, representando el fallo subyacente. En todas las variantes, resulta imperativo delegar la inicialización básica del sistema operativo (mensaje y pila de llamadas) mediante la invocación a `super(...)` antes de asignar el estado propio de la composición.

A continuación, se detalla la implementación de este diseño estructural:

```java
// Clase auxiliar simplificada para ilustrar la composición
class Usuario {
    private String id;
    private String nombre;

    public Usuario(String id, String nombre) {
        this.id = id;
        this.nombre = nombre;
    }

    public String getId() {
        return id;
    }
    
    @Override
    public String toString() {
        return "Usuario[ID: " + id + ", Nombre: " + nombre + "]";
    }
}

// Creación de la excepción personalizada no controlada
class UsuarioNoEncontradoException extends RuntimeException {
    
    // COMPOSICIÓN: La excepción almacena el estado del objeto problemático
    private Usuario usuarioInvolucrado;

    // Constructor 1: Inicialización estándar con mensaje y el objeto asociado
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        // Se delega el mensaje a la superclase RuntimeException
        super(mensaje); 
        this.usuarioInvolucrado = usuario;
    }

    // Constructor 2: Sobrecarga para permitir encadenamiento de excepciones (causa)
    public UsuarioNoEncontradoException(String mensaje, Throwable causa, Usuario usuario) {
        // Se delega el mensaje y la causa original a la superclase
        super(mensaje, causa); 
        this.usuarioInvolucrado = usuario;
    }

    // Método de acceso para que el bloque 'catch' pueda recuperar el usuario fallido
    public Usuario getUsuarioInvolucrado() {
        return usuarioInvolucrado;
    }
}

// Ejemplo de uso teórico (no ejecutable por sí solo)
class SistemaAutenticacion {
    public void verificarUsuario(Usuario u) {
        try {
            // Simulando un error interno previo (ej: fallo de red)
            throw new IllegalStateException("Conexión a BD interrumpida");
        } catch (IllegalStateException e) {
            // Se lanza la excepción personalizada, componiendo el usuario 
            // y adjuntando la excepción original 'e' como causa.
            throw new UsuarioNoEncontradoException(
                "No se pudo validar al usuario en el sistema", 
                e, 
                u
            );
        }
    }
}
```

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

### Respuesta    
El uso de la herencia con el único propósito de reutilizar código se considera un antipatrón de diseño porque pervierte la semántica fundamental de la orientación a objetos. Como se ha establecido previamente, la herencia define una relación estricta y estructural del tipo "es-un". Si se fuerza esta conexión genérica simplemente para acceder a un método útil, se corrompe el modelo lógico del dominio. Por ejemplo, si la clase `Artillero` requiere realizar cálculos trigonométricos complejos para apuntar sus cohetes, y ya existe una clase `CalculadoraBalistica` que posee esos algoritmos, hacer que la clase `Artillero` herede de `CalculadoraBalistica` sería un error conceptual grave: un soldado no es una calculadora, y bajo ninguna circunstancia debería ser tratado como tal por el sistema de tipos.

A nivel arquitectónico, la herencia introduce el grado más alto de acoplamiento posible entre dos clases (conocido como acoplamiento fuerte). La subclase queda íntimamente ligada a la implementación, al estado y a la jerarquía completa de la superclase. En consecuencia, cualquier modificación futura en la clase base —como alterar la firma de un método, cambiar su comportamiento interno o añadir nuevos requisitos de inicialización en sus constructores— impactará inevitablemente y de forma directa en todas y cada una de sus subclases, aumentando drásticamente la fragilidad del software. Cuando el único objetivo es reutilizar una rutina aislada, someter el sistema a este nivel de rigidez estructural resulta innecesario y contraproducente para el mantenimiento a largo plazo.

Para resolver la necesidad de reutilizar código sin comprometer la integridad del diseño, el principio fundamental y universalmente aceptado en la orientación a objetos es **"favorecer la composición sobre la herencia"**. Mediante la composición, ya conocida, se establece una relación del tipo "tiene-un". En lugar de heredar de la clase que contiene la utilidad, la nueva clase simplemente declara un atributo privado de ese tipo, lo instancia y le delega el trabajo cuando es necesario. Este enfoque garantiza un bajo acoplamiento, ya que la interacción se limita al uso estricto de la interfaz pública del objeto compuesto, protegiendo a la clase principal de los cambios internos de la herramienta que está utilizando.

A continuación, se ilustra la diferencia entre ambos enfoques para aclarar la recomendación:

```java
// ❌ ANTIPATRÓN: Heredar solo para reutilizar código (Rompe la regla "A es-un B")
// Problema: Ahora el Artillero expone públicamente todos los métodos de una calculadora.
class Artillero extends CalculadoraBalistica {
    public void dispararCohete() {
        // Se usa un método heredado que no tiene sentido en la identidad de un soldado
        double trayectoria = calcularParabola(15.5, 45); 
        System.out.println("Fuego con trayectoria " + trayectoria);
    }
}

// ✅ DISEÑO CORRECTO: Composición para reutilizar código (Relación "A tiene-un B")
// Beneficio: Se mantiene la jerarquía real ("es-un Soldado") y se reutiliza el código limpiamente.
class Artillero extends Soldado {
    
    // COMPOSICIÓN: Se adquiere la herramienta para reutilizar sus algoritmos
    private CalculadoraBalistica calculadora; 

    public Artillero(String nombre) {
        super(nombre);
        this.calculadora = new CalculadoraBalistica();
    }

    public void dispararCohete() {
        // DELEGACIÓN: Se solicita el cálculo al objeto compuesto, manteniendo la encapsulación
        double trayectoria = calculadora.calcularParabola(15.5, 45); 
        System.out.println("Fuego con trayectoria " + trayectoria);
    }
}
```

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

### Respuesta    
El principio de diseño que dicta "favorecer la composición frente a la herencia" surge fundamentalmente para minimizar el acoplamiento y proteger la encapsulación del sistema. La herencia establece la relación estructural más rígida posible en la orientación a objetos, conocida como "reutilización de caja blanca", donde los detalles internos de la superclase a menudo quedan expuestos o condicionan fuertemente a las subclases. Esto genera el conocido problema de la "clase base frágil": cualquier modificación en la jerarquía superior puede romper en cascada el funcionamiento de las clases derivadas. En contraparte, la composición ofrece una "reutilización de caja negra", donde los objetos interactúan exclusivamente a través de sus interfaces públicas o métodos de acceso, manteniendo sus implementaciones internas estrictamente aisladas.

Otra ventaja decisiva de la composición es su enorme flexibilidad y su capacidad de adaptación en tiempo de ejecución. La herencia es completamente estática; la relación entre una superclase y una subclase se fija irreversiblemente en el momento de escribir y compilar el código. Por el contrario, al utilizar la composición, un objeto delega tareas a sus componentes internos mediante referencias, las cuales pueden reasignarse dinámicamente mientras el programa se ejecuta. Esto permite alterar el comportamiento de una entidad "en caliente", simplemente intercambiando la pieza o componente interno por otro diferente sin necesidad de destruir el objeto principal ni crear clases nuevas.

Además, abusar de la herencia para mezclar diferentes características conduce invariablemente a una explosión combinatoria de clases y jerarquías inmanejables. Si en el simulador militar se requiriera clasificar a los soldados por su especialidad (Artillero, Zapador) y simultáneamente por su rango (Recluta, Sargento, Capitán), emplear herencia pura obligaría a crear una clase para cada cruce posible (`ArtilleroSargento`, `ZapadorRecluta`, etc.). La composición resuelve esto separando responsabilidades: la entidad principal simplemente se ensambla asignándole componentes independientes, resultando en un código modular, altamente cohesivo y mucho más fácil de mantener.

A continuación, se demuestra la flexibilidad de la composición al permitir que un soldado intercambie su armamento dinámicamente en tiempo de ejecución, algo imposible de lograr con una arquitectura basada exclusivamente en herencia estática:

```java
// Componentes independientes que representan el armamento
class Rifle {
    public void disparar() {
        System.out.println("Ratatatata...");
    }
}

class Lanzacohetes {
    public void disparar() {
        System.out.println("¡Fiuuuuuu... PUM!");
    }
}

// Entidad principal que UTILIZA composición en lugar de herencia
class SoldadoModerno {
    private String nombre;
    // COMPOSICIÓN: El soldado "tiene un" arma. No hereda de ella.
    private Object armaActual; 

    public SoldadoModerno(String nombre, Object armaInicial) {
        this.nombre = nombre;
        this.armaActual = armaInicial;
    }

    // Flexibilidad en TIEMPO DE EJECUCIÓN: Se puede cambiar el componente
    public void equiparArma(Object nuevaArma) {
        this.armaActual = nuevaArma;
        System.out.println(nombre + " ha cambiado de arma.");
    }

    public void atacar() {
        // Se delega la acción al componente actualmente equipado
        if (armaActual instanceof Rifle) {
            ((Rifle) armaActual).disparar();
        } else if (armaActual instanceof Lanzacohetes) {
            ((Lanzacohetes) armaActual).disparar();
        } else {
            System.out.println(nombre + " ataca cuerpo a cuerpo.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        // Se instancia un componente y se ensambla dentro del soldado
        Rifle miRifle = new Rifle();
        SoldadoModerno soldado = new SoldadoModerno("Ramírez", miRifle);
        
        soldado.atacar(); // Usa el rifle
        
        // Ventaja de la composición: El comportamiento cambia sin alterar la clase
        Lanzacohetes bazuca = new Lanzacohetes();
        soldado.equiparArma(bazuca); // Se sustituye la pieza interna
        
        soldado.atacar(); // Ahora dispara cohetes, el soldado es el mismo objeto
    }
}
```

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta    
La afirmación "la herencia rompe la encapsulación" hace referencia al nivel extremo de acoplamiento que se genera entre una superclase y sus subclases. El principio fundamental de la encapsulación dicta que los detalles de implementación interna de un objeto deben permanecer ocultos (como en una "caja negra") y que la interacción con él debe realizarse exclusivamente a través de una interfaz pública y estandarizada. Sin embargo, cuando se emplea la herencia, la subclase adquiere una dependencia profunda e íntima de la estructura interna de su clase padre. Esto se conoce conceptualmente como una "reutilización de caja blanca", donde las fronteras protectoras del objeto se difuminan y los detalles internos quedan expuestos a lo largo de la jerarquía.

Esta exposición interna da lugar a una vulnerabilidad arquitectónica conocida como el "problema de la clase base frágil". Dado que la subclase asume y depende no solo de *qué* hace la superclase, sino de *cómo* lo hace internamente, cualquier cambio rutinario en el código de la clase padre puede provocar fallos catastróficos y silenciosos en las clases hijas. Si un método de la superclase altera su flujo lógico interno —por ejemplo, invocando a otro método dentro de la misma clase que antes no utilizaba—, una subclase que había adaptado o sobrescrito esos comportamientos se desestabilizará por completo, a pesar de que las declaraciones de los métodos no hayan sufrido ninguna modificación.

Por consiguiente, para desarrollar una subclase segura y sin errores, el programador se ve forzado a estudiar detalladamente el código fuente original de la superclase para prever cómo interactúan sus métodos entre sí. Esto anula por completo el beneficio de la abstracción que prometía la encapsulación original. Este fenómeno no ocurre al utilizar la composición, ya que el objeto principal se limita a utilizar la funcionalidad del objeto compuesto invocando sus métodos públicos, permaneciendo completamente inmune a cómo este último decide ejecutar la tarea internamente.

A continuación, se ilustra este problema con un ejemplo clásico donde la subclase se rompe debido a que desconoce cómo la superclase implementa sus métodos internamente:

```java
// Superclase desarrollada por un equipo
class ArmaBase {
    public void dispararUnaVez() {
        System.out.println("PUM");
    }

    // El desarrollador original decide que una ráfaga son 3 disparos,
    // y para reutilizar código, llama a su propio método internamente.
    public void dispararRafaga() {
        dispararUnaVez();
        dispararUnaVez();
        dispararUnaVez();
    }
}

// Subclase desarrollada por otro equipo que hereda la implementación
class ArmaConContador extends ArmaBase {
    private int municionGastada = 0;

    // La subclase sobrescribe el comportamiento para llevar la cuenta
    @Override
    public void dispararUnaVez() {
        municionGastada += 1;
        super.dispararUnaVez();
    }

    // El desarrollador asume que una ráfaga gasta 3 balas
    @Override
    public void dispararRafaga() {
        municionGastada += 3;
        super.dispararRafaga(); 
        // ¡ERROR! Al llamar a super.dispararRafaga(), la superclase ejecutará 
        // 3 veces el método dispararUnaVez() de esta misma subclase.
        // Resultado: municionGastada sumará 3, y luego 1+1+1 = ¡6 balas gastadas por ráfaga!
    }

    public int getMunicionGastada() {
        return municionGastada;
    }
}
```

Como se observa, el intento de la clase `ArmaConContador` de llevar su propio registro falla estrepitosamente porque la encapsulación se ha roto: el correcto funcionamiento de la subclase dependía de conocer el secreto interno de que `dispararRafaga()` invocaba a `dispararUnaVez()` dentro del código fuente de `ArmaBase`. Si en una actualización futura el desarrollador de `ArmaBase` cambiara la implementación interna, la subclase volvería a arrojar cálculos erróneos.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta    
Para modelar entidades que comparten información, el enfoque basado en la herencia recurre a la extracción de las características comunes hacia una superclase. En este escenario, se define una clase `Persona` que encapsula el estado base, correspondiente al identificador (DNI) y al nombre. Posteriormente, las clases `Estudiante` y `Trabajador` se derivan de ella estableciendo una relación estructural del tipo "es-un". Este diseño permite que ambas subclases hereden automáticamente la presencia de los datos personales en memoria, delegando su inicialización obligatoria mediante la invocación a `super` en sus respectivos constructores.

```java
// --- ALTERNATIVA 1: MODELADO POR HERENCIA ---

class Persona {
    private String dni;
    private String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getNombre() { return nombre; }
    public String getDni() { return dni; }
}

class Estudiante extends Persona {
    private String gradoAcademico;

    public Estudiante(String dni, String nombre, String gradoAcademico) {
        super(dni, nombre); // Delegación a la superclase para inicializar estado base
        this.gradoAcademico = gradoAcademico;
    }
}

class Trabajador extends Persona {
    private double salario;

    public Trabajador(String dni, String nombre, double salario) {
        super(dni, nombre); // Delegación a la superclase
        this.salario = salario;
    }
}
```

Por otro lado, el enfoque basado en la composición resuelve el mismo problema de duplicación de código evitando por completo la creación de una jerarquía de herencia. En lugar de extender una clase padre, se agrupa la información común en un componente independiente y altamente cohesivo denominado `DatosPersonales`. Las clases `Estudiante` y `Trabajador` establecen entonces una relación del tipo "tiene-un" al declarar un atributo privado de este nuevo tipo. Tal y como exige el planteamiento, en lugar de recibir tipos de datos primitivos (`String`), los constructores de estas entidades exigen recibir una instancia ya inicializada del componente para ensamblarse.

```java
// --- ALTERNATIVA 2: MODELADO POR COMPOSICIÓN ---

class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getNombre() { return nombre; }
    public String getDni() { return dni; }
}

class Estudiante {
    // COMPOSICIÓN: El estudiante "tiene" datos personales, no "es" unos datos personales
    private DatosPersonales datos; 
    private String gradoAcademico;

    // Se recibe la instancia completa en el constructor (Inyección de dependencias)
    public Estudiante(DatosPersonales datos, String gradoAcademico) {
        this.datos = datos; 
        this.gradoAcademico = gradoAcademico;
    }

    // Se requiere delegación explícita para exponer la información hacia el exterior
    public String obtenerNombre() {
        return datos.getNombre();
    }
}

class Trabajador {
    private DatosPersonales datos;
    private double salario;

    public Trabajador(DatosPersonales datos, double salario) {
        this.datos = datos;
        this.salario = salario;
    }
    
    public String obtenerNombre() {
        return datos.getNombre();
    }
}
```

Ambas alternativas logran el objetivo fundamental de no duplicar la declaración de las variables compartidas, pero sus implicaciones a largo plazo divergen significativamente. El modelado por herencia habilita la **compatibilidad de tipos**; es decir, se podría crear un arreglo de tipo `Persona` y mezclar estudiantes y trabajadores para procesarlos en bucle. En contraste, la composición sacrifica esa compatibilidad genérica a cambio de un diseño **completamente desacoplado**. Con la composición, si un trabajador cambiara legalmente su identidad o se detectara un error de registro documental, bastaría con reasignar el objeto `DatosPersonales` por uno nuevo en tiempo de ejecución, una flexibilidad estructural imposible de alcanzar cuando el DNI y el nombre están soldados de forma inmutable en la memoria de la clase base.
