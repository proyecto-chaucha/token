# Chaucha Token
Chaucha al ser una criptomoneda basada en Litecoin no cuenta con un lenguaje de programación Turing Completo como Ethereum. Por lo que la creación de un token en Chaucha no tendrá las mismas características que un token ERC20. Sin embargo esto entrega la flexibilidad de programar la lógica de negocios con el ecosistema tecnológico que se desee, dejando al blockchain de Chaucha como el encargado de validar y realizar las transacciones además de entregar la información de trazabilidad respectiva. 

Este proyecto se enfocará en crear un prototipo que satisfaga los siguientes casos de uso:

- Creación de una transacción génesis de tokens.
- Generar direcciones públicas y privadas de Chaucha.
- Realizar una transferencia de tokens desde una dirección Chaucha a otra.
- Obtener la cantidad de tokens válidos disponibles en una dirección de Chaucha.
- Creación de un cliente básico con reglas de negocio de ejemplo.
- Entregar información complementaria.

Para poder cumplir dichos objetivos el prototipo será un servidor web con una aplicación de API REST, la cual entregará los métodos e información necesarios para dar apoyo a otros sistemas. Los tokens serán chauchas tradicionales con sus características y transacciones normales, la única diferencia es que el sistema prototipo será programado para darle un significado a dichas chauchas y entregar información relevante al negocio.

## Tecnologías
El prototipo usará las siguientes tecnologías:

- Programación en Python 3.6 con framework Flask o similar.
- API REST que entregue información relevante en formato JSON para su consumo por distintas plataformas.
- Uso de plataforma de apoyo (Insight) para difusión y obtención de información de transacciones en el blockchain. 
- Docker.

## Arquitectura
La arquitectura a la cual el sistema estará orientado es la de microservicios. Sin embargo cualquier otra arquitectura puede ser adoptada siempre y cuando el servicio de API Tokens funcione separado del resto del sistema (aunque puede ser re-implementado para integrarse directamente si es lo deseado).

El siguiente diagrama muestra un ejemplo de implementación donde existen dos redes. Una de acceso público y otra de acceso privado y/o restringido con sus respectivos componentes.

