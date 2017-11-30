## Chemical Dosing System for Zamorano

The design for Zamorano was produced before the CDC was incorporated into the design tool, so we need to design it. Namely, we need to figure out the length, number, and diameter of the small-diameter tubes that compose the hamaca.

```python
# %% importing
from aide_design.play import *
from aide_design import cdc_functions as cdc
from aide_design.unit_process_design.prefab import lfom_prefab_functional as lfom

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

The setup for the CDC in the plant is shown in the drawing below:   
![CDC](https://photos.app.goo.gl/VoNx5BNjdy32huxY2)
# LFOM

```python
# %%

HeadlossLfom = 20*u.cm
RatioLfomSafety = 1.5
SdrLfom = 26
#DrillBits = np.array(0.03125, 0.0625, 0.09375, 0.125, 0.15625, 0.1875, 0.21875, 0.25, 0.375, 0.5, 0.625, 0.75, 0.875, 1, 1.25, 1.5, 1.75, 2)*u.inch
DrillBits = np.arange(1.75,2,.25)*u.inch

n_rows= lfom.n_lfom_rows(FlowPlant, HeadlossLfom)
print(n_rows)

NdLfom = lfom.nom_diam_lfom_pipe(FlowPlant, HeadlossLfom, HeadlossLfom, SdrLfom)
OrificeDiam = lfom.orifice_diameter(FlowPlant, HeadlossLfom, DrillBits)
LfomOrificeArray = lfom.n_lfom_orifices(FlowPlant, HeadlossLfom, DrillBits, SdrLfom)
HeightLfomOrifices = lfom.height_lfom_orifices(FlowPlant, HeadlossLfom, DrillBits)

print(OrificeDiam)
print(LfomOrificeArray)
print(HeightLfomOrifices)
```
