<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

### Respuesta

### 1. Abstracción

Respuesta clase
Principalmente sirve para olvidar detalles, para:  

    a)Tratar con temas complejos
    b)Para facilitar el cambio,pej si se quieren cambiar partes del programa sin modificar otra.

La abstracción es el proceso mental y de diseño mediante el cual se identifican las características esenciales de una entidad, ignorando los detalles irrelevantes o de implementación interna. En C, se realiza un ejercicio de abstracción al definir una `struct` que represente, por ejemplo, un "Archivo" (`FILE`), ocultando la complejidad de los buffers y punteros de bajo nivel al programador que utiliza la librería estándar.

En el contexto de Java, la abstracción se eleva a un nivel superior. No solo se modelan los datos (como en una estructura), sino también el comportamiento abstracto. Permite definir "qué" hace un objeto sin especificar "cómo" lo hace internamente, facilitando la creación de sistemas complejos dividiéndolos en componentes lógicos manejables e independientes entre sí.

### 2. Encapsulamiento

Respuesta clase:
Unir estado y comportamiento y ocultar partes. Al ocultar partes en un programa podemos cambiar otras partes q tienen esta oculta sin que cambie el comportamiento por el cambio.

El encapsulamiento es el mecanismo que agrupa los datos y los métodos que operan sobre ellos en una misma unidad (la clase), protegiendo el acceso a los componentes internos. Si en C se acostumbra a acceder directamente a los campos de una estructura (`variable.campo = 5`), el encapsulamiento restringe esta práctica para evitar modificaciones accidentales o inconsistentes del estado del objeto.

Mediante el uso de modificadores de acceso (como `private`), se ocultan los detalles internos, exponiendo solo una "interfaz pública" controlada (métodos). Esto es comparable a tratar una librería en C como una "caja negra" donde solo se conocen las funciones declaradas en el archivo de cabecera (`.h`), mientras que la implementación y las variables estáticas internas del archivo `.c` permanecen inaccesibles para el usuario de la librería.

### 3. Herencia

Respuesta de clase:
Establecen jerarquias, del tipo: mas arriba animal(dormir) y debajo perro(rascar) y gato(maullar) esto a su vez establecen una relacion es_un con animal.

La herencia permite crear nuevas clases a partir de clases existentes, promoviendo la reutilización del código y estableciendo una jerarquía de relaciones. En C, para ampliar la funcionalidad de una estructura, a menudo se recurre a copiar y pegar campos o a incluir una estructura dentro de otra (composición). En Java, la herencia es nativa: una "Clase Hija" adquiere automáticamente los atributos y métodos de una "Clase Padre".

Este mecanismo permite definir características generales en una clase base (por ejemplo, `Vehiculo`) y características específicas en clases derivadas (como `Coche` o `Motocicleta`). Esto evita la redundancia de código, ya que cualquier lógica compartida se escribe una sola vez en la clase padre y es heredada por todas las subclases, facilitando enormemente el mantenimiento del software.

### 4. Polimorfismo

Respuesta de clase:
Misma función, distintas formas(o implementaciones).Pej se pueden tener funciones genericas y con estas se puede tener una especifica para un tipo de objeto. 

El polimorfismo es la capacidad que tienen los objetos de diferentes clases de responder al mismo mensaje (método) de maneras distintas. Desde la perspectiva de C, esto sería equivalente a tener una función que acepta un puntero genérico (o una unión) y, dependiendo del tipo de dato real, ejecuta una lógica diferente, lo cual suele requerir el uso complejo de punteros a funciones.

En Java, esto se gestiona automáticamente. Si se tiene una referencia de tipo `Vehiculo`, se puede invocar el método `moverse()`, y el sistema ejecutará la versión del método correspondiente al objeto real, ya sea un `Coche`, un `Barco` o un `Avion`. Esto permite escribir código genérico que puede funcionar con cualquier objeto que pertenezca a la misma jerarquía, aumentando la flexibilidad y escalabilidad del programa.


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

### Respuesta

**C++**   

Respuestade clase:  
Es un tipo de lenguaje compilado, esto quiere decir que tiene comprobacion estatica de tipos. Pensados para hacer programas mas grandes. No tienen recolector de basura.

Es la referencia más directa para quien proviene de C, ya que fue diseñado originalmente como una extensión de este ("C con clases"). Permite un modelo híbrido donde se puede programar tanto de forma estructurada como orientada a objetos. A diferencia de lenguajes más modernos, mantiene el acceso directo a memoria y el uso de punteros, pero introduce el concepto de clase para encapsular datos y comportamiento, otorgando al programador el control total sobre el hardware que se tiene en C, sumado a la capacidad de abstracción de la POO.

Respuesta de clase:  
Son lenguajes tambien compilados igual que C++, pero tienen recolector de basura(mirar q es esto).

**Java** es el lenguaje que popularizó masivamente la orientación a objetos, obligando a que todo el código resida dentro de clases; no existen funciones "sueltas" o globales como en C. Su principal diferencia con C y C++ es que elimina la gestión manual de memoria (no hay `free`) y el uso de punteros explícitos para evitar errores de segmentación, delegando estas tareas en una Máquina Virtual. Es el estándar académico y empresarial para aprender este paradigma de forma estricta.

**C# (C Sharp)**, desarrollado por Microsoft, comparte muchas similitudes sintácticas con C y Java. Al igual que Java, es un lenguaje de alto nivel que gestiona la memoria automáticamente y está fuertemente tipado. Se utiliza ampliamente en el desarrollo de aplicaciones de escritorio y videojuegos, ofreciendo un entorno donde la orientación a objetos facilita la creación de interfaces gráficas complejas y sistemas de eventos, algo que en C puro requiere un esfuerzo considerable de gestión de librerías.

**Python**   

Respuesta de clase:  
Suelen ser lenguajes dinamicos(ampliar respuesta).

Destaca por ser un lenguaje interpretado y de tipado dinámico, muy diferente a la rigidez sintáctica de C. En Python, absolutamente "todo es un objeto", incluyendo los números y las funciones, lo que permite una flexibilidad extrema. Aunque su sintaxis es muy limpia y casi se lee como pseudocódigo, internamente aplica rigurosamente los conceptos de herencia y polimorfismo, siendo hoy en día uno de los lenguajes más utilizados en ciencia de datos e inteligencia artificial.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

