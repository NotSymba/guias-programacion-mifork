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

### Respuesta   

### Control de Errores Clásico en C

En los lenguajes estructurados tradicionales como C, al carecer de un mecanismo nativo de excepciones, la gestión de anomalías recae en la comprobación explícita dentro del flujo del programa. Cuando una función matemática, como el cálculo de una raíz cuadrada, recibe un dato fuera de su dominio válido (por ejemplo, un número negativo), es indispensable notificar al bloque de código invocador para que este tome las medidas oportunas. Es una buena práctica evitar imprimir el error directamente dentro de la función matemática; delegar esta tarea permite que la función sea completamente reutilizable y mantenga una única responsabilidad, preparando así el terreno para conceptos de encapsulación más avanzados.

La primera opción para reportar este fallo consiste en **retornar un valor centinela** directamente como resultado de la función. Dado que la raíz cuadrada de un número real nunca produce un resultado negativo, es posible utilizar un valor específico, como un `-1.0`, para señalizar inequívocamente que la operación no pudo completarse con éxito. Posteriormente, el código principal que realiza la llamada debe evaluar el valor devuelto y, únicamente si coincide con el centinela pactado, proceder a informar al usuario de la incidencia.

```c
#include <stdio.h>
#include <math.h>

/* Opción 1: Retorno de un valor centinela */
float raiz_centinela(float numero) {
    if (numero < 0.0f) {
        return -1.0f; /* Valor centinela que indica error */
    }
    return sqrt(numero);
}

int main() {
    float resultado = raiz_centinela(-4.0f);
    
    if (resultado == -1.0f) {
        printf("Error: No se puede calcular la raíz de un número negativo.\n");
    } else {
        printf("El resultado es: %f\n", resultado);
    }
    return 0;
}

```

La segunda alternativa se basa en separar conceptualmente el estado de la ejecución del resultado matemático esperado, empleando **un parámetro de salida por referencia (puntero)** junto con el retorno de un código de estado. En este diseño, la función devuelve un valor entero que actúa como indicador de éxito o fracaso (por ejemplo, `1` para éxito y `0` para error), mientras que el cálculo real se almacena directamente en la dirección de memoria proporcionada. Esta técnica resulta fundamental y altamente escalable cuando todos los rangos numéricos posibles de retorno son válidos, haciendo imposible la reserva de un valor centinela.

```c
#include <stdio.h>
#include <math.h>

/* Opción 2: Retorno de código de estado y resultado por referencia */
int raiz_referencia(float numero, float *resultado) {
    if (numero < 0.0f) {
        return 0; /* Código que indica fallo en la operación */
    }
    *resultado = sqrt(numero);
    return 1;     /* Código que indica éxito de la operación */
}

int main() {
    float valor_calculado;
    int estado = raiz_referencia(-4.0f, &valor_calculado);

    if (estado == 0) {
        printf("Error: No se puede calcular la raíz de un número negativo.\n");
    } else {
        printf("El resultado es: %f\n", valor_calculado);
    }
    return 0;
}

```

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

### Respuesta    

### Concepto y Objetivos de las Excepciones

Una **excepción** se define como un evento anómalo que ocurre durante la ejecución de un programa e interrumpe su flujo normal de instrucciones. En contraste con lenguajes como C, donde los fallos se gestionan mediante códigos de retorno numéricos o indicadores globales, en lenguajes orientados a objetos como Java, una excepción es un *objeto* real instanciado a partir de una clase específica. Este objeto encapsula toda la información fundamental sobre el error, tal como el tipo de fallo ocurrido, un mensaje descriptivo y la secuencia de llamadas a métodos (traza de la pila) que condujeron a dicho problema.

Cuando se **implementan funciones o métodos**, el objetivo principal de utilizar excepciones es delegar la responsabilidad de la gestión del error hacia niveles superiores del programa. Quien diseña un método (por ejemplo, el cálculo de una raíz cuadrada) frecuentemente desconoce cómo debe reaccionar la aplicación final ante un fallo; por ello, en lugar de intentar solucionar el problema internamente o imprimir un aviso, se "lanza" un objeto de excepción. Esto permite mantener el código enfocado en su única responsabilidad matemática, separando de forma nítida la lógica normal de la rutina de detección de anomalías.

Por otro lado, cuando se **llaman o invocan** dichas funciones, el programador emplea mecanismos de captura de excepciones con el objetivo de centralizar el control de errores de forma estructurada. En lugar de intercalar el código con múltiples sentencias condicionales (`if`) para comprobar el estado de cada retorno (como se hacía en C), se establece un bloque de código principal asumiendo el éxito de las operaciones (flujo normal) y un bloque secundario diseñado exclusivamente para capturar el objeto lanzado y ejecutar la recuperación. Esto previene la finalización abrupta de la aplicación y produce un código mucho más legible.

A modo de ilustración para contrastar con el código en C anterior, se presenta cómo se diseña este concepto en Java haciendo uso de objetos:

```java
public class Calculadora {
    
    /* Al implementar: Se lanza un objeto de excepción si hay un error */
    public double raiz(double numero) throws Exception {
        if (numero < 0.0) {
            // Se instancia un objeto que encapsula la información del error
            throw new Exception("No se puede calcular la raíz de un número negativo.");
        }
        return Math.sqrt(numero);
    }

    /* Al llamar: Se captura el objeto para manejar el error estructuradamente */
    public void calcularRaiz() {
        try {
            // Flujo normal u optimista
            double resultado = raiz(-4.0); 
            System.out.println("El resultado es: " + resultado);
        } catch (Exception objetoError) {
            // Flujo de manejo de errores
            System.out.println("Error detectado: " + objetoError.getMessage());
        }
    }
}

```

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

### Respuesta    

Para trasladar el diseño de control de errores al paradigma de Java, se emplea una clase `Calculadora` que encapsula la operación matemática. Dentro de su método `raiz`, al detectarse la condición de error (un radicando negativo), se utiliza la palabra reservada `throw` seguida de la instanciación de un objeto de excepción. Además, en el caso de usar la clase base `Exception`, es necesario declarar en la firma del método mediante la cláusula `throws` que dicha función es susceptible de emitir esta anomalía, advirtiendo así formalmente a cualquier código externo que intente invocarla.

