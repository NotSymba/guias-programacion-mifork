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

### Respuesta    
El polimorfismo, en el contexto de la herencia en programación orientada a objetos, es la capacidad de tratar a los objetos de distintas clases derivadas como si fueran objetos de una misma clase base. Trazando un paralelismo con lenguajes como C, equivale a tener un puntero de un tipo de estructura genérica (o "padre") que puede apuntar dinámicamente a diferentes estructuras más específicas (o "hijas"). En Java, esto se traduce en utilizar una variable de referencia del tipo de la superclase para almacenar y manejar un objeto instanciado de cualquiera de las subclases que heredan de ella.

La utilidad principal de este mecanismo radica en la simplificación, flexibilidad y extensibilidad del código. En C, para procesar diferentes tipos de datos estructurados, a menudo se recurre a enumeraciones y largas sentencias `switch` o `if-else` para determinar qué función invocar según el tipo de dato. El polimorfismo elimina esta necesidad: permite invocar un método común sobre una referencia de la clase padre, y el entorno de ejecución se encarga automáticamente de derivar la ejecución hacia el código correcto según el objeto real que se encuentre en memoria. Esto facilita enormemente el procesamiento de colecciones de objetos heterogéneos pero emparentados por la herencia.

Por su parte, la sobreescritura (conocida en inglés como *overriding*) es el mecanismo que permite a una clase hija proporcionar su propia implementación de un método que ya ha sido definido y heredado de su clase padre. Para que el compilador considere que un método está siendo sobreescrito, la nueva función en la subclase debe poseer exactamente la misma firma: debe coincidir el nombre del método, el tipo de dato que retorna y el número y tipo de los parámetros que recibe. 

La sobreescritura es la pieza fundamental que hace funcionar el polimorfismo en tiempo de ejecución. Cuando se invoca un método sobreescrito a través de una referencia genérica de la superclase, el sistema determina qué bloque de código ejecutar basándose en la clase con la que se construyó el objeto (`new`), no en el tipo de la variable que lo apunta. Es un comportamiento dinámico análogo a lo que en C++ se logra "por debajo" mediante el uso de tablas de funciones virtuales (*v-tables*) y punteros a funciones.

```java
class Figura {
    // Método en la clase padre
    void dibujar() {
        System.out.println("Dibujando una figura genérica");
    }
}

class Circulo extends Figura {
    // Sobreescritura del método en la clase hija
    @Override
    void dibujar() {
        System.out.println("Dibujando un círculo");
    }
}

class Ejemplo {
    public static void main(String[] args) {
        // Polimorfismo: Referencia de tipo padre, objeto de tipo hijo
        Figura miFigura = new Circulo(); 
        
        // Se ejecuta la versión sobreescrita de Circulo, no la de Figura
        miFigura.dibujar(); 
    }
}
```

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

### Respuesta    
La ligadura dinámica (también conocida como enlace tardío o *late binding*) es el mecanismo mediante el cual el sistema resuelve a qué bloque de código debe saltar para ejecutar un método en tiempo de ejecución, en lugar de hacerlo durante la fase de compilación. En lenguajes como C, lo habitual es el enlace temprano: el compilador asocia directamente la llamada a una función con su dirección de memoria específica. La ligadura dinámica, por el contrario, retrasa esta decisión. Al ejecutarse el programa, el sistema inspecciona el objeto real que se encuentra en memoria y busca la implementación correcta del método en ese momento. Este mecanismo es el motor interno que hace posible el polimorfismo, actuando de forma análoga a la resolución de un puntero a función que cambia dinámicamente según el contexto de ejecución.

La necesidad de especificar explícitamente este comportamiento depende de la filosofía de diseño de cada lenguaje. En C++, priorizando el rendimiento, el enlace temprano es el comportamiento predeterminado. Para que una función pueda participar en la ligadura dinámica y ser polimórfica, el programador debe indicarlo explícitamente en la clase padre utilizando la palabra reservada `virtual`. En contraste, Java adopta el enfoque inverso para facilitar la orientación a objetos: por defecto, todos los métodos de instancia (que no sean `static`, `final` o `private`) utilizan ligadura dinámica automáticamente, sin necesidad de emplear palabras clave adicionales para habilitar el polimorfismo.

