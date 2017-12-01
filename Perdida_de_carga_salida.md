# Calculación de la pérdida de carga en la salida de la planta de Zamorano

Necesitamos saber la pérdida de carga entre la salida de la planta y los tanques para determinar qué tamaño de válvula necesitamos para controlar el caudal entrando en los tanques. La línea tiene una sección de tubería de PVC de 10" y despues una sección de tubería HG de 8". La foto abajo muestra la configuración:


[![Salida](https://docs.google.com/drawings/d/e/2PACX-1vRZrfkdF6nsGO1nmSohnyUxyZy3dkfulUyFCxD5kyoEUrwH-cRmBkb_GX3DlJ5bglunRZdLfvAEC0GS/pub?w=960&h=720)](https://docs.google.com/drawings/d/1sRee3qasvdkouoNYXYf42Sk0FVRpWxly3OKxnkTIgUY/edit)
Para poder usar la función flow_pipeline, la cual calcula la pérdida en una sola línea, voy a considerar las secciones después de la tee como un sólo tubo de doble área. La siguiente ecuación calcula las pérdidas menores y mayores:

\[
h_{total}=f\frac{8}{\pi^{2} g}\frac{L Q^{2}}{D^{5}}+K\frac{8}{\pi^{2} g}\frac{Q^{2}}{D^{4}}
\]

```python
# %%

#importando

from aide_design.play import *
import matplotlib.pyplot as plt
from IPython.display import display
from aide_design import k_value_of_reductions_utility as red
from aide_design import pipeline_utility as pl

#propiedades de la tubería

pipe_sdr = 26
diam_10 = pipe.ID_SDR(10,pipe_sdr)
diam_8 = pipe.ID_SDR(8,pipe_sdr)
diam_10_double = 2*diam_10
diam_8_double = 2*diam_8
L_10inch = 7.5*(u.m)
L_10_double = 6*(u.m)
L_8_double = 2.5*(u.m)
k_10inch = 4*exp.K_MINOR_90 + exp.K_MINOR_TEE_FLOW_RUN
k_10_double = exp.K_MINOR_90 + red.k_value_reduction(diam_10, diam_8, 40*(u.L/u.s))
k_8_double = exp.K_MINOR_EXP

diam_array = np.array([diam_10.magnitude,diam_10_double.magnitude,diam_8_double.magnitude])*u.inch

lengths_array = np.array([L_10inch.magnitude, L_10_double.magnitude, L_8_double.magnitude])*u.m

k_array = np.array([k_10inch, k_10_double, k_8_double])


HeadlossRange = np.arange(0.1, 25, .1)*u.cm


FlowRange = np.empty_like(HeadlossRange)

for i in range(len(HeadlossRange)):
    FlowRange[i] = pl.flow_pipeline(diam_array, lengths_array, k_array, HeadlossRange[i]).magnitude.to(u.L/u.s).magnitude

plt.plot(FlowRange, HeadlossRange, 'b-');
plt.xlabel('Flow Rate (L/s)');
plt.ylabel('Head Loss (cm)');
plt.title('Flow Rate vs. Head Loss');
plt.grid(True)
plt.show()

```
![graph](images/ExitHeadlossGraph.jpg)



Las pérdidas son:

Caudal (L/s) | Pérdida (cm)
------------ | -------------
20 | 3.5
30 | 7.8
40 | 13.8