En el punto de llamada, que en este escenario es el método principal `main`, se debe instanciar un objeto de la clase `Calculadora` para acceder a sus servicios. Para gestionar el posible error desde fuera de la función, se introduce la estructura `try-catch`, la cual reemplaza a las clásicas comprobaciones condicionales que se empleaban en C tras examinar el valor de retorno. Dentro de la sección `try` se ubican las sentencias correspondientes al flujo normal del programa; si el método `raiz` finaliza con éxito, la ejecución continúa secuencialmente y se ignora por completo la sección de captura.

Sin embargo, si se activa el error dentro de `raiz`, el flujo normal se aborta inmediatamente, impidiendo que se ejecuten las líneas de código restantes dentro del `try` (como por ejemplo, la impresión del resultado). En ese instante, la ejecución salta de forma automática al bloque `catch` correspondiente, donde se recibe como parámetro el objeto lanzado. A través de este objeto capturado, se extrae el mensaje de error y se notifica al usuario, aislando completamente la lógica de tratamiento de errores de la lógica puramente matemática.

```java
public class Calculadora {

    /* El método declara en su firma que puede propagar una excepción */
    public double raiz(double numero) throws Exception {
        if (numero < 0.0) {
            // Se interrumpe la ejecución instanciando y lanzando el error
            throw new Exception("Error: No se puede calcular la raíz de un número negativo.");
        }
        return Math.sqrt(numero);
    }

    public static void main(String[] args) {
        Calculadora miCalculadora = new Calculadora();
        
        try {
            // Flujo optimista: se intenta realizar la operación
            double resultado = miCalculadora.raiz(-4.0);
            
            // Esta línea no se ejecuta si la función lanza una excepción
            System.out.println("El resultado es: " + resultado);
            
        } catch (Exception e) {
            // Flujo de error: se captura el objeto y se gestiona el problema
            System.out.println(e.getMessage());
        }
    }
}

```

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

### Respuesta    

**"Lanzar"** una excepción consiste en instanciar un objeto que representa una condición de error y utilizar la instrucción `throw` para abortar inmediatamente la operación en curso. Es el mecanismo mediante el cual una función notifica que ha encontrado una situación que no puede resolver. Por el contrario, **"capturar"** o **"controlar"** una excepción implica definir una estructura `try-catch` capaz de interceptar ese objeto de error. Al capturarlo, se asume la responsabilidad de gestionar la anomalía, ejecutando rutinas de recuperación que evitan que la aplicación finalice de manera abrupta.

La **"propagación"** de una excepción entra en juego cuando el método donde se lanza el error no dispone de un bloque `try-catch` para controlarlo. En ese escenario, el objeto de excepción viaja automáticamente hacia atrás en la pila de llamadas (la misma estructura de memoria temporal que gestiona las invocaciones de funciones en C), buscando en los métodos invocadores algún bloque `catch` compatible. A medida que la excepción se propaga por la pila, el marco de entorno de cada función por la que transita es destruido; sus variables locales se liberan y la ejecución de dicha función se da por terminada prematuramente.

Es crucial destacar que las funciones que son atravesadas por una excepción en propagación **no se reanudan** en ningún momento. El flujo de control jamás regresa a la línea de código donde la función fue interrumpida. La ejecución únicamente continuará su curso a partir del cierre del bloque `catch` que finalmente logre interceptar la anomalía. A continuación, se amplía el diseño de la calculadora para ilustrar un método intermedio que propaga el error sin capturarlo:

```java
public class Calculadora {

    /* 1. Lanzar: Se emite la excepción y se aborta el cálculo */
    public double raiz(double numero) throws Exception {
        if (numero < 0.0) {
            throw new Exception("Raíz negativa detectada.");
        }
        return Math.sqrt(numero);
    }

    /* 2. Propagar: Este método llama a raiz() pero no usa try-catch.
       Si raiz() falla, este método se interrumpe inmediatamente. */
    public double calculoIntermedio(double num) throws Exception {
        double valor = raiz(num); 
        
        // Esta línea JAMÁS se ejecuta ni se reanuda si ocurre el error
        System.out.println("Cálculo intermedio finalizado con éxito."); 
        return valor;
    }

    /* 3. Controlar: main() detiene la propagación capturando el objeto */
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        
        try {
            calc.calculoIntermedio(-4.0);
        } catch (Exception e) {
            System.out.println("Excepción capturada y controlada: " + e.getMessage());
        }
    }
}

```

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta    

### Ventajas de la Propagación Natural de Excepciones

En lenguajes como C, la gestión de errores mediante códigos de retorno exige que cada función intermedia en la pila de llamadas evalúe y propague explícitamente el fallo hacia la función invocadora. Esto genera un código saturado de continuas comprobaciones condicionales que oscurecen la lógica principal del programa (conocido a menudo como código espagueti). La propagación natural de excepciones en Java resuelve este problema al permitir que el objeto de error escale automáticamente por la pila sin requerir código adicional en los métodos intermedios, logrando una separación nítida y elegante entre el flujo normal de ejecución y el tratamiento de anomalías.

Una segunda ventaja crucial es la eliminación de los fallos silenciosos. En el paradigma estructurado tradicional, si el programador olvida comprobar el valor devuelto por una función matemática o de lectura, la ejecución del programa continuará con un estado inválido o datos corruptos, lo que desencadena comportamientos impredecibles muy difíciles de rastrear. Por el contrario, una excepción propagada no puede ser ignorada pasivamente; si el objeto no es capturado explícitamente mediante una estructura adecuada en algún nivel de la aplicación, continuará propagándose hasta detener el hilo de ejecución, garantizando que los problemas críticos no pasen desapercibidos.

Finalmente, este mecanismo favorece la centralización y delegación del control de errores, aprovechando al máximo los conceptos de encapsulación. En lugar de obligar a que cada pequeña rutina decida cómo reaccionar ante un problema del cual carece de contexto, la propagación automática permite delegar la responsabilidad hacia capas superiores de la arquitectura (como la interfaz de usuario). De este modo, es posible agrupar múltiples llamadas a diferentes operaciones dentro de un único bloque de control, interceptando y procesando cualquier problema de manera uniforme en un solo lugar.

