# What is an LDO ? 

A low-dropout regulator (LDO regulator) is a type of a DC linear voltage regulator circuit that can operate even when the supply voltage is very close to the output voltage. 

The main components for an LDO typically consists of a reference voltage, a means of scaling the output voltage and comparing it to the reference, a error amplifier, and a series pass transistor, whose voltage drop is controlled by the amplifier to maintain the output at the required value.

The dropout voltage is the minimum voltage required across the regulator to maintain regulation. The input voltage minus the voltage drop across the pass element equals the output voltage.

When the voltage difference between the input and output falls below the dropout voltage the transistor cannot maintain stable operation and the output voltage decreases.

<img src="Figures/OTA.png" alt="Step 1.2" width="800"/><br>

# Externally Compensated LDO Regulator

## LTSpice Schematic

<img src="Schematics/45nm_ext_CL.png" alt="Step 1.2" width="800"/><br>

## Calculation of transistor sizes using the gm/Id methodology

## Simulation Outputs

### DC operating points for the MOSFETs

<img src="Sim_SS/45_ext_dc.png" alt="Step 1.2" width="800"/><br>

 | Transistor | Vgs / Vsg | Vthn / \|Vthp\| | Vov   | Vds / Vsd | Region of Operation |
|------------|-----------|-----------------|-------|-----------|---------------------|
| M1         | 0.645     | 0.484           | 0.161 | 0.645     | Saturation          |
| M2         | 0.645     | 0.484           | 0.161 | 0.645     | Saturation          |
| M3         | 0.598     | 0.466           | 0.132 | 0.354     | Saturation          |
| M4         | 0.599     | 0.466           | 0.133 | 0.35      | Saturation          |
| M5         | 0.597     | 0.469           | 0.128 | 0.401     | Saturation          |
| M6         | 0.597     | 0.469           | 0.128 | 0.597     | Saturation          |
| MPass      | 0.648     | 0.487           | 0.161 | 0.401     | Saturation          |
| M8         | 0.597     | 0.469           | 0.128 | 0.999     | Saturation          |

### Loop Gain

<img src="Graphs/45_ext_LG_h.png" alt="Step 1.2" width="800"/><br>

The locations of the two poles and the unity gain frequency is as follows,

| **Pole** | **Frequency** |
|----------|---------------|
| fp1      | 316.23 Hz     |
| f_ugb    | 316.23 KHz    |
| fp2      | 2.51 MHz      |

### Open Loop PSRR

<img src="Graphs/45_ext_OL_h.png" alt="Step 1.2" width="800"/><br>

### Closed Loop PSRR

<img src="Graphs/45_ext_CL_h.png" alt="Step 1.2" width="800"/><br>

# Tech Plots

## Tech Plots for gpdk045

Below are the generated technology plots of three different FOMs for the gpdk045 PDK.

### nmos

#### ngm_ro vs ngm_id
<img src="Screenshots/nmos_gmro.png" alt="Step 1.2" width="800"/><br>

#### nid_w vs ngm_id
<img src="Screenshots/nmos_idw.png" alt="Step 1.2" width="800"/><br>

#### nft vs ngm_id
<img src="Screenshots/nmos_ft.png" alt="Step 1.2" width="800"/><br>

## Tech Plots for ptm_045_hp

Below are the generated technology plots of three different FOMs for the ptm_045_hp PDK.

### pmos

#### pgm_ro vs pgm_id
<img src="Screenshots/pmos_ltspice_gmro.png" alt="Step 1.2" width="800"/><br>

#### pid_w vs pgm_id
<img src="Screenshots/pmos_ltspice_idw.png" alt="Step 1.2" width="800"/><br>

#### pft vs pgm_id
<img src="Screenshots/pmos_ltspice_ft.png" alt="Step 1.2" width="800"/><br>

### nmos

#### ngm_ro vs ngm_id
<img src="Screenshots/nmos_ltspice_gmro.png" alt="Step 1.2" width="800"/><br>

#### nid_w vs ngm_id
<img src="Screenshots/nmos_ltspice_idw.png" alt="Step 1.2" width="800"/><br>

#### nft vs ngm_id
<img src="Screenshots/nmos_ltspice_ft.png" alt="Step 1.2" width="800"/><br>