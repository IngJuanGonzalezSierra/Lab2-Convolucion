# LABORATORIO 2 – Convolución, correlación y transformada de Fourier  
**Nombre:** Juan González Sierra  
**Cédula:** 1000178815  
**Código estudiante:** 5600333  
# Introducción


En el análisis de señales y sistemas, operaciones como la convolución, correlación y transformadas son esenciales para entender cómo interactúan las señales con los sistemas que las procesan.  
- **Convolución**: usada para determinar la salida de un sistema LTI ante una entrada dada.  
- **Correlación**: mide la similitud entre señales, útil en detección de patrones y ruido.  
- **Transformada de Fourier**: descompone una señal en componentes de frecuencia, facilitando su análisis espectral.

- Se observa que ambas señales son funciones armónicas con una frecuencia de 100 Hz y están desfasadas 90° entre sí (una es un coseno y la otra un seno):
![](https://github.com/DAJO2/LAB2-/blob/main/SEÑALES_SIN_COS.png)
El Coeficiente de correlación de Pearson: -0.00000000000000002675899923417194801263392048393010
Este número es prácticamente cero, lo que indica que las señales x1[n] (coseno) y x2[n] (seno) son no correlacionadas en promedio.
 ¿Por qué es casi cero?
La correlación entre un coseno y un seno de la misma frecuencia es cero, ya que están desfasados 90°. Esto indica que, en el dominio del tiempo, las señales son ortogonales y no presentan una relación lineal directa.
### TRANSFORMADA
Por medio de la pagina de physionet se descargo una señal tipo EEG en reposo con ojos cerrados debido a que con los ojos cerrados, el cerebro genera predominantemente ondas alfa en la región occipital y parietal, estas ondas son clave en estudios de neurofisiología y sirven como referencia para identificar patrones normales de actividad cerebral. Ademas a esto se reduce la influencia de estímulos visuales en la actividad cerebral, permitiendo un análisis más limpio de la señal EEG sin artefactos inducidos por el parpadeo o el procesamiento visual. Donde la señal fue grabada con una tasa de 160 muestras por segundo.
![](https://github.com/DAJO2/LAB2-/blob/main/SENALFT.png)

Para analizar la señal EEG en reposo con ojos cerrados, es fundamental calcular sus características en el dominio del tiempo. Esto incluye estadísticos descriptivos y frecuencia de muestreo, los que se calcularon por medio de estas funciones:
 ``` pitón
num_signals = edf.signals_in_file
fs = edf.getSampleFrequency(0)
signal_labels = edf.getSignalLabels()

signal = edf.readSignal(0)
edf.close()

time = np.arange(len(signal)) / fs

data_mean = np.mean(signal)
data_std = np.std(signal, ddof=1)
data_var = np.var(signal, ddof=1)
data_median = np.median(signal)
```
Se aplica la Transformada de Fourier (TF) ya que permite analizar la señal EEG en el dominio de la frecuencia, descomponiéndola en sus componentes espectrales (Power Spectral Density, PSD) que indica cómo se distribuye la energía de la señal en el espectro de frecuencias.
 ``` pitón
N = len(signal)
freqs = np.fft.fftfreq(N, d=1/fs)
spectrum = np.abs(fft(signal))
f_welch, psd = welch(signal, fs, nperseg=1024)
```
- Transformada de Furier de la señal EEG
![](https://github.com/DAJO2/LAB2-/blob/main/TRANSFORMADADEFOURIER.png)

- Densidad espectral de potencia(PSD)
![](https://github.com/DAJO2/LAB2-/blob/main/DENSIDADESPECTRAL.png)

Se analizaron los estadísticos descriptivos en función de la frecuencia para caracterizar la señal EEG en el dominio espectral. Se calcularon la frecuencia media y la frecuencia mediana para identificar la tendencia central de la distribución de frecuencias, junto con la desviación estándar, que mide la dispersión de la señal respecto a su frecuencia promedio. Además, se generó un histograma de frecuencias, permitiendo visualizar la distribución de la energía en diferentes rangos del espectro en estado de reposo con ojos cerrados.
``` pitón
freq_mean = np.sum(f_welch * psd) / np.sum(psd)
freq_median = f_welch[np.cumsum(psd) >= np.sum(psd) / 2][0]
freq_std = np.sqrt(np.sum(psd * (f_welch - freq_mean) ** 2) / np.sum(psd))
```
- Histograma señal EEG
![](https://github.com/DAJO2/LAB2-/blob/main/HISTOGRAMA.png)

- Resultados de los estadisticos descriptivos

![](https://github.com/DAJO2/LAB2-/blob/main/Captura.png)
#### Requirements:
- python 3.9
- matplotlib
- pyedflib

## PROCEDIMIENTO

### 1. CONVOLUCIÓN DISCRETA

- **x[n]** (entrada): formada por los dígitos de la cédula `1000178815` →  
  `x = [1, 0, 0, 0, 1, 7, 8, 8, 1, 5]`
- **h[n]** (respuesta del sistema): formada por los dígitos del código `5600333` →  
  `h = [5, 6, 0, 0, 3, 3, 3]
#### Cálculo manual
Se aplica la definición de convolución discreta:  
$$ y[n] = \sum_{k=0}^{N} x[k] \cdot h[n-k] $$

#### Implementación en Python

```python
import numpy as np
import matplotlib.pyplot as plt

x = np.array([1, 0, 0, 0, 1, 7, 8, 8, 1, 5])
h = np.array([5, 6, 0, 0, 3, 3, 3])
y = np.convolve(x, h, mode='full')

n = np.arange(len(y))

plt.stem(n, y, basefmt=" ")
plt.title("Convolución x[n] * h[n]")
plt.xlabel("n")
plt.ylabel("y[n]")
plt.grid(True)
plt.show()
![Image](https://github.com/user-attachments/assets/c691dd04-4326-4fdc-92f3-91b142393ceb)vvvvvvvvvvvvvv