A continuación, se ilustra la drástica reducción de código que supone esta ventaja conceptual:

```c
/* En C: Propagación manual obligatoria y tediosa */
int calculo_intermedio() {
    int estado_raiz = raiz_referencia(-4.0f, &valor);
    
    /* Si no se incluye este 'if', el error se pierde (fallo silencioso) */
    if (estado_raiz == 0) {
        return 0; /* Se fuerza la propagación manual al nivel superior */
    }
    return 1;
}

```

```java
/* En Java: Propagación natural y automática */
public double calculoIntermedio(double num) throws Exception {
    
    // Si raiz() falla, se interrumpe y el error escala automáticamente.
    // No hay "if", ni variables temporales de estado, ni riesgo de ignorarlo.
    return raiz(num); 
}

```

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta    

### Las Excepciones como Objetos y la Encapsulación

En el paradigma orientado a objetos, las excepciones son efectivamente instancias de clases, es decir, objetos reales que residen en memoria. A diferencia de un simple código de error numérico retornado por una función en C, un objeto de excepción actúa como un contenedor dinámico de información estructurada. Cuando se produce un evento anómalo, se instancia un objeto específico que captura de forma instantánea el estado del programa en el momento del fallo, almacenando internamente datos vitales como un mensaje descriptivo, el tipo de error y la traza exacta de la pila de llamadas (el historial de funciones que condujeron al problema).

La principal ventaja de este enfoque radica precisamente en el principio de encapsulación. Al ocultar los detalles internos del error dentro de una clase, se proporciona una interfaz pública estandarizada (como el método `getMessage()`) para que los bloques `catch` interactúen con la anomalía, sin exponer ni complicar la lógica subyacente. Además, esta encapsulación permite transportar múltiples piezas de contexto (como el valor exacto de la variable que causó el fallo o códigos internos del sistema) dentro de una única entidad indivisible, evitando la necesidad de pasar múltiples variables por referencia (*punteros*) como se requeriría en C para extraer información detallada de un error.

Como consecuencia directa de este diseño, es completamente viable y una práctica habitual crear excepciones personalizadas. Esto se logra definiendo nuevas clases que hereden de las clases de excepción base proporcionadas por el lenguaje (como `Exception` en Java). La creación de tipos propios permite definir errores específicos del dominio del problema que se está resolviendo, dotándolos de atributos adicionales encapsulados y métodos propios que enriquezcan el diagnóstico, superando ampliamente las limitaciones semánticas de los errores genéricos.

A continuación, se muestra cómo evolucionaría el ejemplo de la raíz cuadrada utilizando una excepción personalizada para encapsular el valor infractor:

```java
/* 1. Creación de una excepción personalizada (Clase) */
public class NumeroNegativoException extends Exception {
    
    /* Encapsulación del estado específico que causó el error */
    private double valorErroneo; 

    public NumeroNegativoException(String mensaje, double valor) {
        super(mensaje); // Se inicializa el mensaje en la clase base
        this.valorErroneo = valor;
    }

    /* Método público para acceder al dato encapsulado */
    public double getValorErroneo() {
        return this.valorErroneo;
    }
}

/* 2. Uso en la clase Calculadora */
public class Calculadora {
    
    public double raiz(double numero) throws NumeroNegativoException {
        if (numero < 0.0) {
            // Se instancia y lanza el objeto personalizado con su contexto
            throw new NumeroNegativoException("Dominio matemático inválido.", numero);
        }
        return Math.sqrt(numero);
    }
    
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        try {
            calc.raiz(-4.0);
        } catch (NumeroNegativoException e) {
            // Se recupera la información encapsulada en el objeto
            System.out.println("Error: " + e.getMessage());
            System.out.println("El valor introducido fue: " + e.getValorErroneo());
        }
    }
}

```

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

### Respuesta    

### Información Esencial en los Objetos de Excepción

Al comparar el manejo de errores en C mediante códigos numéricos con el uso de objetos en Java, resulta evidente que la instanciación de una clase aporta una riqueza semántica incomparable. La primera pieza de información esencial que porta cualquier objeto excepción es su **propio tipo o clase**. Mientras que en C un simple `-1` o `0` requiere un conocimiento previo del significado asociado a dicho número, en Java el nombre de la clase instanciada (por ejemplo, `IllegalArgumentException` o `NumeroNegativoException`) indica de manera explícita y autodescriptiva la naturaleza del fallo en el momento en que este llega al bloque de captura.

La segunda pieza fundamental es el **mensaje de detalle**, el cual se almacena internamente como una cadena de texto durante la creación del objeto. Gracias a la encapsulación, este mensaje viaja de forma segura junto con la excepción a lo largo de su propagación. Al ser recuperado en el manejador mediante métodos públicos y estandarizados como `getMessage()`, se facilita enormemente la tarea de registrar la incidencia o informar al usuario sobre la causa exacta del problema, eliminando la necesidad de que el bloque `catch` intente deducir el contexto basándose únicamente en un valor de retorno genérico.

Finalmente, la información más valiosa para el diagnóstico y la corrección de errores es la **traza de la pila de llamadas** (*stack trace*). En los lenguajes estructurados como C, rastrear el origen exacto de un error propagado a través de múltiples funciones intermedias es una tarea compleja que habitualmente requiere el uso de depuradores paso a paso. Por el contrario, al lanzarse una excepción en Java, el entorno de ejecución captura y encapsula automáticamente dentro del objeto la ruta completa de métodos, incluyendo los archivos de código fuente y los números de línea precisos donde se originó y propagó el fallo, siendo totalmente accesible mediante el método `printStackTrace()`.

A continuación, se presenta un fragmento de código que evidencia cómo un manejador extrae esta información esencial de un objeto de excepción estándar:

