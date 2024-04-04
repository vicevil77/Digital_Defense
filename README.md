  # Digital_Defense
Proyecto de ML y DL sobre detección de ataques malware
![ml](https://github.com/vicevil77/Digital_Defense/assets/120662253/609c4974-5640-4ca6-b8df-eb1ad0b7f92c)

Objetivo:

El objetivo de este proyecto es desarrollar un modelo de Machine Learning/Deep Learning (ML/DL) para conseguir el mejor modelo para poder predecir si una conexión es o no maliciosa, teniendo en cuenta los datos técnicos de las conexiones a la red, utilizando para ello, un conjunto de datos de 25 millones de registros y 19 columnas, sin llegar a la parte de producción/industrialización. 

Metodología:

1. Recopilación y limpieza de datos:
Se recopilaron 13 conjuntos de datos de fuentes públicas, realizándose una limpieza de datos para eliminar registros duplicados, inconsistentes o con valores nulos.
Finalmente se combinaron los conjuntos de datos limpios en un único conjunto de datos con 25 millones de registros y 18 columnas.
 ![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/cf002786-03b5-4a59-a6ad-4fda5060eee7)

2. Exploración de datos:
Se realizó un análisis exploratorio de datos para comprender las características del conjunto de datos, identificándose las variables relevantes para la predicción de ataques de malware, destacando:
 ![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/18aac6f8-f059-4f7b-b24e-43232dd16d9c)

A.- El Target como se puede apreciar, esta dividida entre conexiones normales y maliciosas, pero con la finalidad de conseguir mejores resultados se sumaron el “Malicious” todos los tipos de este, quedando la Target (65% 35%), estando desbalanceada, por lo que se usaran hiperparametros y modelos adecuados para evitar un sobreajuste a la clase mayoritaria.
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/d4b1a891-a3fa-4525-b41d-94fa429fd4f1)

B.- En el estudio correlacional entre variables y targets, se encontraron características interesantes:
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/4159fb30-5c82-4233-b097-0e6617373a0e)

Como podemos observar, estamos analizando la variable “conn_state” con la” target”, siendo la primera una variable ayuda a entender cómo funcionan las comunicaciones de red y a rastrear el flujo de datos entre dispositivos, representando las diferentes fases por las que pasa una conexión TCP; siendo muy importante en el análisis de la red para detección de problemas y de actividad maliciosa.
Algunos de los estados de conexión (“conn_state”) más usados son:
S1: Una conexión TCP establecida, permitiendo el intercambio de datos.
SHR: Indica que un dispositivo (cliente) ha iniciado la conexión enviando un paquete SYN (sincronización) al servidor.
SH: Señala que el servidor ha recibido el paquete SYN del cliente y ha respondido con un paquete al cliente. (SYN-ACK
HS: Representa el inicio de una conexión TCP, siendo cuando dos hosts intercambian paquetes SYN y SYN-ACK, estableciendo los parámetros de la conexión.
SF: Estado dónde una de las partes ha enviado un paquete FIN indicando el deseo de finalizar la conexión.
RST (Reseteo): Indica que se desea un restablecimiento forzado de la conexión. Puede ocurrir por un problema, intento de acceso no permitido o en respuesta a un ataque.
OTH: Todo lo que no esta definido en otras categorías (errores de protocolo, Ataques o intrusiones, etc.)
REJ (Rechazo): cuando el host rechaza el establecimiento de una conexión (el puerto falla, firewall, ataques DoS o DDoS, etc.)
RSTO: cuando se envía a un Host un paquete RST (reseteo) para finalizar una conexión TCP de forma abrupta, y ocurren en caso similares a REJ.
RSTOSO: indica que el host ha recibido un paquete RST de otro host, y esta ha finalizado.
Puede ocurrir como respuesta a las mismas situaciones que generan un RSTO.
RSTR: Similar a RSTS, pero indica que el paquete RST se envió en respuesta a un ataque SYN flood, consintiendo en enviar una gran cantidad de paquetes SYN falsos a un servidor con el objetivo de sobrecargarlo y denegar el servicio a usuarios legítimos (DoS)
RSTRH: Similar a RSTR, pero indica que el paquete RST se recibió en respuesta a un ataque SYN flood.
Conclusiones:
-	Las que más conexiones maliciosas han tenido son OTH, RSTOSO Y RSTR las cuales están muy ligadas a actividades maliciosas como se ha explicado, pero en general hay bastantes conexiones maliciosas usando las diferentes categorías de la variable "conn_state", por lo que puede ser útil para identificar conexiones maliciosas, siendo las conexiones OTH, RSTS, RSTO y RSTR son las más propensas a ser "Malicious". En contra las mas propensas a conexiones “Benign” han sido: S1 y SH.
-	Toda esta información se puede utilizar para desarrollar sistemas de detección de intrusiones (IDS), acompañado de un seguimiento de las variables OTH, RSTO y RSTR con la finalidad de identificar características concretas que se asocian a conexiones maliciosas.
  Como se puede observar, del estudio de correlación de Pearson, hay algunas variables que tienen una alta cardinalidad, además de valores similares, por lo que se tendrá en cuenta para proceder a eliminar la que sea menos interesante para este estudio.
 	
 ![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/d839921a-0c90-480b-ac54-4ab0532b519f)
Aquí observan la distribución de ataques por año y hora del día, observando que durante 2018 los ataques fueron mas equilibrados durante todo el año y en 2019 cambiaron la estrategia, y acumularon los ataques entre las 5 AM – 17 PM.
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/a79c73f7-fcd4-44ef-b652-0e3a4ccb28ed)

