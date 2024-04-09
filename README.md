  # Digital_Defense
**Proyecto de ML y DL sobre detección de ataques malware**
![ml](https://github.com/vicevil77/Digital_Defense/assets/120662253/609c4974-5640-4ca6-b8df-eb1ad0b7f92c)

OBJETIVO:

El objetivo de este proyecto es desarrollar un modelo de Machine Learning/Deep Learning (ML/DL) para conseguir el mejor modelo para poder predecir si una conexión es o no maliciosa, teniendo en cuenta los datos técnicos de las conexiones a la red, utilizando para ello, un conjunto de datos de 25 millones de registros y 19 columnas, sin llegar a la parte de producción/industrialización. 

METODOLOGÍA:

**1. Recopilación y limpieza de datos:**
Se recopilaron 13 conjuntos de datos de fuentes públicas, realizándose una limpieza de datos para eliminar registros duplicados, inconsistentes o con valores nulos.
Finalmente se combinaron los conjuntos de datos limpios en un único conjunto de datos con 25 millones de registros y 18 columnas.
 ![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/cf002786-03b5-4a59-a6ad-4fda5060eee7)

**2. Exploración de datos:**
Se realizó un análisis exploratorio de datos para comprender las características del conjunto de datos, identificándose las variables relevantes para la predicción de ataques de malware, destacando:
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/a8b020b4-4620-4173-a078-cc6fbffaf955)

A.- El Target como se puede apreciar, esta dividida entre conexiones normales y maliciosas, pero con la finalidad de conseguir mejores resultados se sumaron el “Malicious” todos los tipos de este, quedando la Target (65% 35%), estando desbalanceada, por lo que se usaran hiperparametros y modelos adecuados para evitar un sobreajuste a la clase mayoritaria.
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/4be97ede-ad6c-4d76-b36b-a7142fa83e3b)

B.- En el estudio correlacional entre variables y targets, se encontraron características interesantes:
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/0f363fc2-0e3e-4b9d-a362-4c4ff971409a)
 Del estudio de correlación de Pearson, se observan ciertas variables que tienen una alta cardinalidad, además de valores similares, por lo que se tendrá en cuenta para proceder a eliminar las que sea menos interesante para este estudio.
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/8bdaf39e-d6b3-45c6-b143-8c68e041d9ee)