### Respuesta

Respuesta de clase:  
Primero aparecion la programacion estructurada, posteriormente la programacion modular y sobre esta la orientada a objetos.  
La programacion estructurada es la actualizacion de la programacion de ensamblador, esto era muy primitivo solo tenia secuencia y un salto arbitrario(GOTO), era todo plano nada estructurado y de vez en cuando un goto para saltar por el codigo (JMP, salto sin resultado).  
En cambio la estructurada nos aporta las estructuras de control modernas: condicional(if), iterativa(while, do-while),... Se eliminan los GOTO, gracias a estos elementos de control. Es el compilador el q ejecuta estos saltos de manera controlada.

A continuacion despues de la programacion estructurada aparecion la modular, ademas de todo lo anterior añade conceptos como: libreria, paquete, modulo. Esto agrupan codigo para facilitar su uso por otros programas o otros módulos. Este serviria para utilizar nuestro programa como librerias para otros programas.


### 3.1. Programación Estructurada

La programación estructurada es un paradigma enfocado en la claridad, calidad y tiempo de desarrollo de un programa mediante el uso exclusivo de subrutinas y tres estructuras de control básicas: **secuencia** (instrucciones ejecutadas una tras otra), **selección** (`if`/`switch`) e **iteración** (`for`/`while`). Históricamente, surgió como solución al "código espagueti", causado por el abuso de la instrucción `GOTO`, la cual permitía saltos arbitrarios en el flujo del programa, haciendo que el código fuera casi imposible de seguir y mantener.

En el contexto de C, la programación estructurada es la forma natural de escribir código dentro de una función. Se basa en el diseño "Top-Down" (de arriba hacia abajo), donde un problema complejo se descompone en subproblemas más pequeños que se resuelven paso a paso. El flujo de ejecución es predecible y jerárquico: se entra a una función, se ejecutan sus bloques lógicos y se retorna, manteniendo un orden lógico estricto que facilita la depuración y la lectura del algoritmo.

### 3.2. Programación Modular

La programación modular da un paso más allá de la simple estructuración del flujo de control; se ocupa de la arquitectura física y lógica del software. Consiste en dividir el programa en partes más pequeñas e independientes llamadas **módulos**. Cada módulo tiene una funcionalidad específica y bien definida, y se comunica con otros módulos a través de interfaces claras. La idea central es el principio de "divide y vencerás", permitiendo que diferentes programadores trabajen en distintas partes del sistema simultáneamente sin interferir entre sí.

Para un programador de C, la programación modular se materializa en la separación del código en archivos de cabecera (`.h`) y archivos de implementación (`.c`). El archivo `.h` actúa como la interfaz pública (qué hace el módulo), mientras que el `.c` contiene el código interno (cómo lo hace). Esto permite ocultar la complejidad: quien usa una librería matemática en C no necesita ver el código de la función `sqrt()`, solo necesita incluir su cabecera y saber cómo llamarla.

La relación con la Programación Orientada a Objetos (POO) es directa: la POO es una evolución de la programación modular. Mientras que un módulo en C agrupa funciones relacionadas (por ejemplo, `math.h` agrupa funciones matemáticas), una **Clase** en POO lleva esta agrupación al siguiente nivel, uniendo no solo las funciones, sino también los datos que esas funciones manipulan, creando módulos más cohesivos y autónomos.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

### Respuesta

### 1. Identidad

Respuesta de clase: la identidad es la dir de memoria en la que esta el objeto, cada objeto tiene identidad. Todos los objetos pueden paracerse pero siempre tienen una identidad unica.

La **identidad** es la propiedad que permite distinguir inequívocamente un objeto de otro, incluso si ambos contienen exactamente la misma información. En el lenguaje C, la identidad de una variable o estructura viene dada por su dirección de memoria única; aunque dos variables `struct` tengan los mismos valores en todos sus campos, son entidades diferentes porque residen en ubicaciones físicas distintas. En Java, aunque no se manipulan direcciones de memoria directamente, la máquina virtual asigna una referencia única a cada objeto creado con `new`, permitiendo diferenciarlos durante toda su ejecución.

### 2. Estado

Respuesta clase: (atributos, lo que en los structs eran campos)

El **estado** se define por el conjunto de valores que almacenan los atributos (variables internas) del objeto en un momento determinado. Si se compara con C, el estado equivale al contenido de los campos de una `struct` en un instante específico. Este estado es dinámico, ya que los valores pueden cambiar a lo largo del tiempo. Por ejemplo, un objeto "CuentaBancaria" puede tener un estado inicial con un saldo de 0, y tras varias operaciones, su estado cambiará para reflejar un saldo diferente. El estado representa la "foto actual" de los datos del objeto.

### 3. Comportamiento

Respueta clase: metodos, son las funciones que todos los objetos de esa clase pueden hacer.  
Pej: 
```
Animal a;
dormir(a);
```
Y ahora seria asi:
```
Animal a;
a.dormir();
```

El **comportamiento** determina lo que el objeto es capaz de hacer y cómo reacciona ante las interacciones con otros objetos. Se implementa mediante los **métodos** (funciones dentro de la clase). En la programación estructurada en C, el comportamiento está separado de los datos: se tienen funciones sueltas que reciben una estructura como argumento para modificarla. En la orientación a objetos, el comportamiento es intrínseco al objeto; es el propio objeto el que contiene la lógica para modificar su estado (sus atributos) o realizar acciones, encapsulando así la funcionalidad junto con los datos.


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

### Respuesta  

Respuesta clase:  
-Clase: Molde para crear instacias durante la ejecución. Define la estructura del estado(atributos) y el comportamiento(metodos).
```
Struct Animal{
    edad;
    nombre;
}
```
Esto seria sustituido por class.

-Objeto: Variables de tipo alguna clase con un estado concreto de sus atributos.

### 5.1. Clase, Objeto e Instancia