3. Selección del modelo:
   
Se probaron diferentes modelos de Machine Learning y Deep Learning para encontrar el más adecuado para la tarea de predicción, resultando satisfactorio en Deep Learning, habiendo sido escalados con las técnicas de MinMaxScaler y LabelEncoder, realizándose una selección dl modelos basados en su precisión, eficiencia y capacidad de generalización, concretamente: RandomForest, LogisticRegression de ML y redes neuronales secuenciales con distintas configuraciones para DL.
5. Entrenamiento y evaluación del modelo:
- Con los modelos Machine Learning no se consiguieron buenos resultados: En el RandonForest con la Target desbalanceada y en el LinearRegrassion realizándole un submuestreo al target, consiguiendo balacearla a costa de una pérdida del 30% de los datos, llegando al llegando al sobreajuste (overfitting).
- Con los modelos de Deep Learning, se han trabajado con varias variaciones en las redes neuronales, para tratar de tener más datos para comparar, evaluándose el rendimiento del modelo utilizando un conjunto de datos de prueba posteriormente:
	+1ª Red Neuronal secuencial. – Esta red esta formada por 1 capa de entrada densa con función de activación “Relu” y un regularizador tipo Lasso(L2), 3 capas ocultas densas, terminado con una capa de salida de 1 valor con la función de activación Sigmoide, dado el fin clasificador del proyecto, usando un “early_stopping” para evitar el sobreajuste, al que llegue con los modelos de ML.
 ![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/df49d954-35ab-48a9-b37f-17aad1454105)
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/2352cecd-4392-4e21-b277-d951a39ee887)
  +2ª modelo de Red Neuronal Secuencial. – Este modelo es igual que el anterior, excepto en el número de unidades de cada capa y la capa de salida que la hice con 2 unidades de salida y la función “softmax”.
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/6dbab71a-c2de-466c-b35d-e2b587db7559)
	+3ª modelo de red neuronal secuencial. – Este modelo es similar a los anteriores, pero en este caso juego con capas “Relu” y “elu”, las cuales pueden llegar a ser mas eficientes evitando, lo que se conoce como “muerte neuronal” y mejorando con ciertos hiperparametros el optimizador Adam, para intentar mejorar el rendimiento del gradiente, previniendo las divisiones entre cero.
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/0f9a3161-0432-4000-b28c-d252963e120c)
+Resultados:
1ª modelo				2ªmodelo				3ªmodelo
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/19187108-5499-4ea7-a6c4-ee316eb6b092)

   Como podemos observar los valores son muy similares, consiguiendo un modelo que generaliza muy bien, llegando a una precisión un 10% mas alta en la clase mayoritaria (98%) y el minoritaria (88%) sumado a un “recall” también alto de un 93% a la clase mayoritaria (Malicius) y un 97% en la clase minoritaria (Benign), es decir acierta en los valores declarados como positivos y en los identificados como positivos en cada clase en unas proporciones altas,  esperándose,  que estos resultados,  sean útiles para prevenir ataques de malware y proteger los sistemas informáticos.

4. Reflexiones finales:

Este proyecto puede ayudar a mejorar la seguridad informática de las empresas y organizaciones, protegiendo a los usuarios de ataques de malware, mostrándose el Machine Learning/Deep Learning como procesos tecnológicos internos que abordan problemas reales de las personas en el mundo real.
Sería muy interesante para un futuro contactar con las diferentes empresas u organismos afectados para que nos aporten información nueva y actual, relacionándola con las diferentes categorías analizadas aquí, con la finalidad de conseguir que los modelos generativos den resultados aun más precisos.

	