Por su parte, Python opera bajo un paradigma completamente diferente al ser un lenguaje interpretado y de tipado dinámico. En Python no existe una fase de compilación que verifique estrictamente los tipos o enlace funciones de antemano; por lo tanto, el enlace tardío es absoluto y natural. Toda llamada a un método se evalúa en el momento exacto de su ejecución. Incluso se prescinde de la necesidad de una herencia estricta: si en tiempo de ejecución el objeto actual posee un método con el nombre solicitado, este se ejecuta, un concepto que maximiza la flexibilidad del enlace dinámico sin requerir declaraciones explícitas.

```cpp
// Ejemplo en C++: Requiere indicación explícita
class Figura {
public:
    // La palabra 'virtual' habilita la ligadura dinámica
    virtual void dibujar() { 
        cout << "Dibujando figura"; 
    }
};
```

```java
// Ejemplo en Java: Comportamiento implícito
class Figura {
    // No requiere palabras clave, la ligadura dinámica es por defecto
    void dibujar() {
        System.out.println("Dibujando figura");
    }
}
```

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

### Respuesta   
En el siguiente bloque de código se ilustra la aplicación práctica del polimorfismo mediante una jerarquía de clases simple. Se define una clase base `Soldado` con un método predeterminado `saludar()`. A partir de ella, se crean dos subclases: `Zapador` y `Artillero`. Siguiendo los principios de la herencia que ya se dominan, ambas clases hijas adquieren las características del padre. Sin embargo, `Zapador` proporciona su propia implementación de `saludar()`, reemplazando por completo el comportamiento original mediante sobreescritura, mientras que `Artillero` opta por heredar y mantener la funcionalidad base intacta.

Para demostrar el comportamiento polimórfico, se construye un arreglo tipado explícitamente con la clase base `Soldado`. En C, intentar agrupar diferentes estructuras en un mismo arreglo requeriría el uso de uniones o arreglos de punteros genéricos (`void*`), perdiendo la seguridad de tipos y complicando el acceso a los datos. En Java, el polimorfismo permite que sea perfectamente válido y seguro almacenar referencias a objetos instanciados como `Zapador` o `Artillero` dentro de este arreglo de `Soldado`, ya que la herencia garantiza que ambas clases hijas "son un" tipo de soldado.

El aspecto más destacado del polimorfismo se observa durante el recorrido iterativo de dicho arreglo. Al invocar el método `saludar()` empleando la variable de iteración (que es una referencia genérica de tipo `Soldado`), el entorno de ejecución aplica la ligadura dinámica. Si el objeto subyacente en esa posición de memoria fue construido como un `Zapador`, se ejecuta su método sobreescrito; si fue construido como un `Artillero`, se ejecuta el método heredado del padre. Se logra así procesar elementos distintos de forma uniforme, sin necesidad de escribir complejas estructuras de control `if` o `switch` para consultar el tipo específico de cada elemento antes de actuar.

```java
class Soldado {
    public void saludar() {
        System.out.println("¡Señor, sí, señor! Soy un soldado estándar.");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("¡Despejando el camino! Soy un zapador de combate.");
    }
}

class Artillero extends Soldado {
    // Al no sobreescribir el método, hereda exactamente el comportamiento de Soldado
}

public class Campamento {
    public static void main(String[] args) {
        // Creación de un arreglo polimórfico
        Soldado[] peloton = new Soldado[3];
        
        // Se almacenan objetos de las clases hijas en referencias de la clase padre
        peloton[0] = new Zapador();
        peloton[1] = new Artillero();
        peloton[2] = new Zapador();

        System.out.println("--- Inspeccionando el pelotón ---");
        
        // Se recorre el arreglo operando de forma abstracta
        for (int i = 0; i < peloton.length; i++) {
            // El sistema decide qué versión de saludar() ejecutar en tiempo real
            peloton[i].saludar();
        }
    }
}
```

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

### Respuesta   
Es totalmente posible y, de hecho, constituye una práctica de diseño muy recomendada. La sobreescritura no obliga a descartar por completo el código de la clase base; permite reutilizar su lógica interna e incorporar comportamiento adicional. Esta técnica previene la duplicación de código, de forma análoga a cómo en C una función especializada puede delegar parte de su trabajo llamando internamente a una función de utilidad más general, para luego añadir su propio procesamiento al resultado obtenido.