![imagen](https://user-images.githubusercontent.com/292738/45921179-3131a680-be87-11e8-96f2-c2c1fb93838d.png)

La red pública consta de un servidor de puerta principal (API Gateway), el cual puede ser proporcionado por herramientas como Kong o AWS API Gateway. 

Este servidor se conecta a los servicios de la red privada y entrega un acceso estandarizado para las aplicaciones web/escritorio o mobile.

Entre los componentes de la red privada se pueden mencionar los siguientes servicios:

#### API Tokens
Encargado de realizar las transacciones de tokens y entregar información relativa a ellos. No tiene lógica de negocios asociada. Como los tokens son simplementes Chauchas tradicionales, una dirección de Chaucha puede tener tanto tokens como chauchas normales. Esta API se encargará de identificar dichos tokens y realizar la transferencia adecuada.

#### Servidor Insight
Encargado de proporcionar información para el servidor de API Tokens. Se recomienda tener una copia interna del Insight para evitar dependencias con servicios externos, aumentando la seguridad y control sobre las transacciones.

#### API Business Logic
Encargado de implementar toda la lógica de negocios relativa al proceso de envío de tokens y sus detalles. Ejecuta los métodos de API Tokens según corresponda. Este servicio puede ser implementado en el lenguaje de programación que se considere apropiado. Ya que las interacciones serán utilizando la API Rest proporcionada por API Tokens. Este sistema entregará información relacionada a las reglas de negocio programadas. Los tokens no tendrán información adicional a la tradicional en cualquier transacción, siendo este sistema el encargado de proporcionar el significado de cada token.


Para el contexto de este sistema solamente se implementará el servicio de API Tokens y un API Business Logic básico de ejemplo.

## Preguntas Frecuentes

### ¿Por qué utilizar Blockchain Público?

Si bien el sistema planteado también podría ser implementado sin la necesidad de un sistema basado en blockchain, se recomienda la implementación de un blockchain público por los siguientes motivos:

- Un blockchain permite la generación de transacciones con información inmutable y de total transparencia. Lo que entrega una mayor seguridad para los usuarios.

- Al ser un sistema ya implementado para el intercambio de activos digitales se puede destinar los recursos a la elaboración de los sistemas de prioridad crítica para el negocio.

- Se puede promocionar esta característica del sistema para destacar frente a otros competidores que no lo usan.

- Se puede incluir información inmutable (OP_RETURN) en cada transacción para entregar aún más detalles en cada transferencia de tokens.

- Los tokens pueden tener el significado que se desee. Representando un producto físico o digital.

- Los tokens pueden tener una fecha de vencimiento, tener condiciones de transferencias o ser eliminados permanentemente.

### ¿Por qué no crear una Criptomoneda nueva?

La creación de una criptomoneda requiere de un arduo trabajo en evaluar los parámetros, tecnologías y programación. Además del esfuerzo requerido en sumar a personas que actúen como nodos y mineros, dándole seguridad y movimiento a las transacciones. Elaborar una nueva criptomoneda solamente es justificable si las tecnologías disponibles no permitieran suplir las necesidades del sistema tanto funcionales como no funcionales. En términos simples, el crear una criptomoneda es un proyecto totalmente aparte del sistema y requiere una asignación de recursos que estarían mejor destinados al sistema y no a una criptomoneda. Por esa razón se recomienda la elaboración de un token que funcione sobre una criptomoneda ya funcionando como Chaucha.

### ¿En qué se diferencia un Token de una Criptomoneda?

Básicamente, una criptomoneda como Chaucha cuenta con su propio sistema de Blockchain. Mientras que un token funciona utilizando el Blockchain de una criptomoneda.  El token normalmente es usado para representar un objeto o concepto y su valor depende de quien lo emita. Los tokens tienen un mayor control sobre sus características y reglas. Una criptomoneda varía normalmente su valor dependiendo de las condiciones del mercado y no puede ser fácilmente controlable una vez que se ha liberado. 

## Token
El token será simplemente la unidad mínima permitida por la red Chaucha que es `1/10^8` CHA (1 Chatoshi). Esto permite generar hasta `10^8` (cien millones) tokens por cada Chaucha usada en el proceso de génesis (sin considerar las comisiones de la red al realizar transferencias). Este no contendrá información adicional a la disponible normalmente. Toda información relativa a reglas de negocio serán programadas en sistemas complementarios al sistema de tokens. 

### Proceso de Génesis y Cadena de Transacciones
Para poder crear los tokens se debe realizar una transacción hacia una dirección la cual se tenga control de la llave privada. Esta transacción génesis será la utilizada para identificar la procedencia de tokens. Una vez que la dirección “madre” tenga los tokens disponibles se pueden enviar hacia las direcciones de los usuarios finales y trazar sus movimientos.

El siguiente diagrama ([ http://whatdoesthequantsay.com/2014/05/04/bitcoin-transactions-technically-part-2]( http://whatdoesthequantsay.com/2014/05/04/bitcoin-transactions-technically-part-2)) muestra la estructura de una transacción. Donde los datos de salida se transforman en datos de entrada para otras transacciones. Creando una cadena de transacciones. Esta información de transacciones relacionadas es suficiente para tener la trazabilidad de movimientos que ha tenido un token desde la transacción inicial. La red de Chaucha no mantiene un registro del balance de cada dirección puesto que este se puede calcular usando el historial de transacciones.

![imagen](https://user-images.githubusercontent.com/292738/45921249-6db1d200-be88-11e8-9a32-cb1491618d99.png)

El siguiente diagrama ejemplifica el proceso de generación de tokens:

![imagen](https://user-images.githubusercontent.com/292738/45921256-8621ec80-be88-11e8-8b9f-12d458242f2e.png)

Dependiendo de los requerimientos del sistema y la lógica de negocios, la llave privada puede estar en posesión del usuario o ser controlada por el sistema. Es decir que el usuario solamente tenga acceso a su llave pública para verificar sus tokens disponibles.

El siguiente diagrama muestra el proceso de validación de tokens:

![imagen](https://user-images.githubusercontent.com/292738/45921259-9cc84380-be88-11e8-8ca9-ce4abeae0378.png)

### Proceso de Generación de Nuevos Tokens

Si por algún motivo se requieran más tokens simplemente se debe iniciar un nuevo proceso de génesis con una nueva transacción. Esta transacción se deberá considerar como una transacción de tokens válidas y compatibles con la transacción génesis anterior.

### Tokens Condicionados

Gracias a que los tokens serán Chauchas tradicionales se pueden programar transacciones condicionadas. Por ejemplo se pueden programar para que sean válidos por un tiempo determinado o que cumpla condiciones específicas para permitir su transferencia hacia otra dirección. Para más detalles revisar los proyectos [OP_HASH256](https://github.com/proyecto-chaucha/OP_HASH256) y [OP_CHECKLOCKTIMEVERIFY](https://github.com/proyecto-chaucha/CLTV).

### Costos de Operación

Dentro de los parámetros de funcionamiento de Chaucha se estableció el impuesto mínimo de 0.001 CHA para minar una transacción (el cual es entregado en un 100% para los mineros), junto con la definición de polvo a transacciones con un monto menor a este impuesto (Las transacciones polvo son aquellas que no serán minadas).

A partir de esto, se puede estimar que el costo asociado para realizar transacciones cada 10 minutos durante un año es de 52.56 CHA, lo que es un precio razonable considerando los beneficios de este sistema. Además de que es relativamente más económico que utilizar otras criptomonedas como Ethereum o Bitcoin.

Es posible disminuir el costo de las transacciones al configurar una mining pool que acepte el minar transacciones con cero impuesto, lo que disminuiría el costo de utilización del sistema pero comprometería la descentralización y seguridad de la red. Además de que no se garantiza que las transacciones generadas por dicha pool sean incluidas en el bloque oficial (puede que otra pool que priorice transacciones con impuesto gane ese derecho antes).

### Velocidad de Transacción
La red Chaucha está programada para insertar un bloque por minuto. Por lo que una transferencia de tokens podría tardar entre 0 a 6 minutos dependiendo de la cantidad de confirmaciones que se quiera esperar (Una confirmación es la cantidad de bloques ingresados posteriormente al bloque donde la transacción está incluida). El mínimo recomendable es una confirmación (un minuto) para transferencias de pequeños montos y seis (minutos) o más para transferencias de montos elevados. Este número es variable dependiendo del estado de la red, sin embargo es bastante estable gracias al algoritmo DGW utilizado por Chaucha.