```java
public class AnalisisExcepcion {
    public static void main(String[] args) {
        try {
            // Simulación de una operación matemática o de conversión que falla
            int valor = Integer.parseInt("TextoInvalido");
        } catch (NumberFormatException e) {
            // 1. El tipo se identifica por la clase declarada en el catch
            System.out.println("Clase de la excepción: " + e.getClass().getName());
            
            // 2. Se obtiene el mensaje descriptivo encapsulado
            System.out.println("Mensaje de error: " + e.getMessage());
            
            // 3. Se imprime la traza de la pila para conocer el origen exacto
            System.out.println("--- Traza de la pila ---");
            e.printStackTrace();
        }
    }
}

```

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta    

### Uso de Múltiples Bloques Catch

En Java, es completamente válido y habitual asociar más de un bloque `catch` a una única estructura `try`. Esta característica se asemeja conceptualmente a una estructura `switch` o a una cadena de sentencias `if-else if` en C, donde se evalúan diferentes códigos de error devueltos por una función (por ejemplo, `-1` para una raíz negativa o `-2` para una división por cero). Dado que un conjunto de instrucciones puede desencadenar múltiples tipos de anomalías instanciando diferentes objetos de error, la sintaxis permite definir un manejador específico para cada clase de excepción, separando así la lógica de recuperación según la naturaleza particular del fallo.

En cuanto a la ejecución, es fundamental comprender que, independientemente de la cantidad de bloques `catch` definidos, **solo se ejecuta uno como máximo**. Cuando se lanza una excepción dentro de la sección `try`, el entorno de ejecución evalúa los bloques `catch` secuencialmente, de arriba hacia abajo. En el momento en que se encuentra el primer bloque cuyo parámetro coincide con el tipo del objeto excepción lanzado, ese bloque asume el control de la situación. Una vez finalizada su ejecución, el programa ignora sistemáticamente el resto de los bloques `catch` y continúa con la siguiente instrucción fuera de la estructura de control de errores.

Debido a este mecanismo de evaluación estrictamente secuencial, se impone una regla de diseño inquebrantable: las excepciones más específicas deben capturarse siempre antes que las más generales o genéricas. Al estar las excepciones basadas en clases y objetos, si se colocara una clase base o genérica (como `Exception`) en el primer bloque `catch`, esta interceptaría cualquier objeto de error derivado, haciendo que los bloques posteriores fuesen inalcanzables. El compilador de Java previene este fallo lógico evaluando el código y deteniendo la compilación si detecta un orden incorrecto.

A continuación, se ilustra cómo se estructuran múltiples bloques `catch` para gestionar distintos fallos en una misma sección de código:

```java
public class ManejoMultiple {
    public static void main(String[] args) {
        try {
            // Este bloque podría generar diferentes tipos de errores
            int[] valores = {10, 0};
            
            // Si el divisor es 0, se lanza ArithmeticException
            // Si se usara valores[5], se lanzaría ArrayIndexOutOfBoundsException
            int resultado = valores[0] / valores[1]; 
            
            System.out.println("El resultado es: " + resultado);
            
        } catch (ArithmeticException e) {
            // Se captura exclusivamente el error matemático (específico)
            System.out.println("Fallo matemático: No se permite la división por cero.");
            
        } catch (ArrayIndexOutOfBoundsException e) {
            // Se captura exclusivamente el error de puntero/índice (específico)
            System.out.println("Fallo de memoria: Acceso fuera de los límites del arreglo.");
            
        } catch (Exception e) {
            // Se captura cualquier otro objeto de error no previsto (general)
            // Se coloca obligatoriamente al final de la estructura
            System.out.println("Error crítico inesperado: " + e.getMessage());
        }
    }
}

```

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta    

### Garantía de Ejecución con el Bloque Finally

En la programación estructurada tradicional como C, las interrupciones abruptas del flujo de ejecución (por ejemplo, mediante múltiples retornos de error) a menudo provocan que recursos del sistema, como archivos abiertos o memoria reservada dinámicamente, queden bloqueados o sin liberar. Para solucionar este problema en Java, donde las excepciones alteran drásticamente el flujo de control, se introduce el bloque `finally`. Esta estructura se coloca al final de un bloque `try` y se caracteriza por proporcionar una garantía estricta: las instrucciones contenidas en su interior se ejecutarán siempre de manera incondicional, sin importar si el código se ejecutó con éxito o si se produjo un fallo.

La versatilidad de este mecanismo permite emplearlo en dos escenarios bien diferenciados. Cuando se combina con bloques `catch` (formando la estructura `try-catch-finally`), el sistema intenta primero interceptar y procesar el objeto de error. Una vez que el bloque de captura pertinente finaliza su labor de recuperación, el control pasa automáticamente al bloque `finally` para efectuar las tareas de limpieza necesarias antes de reanudar el flujo normal de la aplicación.

Por el contrario, es posible prescindir totalmente de la captura y utilizar únicamente la estructura `try-finally`. En este escenario, el objetivo no es solventar el error localmente, sino establecer un punto de control que asegure la liberación incondicional de los recursos adquiridos (como cerrar un fichero temporal). Una vez que las sentencias del bloque `finally` concluyen, la excepción retenida temporalmente continúa su propagación natural hacia las funciones invocadoras en la pila de llamadas, delegando la responsabilidad de la captura a niveles superiores.

A continuación, se ilustran ambas modalidades de uso en una misma clase, evidenciando cómo se asegura el cierre de operaciones en distintos contextos:

```java
public class GestorRecursos {

    /* Ejemplo 1: Estructura try-catch-finally (Control del error y limpieza) */
    public void procesarConCaptura() {
        System.out.println("1. Abriendo conexión a la base de datos...");
        try {
            // Se simula un error que interrumpe el procesamiento
            throw new Exception("Error de lectura de datos.");
            
        } catch (Exception e) {
            // El error se captura y se informa
            System.out.println("2. Error capturado: " + e.getMessage());
            
        } finally {
            // Se garantiza el cierre incondicional, incluso habiendo pasado por el catch
            System.out.println("3. Cerrando la conexión a la base de datos de forma segura.");
        }
    }

    /* Ejemplo 2: Estructura try-finally (Solo limpieza, el error se propaga) */
    public void procesarSinCaptura() throws Exception {
        System.out.println("A. Abriendo un archivo de texto...");
        try {
            // Se simula un fallo crítico. Al no haber catch, el método se interrumpe.
            throw new Exception("El archivo ha sido corrompido.");
            
        } finally {
            // El archivo se cierra obligatoriamente ANTES de que el error escale hacia arriba.
            System.out.println("B. Archivo de texto cerrado antes de propagar la excepción.");
        }
    }
}

```

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta    