Para llevar a cabo esta invocación en Java, se emplea la palabra clave **`super`**. Al escribir `super.saludar()` dentro del método sobreescrito de la clase hija, se le indica al entorno de ejecución que debe omitir temporalmente la ligadura dinámica y ejecutar explícitamente la versión del método definida en la superclase. Se trata de un mecanismo semánticamente idéntico a la llamada `super()` que ya se conoce para invocar al constructor del padre, pero aplicado a los métodos de instancia convencionales. En C++, este mismo comportamiento se lograría resolviendo el ámbito estáticamente mediante el operador de resolución (por ejemplo, `Soldado::saludar()`).

```java
class Soldado {
    public void saludar() {
        System.out.print("¡Señor, sí, señor! Soy un soldado estándar. ");
    }
}

class Zapador extends Soldado {
    @Override
    public void saludar() {
        // Se invoca el comportamiento original de la clase base
        super.saludar();
        // Se concatena el comportamiento específico de la clase hija
        System.out.println("¡ZAPADOR A SUS ÓRDENES!");
    }
}

public class PruebaZapador {
    public static void main(String[] args) {
        Soldado miZapador = new Zapador();
        
        // La ligadura dinámica llama al método sobreescrito en Zapador, 
        // el cual a su vez delega internamente en el método de Soldado
        miZapador.saludar(); 
        
        // Salida esperada:
        // ¡Señor, sí, señor! Soy un soldado estándar. ¡ZAPADOR A SUS ÓRDENES!
    }
}
```

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta    
Al llevar a cabo la sobreescritura de un método en Java, las restricciones sobre su definición son estrictas para garantizar el correcto funcionamiento del polimorfismo. Los parámetros deben coincidir de forma exacta en número, orden y tipo con los definidos en la clase base; cualquier mínima alteración provocará que el método no se sobreescriba. Respecto al tipo de retorno, la regla general exige que sea idéntico, aunque Java admite "tipos de retorno covariantes", permitiendo que la subclase devuelva un tipo más específico (una clase hija) del que devolvía el método original. Adicionalmente, el nivel de visibilidad del método sobreescrito no puede volverse más restrictivo; no es posible, por ejemplo, ocultar como `private` un método que en la clase padre fue declarado como `public`.

Es fundamental distinguir la **sobreescritura** (*overriding*) de la **sobrecarga** (*overloading*), ya que resuelven problemas distintos en etapas diferentes. La sobreescritura requiere obligatoriamente una relación de herencia, exige una firma idéntica y se resuelve en tiempo de ejecución mediante la ligadura dinámica para alterar el comportamiento de un objeto. Por el contrario, la sobrecarga —un mecanismo idéntico a la sobrecarga de funciones que existe en C++— consiste en definir múltiples métodos con el mismo nombre pero con diferentes listas de parámetros. La sobrecarga permite tener variantes de una misma operación en una misma clase, no requiere herencia y el compilador decide estáticamente (enlace temprano) qué método ejecutar analizando los argumentos enviados en la llamada.

Para prevenir errores sutiles derivados de la confusión entre ambos conceptos, el lenguaje proporciona la anotación `@Override`. Su función es indicarle al compilador que se tiene la intención explícita de sobreescribir un método de la superclase. Si por un descuido se comete un error tipográfico en el nombre del método o se varían los tipos de los parámetros, sin esta anotación el compilador asumiría que simplemente se ha creado un método nuevo (o sobrecargado), un fallo de lógica sumamente difícil de depurar. Al emplear `@Override`, se fuerza una validación de seguridad: si el compilador no encuentra en la clase padre un método con una firma exactamente igual para sobreescribir, detendrá el proceso y emitirá un error de compilación.