Una **Clase** no es lo mismo que un objeto; es el molde, la plantilla o el esquema conceptual que define la estructura y el comportamiento. En términos de C, una clase es equivalente a la definición de un tipo `struct` (por ejemplo, `struct Persona { ... };`). Esta definición describe qué datos tendrá (nombre, edad) y, en el caso de la orientación a objetos, qué funciones podrá ejecutar, pero por sí misma no ocupa memoria para almacenar datos ni existe físicamente en tiempo de ejecución.

Un **Objeto** es la entidad concreta creada a partir de ese molde. Es el elemento que ocupa memoria RAM y tiene valores específicos. Si la clase es el plano de un edificio, el objeto es el edificio construido. La relación entre ambos es la misma que existe en C entre un tipo de dato (como `int` o `struct Persona`) y una variable declarada de ese tipo. No se puede vivir en el plano (Clase), se vive en la casa (Objeto).

El término **Instancia** se refiere a la realización específica de una clase. Aunque en la práctica "objeto" e "instancia" se usan casi como sinónimos, "instancia" enfatiza la relación de pertenencia. Se dice que "este objeto es una instancia de la clase Coche". El proceso de **instanciación** es el acto de crear un objeto, equivalente en C a usar `malloc` para reservar memoria para una estructura y asignar esa dirección a un puntero.

### 5.2. ¿Todos los lenguajes usan clases?

