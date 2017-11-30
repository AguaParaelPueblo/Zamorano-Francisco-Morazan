# Calculación de la pérdida de carga en la salida de la planta de Zamorano

```python
from aide_design.play import *
from IPython.display import display
from aide_design import k_value_of_reductions_utility as red

pipe.ID_sch40 = np.vectorize(pipe.ID_sch40)
pipe.ID_sch40 = np.vectorize(pipe.ID_sch40)
PVC_ROUGHNESS = mat.PIPE_ROUGH_PVC
HG_ROUGHNESS = 0.15*(u.mm)
NU_WATER = exp.NU_WATER
pipe_sdr = 26

diam_8 = pipe.ID_SDR(8, pipe_sdr)
diam_10 = pipe.ID_SDR(10,pipe_sdr)


```
La pérdida en la línea de 10":
```python
n_elbows_10 = 10
n_tee_10 = 1
l_tube = 13.5*(u.m)

k_exit = red.k_value_reduction(diam_8, diam_10, flow) + 5*exp.K_MINOR_EL90 + exp.K_MINOR_TEE_FLOW_RUN

pc.headloss(flow, diam_10, l_tube, NU_WATER, PVC_ROUGHNESS, k_exit)

pc.headloss(flow, )
```
```python
# %%
from aide_design.play import *
from IPython.display import display
from aide_design import k_value_of_reductions_utility as red
from aide_design import pipeline_utility as pl
diam_10 = pipe.ID_SDR(10,pipe_sdr)
diam_10_double = 2*pipe.ID_SDR(10,pipe_sdr)
diam_8_double = pipe.ID_SDR(8,pipe_sdr)

L_10inch = 7.5*(u.m)
L_10_double = 6*(u.m)
L_8_double = 2.5*(u.m)
flow = 40*(u.L/u.s)
k_10inch = 4*exp.K_MINOR_90 + exp.K_MINOR_TEE_FLOW_BR
k_10_double = exp.K_MINOR_90 + red.k_value_reduction(diam_10, diam_8, flow)
k_8_double = exp.K_MINOR_EXP

diam_array = np.array([diam_10.magnitude,diam_10_double.magnitude,diam_8_double.magnitude])*u.inch

lengths_array = np.array([L_10inch.magnitude, L_10_double.magnitude, L_8_double.magnitude])*u.m

k_array = np.array([k_10inch, k_10_double, k_8_double])


flow = pl.flow_pipeline(diam_array, lengths_array, k_array,.23*(u.m)).to(u.L/u.s)


print(flow)

```
Las pérdidas son:
* 6 cm para 20 L/s
* 13 cm para 30 L/s
* 23 cm para 40 L/s
