# Laboratorio-2-señales-
>  Problema del coctel 
---

Septiembre 2024

## Tabla de contenidos
* [¿Qué se va a realizar?](#introduccion)
* [Problema del coctel](#problema)
* [Configuracion del sistema](#configuracion)
* [Librerias](#librerias)
* [Cargar audios de voces y rudios](#carga)
* [Graficas de onda del audio](#onda)
* [Mezcla archivos de audio](#espectro)
* [Uso de ICA y separacion](#ica)
* [Calculo SNR](#snr)
* [Menu](#menu)
* [Analisis](#analisis)
* [Contacto](#contacto)
---
<a name="introduccion"></a> 
## ¿Qué se va a realizar?

En un evento tipo coctel, se instalaron varios micrófonos para escuchar lo que las personas estaban hablando; una vez terminó la fiesta, se solicitó a los 
ingenieros que entregaran el audio de la voz de uno de los participantes. Los ingenieros analizaron las señales grabadas por los micrófonos eran mezclas 
de señales que provenían de diferentes fuentes (personas) para todos los casos y se encontraron con el problema de aislar la voz de interés. 

El problema de la "fiesta de cóctel" se refiere a la capacidad de un sistema para concentrarse en una sola fuente sonora mientras filtra las demás en un entorno con múltiples emisores de sonido. Este problema es común en sistemas de audición tanto humanos como artificiales, y su resolución es esencial en 
aplicaciones como la mejora de la voz, el reconocimiento de habla y la cancelación de ruido. 

Para este laboratorio, los estudiantes recrearán el problema de la fiesta de coctel, donde existen 𝑛 fuentes sonoras capturadas por un arreglo de 𝑛 
micrófonos de acuerdo con la siguiente metodología. 
1. Configuración del sistema: 
- Conecta los tres micrófonos al sistema de adquisición de datos y distribuyéndolos estratégicamente en la sala. Los micrófonos deben estar ubicados de manera que cada uno capture diferentes combinaciones de las señales provenientes de las tres fuentes.
- Ubicar tres personas en posiciones fijas dentro de la sala de laboratorio.
- Deben estar localizados a distancias diferentes y orientados en distintas direcciones para simular un entorno de "fiesta de cóctel". 
2. Captura de la señal:
- Generar la señal mediante la voz de los tres sujetos de prueba; cada uno debe decir una frase diferente durante el tiempo de captura de la señal. Las señales de los micrófonos deben ser registradas por el sistema de adquisición y guardas para ser analizadas.
- Grabar el ruido de la sala con los 3 micrófonos y calcular el SNR de cada señal. Repetir la medición en caso de que el SNR de alguna de las señales 
no sea suficiente. Argumentar las razones. 
3. Procesamiento de señales: 
- Realizar un análisis temporal y espectral de las señales capturadas por cada micrófono, identificando las características principales de cada fuente
sonora. Para realizar el análisis espectral, se recomienda utilizar la transformada de Fourier discreta (DFT) o la transformada rápida de Fourier (FFT),
describiendo la información que se puede obtener con cada una de ellas.
- Investigar los métodos de separación de fuentes, como el Análisis de Componentes Independientes (ICA), el Beamforming, entre otros, para aislar 
la señal de interés a partir de las señales capturadas por los micrófonos.  
4. Evaluar los resultados comparando la señal aislada con la señal original utilizado métricas de calidad como la relación señal/ruido para cuantificar el desempeño de la separación. 

<a name="problema"></a> 
## Problema del coctel

El efecto de fiesta de cóctel es un fenómeno que consiste en enfocar la atención auditiva en un estímulo acústico en particular, mientras se trata de filtrar y eliminar el resto de estímulos que pueden actuar como distractores.El nombre de este fenómeno es bastante representativo del efecto, dado que, si lo pensamos, en una fiesta, cuando estamos hablando con algún invitado, tratamos de filtrar lo que nos está diciendo e ignorando la música y otras conversaciones que puedan estar transcurriendo de forma simultánea, conformando el fondo.

Gracias a este fenómeno, somos capaces de diferenciar entre la voz de la persona con quien estamos manteniendo la conversación de la del resto de personas que puedan estar conformando el fondo acústico del ambiente en el que nos estamos encontrando.Este mismo fenómeno también es el que permite que, sin estar del todo concentrado en otras conversaciones, seamos capaces de captar la atención cuando se menciona una palabra que nos resulta importante, como puede ser el que nos llamen por nuestro nombre.

<a name="configuracion"></a> 
## Configuracion del sistema

En este caso como fue mencionado en la introduccion se debia realizar con 3 personas, en un inicio el laboratorio si se hizo de esa manera pero los audios tomados fueron de un tiempo muy bajo (1 segundo), por esta razon al no obtener un buen muestro decidio realizarse una nueva toma con 2 personas de  la siguiente manera:

Las personas se organizaron una en frente de la otra con una distancia de 27,9 cm equivalemte a lo que mide una hoja carta de largo desde los pies de la persona 1 al celular 1, luego otra hoja desde el celular 1 al celular 2 y una ultima hoja desde el celular 2 a la persona 2. 

![Agregar](captura.jpg)

La grabacion se realizo en este caso por medio una aplicacion llamada "Grabadora de Voz" la cual la podemos encontrar en app store, esto para asegurar que la frecuencia de muestreo que utilizaramos sea la misma.

![Agregar](grabadora.jpg)

Para la grabacion la frecuencia de muestreo fue de 16 kHz en cada celular, se grabaron 4 segundos del ruido del ambiente y 4 segundos a las personas hablando.

![Agregar](frecuencia.jpg)

1. Audios persona 1  (Mariana):


https://github.com/user-attachments/assets/e087082f-2b14-4eb6-b42e-ab8e2317dc59

Dialogo: Mi nombre es Mariana Higuera, tengo 18 años y mi mamá se llama Lilian Caicedo.

https://github.com/user-attachments/assets/cd83c1c9-4014-4715-8696-32f6dae8d133

2. Audio persona 2 (Mamá):

https://github.com/user-attachments/assets/880810c3-7b88-43a7-87cd-1a991a7554f7

Dialogo: Mi mamá se llama Lilia Vasquez, yo tengo 51 años y mi nombre es Lilian Caicedo.

https://github.com/user-attachments/assets/ca4394b2-ae84-47e0-8e82-1a0b656d8360

<a name="librerias"></a> 
## Librerias
```c
import matplotlib.pyplot as plt
import numpy as np
import librosa
import sounddevice as sd
from sklearn.decomposition import FastICA
```
1. Librosa: Es una biblioteca especializada en el procesamiento de señales de audio. Es ampliamente utilizada para tareas como la extracción de características de audio, la generación de espectrogramas, y la manipulación de archivos de audio.

2. SoundDevice: Es una biblioteca que permite la grabación y reproducción de audio directamente desde Python usando dispositivos de audio.

3. FastICA (de sklearn.decomposition): Es una implementación del algoritmo de Análisis de Componentes Independientes (ICA) en la biblioteca scikit-learn. ICA es una técnica de separación de fuentes ciega (BSS) que intenta descomponer una señal multivariable en componentes estadísticamente independientes.

<a name="carga"></a> 
## Cargar aduios de voces y ruidos

```c

```

<a name="onda"></a> 
## Graficas de onda del audio

```c

```

<a name="espectro"></a> 
## Mezcla archivos de audio

```c

```

<a name="ica"></a> 
## Uso de ICA y separacion

```c

```

<a name="snr"></a> 
## Calculo SNR

```c

```

<a name="menu"></a> 
## Menu

```c

```

<a name="analisis"></a> 
## Analisis

```c

```

<a name="contacto"></a> 
## Contacto
* Creado por [Marianitex](https://github.com/Marianitex) - sígueme en mis redes sociales como @_mariana.higuera











