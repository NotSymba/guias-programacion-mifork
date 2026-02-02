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

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
