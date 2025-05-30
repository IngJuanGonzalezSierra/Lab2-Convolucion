# LABORATORIO 2 – Convolución, correlación y transformada de Fourier  
**Nombre:** Juan González Sierra  
**Cédula:** 1000178815  
**Código estudiante:** 5600333  
# Introducción
## INTRODUCCIÓN  

En el análisis de señales y sistemas, operaciones como la convolución, correlación y transformadas son esenciales para entender cómo interactúan las señales con los sistemas que las procesan.  
- **Convolución**: usada para determinar la salida de un sistema LTI ante una entrada dada.  
- **Correlación**: mide la similitud entre señales, útil en detección de patrones y ruido.  
- **Transformada de Fourier**: descompone una señal en componentes de frecuencia, facilitando su análisis espectral.

---

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

