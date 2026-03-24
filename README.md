# Curso OpenCV

## cv2.imread(filename, flags)

Carga una imagen desde un archivo hacia la memoria. Por defecto, OpenCV lee las imágenes en formato de color BGR (Azul, Verde, Rojo), no en el estándar RGB.

- filename: (String) La ruta exacta del archivo de imagen.
- flags: (Opcional) Indica cómo se debe leer la imagen.
    - `cv2.IMREAD_COLOR` (o `1`): Para color.
    - `cv2.IMREAD_GRAYSCALE` (o `0`): Para escala de grises.

| `Original` | `cv2.IMREAD_COLOR` | `cv2.IMREAD_GRAYSCALE` |
| --- | --- | --- |
| ![Logo.jpeg](img/Logo.jpeg) | ![image.png](img/image.png) | ![image.png](img/image%201.png)

## cv2.imshow(winname, mat)

Muestra una imagen (o frame) en una ventana del sistema operativo. La ventana se ajusta automáticamente a las dimensiones de la matriz.

- winname: (String) El título que aparecerá en la parte superior de la ventana.
- mat: (NumPy Array) La matriz de la imagen a visualizar.

```python
import cv2

img = cv2.imread('Logo.jpeg', 1)
cv2.imshow("01001011 01001011 01001011", mat)
```

![image.png](img/image%202.png)

## cv2.waitKey(delay)

Detiene la ejecución y espera un evento de teclado. Es estrictamente obligatorio usarlo después de `cv2.imshow()`, ya que le da tiempo a la interfaz gráfica para procesar y dibujar el frame.

- delay: (Int) Tiempo de espera en milisegundos.
    - `<= 0`: Pausa el programa infinitamente hasta que se presione cualquier tecla.
    - `[1, ∞)`: El programa espera la cantidad de milisegundos indicada y luego continúa. Para video en tiempo real, `1` es el valor estándar.

## cv2.destroyAllWindows()

Cierra y destruye todas las ventanas que OpenCV haya abierto, liberando la memoria del sistema. Se coloca siempre al final del script.

- No requiere hiperparámetros.

## cv2.VideoCapture(index)

Inicializa un objeto para capturar un flujo de video, preparándolo para ser leído frame por frame.

- index: (Int o String) El origen del video.
    - `0`: Selecciona la cámara web principal del equipo.
    - `> 0`: Selecciona cámaras secundarias conectadas (1, 2, etc.).
    - `"ruta/video.mp4"`: Lee un archivo de video local.

## VideoCapture.read()

Método del objeto creado por `cv2.VideoCapture`. Captura el siguiente frame disponible en el flujo de video.

- No requiere hiperparámetros. Devuelve una tupla `(ret, frame)`.
    - ret: (Booleano) `True` si el frame se leyó correctamente, `False` si el video terminó o hay un error.
    - frame: (NumPy Array) La matriz de la imagen capturada.

## cv2.cvtColor(src, code)

Convierte una imagen de un espacio de color a otro. Paso fundamental para simplificar el análisis o adaptar imágenes a modelos de machine learning.

- src: (NumPy Array) La imagen o frame de origen.
- code: (Int) El código de la conversión matemática.
    - `cv2.COLOR_BGR2RGB`: Pasa del formato nativo de OpenCV al estándar RGB.
    - `cv2.COLOR_BGR2GRAY`: Convierte la imagen a escala de grises (de 3 canales a 1).

## cv2.flip(src, flipCode)

Invierte una matriz 2D a lo largo de los ejes.

- src: (NumPy Array) La imagen a invertir.
- flipCode: (Int) Define el eje de inversión.
    - `1`: Inversión horizontal (eje Y, efecto espejo).
    - `0`: Inversión vertical (eje X).
    - `1`: Inversión total en ambos ejes.

## cv2.resize(src, dsize, fx, fy, interpolation)

Cambia las dimensiones espaciales de una imagen.

- src: (NumPy Array) Imagen de origen.
- dsize: (Tupla) Tamaño absoluto deseado `(ancho, alto)`. Si se usa, `fx` y `fy` se ignoran.
- fx, fy: (Float) Factores de escala relativa.
    - `(0, 1)`: Disminuye el tamaño.
    - `> 1`: Aumenta el tamaño.
- interpolation: (Int) Algoritmo de cálculo de píxeles.
    - `cv2.INTER_LINEAR`: Rápido, ideal para hacer zoom.
    - `cv2.INTER_AREA`: Ideal para reducir tamaño sin perder información.

## cv2.circle(img, center, radius, color, thickness)

Dibuja un círculo en la imagen. Se usa mucho para marcar puntos clave (landmarks) en la pantalla.

- img: (NumPy Array) La imagen sobre la que se dibuja.
- center: (Tupla) Coordenadas `(X, Y)` del centro.
- radius: (Int) Tamaño del radio.
- color: (Tupla) Color del trazo en formato BGR.
- thickness: (Int) Grosor de la línea.
    - `1`: Círculo completamente relleno.
    - `[1, ∞)`: Grosor del borde exterior.

## cv2.GaussianBlur(src, ksize, sigmaX)

Aplica un filtro de desenfoque gaussiano para reducir el ruido y los detalles innecesarios en la imagen. Es crucial antes de detectar bordes.

