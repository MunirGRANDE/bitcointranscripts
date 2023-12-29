---
title: Bitcoin Optech 
transcript_by: Bryan Bishop
translation_by: Blue Moon
categories: ['core-dev-tech']
tags: ['lightning', 'segwit']
date: 2018-10-09
---
<https://twitter.com/kanzure/status/1049527415767101440>

<https://bitcoinops.org/>

Bitcoin Optech trata de animar a las empresas bitcoin a adoptar mejores técnicas y tecnologías de escalado, cosas como batching, segwit, más adelante quizá firmas Schnorr, firmas agregables, quizá lightning. Ahora mismo nos estamos centrando en cosas que las empresas podrían estar haciendo ahora mismo. Los intercambios podrían ser por lotes, y algunos no lo son. Estamos hablando con esas empresas y escuchando sus preocupaciones y lo que están haciendo, y sólo tratando de empujarlos en la dirección correcta.

También producimos contenidos. Producimos un boletín semanal, en gran parte gracias a David Harding. Hemos recibido una respuesta muy positiva a ese boletín. Es difícil para los ingenieros de bitcoin estar al tanto de todo lo que pasa en Bitcoin Core y el desarrollo de protocolos y la lista de correo y github.

También organizamos talleres. Hicimos uno en julio en San Francisco. El mes que viene celebraremos otro en París. También estamos elaborando documentación sobre las mejores tecnologías de ampliación. Lo llamamos "libro de cocina de la ampliación", y en él se resumen aspectos como la sustitución por cuotas, el pago de los padres por los hijos y los mejores enfoques de la ampliación. Así que lo que queremos hablar con usted es si usted está interesado en ayudar, y si hay algo que podamos hacer para retroalimentar el conocimiento de los ingenieros de la industria a la gente en esta sala.

Para retroceder un paso en cuanto a por qué optech es una buena idea... Creo que todos sabemos que los negocios bitcoin no son la única constinuencia para la que estamos construyendo Bitcoin Core y tal vez no la más importante. Pero son una gran circunscripción. Mucha gente que usa bitcoin y se beneficia de bitcoin lo hace a través de estos negocios, y si estos negocios pueden usar bitcoin mejor, eso es mejor para los usuarios. Hace uno o dos años vimos que había una gran desconexión entre la comunidad de desarrollo de código abierto y la industria sobre lo que era el desarrollo y hacia dónde se dirigía... no había ningún bucle de retroalimentación de ellos hacia nosotros o viceversa. En mi opinión, ésta es una forma estupenda de mejorar esa comunicación. Pero debe ir en ambas direcciones. La retroalimentación también debe ir en la otra dirección. Idealmente, en el futuro, tal vez hay más personas que trabajan para una empresa y contribuir al software de código abierto, pero creo que hay otras maneras que podría suceder.

Nuestro primer taller fue en San Francisco en julio de 2018. Tuvimos ingenieros de seis compañías de bitcoin en el Área de la Bahía que vinieron y hablaron sobre la selección de monedas, la estimación de tarifas, el reemplazo por tarifas, el niño paga por el padre, etc. Andrew Chow vino y habló sobre esas cosas también. Tuvimos intercambio de ideas entre esas empresas y obtuvimos una comprensión de cómo la gente está utilizando bitcoin en el campo. Si alguno de ustedes está interesado en asistir a futuros talleres o participar en nuestro foro en línea o hablar con ingenieros de empresas y descubrir lo que hemos estado haciendo, háganoslo saber.

Replace-by-fee es difícil. A la gente le gusta child-pays-for-parent.

¿Cómo utiliza la gente el software Bitcoin Core? ¿Las empresas utilizan el monedero? No. Nos reunimos con dos docenas de empresas bitcoin, todas menos una utilizan Bitcoin Core, pero todas lo utilizan para la validación pero no para nada más. Monedero utiliza otra cosa para su validación. Algunos intercambios en Asia utilizan el monedero Bitcoin Core. Sus monederos han crecido mucho, son muy lentos y están sufriendo. Tienen un miedo mortal a dejar ese monedero. Otro patrón que hemos visto en todas las empresas es que van a utilizar software de código abierto, pero tienen que elegir en función de su infraestructura interna y el lenguaje de elección en la empresa, y que varía como java, scala, golang, ruby, python. Creo que otra cosa que veremos en el espacio de intercambio son módulos de seguridad de hardware, carteras de hardware, cosas así. Los cambios en los monederos de los que se ha hablado hoy podrían aplicarse ahí. Sí, usan RPC. Utilizan Bitcoin Core para la validación, y luego vuelven a buscar en él. Construyen sus propios índices de transacciones y sus propias transacciones. Les encantaría usar alguna versión de código abierto de alta calidad, el problema es que hay diferentes lenguajes e infraestructuras y nadie quiere escribir bindings.