### Uso Avanzado y Garantías del Bloque Finally

En Java, es perfectamente válido utilizar un bloque `finally` asociado únicamente a un bloque `try`, prescindiendo por completo de la sección `catch`. Esta estructura, conocida como `try-finally`, se emplea cuando no se desea gestionar el error en ese nivel específico de la pila de llamadas, pero resulta imprescindible garantizar la ejecución de ciertas rutinas de limpieza, como el cierre de descriptores de ficheros (un concepto bien conocido en C). En esta configuración, si se produce una excepción, las instrucciones del `finally` se ejecutarán incondicionalmente para liberar los recursos antes de que el objeto de error continúe su propagación natural hacia las funciones invocadoras.

La característica fundamental del bloque `finally` es, por tanto, su garantía de ejecución. Se ejecutará indefectiblemente tanto si el código dentro del `try` finaliza con éxito y sin contratiempos, como si se interrumpe de forma prematura debido al lanzamiento de una anomalía. Esta cualidad lo convierte en el lugar idóneo para ubicar el código de finalización, evitando la tediosa duplicación de instrucciones de limpieza que, en lenguajes puramente estructurados, habría que replicar manualmente antes de cada posible punto de salida o de retorno de error de una función.

Incluso en el escenario donde se ejecute una sentencia `return` en el interior del bloque `try` (o dentro de un `catch`), el comportamiento del bloque `finally` se mantiene inalterable. Cuando el flujo del programa alcanza dicho `return`, la finalización de la función se suspende momentáneamente; en ese preciso instante, el control salta de forma automática al bloque `finally` para ejecutar todas sus instrucciones. Únicamente tras la conclusión de este bloque de cierre, se hace efectivo el retorno del valor hacia el código llamador, demostrando así la robustez de este mecanismo de seguridad frente a salidas anticipadas.

A continuación, se presenta un ejemplo que demuestra cómo se altera el flujo temporalmente para dar prioridad al bloque `finally` ante una instrucción `return`:

```java
public class DemostracionFinally {
    
    public int operacionConRetorno() {
        System.out.println("1. Entrando al bloque try e iniciando la operación.");
        try {
            // Se intenta salir anticipadamente de la función devolviendo un valor
            return 42; 
            
        } finally {
            // Este bloque interrumpe el 'return' temporalmente.
            // Se ejecuta de forma obligatoria ANTES de que el valor sea devuelto al llamador.
            System.out.println("2. Ejecutando el bloque finally antes de consumar el return.");
        }
    }

    public static void main(String[] args) {
        DemostracionFinally demo = new DemostracionFinally();
        
        // Se llama a la función y se almacena el valor retornado
        int resultado = demo.operacionConRetorno();
        
        System.out.println("3. La función terminó. El resultado recibido es: " + resultado);
    }
}

```

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta    

### Excepciones Controladas vs. No Controladas

En Java, las excepciones se dividen fundamentalmente en dos grandes categorías desde la perspectiva del compilador: las **controladas** (*checked*) y las **no controladas** (*unchecked*). Las excepciones controladas son aquellas que el entorno de compilación obliga estrictamente a gestionar; si una función lanza una de estas excepciones, es imperativo rodear la llamada con un bloque `try-catch` o delegarla añadiendo la cláusula `throws` en la firma del método. Por el contrario, las excepciones no controladas no imponen esta restricción en tiempo de compilación. En esta arquitectura, la clase `RuntimeException` desempeña un rol central: cualquier excepción que herede directa o indirectamente de `RuntimeException` es considerada "no controlada", mientras que aquellas que heredan directamente de la clase base `Exception` son "controladas".

La elección de un tipo u otro radica conceptualmente en el origen y la capacidad de recuperación del fallo. Las excepciones controladas se emplean para representar condiciones anómalas que provienen del exterior del programa y sobre las cuales no se tiene un control absoluto (similar a cuando en C falla la función `fopen` al buscar un archivo inexistente). Se asume que la aplicación debe estar preparada para recuperarse de estos eventos. En cambio, las excepciones no controladas (`RuntimeException`) se reservan casi en exclusividad para errores de lógica, defectos de programación o precondiciones violadas (problemas que en C típicamente desembocarían en una falla de segmentación o desbordamiento de memoria). Ante estos fallos, la aplicación generalmente no debe intentar recuperarse dinámicamente, sino que es necesario corregir el código fuente para prevenir que ocurran.

A continuación, se ilustra cómo se pueden emplear ambos tipos de excepciones utilizando clases estándar del lenguaje. Se observa que la excepción controlada (`IOException`) exige una declaración explícita de su propagación, mientras que la no controlada (`IllegalArgumentException`) permite mantener limpia la firma de la función:

```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class ValidacionYRecursos {
    
    /* Excepción Controlada: Obligatorio declarar 'throws' en la firma */
    public void leerConfiguracion(String ruta) throws IOException {
        File archivo = new File(ruta);
        // FileReader lanza FileNotFoundException (que hereda de IOException)
        // si el archivo no existe en el sistema operativo.
        FileReader lector = new FileReader(archivo);
        lector.close();
    }

    /* Excepción No Controlada: No requiere declaración en la firma */
    public void procesarEdad(int edad) {
        if (edad < 0) {
            // IllegalArgumentException hereda de RuntimeException.
            // Indica un defecto en la lógica que llamó a esta función.
            throw new IllegalArgumentException("La edad no puede ser negativa.");
        }
        System.out.println("Edad válida: " + edad);
    }
}

```

Para consolidar el criterio de diseño subyacente a esta separación, se enumeran los escenarios típicos donde la arquitectura del lenguaje orienta hacia el uso de uno u otro tipo de excepción:

**Situaciones donde se prefiere una excepción controlada (*Checked*):**

* Intento de lectura o escritura en un archivo que podría haber sido borrado por un agente externo o carecer de permisos del sistema operativo.
* Establecimiento de una comunicación a través de red (sockets) o una base de datos, donde la conexión puede perderse inesperadamente.
* Procesamiento de un documento proporcionado por el usuario cuyo formato sea incorrecto o esté corrupto.

