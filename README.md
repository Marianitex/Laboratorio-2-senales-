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
* [Espectro de frecuencia](#espectro)
* [Mezcla archivos de audio](#mezcla)
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

- Compatibilidad y Estandarización: .wav es un formato ampliamente compatible con diversas herramientas y bibliotecas de procesamiento de audio, incluidas librosa y sounddevice, que se utilizan en este código. Esto asegura que no haya problemas de compatibilidad al cargar o reproducir archivos de audio.

<a name="carga"></a> 
## Cargar audios de voces y ruidos

Este código está diseñado para trabajar con archivos de audio, específicamente para cargar y reproducir grabaciones de voces y ruidos almacenados en archivos de formato `.wav`. El código comienza definiendo dos listas, `audio_voces` y `audio_ruido`, que contienen los nombres de los archivos de audio correspondientes a las voces y ruidos que se desean manipular.

A continuación, se define una función llamada `loadAudio`, que utiliza la biblioteca `librosa` para cargar un archivo de audio. Esta función toma como parámetro el nombre o la ruta de un archivo de audio y devuelve dos cosas: un array NumPy que contiene los datos de la señal de audio y la tasa de muestreo con la que fue grabado el archivo. La tasa de muestreo es la cantidad de muestras por segundo que se toman del audio, y se mantiene intacta (sin cambios) al cargar el archivo.

La otra función, `playAudio`, se encarga de reproducir el audio que ha sido cargado. Esta función utiliza la biblioteca `sounddevice` para enviar los datos de audio al dispositivo de salida, como los altavoces o auriculares. La función recibe los datos de audio y la tasa de muestreo, los cuales son necesarios para que la reproducción sea precisa. Además, se incluye un comando para esperar hasta que la reproducción del audio haya terminado antes de proceder con cualquier otra operación en el código.

```c
# Archivos de voces y ruidos ya cargados
audio_voces = ["voz_mama.wav", "voz_mari.wav"]
audio_ruido = ["Ruidomama.wav", "ruidomari.wav"]

# Función para cargar un archivo de audio
def loadAudio(archivo):
    data, samplerate = librosa.load(archivo, sr=None)  # Carga el archivo de audio sin cambio de tasa de muestreo
    return data, samplerate

# Función para reproducir un audio
def playAudio(data, sr):
    sd.play(data, sr)  # Reproduce el audio
    sd.wait()  # Espera a que termine la reproducción
```

<a name="onda"></a> 
## Graficas de onda del audio

La función `graficarSonido` está diseñada para crear una visualización de la forma de onda de un archivo de audio. En primer lugar, la función calcula el tiempo en segundos para cada muestra de audio. Esto se realiza generando un array de índices que representan cada muestra en el archivo y dividiendo estos índices por la tasa de muestreo, `sr`. El resultado es un array que indica el tiempo correspondiente a cada muestra, permitiendo que el eje x del gráfico represente el tiempo en segundos.

Luego, la función crea una figura de tamaño 12x6 pulgadas para la visualización. Utiliza `matplotlib` para graficar los datos de audio, donde el eje x muestra el tiempo y el eje y muestra la amplitud de la señal. La línea del gráfico se dibuja en color azul con un grosor de 1.5 puntos. Para enfocar la visualización, el eje x se limita a los primeros 2 segundos del audio, lo que es útil si se desea examinar solo una parte específica de la señal.

```c
# Función para graficar la forma de onda del audio
def graficarSonido(data, sr, title=""):
    t = np.arange(len(data)) / sr  # Calcula el tiempo en segundos
    plt.figure(figsize=(12, 6))  # Tamaño de la figura
    plt.plot(t, data, color='royalblue', lw=1.5)  # Grafica la forma de onda en color azul
    plt.xlim(0, 2)  # Limita el eje x a los primeros 2 segundos
    plt.xlabel('Tiempo (s)')
    plt.ylabel('Amplitud')
    plt.title(title)
    plt.grid(True, linestyle='--', alpha=0.7)  # Añade una cuadrícula punteada
    plt.show()
```

<a name="espectro"></a> 
## Espectro de frecuencia

La función `analisisEspectral` está diseñada para analizar y visualizar el espectro de frecuencias de una señal de audio. Primero, la función calcula el número total de muestras del audio mediante `n = len(data)`. También determina el intervalo de muestreo `T`, que es el inverso de la tasa de muestreo `sr`, indicando el tiempo entre cada muestra.

La función luego aplica la Transformada Rápida de Fourier (FFT) a los datos de audio con `np.fft.fft(data)`. La FFT convierte la señal del dominio del tiempo al dominio de la frecuencia, permitiendo analizar las frecuencias presentes en el audio. Para obtener las frecuencias correspondientes a la FFT, se usa `np.fft.fftfreq(n, T)`, y se toma solo la primera mitad de las frecuencias (`[:n // 2]`), ya que la FFT produce una imagen simétrica en el dominio de la frecuencia.

- Transformada rápida de Fourier FFT: Es un algoritmo utilizado para calcular la transformada discreta de Fourier (DFT) y su inversa de manera más eficiente. La DFT es una transformada utilizada en el procesamiento de señales y de imágenes, entre muchas otras áreas, para transformar una señal discreta en su representación en el dominio de la frecuencia. La FFT acelera el proceso de cálculo de la DFT, lo que permite su uso en aplicaciones en tiempo real y para grandes conjuntos de datos.

```c
# Función para analizar el espectro de frecuencias del audio
def analisisEspectral(data, sr, title="Espectro de frecuencias del audio"):
    n = len(data)  # Número de muestras
    T = 1 / sr  # Intervalo de muestreo
    yf = np.fft.fft(data)  # Transformada de Fourier del audio
    xf = np.fft.fftfreq(n, T)[:n // 2]  # Frecuencias correspondientes a la FFT
    
    plt.figure(figsize=(12, 6))  # Tamaño de la figura
    plt.plot(xf, 2.0 / n * np.abs(yf[:n // 2]), color='darkorange', lw=1.5)  # Grafica el espectro en color naranja
    plt.xlim(0, 2000)  # Limita el eje x a 2000 Hz
    plt.xlabel('Frecuencia (Hz)')
    plt.ylabel('Amplitud')
    plt.title(title)
    plt.grid(True, linestyle='--', alpha=0.7)  # Añade una cuadrícula punteada
    plt.show()
```

<a name="mezcla"></a> 
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