```java
class Calculadora {
    // Método original
    public void procesar(int numero) {
        System.out.println("Procesando entero: " + numero);
    }
    
    // SOBRECARGA (Overloading): Mismo nombre, distintos parámetros. Se resuelve al compilar.
    public void procesar(double numero) {
        System.out.println("Procesando decimal: " + numero);
    }
}

class CalculadoraAvanzada extends Calculadora {
    // SOBREESCRITURA (Overriding): Mismo nombre, mismos parámetros. Se resuelve al ejecutar.
    // La anotación @Override asegura que la firma coincida con la del padre.
    @Override
    public void procesar(int numero) {
        System.out.println("Procesamiento avanzado del entero: " + numero);
    }
}
```

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta   
En efecto, se hace uso del polimorfismo desde las primeras etapas del aprendizaje en Java, incluso antes de ser plenamente consciente de ello. La clave reside en la jerarquía fundamental del lenguaje: todas las clases creadas en Java, sin excepción, heredan de manera implícita de una única clase base o superclase raíz llamada `Object`. Al igual que en C se podría imaginar un tipo de estructura base genérica de la cual derivan conceptualmente todos los demás datos, en Java esta superclase universal provee un conjunto de métodos estándar, entre los que destacan precisamente `toString()` y `equals()`, garantizando que cualquier objeto posea estos comportamientos.

Al proporcionar una implementación propia para `toString()` en una nueva clase, se está efectuando una sobreescritura de este método heredado de `Object`. El comportamiento polimórfico entra en acción, por ejemplo, al utilizar funciones de impresión como `System.out.println()`. Internamente, estas rutinas de la biblioteca estándar están diseñadas para aceptar un parámetro genérico de tipo `Object`. Al pasarle la instancia de una clase particular, la ligadura dinámica reconoce el tipo real almacenado en memoria y desvía la ejecución hacia la versión de `toString()` que ha sido sobreescrita, evitando ejecutar la versión genérica del padre.

Un proceso idéntico ocurre al emplear el método `equals()`. La clase padre `Object` proporciona una implementación básica que se limita a comparar si las direcciones de memoria de dos referencias son idénticas, un comportamiento análogo a comparar dos punteros en C (`ptr1 == ptr2`). Al sobreescribir este método en una subclase, se altera el comportamiento en tiempo de ejecución para evaluar el estado interno de los atributos. Así, cualquier algoritmo o estructura de datos genérica que dependa de comparar elementos operará invocando `equals()` sobre referencias `Object`, confiando en que el polimorfismo garantizará la ejecución de la lógica específica de cada clase.

Esta ubicuidad del polimorfismo marca una diferencia sustancial respecto a C. Mientras que en C la impresión o comparación de estructuras requiere indicar el formato manualmente (como en `printf`) o pasar punteros a funciones comparadoras específicas (como requiere `qsort`), en Java el entorno de ejecución automatiza esta abstracción. La combinación de una clase raíz universal y la ligadura dinámica implícita permite que el sistema opere con cualquier dato de forma genérica y segura desde el primer programa que se escribe.

```java
class Punto {
    private int x, y;

    public Punto(int x, int y) {
        this.x = x;
        this.y = y;
    }

    // Se sobreescribe el método heredado de la clase base Object
    @Override
    public String toString() {
        return "Coordenadas: (" + x + ", " + y + ")";
    }
}

public class PruebaObject {
    public static void main(String[] args) {
        Punto miPunto = new Punto(10, 20);
        
        // println está programado para recibir un 'Object'. 
        // Gracias al polimorfismo, invoca automáticamente miPunto.toString()
        System.out.println(miPunto); 
    }
}
```

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta    
Una clase abstracta es una plantilla o diseño base incompleto que agrupa características comunes para que sean heredadas por otras clases, pero que por su nivel de generalidad no se concibe como una entidad completa por sí sola. Debido a esta naturaleza deliberadamente inacabada, existe una restricción fundamental: no es posible crear instancias de una clase abstracta utilizando el operador `new`. Su único propósito estructural es servir como superclase dentro de una jerarquía de herencia, estableciendo una base sólida pero obligando a las clases hijas a completar los detalles de implementación faltantes para poder existir en la memoria.