**Situaciones donde se prefiere una excepción no controlada (`RuntimeException`):**

* Detección de argumentos inválidos proporcionados a un método, como intentar invocar operaciones sobre una referencia nula (similar a desreferenciar un puntero `NULL` en C).
* Acceso a una posición de memoria fuera de los límites de un arreglo o matriz (equivalente a desbordar un *buffer* en C).
* Ejecución de operaciones matemáticas imposibles, como una división por cero, provocadas por una falta de validación previa de los operandos.


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta    

En Java, la palabra reservada `throws` se utiliza en la firma de un método para declarar formalmente que dicha función es susceptible de emitir una o varias excepciones durante su ejecución. A diferencia de C, donde las posibles anomalías de una función solo se advierten leyendo la documentación o examinando los códigos de retorno numéricos esperados, `throws` establece un contrato explícito y validado por el compilador. Esta declaración advierte inequívocamente a cualquier código externo que intente invocar el método de que debe estar preparado para gestionar dichos eventos anómalos instanciados como objetos.

La sintaxis `throws` se presenta como la alternativa directa a la captura interna mediante `try-catch` cuando se trata de excepciones controladas (*checked exceptions*). Dado que el compilador exige estrictamente el manejo de este tipo de excepciones, en el momento de diseñar una función es necesario tomar una decisión: resolver el problema localmente capturando el error, o delegarlo al invocador. Al optar por añadir `throws` a la firma del método, se renuncia a capturar el objeto de excepción internamente, autorizando que este se propague de manera natural a través de la pila de llamadas hacia la función constructora o niveles superiores.

Esta alternativa de delegación es fundamental para mantener una correcta separación de responsabilidades y aprovechar los principios del diseño orientado a objetos. Con frecuencia, una función de bajo nivel (como una rutina de lectura de archivos o un cálculo matemático complejo) carece del contexto necesario para decidir cómo reaccionar ante un fallo; por ejemplo, no debería asumir que imprimir un mensaje por consola o finalizar la aplicación es la solución adecuada. Al utilizar `throws`, la rutina se mantiene enfocada exclusivamente en su propósito original, transfiriendo la responsabilidad de la recuperación al código llamador (como la interfaz gráfica), el cual dispone de la información global adecuada para decidir cómo proceder.

A continuación, se contrasta la captura interna frente a la delegación mediante `throws` en un método que interactúa con el sistema de archivos:

```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class GestorArchivos {

    /* Alternativa 1: Captura interna. El llamador nunca se entera del error. */
    public void leerConCaptura(String ruta) {
        try {
            FileReader lector = new FileReader(new File(ruta));
        } catch (IOException e) {
            System.out.println("Error interno gestionado localmente.");
        }
    }

    /* Alternativa 2: Uso de 'throws'. Se delega la responsabilidad al llamador. */
    public void leerConDelegacion(String ruta) throws IOException {
        // Si FileReader lanza una IOException (excepción controlada), 
        // la ejecución se interrumpe y el objeto se propaga automáticamente hacia arriba.
        // El compilador obligará a quien llame a este método a usar un try-catch.
        FileReader lector = new FileReader(new File(ruta));
    }
}

```

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta     

### Delegación de Excepciones con Limpieza de Recursos

Para ilustrar la combinación de la delegación de errores y la limpieza incondicional de recursos, se diseña un método encargado de interactuar con el sistema de archivos. En la firma del método, se emplea la palabra reservada `throws` seguida de la clase `IOException` (o su derivada más específica `FileNotFoundException`). Esta declaración formal indica que el método no asume la responsabilidad de solventar el problema si el fichero solicitado no existe, permitiendo que el objeto de error generado se propague de forma natural hacia la función invocadora, la cual tendrá el contexto adecuado para decidir si debe solicitar una nueva ruta al usuario o cancelar la operación.

A pesar de renunciar a la captura local del error omitiendo deliberadamente la sección `catch`, es imperativo garantizar que el estado del programa se mantenga consistente. En escenarios donde se manejan recursos externos, como flujos de datos o descriptores de archivos equivalentes a los utilizados en C, es necesario asegurar su correcta liberación antes de que la excepción abandone el contexto de la función. Para resolver esta necesidad técnica, se recurre a la estructura `try-finally`.

Al implementar este diseño, las instrucciones susceptibles de fallar se agrupan dentro del bloque `try`. Si la apertura del fichero fracasa, la ejecución normal se aborta de inmediato. No obstante, antes de que el objeto de excepción inicie su viaje ascendente por la pila de llamadas (*stack*), el flujo de control se desvía de manera obligatoria hacia el bloque `finally`. Una vez que las rutinas de cierre concluyen de forma segura, la anomalía retenida temporalmente continúa su propagación hacia los niveles superiores.

A continuación, se detalla el código correspondiente a este patrón de diseño estructural:

```java
import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class LectorDocumentos {

    /* La firma declara explícitamente que propaga la excepción controlada */
    public void procesarArchivo(String ruta) throws IOException {
        FileReader lector = null;
        System.out.println("1. Preparando el entorno para abrir el fichero...");

        try {
            // Si el fichero no existe, se instancia y lanza FileNotFoundException.
            // Al no existir un bloque 'catch', la ejecución del 'try' se interrumpe aquí.
            lector = new FileReader(new File(ruta));
            
            // Estas líneas de código normal solo se ejecutan si el fichero se abre con éxito
            System.out.println("2. Fichero abierto correctamente. Iniciando lectura...");

        } finally {
            // Este bloque se ejecuta SIEMPRE: tanto si hubo éxito como si se lanzó la excepción.
            // Garantiza que el recurso se libere ANTES de que el error siga subiendo.
            System.out.println("3. Ejecutando rutinas de cierre en el bloque finally.");
            
            if (lector != null) {
                lector.close(); // Se cierra el descriptor del fichero
            }
        }
    }
}

```

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta    

### Declaración de Excepciones No Controladas