Como podemos observar, analizando la variable “conn_state” con la” target”, siendo la primera una variable que ayuda a entender cómo funcionan las comunicaciones de red y a rastrear el flujo de datos entre dispositivos, representando las diferentes fases por las que pasa una conexión TCP; siendo muy importante en el análisis de la red para detección de problemas y de actividad maliciosa.
Algunos de los estados de conexión (“conn_state”) más usados son:
- S1: Una conexión TCP establecida, permitiendo el intercambio de datos.
-SHR: Indica que un dispositivo (cliente) ha iniciado la conexión enviando un 	paquete - SYN (sincronización) al servidor.
- SH: Señala que el servidor ha recibido el paquete SYN del cliente y ha respondido con un paquete al cliente. (SYN-ACK
- HS: Representa el inicio de una conexión TCP, siendo cuando dos hosts intercambian paquetes SYN y SYN-ACK, estableciendo los parámetros de la conexión.
- SF: Estado dónde una de las partes ha enviado un paquete FIN indicando el deseo de finalizar la conexión.
- RST (Reseteo): Indica que se desea un restablecimiento forzado de la conexión. Puede ocurrir por un problema, intento de acceso no permitido o en respuesta a un ataque.
- **OTH**: Todo lo que no esta definido en otras categorías (errores de protocolo, Ataques o intrusiones, etc.)
REJ (Rechazo): cuando el host rechaza el establecimiento de una conexión (el puerto falla, firewall, ataques DoS o DDoS, etc.)
RSTO: cuando se envía a un Host un paquete RST (reseteo) para finalizar una conexión TCP de forma abrupta, y ocurren en caso similares a REJ.
- **RSTOSO**: indica que el host ha recibido un paquete RST de otro host, y esta ha finalizado.
Puede ocurrir como respuesta a las mismas situaciones que generan un RSTO.
- **RSTR**: Similar a RSTO, pero indica que el paquete RST se envió en respuesta a un ataque SYN flood, consintiendo en enviar una gran cantidad de paquetes SYN falsos a un servidor con el objetivo de sobrecargarlo y denegar el servicio a usuarios legítimos (DoS)
- RSTRH: Similar a RSTR, pero indica que el paquete RST se recibió en respuesta a un ataque SYN flood.
	+Conclusiones:

-	Las que más conexiones maliciosas han tenido son OTH, RSTOSO Y RSTR las cuales están muy ligadas a actividades maliciosas como se ha explicado, pero en general hay bastantes conexiones maliciosas usando las diferentes categorías de la variable "conn_state", por lo que puede ser útil para identificar conexiones maliciosas, siendo las conexiones OTH, RSTS, RSTO y RSTR son las más propensas a ser "Malicious". En contra las mas propensas a conexiones “Benign” han sido: S1 y SH.
-	Toda esta información se puede utilizar para desarrollar sistemas de detección de intrusiones (IDS), acompañado de un seguimiento de las variables OTH, RSTO y RSTR con la finalidad de identificar características concretas que se asocian a conexiones maliciosas.
  Como se puede observar, del estudio de correlación de Pearson, hay algunas variables que tienen una alta cardinalidad, además de valores similares, por lo que se tendrá en cuenta para proceder a eliminar la que sea menos interesante para este estudio.
 C.- En ta gráfica podemos observar que de los tres puertos existentes en los datos (ICMP, TCP y UDP) el mas usado en conexiones maliciosas, superando a las conexiones normales es el TCP. Hay que decir que el protocolo ICMP, supervisa si ha habido errores y tareas de control en las comunicaciones, no siendo un medio propiamente dicho un sistema de comunicacion, despues tenemos TCP que es la mas fiable y mas usada para comunicaciones en intenet, ya que es la qie se orienta a usar con internet y sus capas transposrtan datos,  siendo la mas vulnerable a los ataques y finalmente la UDP no es tan fiable como la anterior pero es menos vulnerable a los ataques, ya que trabaja con transferecia de datos de baja latencia, no teniendo muchos usarios.	

 D.- Aquí se observa la distribución de ataques por año y hora del día, observando que durante 2018 los ataques fueron mas equilibrados durante todo el año y en 2019 cambiaron la estrategia, y acumularon los ataques entre las 5 AM – 17 PM.
 ![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/46084eb5-e558-4820-8dc4-7f3e37251353)

**3. Selección del modelo:**
   
Se probaron diferentes modelos de Machine Learning y Deep Learning para encontrar el más adecuado para la tarea de predicción, resultando satisfactorio en Deep Learning, habiendo sido escalados con las técnicas de MinMaxScaler,LabelEncoder, hashing y estadarización, realizándose una selección dl modelos basados en su precisión, eficiencia y capacidad de generalización, concretamente: RandomForest, LogisticRegression de ML y redes neuronales secuenciales con distintas configuraciones para DL.
5. Entrenamiento y evaluación del modelo:
- A.- Con los modelos Machine Learning se ha conceguido buenos resultados:
- En el RandonForest con desbalanceo de la Target hacia la clase mayoritaria(Malicious), se ha consiguido un recall medio de 0.74 y un f1-score de 0.85, auqnue en tenga que mejorar
- 
-![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/c54fe7c4-c831-4f40-9018-097bbd5d9f6a)

- En el modelo de LogisticRegression consiguiendo un modelo mas equilibrado con una accuracy del 70% y un recall en ambas clases muy similar y una precision aceptable, por lo que estamos ante un buen modelo:
- 
- ![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/eb4790e1-352a-402a-bce4-0573f7e922ae)
- 
- Se ha realizado un ressanpled de la clase minoritaria consiguiendo igualarar los valores de la target,a costa de una pérdida del 30% de los datos:
- En el modelo LogisticRegrassion con submuestreo al target, se han conseguido resultados aun mejores que en los modelos anteriores, consiguiendo una precision media, un recall medio y una accuracy del 70%, encontrandonos ante un buen modelo capaz de clasificar con gran precision.
- ![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/0a785afc-5b44-440a-8631-decd5c89ffb2)
- 
- En el modelo RandomForest con submuestreo al target, resultados muy similares al logistic anterior, por lo que aqun con la perdida de informacion de un 30%, se ha ganado en eficacia en los modelos estudiados:
- 
- ![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/d66caa0e-6ce8-4e5e-bbbd-6f76800adfcc)

- B.- Con los modelos de Deep Learning, se han trabajado con varias variaciones en las redes neuronales, para tratar de tener más datos para comparar, evaluándose el rendimiento del modelo utilizando un conjunto de datos de prueba posteriormente:
	+1ª Red Neuronal secuencial. – Esta red esta formada por 1 capa de entrada densa con función de activación “Relu” y un regularizador tipo Lasso(L2), 3 capas ocultas densas con un regularizador Lasso(2) en la segunda capa, siendo la 1º Relu y las dos ultimas densas "Elu",jugando con capas “Relu” y “elu”, para llegar a ser mas eficientes evitando, lo que se conoce como “muerte neuronal”. El modelo finaliza con una capa de salida de 1 valor con la función de activación Sigmoid, dado el fin clasificador del proyecto, usando un “early_stopping” para evitar el sobreajuste, al que llegue con los modelos de M, reduce_lr , la cual reduce la tasa dea aprendizaje cuando el rendimiento de la validacion no mejora.
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/53fd3683-d8fd-4c0d-959c-f457488fca77)

  	+2ª modelo de Red Neuronal Secuencial. – Este modelo es igual que el anterior, excepto en el número de unidades de cada capa y que se usa optimizador SGD:
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/b57a29c4-6999-4d68-87fe-1a1e990215a9)

	+3ª y 4º modelo de red neuronal secuencial resampled. – Esate modelo es identico al 1º con la excepcion que los datos de entrenamiento han si submuestreados a la clase minoritaria, como ya hicimos en los modelos de machine learning, por lo que no copiare de nuevo los modelos e iremos directamente a los resultados