Un método abstracto es la herramienta mediante la cual dicha clase define una acción genérica exigida, pero sin proporcionar el código que la lleva a cabo. Este método carece de cuerpo (no tiene llaves `{}`) y su definición finaliza directamente con un punto y coma, estableciendo un contrato estricto. Este concepto es el equivalente exacto a las "funciones virtuales puras" en C++ (aquellas que se declaran igualándolas a cero, como `virtual void atacar() = 0;`). Cuando una subclase hereda de una superclase que contiene métodos abstractos, el compilador le exige de forma ineludible que sobreescriba y proporcione código para todos y cada uno de esos métodos; de omitirse, la clase hija también pasaría a ser abstracta y no podría instanciarse.

Para trasladar esto a la práctica, la palabra reservada **`abstract`** debe colocarse en dos lugares precisos: antes de la palabra `class` en la definición general, y en la firma de cada método que carezca de implementación. En el modelo militar propuesto, se define el `Soldado` como abstracto. Esto le permite conservar métodos concretos como `saludar()`, de modo que el código común se siga heredando con normalidad. Simultáneamente, al declarar `atacar()` como abstracto, se delega la responsabilidad absoluta de la táctica ofensiva a las clases concretas (`Zapador` y `Artillero`), garantizando a través del polimorfismo que cualquier soldado en el arreglo poseerá una forma específica y garantizada de efectuar un ataque.

```java
// La palabra clave 'abstract' se coloca en la declaración de la clase
abstract class Soldado {
    
    // Método concreto: Posee implementación interna y se hereda tal cual
    public void saludar() {
        System.out.println("¡Señor, sí, señor!");
    }

    // Método abstracto: La palabra 'abstract' va en la firma. No lleva llaves {}.
    public abstract void atacar();
}

class Zapador extends Soldado {
    // El compilador exige implementar el método abstracto heredado
    @Override
    public void atacar() {
        System.out.println("¡Zapador colocando cargas explosivas en el búnker!");
    }
}

class Artillero extends Soldado {
    // El compilador exige implementar el método abstracto heredado
    @Override
    public void atacar() {
        System.out.println("¡Artillero disparando proyectiles de artillería pesada!");
    }
}

public class FrenteDeBatalla {
    public static void main(String[] args) {
        // ERROR: No se puede instanciar una clase abstracta
        // Soldado s = new Soldado(); 

        // Se crea un arreglo polimórfico empleando la referencia genérica
        Soldado[] escuadron = new Soldado[2];
        escuadron[0] = new Zapador();
        escuadron[1] = new Artillero();

        System.out.println("--- Iniciando maniobras ---");
        for (int i = 0; i < escuadron.length; i++) {
            // Ejecuta el código concreto heredado de la superclase
            escuadron[i].saludar(); 
            // La ligadura dinámica ejecuta el ataque específico de cada objeto
            escuadron[i].atacar();  
            System.out.println("-------------------------");
        }
    }
}
```

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta   
La palabra reservada **`final`** en Java actúa como un mecanismo restrictivo diseñado para establecer límites estrictos en la jerarquía de herencia. Cuando se aplica a la declaración de una clase completa, se prohíbe terminantemente que otras clases puedan heredar de ella; se convierte en el último eslabón de la cadena y no puede tener clases hijas. Por otro lado, si `final` se aplica de manera aislada a un método específico dentro de una clase normal, la herencia de la clase sigue permitiéndose, pero se bloquea la capacidad de cualquier subclase para sobreescribir ese método en particular. Esto garantiza que la implementación dictada por la superclase permanezca inmutable en toda su descendencia.

La relación de este modificador con el polimorfismo es esencialmente antagónica, puesto que su propósito principal es restringir o desactivar el comportamiento polimórfico. Al impedir la sobreescritura, se elimina la base sobre la que opera la ligadura dinámica. En consecuencia, el compilador puede realizar una optimización muy similar a la que ocurre por defecto en C: al tener la certeza absoluta de que el método no tendrá versiones alternativas en tiempo de ejecución, establece un enlace temprano (ligadura estática) que apunta directamente a la dirección de memoria de la función. Esto no solo mejora el rendimiento, sino que protege la lógica de negocio crítica para que no sea alterada mediante polimorfismo.