- src: (NumPy Array) La imagen de entrada.
- ksize: (Tupla) El tamaño del núcleo (kernel) de desenfoque `(ancho, alto)`.
    - Sus valores deben ser números impares y positivos (ej. `(3, 3)`, `(5, 5)`). A mayor tamaño, mayor desenfoque.
- sigmaX: (Float) Desviación estándar en el eje X.
    - `0`: OpenCV lo calcula automáticamente en base al tamaño del kernel.

## cv2.Canny(image, threshold1, threshold2)

Aplica el algoritmo de Canny para encontrar los bordes de los objetos dentro de una imagen.

- image: (NumPy Array) Imagen de entrada, preferiblemente ya desenfocada y en escala de grises.
- threshold1: (Int) Primer umbral (mínimo) para la detección por histéresis.
- threshold2: (Int) Segundo umbral (máximo).
    - Rango recomendado: Se suele mantener una proporción de 1:2 o 1:3 entre ambos umbrales (por ejemplo, 75 y 200). Los bordes con intensidad superior a threshold2 se consideran seguros; los menores a threshold1 se descartan.

## cv2.findContours(image, mode, method)

Encuentra los contornos (curvas que unen puntos continuos de un mismo color o intensidad) en una imagen binaria.

- image: (NumPy Array) Imagen de entrada binaria (como la que devuelve `cv2.Canny`).
- mode: (Int) Modo de recuperación de contornos.
    - `cv2.RETR_LIST`: Recupera todos los contornos sin crear jerarquías.
    - `cv2.RETR_EXTERNAL`: Recupera solo los contornos más externos.
- method: (Int) Método de aproximación.
    - `cv2.CHAIN_APPROX_SIMPLE`: Comprime segmentos horizontales, verticales y diagonales, guardando solo sus puntos finales (ahorra memoria).

## cv2.contourArea(contour)

Calcula el área encerrada por un contorno. Es muy útil para filtrar ruidos pequeños y quedarse solo con los objetos grandes (como una hoja de papel).

- contour: (NumPy Array) El contorno individual del cual se quiere calcular el área.

## cv2.arcLength(curve, closed)

Calcula el perímetro del contorno o la longitud de la curva.

- curve: (NumPy Array) El contorno a medir.
- closed: (Booleano) Indica si la curva es cerrada (`True`) o es solo una línea abierta (`False`). Para documentos y formas geométricas, suele ser `True`.

## cv2.approxPolyDP(curve, epsilon, closed)

Aproxima una curva poligonal a otra con menos vértices, basándose en la precisión especificada.

- curve: (NumPy Array) El contorno a simplificar.
- epsilon: (Float) Parámetro de precisión que define la distancia máxima entre el contorno original y el aproximado.
    - Es común calcularlo dinámicamente: ej. `0.02 * cv2.arcLength(contour, True)`.
- closed: (Booleano) `True` si el polígono debe estar cerrado.

## cv2.getPerspectiveTransform(src, dst)

Calcula la matriz de transformación de perspectiva (3x3) necesaria para mapear un conjunto de 4 puntos de origen a 4 puntos de destino.

- src: (NumPy Array) Coordenadas de los 4 vértices en la imagen original (ej. las esquinas del papel deformadas por la perspectiva).
- dst: (NumPy Array) Coordenadas de los 4 vértices correspondientes en la imagen de destino (ej. un rectángulo perfecto).

## cv2.warpPerspective(src, M, dsize)

Aplica una transformación de perspectiva a una imagen usando la matriz calculada previamente. Es la función que logra la vista de "pájaro" o escáner plano.

- src: (NumPy Array) La imagen original que se va a transformar.
- M: (NumPy Array) La matriz de transformación de 3x3 (obtenida de `cv2.getPerspectiveTransform`).
- dsize: (Tupla) El tamaño `(ancho, alto)` de la imagen final de salida.

## cv2.adaptiveThreshold(src, maxValue, adaptiveMethod, thresholdType, blockSize, C)

Aplica un umbral a la imagen, pero a diferencia del umbral global, calcula el valor del umbral para pequeñas regiones. Esto da excelentes resultados en documentos con iluminación variable (sombras, destellos).

- src: (NumPy Array) Imagen de origen en escala de grises.
- maxValue: (Int) Valor no nulo asignado a los píxeles que superan el umbral (usualmente `255` para blanco).
- adaptiveMethod: (Int) Algoritmo para calcular el umbral.
    - `cv2.ADAPTIVE_THRESH_GAUSSIAN_C`: El umbral es la suma ponderada gaussiana de los valores del vecindario.
    - `cv2.ADAPTIVE_THRESH_MEAN_C`: El umbral es la media del vecindario.
- thresholdType: (Int) Tipo de umbralización.
    - `cv2.THRESH_BINARY`: Píxeles que superan el umbral se vuelven blancos, el resto negros.
- blockSize: (Int) Tamaño de la región de píxeles usada para calcular el umbral.
    - Debe ser un número impar mayor a 1 (ej. `3`, `5`, `11`).
- C: (Float o Int) Constante restada a la media o suma ponderada calculada. Ayuda a ajustar el contraste fino.

![image.png](img/image%203.png)

[https://www.notion.so](https://www.notion.so)

![image.png](img/image%204.png)