No, no todos los lenguajes orientados a objetos utilizan el concepto de "clase". Aunque los lenguajes más extendidos académica e industrialmente (como Java, C++, C# y Python) son **basados en clases**, existe una familia de lenguajes que utiliza la **programación basada en prototipos**.

En lenguajes basados en prototipos, como JavaScript (en su modelo original) o Lua, no existen las clases como moldes estáticos. En su lugar, la creación de nuevos objetos se realiza "clonando" un objeto existente (el prototipo) y modificándolo según sea necesario. Es como si, en lugar de usar un plano para hacer una casa, se tomara una casa ya construida, se duplicara y se le pintaran las paredes de otro color. Esto ofrece una flexibilidad diferente a la estructura más rígida y organizada que imponen las clases.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta  

Respuesta de clase: Los objetos se almacenan en memoria en el Heap en la mayoría de lenguajes, hay otros lenguajes que permiten tambien crear objetos en el Stack. Los objetos se crean con el new.   
Ventajas de usar el Heap:   
-Reservo dinámicamente, el tamaño se decide en ejecución. Hace que el programa use justo lo que se va necesitar sin malgastar recursos.  
-Lo que está en el heap, vive más alla que el método o función donde se ha creado.   
Desventajas:   
-Hay que liberarla cuando ya no se necesita. Se puede hacerde manera manual, pero es difícil de hacer y puede no estar claro como hacerlo. O automáticamente, por ejemplo con un "Recolector de basura". En java no es necesario usar el delete ya que lo hace ya todo el recolector de basura, pero a veces este recolector de basura pasa y no hace falta lo que consume rendimiento, puede haber veces que no sea necesario.

### 6.1. Almacenamiento de Objetos en Memoria

En Java, todos los objetos se almacenan en el área de memoria dinámica conocida como **Heap** (montículo). Cuando se escribe `new Clase()`, el sistema realiza una operación equivalente a un `malloc` en C, buscando un bloque de memoria libre suficiente para alojar los atributos del objeto y devolviendo una referencia a esa dirección. Por otro lado, las variables que contienen esa referencia (los "nombres" de los objetos) se almacenan en la **Pila (Stack)**, comportándose exactamente igual que un puntero en C (`struct Tipo* puntero`).

Es crucial entender esta distinción: la variable local es solo un puntero que vive brevemente en la pila mientras se ejecuta el método, pero el objeto al que apunta persiste en el Heap hasta que ya no se necesita. Si se asigna un objeto a otro (`obj1 = obj2`), no se copia el objeto entero (como pasaría al igualar dos `struct` en C), sino que simplemente se copia la dirección de memoria, haciendo que ambas variables apunten al mismo objeto físico en el Heap.

### 6.2. Diferencias entre Lenguajes

No todos los lenguajes siguen este modelo estricto. En C++, que es un lenguaje híbrido, se permite instanciar objetos tanto en la Pila (como una variable `struct` local en C) como en el Heap (usando `new`). Los objetos en la pila se destruyen automáticamente cuando la ejecución sale del bloque de código (scope), lo que ofrece un control muy preciso y determinista sobre la vida del objeto, similar a las variables locales en C.

Por el contrario, lenguajes como Java, C# o Python fuerzan a que todos los objetos vivan en el Heap. Esta decisión de diseño simplifica el lenguaje al eliminar la distinción sintáctica entre acceder a un objeto estático (`.`) y uno dinámico (`->`) que existe en C y C++. En Java, siempre se utiliza la referencia, asumiendo que el acceso es indirecto, lo que facilita la programación a costa de abstraer el control de bajo nivel sobre la ubicación de memoria.

### 6.3. La Recolección de Basura (Garbage Collection)

La Recolección de Basura es un proceso automático que gestiona la liberación de memoria, solucionando uno de los problemas más frecuentes en C: las fugas de memoria (*memory leaks*) por olvidar llamar a `free()`. En Java, un proceso en segundo plano monitorea constantemente el Heap para identificar objetos que ya no son accesibles; es decir, objetos a los que no apunta ninguna variable o referencia activa en el programa.

Cuando el Recolector de Basura detecta estos objetos "huérfanos", recupera automáticamente el espacio que ocupan, haciéndolo disponible para nuevas asignaciones. Esto significa que el programador no necesita (ni puede) destruir explícitamente los objetos. Aunque esto previene errores fatales y simplifica el desarrollo, introduce una pequeña sobrecarga de rendimiento y hace que el momento exacto de la liberación de memoria sea impredecible, a diferencia del control determinista que ofrece `free()` en C.



## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

### Respuesta  

Respuesta de clase: La sobrecarga de metodos es escribir un metodo con el mismo nombre, esto se puede hacer cambio los atributos del metodo, asi tendriamos metodos con el mismo nombre pero sin q se produzca sobrecarga de metodos.

```java
class Calculadora{
    int sumar(int a, int b){
        return a+b;
    }

    //Es necesario cambiar al menos alguno de los parametros. No es suficiente, ni necesario el tipo de retorno.
    double sumar(double a, double b){
        return a+b;
    }
}

main(){
    Calculadora miCalculadora = new Calculadora();
    int resultado = miCalculadora.sumar(3,5);
    double resultado2 = miCalculadora.sumar(3.7,5.4);
}

```



### 7.1. El Método

Un método en la Programación Orientada a Objetos es el equivalente directo a una **función** en C, pero con una diferencia estructural fundamental: está definido dentro del contexto de una clase y opera sobre los datos de esa clase. Mientras que en C las funciones son entidades independientes ("flotantes") que reciben estructuras de datos externas para procesarlas, en Java el método representa una acción intrínseca que el objeto puede realizar. Constituye la implementación del **comportamiento** del objeto.

Técnicamente, se puede visualizar el método como una función que tiene un acceso implícito e inmediato a los atributos del objeto que lo invoca, sin necesidad de pasarlos como argumentos. Si en C se requiere escribir `calcular_area(&miRectangulo)` pasando un puntero explícitamente, en Java el método reside dentro de la definición del rectángulo, por lo que se invoca como `miRectangulo.calcularArea()`. Esto agrupa lógicamente la operación con los datos que manipula, reduciendo la posibilidad de errores al manipular estructuras incorrectas.

### 7.2. La Sobrecarga de Métodos

La sobrecarga de métodos (*Method Overloading*) es la capacidad de definir múltiples métodos con el **mismo nombre** dentro de una misma clase, siempre y cuando sus listas de parámetros (la cantidad o el tipo de los argumentos) sean diferentes. Esto es algo imposible en C estándar, donde cada nombre de función debe ser único en todo el ámbito del programa; en C, para realizar la misma operación sobre tipos distintos, es obligatorio crear nombres diferentes, como `abs()` para enteros y `fabs()` para flotantes.

En Java, el compilador resuelve esta ambigüedad analizando la "firma" del método, la cual se compone del nombre y de los tipos de sus parámetros. Esto permite escribir código mucho más limpio e intuitivo, utilizando un único concepto lógico para acciones similares. Por ejemplo, se puede tener un método `sumar(int a, int b)` y otro `sumar(double a, double b)`; al llamar a la función, el sistema elegirá automáticamente la versión correcta basándose en los datos que se le envíen, facilitando la legibilidad y el mantenimiento del software.


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta  

Respuesta de clase:

```java
class Punto{
    //Estado o atributos
    int x;
    int y;

    double calcular DistaciaAOrigen(){
        return sqrt(x*x + y*y);
    }
}

public class Ejercicio1{
    public static void main(String[] args){
        Punto mipunto=new Punto();
        mipunto.x = 5;
        mipunto.y = 6;

        double aOrigen = mipunto.calcular DistaciaAOrigen();
    }
}
```

A continuación se presenta la definición de la clase `Punto` y un ejemplo de su utilización. En el código de la clase, se observa que los atributos `x` e `y` se declaran de forma similar a los miembros de una `struct` en C. La novedad radica en que la función `calculaDistanciaAOrigen` está encapsulada dentro de la misma definición, accediendo directamente a los datos del objeto sin necesidad de recibirlos como punteros o argumentos, ya que comparte el ámbito de las variables de la instancia.

Para la lógica matemática, Java proporciona la clase utilitaria `Math`, que contiene funciones estáticas equivalentes a las de la librería `<math.h>` de C. En el ejemplo de uso, se destaca el operador `new`, el cual realiza la reserva de memoria para el objeto (instanciación). El acceso tanto a los atributos para asignarles valor como al método para ejecutar el cálculo se realiza mediante la notación de punto, unificando la sintaxis de acceso a datos y comportamiento.

```java
// Definición de la clase (El "plano")
public class Punto {
    // Atributos con visibilidad por defecto (sin public/private)
    // Se usa double para permitir decimales, similar a float/double en C
    double x;
    double y;

    // Método que calcula la distancia (El "comportamiento")
    double calculaDistanciaAOrigen() {
        // Se aplica el Teorema de Pitágoras: raíz cuadrada de (x² + y²)
        return Math.sqrt((x * x) + (y * y));
    }
}

```

```java
// Clase principal para ejecutar el ejemplo (Uso)
public class Principal {
    public static void main(String[] args) {
        // 1. Instanciación: Se crea el objeto en memoria (heap)
        Punto miPunto = new Punto();

        // 2. Estado: Se asignan valores directamente a los atributos
        miPunto.x = 3.0;
        miPunto.y = 4.0;

        // 3. Comportamiento: Se invoca el método sobre el objeto creado
        double distancia = miPunto.calculaDistanciaAOrigen();

        // Salida por consola (equivalente a printf)
        System.out.println("La distancia al origen es: " + distancia);
    }
}

```

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta  
Respuesta de clase:
static:   
-Dice que el atributo o método pertenece a la clase, no a una instancia concreta.  
-No se necesita un objeto para usarlos.   
-No existe this.    
-No puedo usar desde un método static nada que no sea static.   
-No abusar!!    
Utilizar static final se usa pra constantes como Pi.

### 9.1. El Punto de Entrada: `main`

En Java, al igual que en C, la ejecución de un programa comienza por un método llamado `main`. Sin embargo, dada la naturaleza puramente orientada a objetos de Java, este método no puede existir de forma aislada o global; debe estar obligatoriamente encapsulado dentro de una clase. La firma estándar es `public static void main(String[] args)`.

Este método recibe un array de cadenas de texto (`String[] args`), que es funcionalmente equivalente al `char *argv[]` de C, permitiendo capturar los argumentos de la línea de comandos. La diferencia principal es que en Java no se retorna un entero (`int`) al sistema operativo para indicar éxito o error (como el `return 0` en C), sino que el programa termina cuando finaliza el hilo de ejecución principal o se invoca explícitamente `System.exit()`.

```java
public class Principal {
    // Punto de entrada estándar
    public static void main(String[] args) {
        System.out.println("Hola mundo");
    }
}

```

### 9.2. El significado de `static`

La palabra clave `static` indica que un miembro (atributo o método) pertenece a la clase en sí misma y no a una instancia específica (objeto). En términos de C, un método `static` se comporta como una función global tradicional: no requiere que se haya creado una estructura ni un objeto con `new` para ser invocada. Por esta razón, el método `main` **debe** ser estático: la Máquina Virtual de Java (JVM) necesita poder llamarlo para iniciar el programa antes de que exista ningún objeto en memoria.

Si un método no fuera `static`, sería necesario instanciar la clase para poder usarlo. Como el `main` es el primer código que se ejecuta, se produce la paradoja de "el huevo y la gallina": no se podría crear el objeto para llamar a `main` porque el código para crear el objeto estaría, precisamente, dentro de `main`. Al hacerlo estático, la JVM puede invocar `Principal.main()` directamente desde la definición de la clase, sin necesidad de reservar memoria para un objeto `Principal`.

### 9.3. Uso de `static` más allá de `main`

El modificador `static` no es exclusivo del `main`; se utiliza ampliamente para definir atributos y métodos compartidos. Un **atributo estático** funciona de manera similar a una variable global en C, pero restringida al ámbito de la clase ("namespace"). Existe una única copia de esa variable en memoria para todo el programa, independientemente de cuántos objetos se creen. Si un objeto modifica una variable estática, el cambio es visible para todos los demás objetos de esa clase.

Por otro lado, los **métodos estáticos** se utilizan para crear funciones de utilidad que no dependen del estado de un objeto particular. Un ejemplo clásico es la clase `Math`. Al igual que en C se usa `<math.h>` y se llama a `sqrt()` sin asociarlo a ninguna estructura, en Java se llama a `Math.sqrt()` directamente. Estos métodos no pueden acceder a los atributos de instancia (no estáticos) porque no tienen un puntero `this` implícito.

```java
public class Utilidad {
    // Variable compartida (similar a global)
    static int contadorGlobal = 0;

    // Método utilitario (similar a función pura de C)
    static int sumar(int a, int b) {
        return a + b;
    }
}

```

### 9.4. La combinación `static final` (Constantes)

Cuando se combina `static` con el modificador `final`, se crea una constante de clase. El modificador `final` en Java indica que el valor de una variable no puede cambiar una vez asignado (inmutabilidad). Al añadirle `static`, se consigue que ese valor inmutable sea único y compartido para toda la aplicación, evitando tener una copia del mismo valor constante en cada objeto.

Esta combinación es el reemplazo directo y seguro de las macros `#define` o de las variables `const` globales de C. Por convención, estas constantes se escriben en mayúsculas. Por ejemplo, `static final double PI = 3.1416;` permite usar `PI` en cualquier parte del código accediendo a través de la clase, garantizando que su valor es constante y eficiente en términos de memoria.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta  
Respuesta de clase:   
javac es el compilador de java y crea el archivo .class (Tiempo de compilación) y con java se lanza la aplicación (Tiempo de ejecución).

### 10.1. Compilación y Ejecución desde la Línea de Comandos

Para transformar el código fuente en un programa ejecutable, Java utiliza dos herramientas principales: `javac` (el compilador) y `java` (el lanzador de la aplicación). El proceso comienza con el comando `javac NombreArchivo.java`. Este paso es análogo a ejecutar `gcc -c` en C: verifica la sintaxis y genera un archivo binario intermedio, pero en lugar de código máquina nativo, genera un archivo con extensión `.class`.

Una vez obtenido el archivo `.class`, el programa no se ejecuta directamente por el sistema operativo (como haría un `.exe` o `a.out` generado por C). En su lugar, se invoca el comando `java NombreClase` (sin la extensión). Este comando inicia el entorno de ejecución, carga la clase especificada y busca el método `main` estático para comenzar la ejecución. Es fundamental notar que mientras `javac` trabaja con archivos, `java` trabaja con **clases**.

```bash
# 1. Compilación: Genera el archivo 'HolaMundo.class'
javac HolaMundo.java

# 2. Ejecución: Arranca la JVM y ejecuta la clase
java HolaMundo

```

### 10.2. Bytecode y Archivos `.class`

Los archivos `.class` contienen lo que se conoce como **Bytecode**. El bytecode es un conjunto de instrucciones de bajo nivel, muy similar al lenguaje ensamblador, pero diseñado para un procesador abstracto en lugar de una CPU física específica (como x86 o ARM). En C, el compilador traduce el código fuente directamente a instrucciones específicas de la arquitectura del hardware (código máquina); si se cambia de sistema operativo o procesador, se debe recompilar el código fuente.

En Java, el bytecode es universal y neutral respecto a la arquitectura. Esto significa que el mismo archivo `.class` generado en Windows puede ser llevado a Linux o macOS y ejecutarse sin necesidad de recompilación. El bytecode actúa como un "lenguaje máquina universal", desacoplando el desarrollo del software de la plataforma de hardware subyacente.

### 10.3. La Máquina Virtual de Java (JVM)

La **Máquina Virtual de Java (JVM)** es el componente de software encargado de interpretar y ejecutar el bytecode. Actúa como un intermediario entre el bytecode y el hardware real. Cuando se ejecuta el comando `java`, en realidad se está iniciando una instancia de la JVM, la cual funciona como una "computadora dentro de la computadora".

Su función principal es cargar los archivos `.class` y traducir las instrucciones de bytecode a código máquina nativo del sistema anfitrión en tiempo de ejecución. Esto se realiza a menudo mediante una técnica llamada compilación JIT (Just-In-Time), que optimiza el rendimiento acercándolo al de C. Mientras que en C el sistema operativo gestiona directamente la memoria y el procesador, en Java la JVM abstrae estos recursos, proporcionando servicios como la Recolección de Basura y garantizando la seguridad de tipos, impidiendo accesos ilegales a memoria que serían posibles en C.


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta  
Respuesta de clase: El new reserva memoria, invoca al constructor y es una expresión(puedo asignarla a una variable, o usarla directamente en linea).

### 11.1. El operador `new`

El operador `new` es la instrucción encargada de solicitar memoria al sistema para almacenar un nuevo objeto. En C, cuando se necesita memoria dinámica para una estructura, se utiliza la función `malloc()`, la cual devuelve un puntero genérico a un bloque de bytes vacíos en el *heap*. En Java, `new` realiza una función idéntica: reserva el espacio necesario en el *heap* para alojar todos los atributos de la clase y devuelve una **referencia** (un puntero gestionado) a esa ubicación de memoria.

Sin embargo, `new` es más sofisticado que `malloc`. Mientras que `malloc` simplemente entrega memoria "cruda" (que puede contener basura si no se usa `calloc` o se inicializa manualmente), el operador `new` en Java garantiza que la memoria se limpie y, acto seguido, invoca automáticamente al **constructor** de la clase. Por lo tanto, `new` no solo reserva el espacio, sino que también asegura que el objeto se inicialice correctamente antes de devolver su referencia a la variable.

### 11.2. El Constructor

Un **constructor** es un bloque de código especial dentro de la clase que tiene como única función inicializar el objeto recién creado. Se reconoce fácilmente porque **tiene el mismo nombre que la clase** y, a diferencia de los métodos normales, **no devuelve ningún tipo de dato** (ni siquiera `void`). Su objetivo es establecer los valores iniciales de los atributos, asegurando que el objeto nazca con un estado válido y coherente.

Si se compara con C, imaginemos que cada vez que se hace un `malloc` para una `struct`, fuera obligatorio llamar inmediatamente a una función `inicializar_struct()`. En C, esto depende de la disciplina del programador; si se olvida, la estructura puede tener datos corruptos. En Java, el constructor fuerza esta inicialización: es imposible crear un objeto con `new` sin que se ejecute un constructor. Si el programador no define ninguno, Java proporciona uno "por defecto" invisible que pone todos los números a cero y las referencias a `null`.

### 11.3. Ejemplo: Clase `Empleado`

A continuación se muestra la clase `Empleado` con un constructor que recibe parámetros. Se utiliza la palabra clave `this`, que es un puntero que hace referencia al "propio objeto" actual. Es equivalente a cuando en C una función recibe un puntero a la estructura (`struct Empleado *this`) para saber qué datos modificar. Aquí, `this.dni` se refiere al atributo del objeto, mientras que `dni` (sin `this`) se refiere al parámetro recibido en el constructor.

```java
public class Empleado {
    // Atributos
    String dni;
    String nombre;
    String apellidos;

    // Constructor: Se llama igual que la clase y recibe parámetros
    public Empleado(String dni, String nombre, String apellidos) {
        // 'this' diferencia el atributo de la clase del parámetro del método
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

// Ejemplo de uso en otra clase (Main)
class Principal {
    public static void main(String[] args) {
        // Al usar 'new', estamos OBLIGADOS a pasar los 3 datos
        // No se puede crear un empleado "vacío" si no existe un constructor vacío
        Empleado e1 = new Empleado("12345678A", "Ana", "García");
    }
}

```


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta  
Respuesta de clase: this hacer referencia al objeto actual, sirve para desambiguar

### 12.1. Concepto y Funcionamiento

La referencia `this` es una palabra clave que actúa como un puntero implícito al propio objeto que está ejecutando el código en ese momento. Para un programador de C, `this` es exactamente equivalente al puntero a la estructura que se suele pasar como primer argumento en las funciones que manipulan datos (por ejemplo, `void funcion(struct Estructura *ptr)`). En Java, este puntero no se declara en los parámetros del método, sino que existe automáticamente dentro de cualquier método no estático, permitiendo al objeto referirse a sus propios miembros (atributos y métodos).

Su uso más frecuente y necesario ocurre cuando hay colisión de nombres, un fenómeno conocido como *shadowing* (ocultación). Si un parámetro de un método o constructor tiene el mismo nombre que un atributo de la clase, el parámetro tiene prioridad y "tapa" al atributo. En ese caso, `this` sirve para desambiguar: `x` se refiere al parámetro local, mientras que `this.x` accede explícitamente al atributo almacenado en la memoria del objeto (el *heap*).

### 12.2. Nomenclatura en otros lenguajes

Aunque el concepto de autorreferencia es universal en la Programación Orientada a Objetos, la palabra clave varía según el lenguaje. Los lenguajes con sintaxis derivada de C (como C++, C#, Java y JavaScript) utilizan mayoritariamente `this`. Sin embargo, otros lenguajes populares optan por términos diferentes. Python y Swift, por ejemplo, utilizan `self`, mientras que Visual Basic utiliza `Me`.

Una diferencia técnica importante respecto a Python es la declaración explícita. En Python, es obligatorio escribir `self` como el primer parámetro de cada método (`def metodo(self, arg):`). En Java y C++, el paso de este puntero es transparente y gestionado por el compilador; solo se escribe `this` cuando se necesita resolver una ambigüedad o pasar el objeto actual como argumento a otro método, lo que limpia la firma de las funciones.

### 12.3. Ejemplo en la clase `Punto`

En el siguiente ejemplo se ha modificado el constructor de la clase `Punto`. Obsérvese cómo los argumentos del constructor se llaman `x` e `y`, coincidiendo con los nombres de los atributos. Sin el uso de `this`, la asignación `x = x` sería inocua (asignaría el parámetro a sí mismo) y los atributos del objeto quedarían sin inicializar.

```java
public class Punto {
    double x;
    double y;

    // Constructor con parámetros que tienen EL MISMO NOMBRE que los atributos
    public Punto(double x, double y) {
        // ERROR: x = x; // Esto solo asigna el parámetro a sí mismo.
        
        // CORRECTO: Usamos 'this' para referirnos al atributo de la clase
        this.x = x; 
        this.y = y;
    }

    // Método para desplazar el punto actual sumando coordenadas
    public void desplazar(double dx, double dy) {
        // Aquí no es estrictamente necesario 'this' porque no hay ambigüedad,
        // pero se puede usar para mayor claridad.
        this.x += dx;
        this.y += dy;
    }
}

```


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta  

### 13.1. Interacción entre objetos del mismo tipo

En la programación estructurada en C, para calcular la distancia entre dos puntos, sería necesario definir una función que recibiera dos estructuras como argumentos: `double distancia(struct Punto p1, struct Punto p2)`. En la Programación Orientada a Objetos, la lógica cambia ligeramente: se le "pide" a un objeto (el sujeto) que calcule su distancia respecto a otro (el objeto pasado como parámetro). Por tanto, el método solo requiere un argumento, ya que el primer punto es el propio objeto que ejecuta el código (`this`).

Dentro del método, se accede a las coordenadas del objeto actual mediante `this.x` y `this.y` (o simplemente `x` e `y` si no hay ambigüedad), y a las coordenadas del objeto externo utilizando la notación de punto sobre el parámetro: `otroPunto.x` y `otroPunto.y`. Es importante notar que, al estar dentro de la definición de la clase `Punto`, se tiene permiso para acceder a los atributos de cualquier objeto `Punto`, incluso si fueran privados, ya que el control de acceso es a nivel de clase, no de instancia.

### 13.2. Implementación en Java

El siguiente código muestra la clase actualizada. Se utiliza la fórmula matemática habitual de la distancia euclidiana .

```java
public class Punto {
    double x;
    double y;

    // Constructor
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método que calcula la distancia entre 'this' y 'otroPunto'
    public double distanciaA(Punto otroPunto) {
        // 1. Calculamos la diferencia de coordenadas
        // 'this.x' es mi coordenada, 'otroPunto.x' es la del argumento
        double cateto1 = this.x - otroPunto.x;
        double cateto2 = this.y - otroPunto.y;

        // 2. Aplicamos Pitágoras (hipotenusa)
        return Math.sqrt((cateto1 * cateto1) + (cateto2 * cateto2));
    }
}

```

```java
// Ejemplo de uso
public class Principal {
    public static void main(String[] args) {
        Punto p1 = new Punto(0, 0);
        Punto p2 = new Punto(3, 4);

        // Se lee: "p1, calcula tu distancia a p2"
        double d = p1.distanciaA(p2); 
        
        System.out.println("Distancia: " + d); // Salida: 5.0
    }
}

```


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta  
Respuesta de clase: Los datos primitivos se pasar por valor(int,char,etc. siempre los que tengan minisula). Y los objetos por referencia (copia de la referencia) 

### 14.1. Paso de Objetos: Valor de la Referencia

En Java, técnicamente **todo se pasa por valor**, pero es crucial entender qué es ese "valor" en el caso de los objetos. Cuando se pasa un objeto (como `Punto`) a un método, no se copia el objeto entero (todos sus datos en el Heap), sino que se **copia la referencia** (la dirección de memoria). Para un programador de C, esto es exactamente equivalente a pasar un puntero a una función (`void funcion(struct Punto *p)`).

Dado que el método recibe una copia de la dirección de memoria que apunta al objeto original, si dentro del método se modifican los atributos del objeto (por ejemplo, `p.x = 50;`), **esos cambios sí afectan al objeto fuera del método**. Esto ocurre porque tanto la variable original como el parámetro del método están "mirando" al mismo bloque de memoria física en el Heap. Sin embargo, si dentro del método se hiciera `p = new Punto(0,0)`, solo se cambiaría hacia dónde apunta la variable local del método, sin afectar a la referencia original externa.

### 14.2. Paso de Primitivos: Copia del Dato

Cuando se pasa un tipo primitivo (como `int`, `double`, `boolean`), el comportamiento es el de un **paso por valor** estricto y tradicional, idéntico al de C. Se crea una copia exacta del dato en la Pila (stack) para uso exclusivo del método. No existe ninguna conexión de memoria entre la variable original y el parámetro recibido.

Por consiguiente, si se recibe un entero y se modifica dentro de la función (por ejemplo, `n = n + 1;`), este cambio es puramente local. La variable original que se pasó como argumento permanece inalterada una vez que el método termina su ejecución. En Java no existen los punteros explícitos a primitivos (como `int *` en C), por lo que no es posible modificar un `int` externo directamente dentro de un método mediante argumentos.

### 14.3. Ejemplo demostrativo

```java
public class PruebaPaso {
    
    // Intenta modificar un primitivo (NO funcionará fuera)
    static void intentarCambiarInt(int numero) {
        numero = 999; 
        // 'numero' ahora vale 999, pero solo aquí dentro
    }

    // Intenta modificar un objeto (SÍ funcionará fuera)
    static void modificarPunto(Punto p) {
        // Accedemos a la dirección de memoria y cambiamos el dato real
        p.x = 100.0; 
        p.y = 100.0;
    }

    public static void main(String[] args) {
        int valor = 10;
        Punto miPunto = new Punto(0, 0);

        // 1. Prueba con Entero
        intentarCambiarInt(valor);
        System.out.println("Valor del int: " + valor); 
        // Imprime 10 (Sin cambios)

        // 2. Prueba con Objeto
        modificarPunto(miPunto);
        System.out.println("Valor del Punto: " + miPunto.x); 
        // Imprime 100.0 (El objeto fue modificado)
    }
}

```


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta  

### 15.1. El método `toString()`

El método `toString()` es un mecanismo fundamental en Java que permite obtener una representación en forma de texto (cadena de caracteres) de cualquier objeto. En C, para visualizar el contenido de una `struct`, el programador debe escribir manualmente una sentencia `printf` con los especificadores de formato adecuados para cada campo cada vez que quiera imprimirla, o bien crear una función específica como `imprimir_punto()`. En Java, se estandariza este comportamiento: se define dentro de la clase cómo debe convertirse el objeto a texto.

Técnicamente, este método se hereda de la clase cósmica `Object`, que es la "clase padre" de todas las clases en Java. Por defecto, `toString()` devuelve una cadena técnica poco útil (el nombre de la clase seguido de una arroba y un código hash hexadecimal, similar a una dirección de memoria). Al **sobrescribir** (redefinir) este método en una clase propia, se permite que al pasar el objeto a `System.out.println()`, Java invoque automáticamente esta función para mostrar una descripción legible y formateada del estado del objeto.

### 15.2. Presencia en otros lenguajes

Este concepto es un estándar de facto en la mayoría de los lenguajes orientados a objetos modernos, aunque la nomenclatura varía. La necesidad de convertir estructuras de datos complejas a texto para depuración (logging) o visualización es universal.

* En **Python**, existen dos métodos equivalentes: `__str__` (para una representación legible por el usuario final) y `__repr__` (para una representación técnica interna).
* En **C#**, el método se llama `ToString()` (con mayúscula inicial), cumpliendo exactamente la misma función que en Java.
* En **JavaScript**, también existe el método `toString()` en el prototipo de los objetos, invocándose automáticamente cuando se intenta concatenar un objeto con una cadena.

### 15.3. Ejemplo en la clase `Punto`

A continuación se implementa el método en la clase `Punto`. Nótese el uso de la etiqueta `@Override`, que indica al compilador que la intención es reemplazar el método base de Java. Al ejecutar el código, se observa que no es necesario llamar explícitamente a `.toString()`; la función `println` lo hace internamente al detectar que recibe un objeto.

```java
public class Punto {
    double x;
    double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Sobrescritura del método para devolver una cadena personalizada
    @Override
    public String toString() {
        // Retornamos el formato "(x, y)"
        // En C sería similar a preparar un buffer con sprintf
        return "(" + this.x + ", " + this.y + ")";
    }
}

// Ejemplo de uso
public class Principal {
    public static void main(String[] args) {
        Punto p = new Punto(5, 10);

        // Sin toString(), esto imprimiría algo como "Punto@15db9742"
        // Con toString(), imprime automáticamente: "(5.0, 10.0)"
        System.out.println(p); 
        
        // También funciona en concatenaciones
        System.out.println("El punto detectado es " + p);
    }
}

```


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

### Respuesta  

### 1. Similitudes Estructurales

Efectivamente, una clase en Java puede considerarse, en su nivel más básico, como un superconjunto de una `struct` en C. Ambas cumplen la función fundamental de **agregación de datos**: permiten definir un nuevo tipo de dato compuesto que agrupa variables de diferentes tipos bajo un mismo identificador. Si se eliminaran todos los métodos y se dejaran todos los atributos como públicos en una clase de Java, el resultado sería funcionalmente idéntico a una `struct` de C (un contenedor de datos pasivo).

### 2. La Integración del Comportamiento

La primera gran carencia de la `struct` para convertirse en clase es la **ausencia de métodos vinculados**. En C, los datos (la estructura) y la lógica (las funciones) viven en mundos separados; las funciones reciben estructuras, pero no forman parte de ellas. Para transformar una `struct` en una clase, es necesario mover las funciones al interior de la definición de la estructura. Esto cambia el paradigma: los datos dejan de ser algo pasivo que se pasa de un lado a otro, y se convierten en entidades activas responsables de sus propias operaciones.

### 3. El Control de Acceso y Ciclo de Vida

El segundo elemento ausente es la **seguridad y la autogestión**. Una `struct` en C es "transparente" y vulnerable; cualquier parte del código puede modificar sus campos sin restricciones, y su inicialización depende totalmente de la disciplina del programador. Para ser una clase, se requiere añadir **encapsulamiento** (poder ocultar datos con `private`) y **constructores**. Esto garantiza que la "instancia" no sea simplemente un bloque de memoria reservada (`malloc`), sino un objeto que nace con un estado válido garantizado y que protege su integridad interna contra modificaciones arbitrarias.

### 4. Relaciones Jerárquicas

Finalmente, a la `struct` le falta la capacidad de relacionarse jerárquicamente con otras estructuras. En C, las estructuras son islas independientes; no existe un mecanismo nativo para decir que una `struct Coche` *es un tipo de* `struct Vehiculo` y heredar sus campos automáticamente. Para alcanzar la categoría de clase, el sistema debe soportar **herencia y polimorfismo**, permitiendo extender y especializar el comportamiento de los tipos base sin reescribir código, algo que la estructura plana de C no permite por diseño.



## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta  

### 17.1. La Emulación: Estructura + Función Externa

Para emular una clase en C, se debe separar la definición de los datos de la definición del comportamiento. Primero, se define un `struct Punto` que contenga las coordenadas `x` e `y`. Dado que C no permite escribir funciones dentro de la estructura, la función `calcularDistancia` debe escribirse fuera, en el ámbito global.

La clave para vincular esa función con la estructura reside en los parámetros. La función no puede "adivinar" sobre qué punto debe operar, por lo que es obligatorio pasarle un puntero a la estructura `Punto` como argumento. De esta forma, la función recibe la dirección de memoria de los datos que debe procesar, accediendo a ellos mediante el operador flecha (`->`). Lo que en Java es una unidad indivisible (objeto), en C se convierte en dos piezas separadas que colaboran explícitamente.

### 17.2. La revelación de `this`

Al analizar la implementación en C, se descubre el "truco" de la referencia `this`. En el código de C que se muestra abajo, el primer parámetro de la función se ha llamado deliberadamente `this` (`Punto *this`). Este puntero cumple exactamente la misma función que la palabra clave `this` en Java: permite a la función saber cuál es la instancia concreta sobre la que está operando.

En Java, el compilador oculta este primer parámetro para simplificar la sintaxis. Cuando se escribe `p.calcularDistancia()`, internamente el compilador lo traduce a algo muy similar a `Punto_calcularDistancia(p)`. Por tanto, `this` no ha desaparecido; simplemente se ha vuelto implícito. La "magia" de la POO es, en realidad, un paso automático de un puntero a la estructura como primer argumento oculto de cada método.

```c
#include <stdio.h>
#include <math.h>

// 1. Los Datos (La "Clase" sin métodos)
typedef struct {
    double x;
    double y;
} Punto;

// 2. El Comportamiento (El "Método" externo)
// Observa el primer parámetro: es el equivalente explícito a 'this'
double calcularDistanciaOrigen(Punto *this) {
    // En Java sería: return Math.sqrt(this.x * this.x + this.y * this.y);
    // En C usamos el operador flecha sobre el puntero recibido
    return sqrt((this->x * this->x) + (this->y * this->y));
}

int main() {
    // 3. Instanciación (en el stack para simplificar)
    Punto p;
    
    // 4. Estado (Inicialización manual, sin constructor)
    p.x = 3.0;
    p.y = 4.0;

    // 5. Llamada al método
    // En Java: p.calcularDistanciaOrigen();
    // En C: Pasamos explícitamente la dirección de 'p' como argumento
    double distancia = calcularDistanciaOrigen(&p);

    printf("Distancia: %f\n", distancia);
    return 0;
}

```