En la biblioteca estándar de Java abundan los ejemplos donde esta seguridad es un requisito ineludible. El caso más representativo es la clase **`String`**. Al estar definida explícitamente como una clase `final`, el entorno de ejecución garantiza que es imposible crear una subclase que modifique maliciosamente el comportamiento de las cadenas de texto (por ejemplo, alterando la forma en que se compara su longitud o contenido). Si `String` permitiera el polimorfismo, operaciones críticas como la validación de contraseñas o el acceso a rutas de archivos podrían verse comprometidas. Otras clases muy utilizadas que emplean esta misma restricción para blindar su estructura y funcionamiento son la clase utilitaria `Math` y las clases envolventes numéricas como `Integer` o `Double`.

```java
// Ejemplo 1: Método final
class Soldado {
    // Las clases hijas podrán heredar, pero NO sobreescribir este método
    public final void identificarse() {
        System.out.println("Soy un miembro del ejército.");
    }
}

class Zapador extends Soldado {
    // ERROR DE COMPILACIÓN: No se puede sobreescribir un método final
    /*
    @Override
    public void identificarse() {
        System.out.println("Soy un zapador."); 
    }
    */
}

// Ejemplo 2: Clase final
final class Comandante {
    public void darOrden() {
        System.out.println("¡Ataquen!");
    }
}

// ERROR DE COMPILACIÓN: No se puede heredar de una clase final
/*
class SubComandante extends Comandante { 
}
*/
```


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

### Respuesta    
Una interfaz en Java es, fundamentalmente, un contrato estricto de comportamiento. En su forma más tradicional, se puede visualizar como una estructura puramente abstracta compuesta de forma exclusiva por constantes y firmas de métodos vacíos. Trazando un paralelismo con C, una interfaz actúa de manera muy similar a un archivo de cabecera (`.h`) que únicamente declara los prototipos de las funciones sin proporcionar el código fuente que las resuelve; establece de forma unívoca qué acciones deben poder realizarse, pero omite por completo el "cómo". Al momento en que una clase decide "implementar" una interfaz, adquiere la obligación ineludible de escribir el código interno para todas y cada una de esas firmas.

Aunque las interfaces guardan fuertes similitudes conceptuales con las clases abstractas —dado que ninguna de las dos puede instanciarse con `new` y ambas delegan la implementación a las clases hijas— la diferencia radica en su propósito y en su nivel de generalidad. Una clase abstracta está diseñada para establecer una jerarquía troncal de parentesco bajo la premisa "es un" (un Zapador *es un* Soldado), y permite heredar atributos de estado (variables) y bloques de código ya programados. En contraste, una interfaz define una capacidad, habilidad o rol transversal bajo la premisa "puede hacer" (un Soldado *puede ser* "Conductor"). Las interfaces tradicionales carecen de variables de instancia y de código concreto, actuando como moldes puros de comportamiento.

La distinción arquitectónica más importante es que las interfaces proporcionan la solución de Java a la necesidad de la herencia múltiple. Lenguajes como C++ permiten que una clase herede directamente de múltiples clases padre simultáneamente, lo cual suele generar conflictos y ambigüedades graves si dichos padres comparten atributos o métodos (el conocido "problema del diamante"). Para mantener la seguridad y evitar estos conflictos estructurales, Java prohíbe tajantemente la herencia múltiple de clases. Sin embargo, permite sortear esta limitación autorizando a una clase a implementar múltiples interfaces de manera simultánea. Mediante la palabra clave `implements`, el objeto se dota de un polimorfismo multifacético, pudiendo ser referenciado o tratado en tiempo de ejecución por cualquiera de los "contratos" que haya suscrito.