Entonces, ¿por qué no han implementado segwit todavía? Algunos de ellos quieren, pero utilizan bitcoinj, y no implementa segwit. Entonces, ¿esperan o lo hacen ellos mismos? Además, algunos de ellos quieren tratar bitcoin y litecoin igual. La biblioteca bitcoins puede representar las diferencias entre las direcciones a través de las monedas, que incluyen diferentes bytes de red. La mayoría de estas empresas soportan múltiples monedas, y tienen capas de abstracción interna que es generalmente una buena práctica de ingeniería de software, pero es difícil abstraer el modelo de Ethereum y un modelo utxo. Esto les lleva a introducir errores cuando no manejan bien las reorgs.

He experimentado bastante a menudo, que incluso si un intercambio quiere usar Bitcoin Core por razones de calidad, luchan porque quieren algo como los índices de direcciones normalmente por malas razones pero nadie puede evitar que quieran usarlos para cosas malas. Como resultado, terminan usando mal abandonware.

David Harding tiene una página wiki con una lista de optimizaciones disponibles para escalar bitcoin, ¿por qué no se han adoptado? El procesamiento por lotes de transacciones, segwit, la consolidación utxo son generalmente bien entendidos por los ingenieros y o bien los han hecho o quieren hacerlos, pero no ha sido una prioridad todavía. En cambio, hay mucho más misterio sobre cómo hacerlo y cuál es la forma correcta o la mejor. También hay que tener en cuenta la interfaz de usuario o UX. Por ejemplo, si una gran empresa lo implementa, y si vas a transferir una moneda a otro monedero, y te cargas la comisión, y miras el explorador de bloques que no ha actualizado la UX para el reemplazo por comisión, podrías pensar que la moneda se ha perdido. Tampoco es trivial hacer eso porque podrías hacer una redirección a la nueva transacción, pero ¿es realmente la misma transacción? Si sólo algunas entradas son las mismas, ¿es la misma transacción? No es trivial. RBF y CPFP es un área en la que optech puede ayudar. John ha iniciado un documento sobre las mejores prácticas en materia de sustitución de tarifas.

Otra cosa que viene en camino es la firma Schnorr. La libsecp256k1 de Bitcoin Core tiene enlaces java pero se necesitan nuevas versiones de esos enlaces en la libsecp256k1 de Bitcoin Core. Necesitamos ofrecer maneras fáciles para que los lenguajes de programación populares utilicen esas características. Creo que Jonas está trabajando en el módulo Schnorr sig ahora mismo, así que deberíamos pensar en hacer eso también para que realmente sea adoptado por los intercambios o lo que sea, de lo contrario no tienen incentivos para adoptar esto.

Una de las cosas que salió mal antes fue que la gente recibiera un mensaje en nombre de Bitcoin Core o de los desarrolladores de código abierto con el que otras personas no estaban de acuerdo. Definitivamente no queremos que eso vuelva a ocurrir. Creo que estos chicos están haciendo un esfuerzo concertado para no hablar en nombre de Bitcoin Core, y ser muy públicos. Así que debemos prestar atención y hablar si no estamos de acuerdo. Estamos tratando de mejorar la comprensión de Bitcoin Core y cómo los desarrolladores piensan sobre él.

No hay manera de obtener una solicitud de una característica de un usuario en un formato sobre sí aquí es un usuario que quiere una característica. Para los intercambios de allí en la sala, que tienen como una consulta n + 1, como Ruby on Rails, donde le preguntas a Bitcoin Core una cosa, se obtiene un reuslt, entonces usted le pide n cosas. En la primera consulta, tienes todas las n cosas que podría ser, entonces terminas consultando el RPC y termina siendo un cuello de botella. No hay manera de obtener ese informe sobre esta extensión sería útil de una manera estandarizada. Eso podría ser útil. Recibir informes de errores como este y retroalimentación es importante. Es la formalidad de ello. He tenido gente que pedía estas cosas, y luego la gente pregunta, bueno, ¿quién ha pedido esto? ¿Por qué no hay un tema en github discutiendo esa petición? Si no está en github, entonces no vas a conseguir tu característica. Si consigues que la gente lo comente, entonces estarás votando a favor, lo que podría ser un poco incorrecto.

Si hay una reorganización de 50 bloques, ¿cuál va a ser la respuesta de Coinbase? Quizá no lo anuncien, pero deberían tener un plan. Tal vez tengamos otra red de pruebas, específica para empresas, y podamos simular cosas en las que queremos que la gente piense un poco más. Si utilizan esa red de pruebas, experimentarán esas grandes reorganizaciones para que sus sistemas las capten y se ocupen de ellas. No hacer que testnet haga eso, sino hacer otra testnet. Si puedes sobrevivir a esta red de prueba, entonces tu sistema es bueno.

En el caso de las empresas que están elaborando informes para los reguladores sobre el funcionamiento de bitcoin, nos gustaría ponernos a su disposición para una revisión paritaria en nombre de los reguladores o de las empresas.

Desde la perspectiva de las bolsas, tienen planes para los sistemas de monedas que fallan, especialmente para las bolsas que listan todas las monedas bajo el sol. Probablemente aplicarían cualquier estrategia que tengan para alguna pequeña altcoin, para bitcoin. Pero la mayoría de las altcoins tienen un número de teléfono para que alguien llame... pero realmente bitcoin no.