En Java, la sintaxis del lenguaje permite perfectamente incluir excepciones no controladas, como aquellas derivadas de `RuntimeException`, dentro de la cláusula `throws` en la firma de un método. Sin embargo, a diferencia de lo que ocurre con las excepciones controladas (*checked*), el compilador ignora por completo esta declaración a efectos de obligatoriedad. Esto significa que el entorno de desarrollo no emitirá ningún error ni forzará al método llamador a envolver la invocación en un bloque `try-catch`, ni a propagar el error mediante otro `throws`, manteniendo exactamente la misma libertad de diseño que si la cláusula no existiera.

En consecuencia, no se considera estrictamente necesario ni es la práctica recomendada que el método invocador implemente un `try-catch` para capturar estas excepciones, a pesar de estar declaradas en la firma de la función. Como se ha expuesto anteriormente, las excepciones no controladas suelen representar defectos en la lógica de programación o violaciones de las precondiciones de un método (situaciones análogas a desreferenciar un puntero `NULL` o acceder a un índice de array inválido en C). Por lo tanto, la solución óptima desde el punto de vista arquitectónico no consiste en capturar el error una vez ocurrido, sino en escribir un código robusto que valide los datos previamente para prevenir que la anomalía llegue a desencadenarse.

El sentido principal de incluir una excepción no controlada en la directiva `throws` es puramente informativo y de documentación. Actúa como una advertencia explícita para el programador que vaya a consumir dicha función, indicándole qué precondiciones deben respetarse y qué objeto de error específico se instanciará si se violan las reglas de entrada. Esta práctica se complementa frecuentemente con herramientas de documentación (como *JavaDoc*), enriqueciendo la interfaz pública de la clase y reforzando la encapsulación, ya que expone claramente el contrato de uso sin revelar los detalles internos del algoritmo.

A continuación, se ilustra este concepto, donde el `throws` funciona como un mecanismo de comunicación para el desarrollador y no como una imposición del compilador:

```java
public class OperacionesMatematicas {

    /* El throws es informativo: advierte sobre un posible fallo lógico */
    /**
     * @param dividendo El número a dividir.
     * @param divisor El número por el cual dividir.
     * @throws ArithmeticException Si el divisor proporcionado es igual a cero.
     */
    public int dividir(int dividendo, int divisor) throws ArithmeticException {
        if (divisor == 0) {
            throw new ArithmeticException("El divisor no puede ser cero.");
        }
        return dividendo / divisor;
    }

    public void procesar() {
        int d = 0;
        
        // El compilador NO obliga a poner try-catch, a pesar del 'throws'.
        // En lugar de capturar el error, se previene con una estructura de control típica de C.
        if (d != 0) {
            int resultado = dividir(10, d);
            System.out.println("Resultado: " + resultado);
        } else {
            System.out.println("Operación cancelada preventivamente.");
        }
    }
}

```

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta    

### Criterios de Uso y Paradigmas en Otros Lenguajes

Se recomienda emplear excepciones controladas (como `IOException`) cuando el error representa una condición excepcional del entorno de ejecución, completamente ajena a la lógica interna del algoritmo, y de la cual se espera que la aplicación pueda recuperarse de forma razonable. Ejemplos clásicos incluyen la interrupción de una conexión de red o la denegación de permisos del sistema operativo al intentar abrir un fichero. Al imponer que el compilador verifique su tratamiento, se garantiza la construcción de programas robustos frente a contingencias externas. Por el contrario, se prefiere el uso de excepciones no controladas (como `IllegalArgumentException`) para señalar defectos directos en el código fuente, tales como la violación del contrato de un método, el acceso a índices fuera de rango o el equivalente a desreferenciar un puntero `NULL` en C. En estos escenarios, el programa no debe intentar recuperarse dinámicamente, sino abortar para que el desarrollador corrija la lógica defectuosa.

Respecto a la existencia de ambas categorías en el panorama general de la programación, la respuesta es negativa. La división estricta entre excepciones controladas por el compilador y excepciones no controladas es una característica de diseño estructural casi exclusiva de Java. Históricamente, fue un experimento arquitectónico diseñado para forzar a los programadores a manejar errores críticos, pero que no fue replicado en la gran mayoría de lenguajes orientados a objetos posteriores.

En los lenguajes de programación donde únicamente existe un paradigma, el estándar absoluto es el uso exclusivo de **excepciones no controladas**. Lenguajes ampliamente utilizados como C++, C#, Python, e incluso evoluciones directas del ecosistema Java como Kotlin, operan bajo la premisa de que el compilador no debe obligar a capturar ni a declarar ninguna excepción. Se adoptó esta decisión arquitectónica al observar que la obligación de gestionar excepciones controladas frecuentemente genera un código excesivamente verboso y vuelve rígidas las interfaces públicas; modificar la excepción que lanza una función de bajo nivel en Java obliga a actualizar las firmas (`throws`) de todas las funciones intermedias en la pila de llamadas, rompiendo la encapsulación y dificultando el mantenimiento.

A modo de ilustración, se expone cómo C++ (el cual introduce orientación a objetos sobre C) maneja el lanzamiento de errores bajo un paradigma exclusivamente no controlado:

```cpp
#include <iostream>
#include <stdexcept>

/* En C++ todas las excepciones se comportan como "no controladas". 
   No existe la obligación de usar 'throws' en la firma, ni el 
   compilador forzará al invocador a rodear la llamada con try-catch. */
void procesar_dato(int valor) {
    if (valor < 0) {
        // Se instancia y lanza un objeto de error estándar de C++
        throw std::invalid_argument("Error lógico: valor negativo inaceptable.");
    }
    std::cout << "Procesamiento exitoso." << std::endl;
}

int main() {
    // El código compila perfectamente sin try-catch, asumiendo el riesgo
    // de que el programa finalice abruptamente si ocurre el error lógico.
    procesar_dato(-5); 
    return 0;
}

```

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta    

### Lanzamiento y Relanzamiento de Excepciones en el Bloque Catch