```java
// Interfaz 1: Define una capacidad
interface Conductor {
    // Por defecto en las interfaces, los métodos son 'public' y 'abstract'
    void conducirVehiculo(); 
}

// Interfaz 2: Define otra capacidad distinta
interface Francotirador {
    void apuntarLargoAlcance();
}

// Clase base genérica
class Soldado {
    public void saludar() {
        System.out.println("¡Señor, sí, señor!");
    }
}

// HERENCIA + MÚLTIPLES INTERFACES:
// Solo se extiende UNA clase, pero se implementan VARIAS interfaces separadas por comas.
class OperadorEspecial extends Soldado implements Conductor, Francotirador {

    // Cumpliendo el contrato de Conductor
    @Override
    public void conducirVehiculo() {
        System.out.println("Operador al volante del jeep táctico.");
    }

    // Cumpliendo el contrato de Francotirador
    @Override
    public void apuntarLargoAlcance() {
        System.out.println("Operador ajustando la mira telescópica.");
    }
}
```   

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta    
El diseño propuesto ilustra la potencia combinada de la abstracción y el polimorfismo. Se parte de una clase abstracta `Punto` que define el contrato genérico: la capacidad de calcular la distancia hacia otro punto. Al recibir como parámetro una referencia de la propia clase base `Punto`, el método se vuelve polimórfico, pudiendo aceptar cualquier tipo de coordenada geométrica que se defina en el futuro. Las subclases `Punto2D` y `Punto3D` heredan esta estructura y proporcionan las fórmulas matemáticas exactas (el teorema de Pitágoras) correspondientes a su número de dimensiones.

Sin embargo, al operar con parámetros genéricos, surge un desafío: un cálculo matemático carece de sentido si se intenta medir la distancia entre un punto de dos dimensiones y uno de tres. Para resolver esto y garantizar la seguridad espacial, se emplea el operador **`instanceof`**, el cual permite consultar en tiempo de ejecución si el objeto que se ha recibido pertenece a una clase específica. Si la comprobación es exitosa, se procede a realizar un **downcasting** (moldeo hacia abajo). Esto es el equivalente directo a tomar un puntero genérico `void*` en C y forzar su conversión (`cast`) a un puntero de una estructura específica (`struct Punto2D*`). Una vez moldeado, se obtiene acceso a los atributos internos (`x`, `y`, `z`) del punto recibido para aplicar la fórmula. Si la comprobación falla, se lanza una excepción para abortar la operación, un concepto ya conocido.

Finalmente, el verdadero valor arquitectónico del polimorfismo se materializa en la clase `Linea`. Mediante el mecanismo de composición, una línea se define por dos puntos de inicio y fin. La clave radica en que `Linea` almacena referencias genéricas de tipo `Punto` y, por consiguiente, ignora por completo si está trabajando en un plano bidimensional o en un espacio tridimensional. Cuando se le solicita calcular su longitud, simplemente delega la tarea invocando el método `calcularDistanciaA` sobre sus puntos. La ligadura dinámica se encarga de todo lo demás, ejecutando la matemática correcta sin necesidad de escribir algoritmos separados para "Líneas 2D" o "Líneas 3D".

```java
abstract class Punto {
    // El parámetro es genérico para aceptar cualquier tipo de punto
    public abstract double calcularDistanciaA(Punto otro);
}

class Punto2D extends Punto {
    private double x, y;

    public Punto2D(double x, double y) {
        this.x = x;
        this.y = y;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        // Se verifica en tiempo de ejecución si el objeto es compatible
        if (!(otro instanceof Punto2D)) {
            throw new IllegalArgumentException("Error: No se puede mezclar 2D con otros tipos.");
        }
        
        // Downcasting: Se moldea la referencia genérica a la específica
        Punto2D p2 = (Punto2D) otro;
        
        return Math.sqrt(Math.pow(this.x - p2.x, 2) + Math.pow(this.y - p2.y, 2));
    }
}

class Punto3D extends Punto {
    private double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x;
        this.y = y;
        this.z = z;
    }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (!(otro instanceof Punto3D)) {
            throw new IllegalArgumentException("Error: No se puede mezclar 3D con otros tipos.");
        }
        
        Punto3D p3 = (Punto3D) otro;
        
        return Math.sqrt(Math.pow(this.x - p3.x, 2) + 
                         Math.pow(this.y - p3.y, 2) + 
                         Math.pow(this.z - p3.z, 2));
    }
}

class Linea {
    private Punto inicio;
    private Punto fin;

    // Composición: La línea se construye con dos puntos genéricos
    public Linea(Punto inicio, Punto fin) {
        this.inicio = inicio;
        this.fin = fin;
    }

    public double calcularLongitud() {
        // Polimorfismo puro: La línea no sabe qué tipo de puntos contiene,
        // simplemente invoca la operación y confía en la ligadura dinámica.
        return inicio.calcularDistanciaA(fin);
    }
}

public class Geometria {
    public static void main(String[] args) {
        Punto p1_2D = new Punto2D(0, 0);
        Punto p2_2D = new Punto2D(3, 4);
        
        Punto p1_3D = new Punto3D(0, 0, 0);
        Punto p2_3D = new Punto3D(3, 4, 12);

        // La misma clase Linea sirve para cualquier dimensión
        Linea lineaPlana = new Linea(p1_2D, p2_2D);
        Linea lineaEspacial = new Linea(p1_3D, p2_3D);

        System.out.println("Longitud 2D: " + lineaPlana.calcularLongitud());
        System.out.println("Longitud 3D: " + lineaEspacial.calcularLongitud());
        
        try {
            // Esto provocará una excepción gracias a instanceof
            Linea lineaInvalida = new Linea(p1_2D, p1_3D);
            lineaInvalida.calcularLongitud();
        } catch (IllegalArgumentException e) {
            System.out.println("Excepción capturada: " + e.getMessage());
        }
    }
}
```

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta   
La herencia de interfaces en Java es un mecanismo que permite a una interfaz incorporar y expandir el contrato de otra. De manera análoga a cómo una clase hija hereda los atributos y métodos de una clase padre, una interfaz derivada hereda las firmas de métodos de su interfaz base empleando la palabra reservada `extends`. Esto facilita la creación de jerarquías de comportamiento, permitiendo construir contratos cada vez más específicos y rigurosos a partir de definiciones más generales, operando siempre a un nivel puramente abstracto, sin involucrar código de implementación.

