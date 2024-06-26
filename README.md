# Trabajo Practico 2

Notas del progreso:
## Primer Modelo: 
Se copio el repositorio de la cátedra y se pusieron funcionalidades para realizar un EDA y entender mejor los resultados. 
Luego se aumento la cantidad de capas, puse 2 extra con activacion softmax y 100 unidades y el modelo EMPEORO significativamente. 
De 22% a 0.8% que ya parece imposible de conseguir. La razon fue porque se agregó una capa soft max en el medio. 

Conviene poner capas intermedias con activaciones como RELU. 
La activacion softmax devuelve un resultado probabilistico entre 0 y 1, al poner una intermedia con softmax lo que pueda aprender la proxima capa esta muy limitado. 
Tambien esta el problema de **gradient flow**: En el proceso de backpropagation el gradiente a partir de softmax se hace mas chico cuando las salidas
son cercanas a 1 o 0 y esto hace que el ajuste de pesos sea poco eficiente. or eso se pone generalmente una sola capa softmax al final. 


## Segundo Modelo: 
Fueron removidas las capas sofmax. Agregue una capa relu. 
model = Sequential() 
model.add(Flatten(input_shape=(32,32,3))) ##CApa de entrada 32*32*3 = 3072 
model.add(Dense(1000, activation="relu")) 
model.add(Dense(1000, activation="relu")) 
model.add(Dense(1000, activation="relu")) 
model.add(Dense(100, activation="softmax")) 
Cambie el train y validation set a imagenes augmentated. Para hacer la augmentacion de la data se usaron los datasets de train y validation, se augmentaron
imagenes yluego se concatenaron las imagenes augmentadas y no augmentadas para tener un mejor set de entrenamiento.
En el train augmentated llega a mejor accuracy. 
Pero en validation ambas igual. 0.26.
Sin data augmentation --> 0.26
Con data augmentation --> 0.26 

## Tercer Modelo (fine tuning): 
Se aumentaron la cantidad de epocs de 10 a 100 y se vio una mejora en los resultados obtenidos. 
Sin data augmentation --> 0.2884 
Con data augmentation --> 0.2878 

## Cuarto Modelo (fine tuning): 
Se aumento el batch size de 128 a 1024, en base a investigar que esto puede traer beneficios a la hora de entrenar al modelo ya que toma más imágenes por iteración. 
Además se agregaron mas capas relu intermedias. Primera capa con 10K neuronas. 
Epochs 1000 Sin data augmentation --> 0.2374 D
ata augmentaiton --> 0.2394 
Noto que a diferencia de antes, con data augmentation los resultados dieron mejores. 
Pero el overall performance empeoro. Luego de ivnestigar concluí que el agregar capas con tantas neuronas 10K no tiene un buen impacto 
ya que esto resulta en una gran cantidad de parámetros que por más que aumente la cantidad de epochs no se llega a entrenar de forma volátil a la red. 

## Quinto Modelo: 
En esta instancia me di cuenta que no estaba entrenando correctamente al modelo con data augmentation, no tomaba todas las imagenes posibles sino un solo batch, 
es por esto que mejoro el performance de la red con data augmentation cuando aumente el batch size. 
Para probar correctamente los datos augmentados concatene y mezcle las imagenes originales con los augmentados, así tengo el doble de datos para entrenar. 
Sin data augmentation --> 0.2374 
Data augmentaiton --> 0.3