+Resultados:

	+1º y 2º red neuronal secuencial:
 ![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/c279ac7d-c906-420a-99e6-ab33b164e029)

  	+3º y 4º red neuronal secuencial balanceada con resempled:
![image](https://github.com/vicevil77/Digital_Defense/assets/120662253/7b3f7f64-2d0b-4e9d-a838-0c3c65927299)

-  Como podemos observar los valores son muy similares, consiguiendo un modelo  que tiene un buen desempeño en la clasificacion de la clase mayoritaria (Malicious), pero respecto a la clase 0 tiene renfimiento bajo, aun habiendo realizado el resempled, aunque puede deberse a falta de entrenamiento del modelo que , por limitaciones tecnicas y tiempos de espera, solo he usado 10 epocas.

**REFLEXIONES FINALES**

Este proyecto puede ayudar a mejorar la seguridad informática de las empresas y organizaciones, protegiendo a los usuarios de ataques de malware, mostrándose el Machine Learning/Deep Learning como procesos tecnológicos internos que abordan problemas reales de las personas en el mundo real.
En general, los mejores modelos con mejores resultados han sido los de ML, habiendo conseguido **un modelo muy bueno el cual es capaz de diferenciar con 100% de posibilidad el 60 % de los arvhivos maliciosos, con una accuracy del 81% y un casi 70% de AUC-ROC, lo que le da un aumemnto a la fabilidad del modelo, siendo un RandonForest, ejecutado en multithearding**, esperando, que estos resultados,  sean útiles para prevenir ataques de malware y proteger los sistemas informáticos.
Sería muy interesante para un futuro contactar con las diferentes empresas u organismos afectados para que nos aporten información nueva y actual, relacionándola con las diferentes categorías analizadas aquí, con la finalidad de conseguir que los modelos generativos den resultados aun más precisos.

	