Respecto a la herencia múltiple, Java **sí permite la herencia múltiple de interfaces**, estableciendo una clara diferencia con las restricciones aplicadas a las clases. Una interfaz puede extender varias interfaces de manera simultánea separando sus nombres por comas. Esta flexibilidad es completamente segura y no genera las ambigüedades clásicas que presenta la herencia múltiple en C++. Al carecer de variables de estado y de bloques de código interno, si dos interfaces base declaran exactamente la misma firma de método, no se produce ninguna colisión lógica; la clase concreta que implemente la interfaz final simplemente estará obligada a proporcionar una única implementación funcional que satisfaga dicho requisito.

Para ilustrar este diseño, se puede estructurar un sistema de gestión de archivos. Se define primero una interfaz básica `Fichero` que actúa como un contrato de solo lectura. Posteriormente, se crea una interfaz más especializada, `FicheroEscribible`, que extiende a la primera. Al establecer esta herencia, el nuevo contrato fusiona las exigencias: cualquier clase que decida implementar `FicheroEscribible` (por ejemplo, una clase que maneje archivos de texto plano) se verá obligada por el compilador a proporcionar el código exacto no solo para escribir y eliminar, sino también para la función de lectura heredada.

```java
// Interfaz base: Contrato de solo lectura
interface Fichero {
    String leerContenido();
}

// Herencia de interfaces: FicheroEscribible adquiere las firmas de Fichero
// Si fuera múltiple, se escribiría: extends Fichero, OtraInterfaz
interface FicheroEscribible extends Fichero {
    void escribirContenido(String texto);
    void eliminar();
}

// Clase concreta que implementa la interfaz derivada
class ArchivoTexto implements FicheroEscribible {
    private String contenidoInterno = "";

    // 1. Método exigido por herencia (proveniente de Fichero)
    @Override
    public String leerContenido() {
        return this.contenidoInterno;
    }

    // 2. Método exigido por la interfaz actual (FicheroEscribible)
    @Override
    public void escribirContenido(String texto) {
        this.contenidoInterno += texto;
        System.out.println("Contenido escrito exitosamente.");
    }

    // 3. Método exigido por la interfaz actual (FicheroEscribible)
    @Override
    public void eliminar() {
        this.contenidoInterno = null;
        System.out.println("Archivo eliminado del sistema.");
    }
}

public class SistemaArchivos {
    public static void main(String[] args) {
        // Uso polimórfico mediante la interfaz más específica
        FicheroEscribible miDocumento = new ArchivoTexto();
        
        miDocumento.escribirContenido("Datos de prueba. ");
        System.out.println("Lectura: " + miDocumento.leerContenido());
        miDocumento.eliminar();
    }
}
```