Sí, tiene pleno sentido lanzar una nueva excepción desde el interior de un bloque `catch`. Esta técnica se conoce habitualmente como traducción o envoltura de excepciones (*exception wrapping*). Se utiliza fundamentalmente para ocultar detalles de implementación de bajo nivel y preservar el principio de encapsulación en el diseño orientado a objetos. Por ejemplo, si un método encargado de la persistencia de datos captura un error de muy bajo nivel de lectura de disco (como un fallo del sistema de archivos, comparable a un error en `fread` en C), propagar ese mismo objeto hacia la interfaz de usuario revelaría detalles técnicos innecesarios. En su lugar, se captura el error original y se lanza un nuevo objeto de excepción personalizado y más abstracto (por ejemplo, `BaseDeDatosException`), el cual puede contener al error original como "causa" para no perder la traza de la pila durante la depuración.

Por otro lado, es totalmente factible relanzar exactamente la misma excepción que acaba de ser interceptada. Para lograr esto, basta con utilizar la instrucción `throw` seguida del identificador de la variable que actúa como parámetro en el bloque `catch`. Cuando se ejecuta esta acción, el objeto de error abandona el manejador actual y retoma su propagación natural hacia arriba en la pila de llamadas (*stack*), conservando intacta toda su información interna original, tal como el mensaje y la secuencia de llamadas que originaron el fallo.

El relanzamiento de la misma excepción cobra especial sentido en escenarios donde se requiere realizar una acción intermedia local antes de abortar definitivamente la función, pero no se dispone de la responsabilidad arquitectónica para solucionar el error por completo. Un caso de uso prototípico es el registro de incidencias o *logging*. Un método puede capturar una anomalía con el único propósito de escribir un aviso detallado en un archivo de registro del servidor y, acto seguido, relanzar el mismo objeto para delegar la decisión final de recuperación o notificación al usuario hacia las capas superiores de la aplicación.

A continuación, se presentan dos ejemplos prácticos que ilustran ambos mecanismos: la traducción de una excepción por otra nueva y el relanzamiento del mismo objeto interceptado.

```java
import java.io.IOException;

/* 1. Ejemplo de lanzar una NUEVA excepción (Traducción / Wrapping) */
public class GestorConfiguracion {
    public void cargarAjustes() throws Exception {
        try {
            // Se simula una llamada que produce un error de bajo nivel
            throw new IOException("El archivo 'config.ini' está bloqueado.");
            
        } catch (IOException errorOriginal) {
            // Se encapsula el error de disco en un error de negocio de alto nivel.
            // Se pasa el 'errorOriginal' como causa para mantener la información.
            throw new Exception("Fallo crítico al inicializar el sistema.", errorOriginal);
        }
    }
}

/* 2. Ejemplo de relanzar la MISMA excepción */
public class ProcesadorTransacciones {
    public void procesarPago(double cantidad) throws IllegalArgumentException {
        try {
            if (cantidad <= 0) {
                throw new IllegalArgumentException("La cantidad debe ser positiva.");
            }
            System.out.println("Procesando pago de: " + cantidad);
            
        } catch (IllegalArgumentException e) {
            // Se realiza una acción local: registrar el intento de fraude o error
            System.out.println("[LOG INTERNO]: Intento de pago inválido detectado.");
            
            // Se relanza el mismo objeto para que el código llamador lo gestione
            throw e; 
        }
    }
}

```

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta    

### El Encadenamiento y la Causa de una Excepción

El concepto de que una excepción actúe como la **"causa"** de otra responde a la técnica arquitectónica conocida como encadenamiento de excepciones (*exception chaining*). Al igual que en C se pierde el contexto original cuando una función de bajo nivel, como la lectura de un fichero, falla y simplemente devuelve un código numérico (`-1`) a una capa superior, en el diseño orientado a objetos se aborda este problema permitiendo que un objeto de error almacene internamente una referencia a la anomalía original que lo provocó. De este modo, se logra encapsular un fallo técnico de bajo nivel (como un error de disco o de red) dentro de una excepción más abstracta y representativa del dominio de la aplicación, manteniendo intacta la información vital sobre el origen del problema gracias al principio de encapsulación.

Cuando una excepción encadenada alcanza el final de la ejecución sin ser solventada y se imprime por pantalla (por ejemplo, mediante el uso del método `printStackTrace()`), la causa original es perfectamente visible y detallada. El entorno de ejecución de Java desglosa de manera automática la jerarquía completa del error, mostrando en primer lugar el fallo de alto nivel y, a continuación, revelando el origen exacto mediante la cláusula estándar **"Caused by:"** (*Causado por:*). Esta salida incluye la traza de la pila completa de la excepción original, lo cual resulta invaluable para la depuración, ya que permite al programador rastrear el problema desde la operación general de negocio hasta la línea de código exacta donde falló la instrucción más básica.

A continuación, se presenta un ejemplo donde se define una excepción personalizada de alto nivel que está diseñada para aceptar y almacenar una causa de bajo nivel en su constructor:

```java
/* 1. Definición de la excepción personalizada de alto nivel */
class FalloSistemaException extends Exception {
    
    // El constructor recibe el mensaje y un objeto 'Throwable' que representa la causa
    public FalloSistemaException(String mensaje, Throwable causa) {
        super(mensaje, causa); // Se delega el almacenamiento a la clase base
    }
}

/* 2. Clase que demuestra el encadenamiento de excepciones */
public class GestorBaseDatos {

    public void cargarDatos() throws FalloSistemaException {
        try {
            // Se simula un error técnico de bajo nivel (ej. formato numérico inválido)
            int puertoDeRed = Integer.parseInt("PuertoDesconocido");
            
        } catch (NumberFormatException errorBajoNivel) {
            
            // Se captura el error técnico y se envuelve (wrap) en la excepción de negocio.
            // El objeto 'errorBajoNivel' se pasa como el segundo parámetro (la causa).
            throw new FalloSistemaException("No se pudo establecer conexión con los datos.", errorBajoNivel);
        }
    }

    public static void main(String[] args) {
        GestorBaseDatos gestor = new GestorBaseDatos();
        try {
            gestor.cargarDatos();
        } catch (FalloSistemaException e) {
            // Al imprimir la traza, se visualizará el mensaje principal 
            // seguido de la sección "Caused by: java.lang.NumberFormatException..."
            e.printStackTrace();
        }
    }
}

```
