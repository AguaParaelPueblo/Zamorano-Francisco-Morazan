# Calculación de la pérdida de carga en la salida de la planta de Zamorano


![Salida](https://docs.google.com/drawings/d/e/2PACX-1vRZrfkdF6nsGO1nmSohnyUxyZy3dkfulUyFCxD5kyoEUrwH-cRmBkb_GX3DlJ5bglunRZdLfvAEC0GS/pub?w=960&h=720)
```python
# %%
from aide_design.play import *
import matplotlib.pyplot as plt
from IPython.display import display
from aide_design import k_value_of_reductions_utility as red
from aide_design import pipeline_utility as pl

pipe_sdr = 26
diam_10 = pipe.ID_SDR(10,pipe_sdr)
diam_8 = pipe.ID_SDR(8,pipe_sdr)
diam_10_double = 2*diam_10
diam_8_double = 2*diam_8

L_10inch = 7.5*(u.m)
L_10_double = 6*(u.m)
L_8_double = 2.5*(u.m)
flow = 40*(u.L/u.s)
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
![graph](https://lh3.googleusercontent.com/baTrH6ddoDK6Lq89V35YGW7-9_oXJJRlP8M-9jEG7MgKRml4_e0sZ5vk0Wnq-rjhLn-WAYf51jYhwOvKfqncjBLgMIDhIB8HtULYE582FDLZAiV_gjV-P66DFxdeStabLKdth3HLUcriovbp2NkTh8OeVdKPE3NzHZQ11ZXfcQJqH-OIoYrcr4qyfdxfFtKTzIc3kNtbCHnVf7jGxYGUXhThM3c8mS61oEwu3NFSNZMJbLyiAlGtL0i-3_XX3We4RCJw-Ae9ajHdb7ip94VzvP5TLCt_Ol64FPwu-S2mDTOZTi8SNlOaNRAUpviCkPY3mQsqoMu5e1VbdP8L4hOuyO_uE2Noh3e9uPfAi5Hp8IPFlepCF7tP0J5tB7Ul_-Ap5ppT7eaOBuUEaiBZNsIb1pzeAqE_L3-suUovtDfHnMvNZKCzOYwbNb9tuKfMlMvYSFkBgGBTDCIy1ECIB3HjGHzy7iRNIDD85R5HIXfIHOZb-DpSgKkP_mGwTKla0Q6ruFyR-wpOK-cZrEG9l9NYHAwZcOYE5WvL1y0_c0bAajo1o355u3tJyyoU9yEjlG94X9ybj2ivBEW4brWel0TLz0fz9wqdqhAU7-pAQQAL3w=w602-h444-no)

Las pérdidas son:
* 3.5 cm para 20 L/s
* 7.8 cm para 30 L/s
* 13.8 cm para 40 L/s
