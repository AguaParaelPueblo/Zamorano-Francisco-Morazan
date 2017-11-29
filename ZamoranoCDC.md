## Chemical Dosing System for Zamorano

The design for Zamorano was produced before the CDC was incorporated into the design tool, so we need to design it. Namely, we need to figure out the length, number, and diameter of the small-diameter tubes that compose the hamaca.

```python
# %% importing
from aide_design.play import *
from aide_design import cdc_functions as cdc
import collections

#Defining variables (stock concentrations, dosages stolen from Mathcad)
FlowPlant = 40*u.L/u.s
T = u.Quantity(20,u.degC)
NuWater = pc.viscosity_kinematic(T)
HeadlossDosingTubeMax = 20*(u.cm)
RatioError = 0.1
KMinor = 2
DiamTubeArray = np.array(np.arange(1/16,6/16,1/16)) * u.inch
LenCDCTubeMax = 1.3*(u.m)
en_chem=1

StockCoag = 159.091*(u.gram/u.L)
DoseCoag = 40*(u.mg/u.L)

StockCl2 = 7.76*(u.gram/u.L)
DoseCl2 = 2*(u.mg/u.L)

StockOrto = 1450*(u.gram/u.L)
DoseOrto = 7.3*(u.mg/u.L)

dose_list = [DoseCoag, DoseCl2, DoseOrto]
stock_list = [StockCoag, StockCl2, StockOrto]

diam_list = []
L_tube_list = []
n_tube_list = []

# Caculating...
for ConcDoseMax, ConcStock in zip(dose_list, stock_list):
  diam_list.append(cdc.diam_cdc_tube(FlowPlant, ConcDoseMax, ConcStock,
                  DiamTubeArray, HeadlossDosingTubeMax, LenCDCTubeMax,
                  T, en_chem, KMinor).to(u.inch))
  L_tube_list.append(cdc.len_cdc_tube(FlowPlant, ConcDoseMax, ConcStock,
                  DiamTubeArray, HeadlossDosingTubeMax, LenCDCTubeMax,
                  T, en_chem, KMinor))
  n_tube_list.append(cdc.n_cdc_tube(FlowPlant, ConcDoseMax, ConcStock,
                  DiamTubeArray, HeadlossDosingTubeMax, LenCDCTubeMax,
                  T, en_chem, KMinor))

print(diam_list)
print(L_tube_list)
print(n_tube_list)

```
All tubes will be 1/8"

The required tube lengths are:
* Coagulant: 0.98 m
* Chlorine: 1.28 m
* Orthophosphate: 1.01 m

The number of tubes are:
* Coagulant: 4
* Chlorine: 4
* Orthophosphate: 2
