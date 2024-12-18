# Table of Contents

- [Table of Contents](#table-of-contents)
- [What is an LDO ?](#what-is-an-ldo-)
- [Externally Compensated LDO Regulator](#externally-compensated-ldo-regulator)
  - [Specification of the Design](#specification-of-the-design)
  - [LTSpice Schematic](#ltspice-schematic)
  - [Calculation of transistor sizes using the gm/Id methodology](#calculation-of-transistor-sizes-using-the-gmid-methodology)
    - [Pass FET Sizing:](#pass-fet-sizing)
    - [Error Amplifier Sizing:](#error-amplifier-sizing)
      - [PMOS Load Sizing](#pmos-load-sizing)
      - [NMOS Load Sizing](#nmos-load-sizing)
    - [Current Mirror Sizing:](#current-mirror-sizing)
  - [Simulation Outputs](#simulation-outputs)
    - [DC operating points for the MOSFETs](#dc-operating-points-for-the-mosfets)
      - [Loop Gain for worst case scenario with heavy load current of 10 mA](#loop-gain-for-worst-case-scenario-with-heavy-load-current-of-10-ma)
      - [Loop Gain for light load current of 2 mA](#loop-gain-for-light-load-current-of-2-ma)
    - [Open Loop PSRR](#open-loop-psrr)
      - [Explanation about the simulation aritfacts](#explanation-about-the-simulation-aritfacts)
      - [Open Loop PSRR Waveform for worst case scenario with heavy load current of 10 mA](#open-loop-psrr-waveform-for-worst-case-scenario-with-heavy-load-current-of-10-ma)
    - [Closed Loop PSRR for worst case scenario with heavy load current of 10 mA](#closed-loop-psrr-for-worst-case-scenario-with-heavy-load-current-of-10-ma)
    - [Closed Loop PSRR with light load current of 2 mA](#closed-loop-psrr-with-light-load-current-of-2-ma)
    - [Transient Response of externally compensated LDO](#transient-response-of-externally-compensated-ldo)
  - [Comparison of Phase Margin and Closed loop PSRR for ILoad\_min and ILoad\_Max](#comparison-of-phase-margin-and-closed-loop-psrr-for-iload_min-and-iload_max)
  - [DC sweep of Vdd for the externally compensated LDO](#dc-sweep-of-vdd-for-the-externally-compensated-ldo)
- [Internally Compensated LDO Regulator](#internally-compensated-ldo-regulator)
  - [Specification of the Design](#specification-of-the-design-1)
  - [LTSpice Schematic](#ltspice-schematic-1)
  - [Calculation of transistor sizes using the gm/Id methodology](#calculation-of-transistor-sizes-using-the-gmid-methodology-1)
    - [Pass FET Sizing](#pass-fet-sizing-1)
    - [Error Amplifier Sizing:](#error-amplifier-sizing-1)
      - [PMOS Load Sizing](#pmos-load-sizing-1)
      - [NMOS Load Sizing](#nmos-load-sizing-1)
    - [Current Mirror Sizing:](#current-mirror-sizing-1)
  - [Simulation Outputs](#simulation-outputs-1)
    - [DC operating points for the MOSFETs](#dc-operating-points-for-the-mosfets-1)
    - [Loop Gain of worst case scenario for minimum load current of 2 mA](#loop-gain-of-worst-case-scenario-for-minimum-load-current-of-2-ma)
    - [Loop Gain for maximum load current of 10 mA](#loop-gain-for-maximum-load-current-of-10-ma)
    - [Open Loop PSRR for minimum load current of 2 mA](#open-loop-psrr-for-minimum-load-current-of-2-ma)
    - [Closed Loop PSRR for minimum load current of 2 mA](#closed-loop-psrr-for-minimum-load-current-of-2-ma)
    - [Closed Loop PSRR for minimum load current of 10 mA](#closed-loop-psrr-for-minimum-load-current-of-10-ma)
    - [Transient Response of internally compensated LDO regulator](#transient-response-of-internally-compensated-ldo-regulator)
  - [Comparison of Phase Margin and Closed loop PSRR for ILoad\_min and ILoad\_Max](#comparison-of-phase-margin-and-closed-loop-psrr-for-iload_min-and-iload_max-1)
  - [Area vs gm/Id Plot for ptm045 PDK](#area-vs-gmid-plot-for-ptm045-pdk)
    - [Transistor Calculations for Different ( g\_m/I\_D ) Ratios](#transistor-calculations-for-different--g_mi_d--ratios)
    - [For ( g\_m/I\_D = 15 )](#for--g_mi_d--15-)
    - [For ( g\_m/I\_D = 12 )](#for--g_mi_d--12-)
    - [For ( g\_m/I\_D = 10 )](#for--g_mi_d--10-)
    - [Overall Area vs ( g\_m/I\_D )](#overall-area-vs--g_mi_d-)
- [Tech Plots](#tech-plots)
  - [Tech Plots for ptm\_045\_hp](#tech-plots-for-ptm_045_hp)
    - [pmos](#pmos)
      - [pgm\_ro vs pgm\_id](#pgm_ro-vs-pgm_id)
      - [pid\_w vs pgm\_id](#pid_w-vs-pgm_id)
      - [pft vs pgm\_id](#pft-vs-pgm_id)
    - [nmos](#nmos)
      - [ngm\_ro vs ngm\_id](#ngm_ro-vs-ngm_id)
      - [nid\_w vs ngm\_id](#nid_w-vs-ngm_id)
      - [nft vs ngm\_id](#nft-vs-ngm_id)
- [Relevance of Tech Plots](#relevance-of-tech-plots)


# What is an LDO ? 

A low-dropout regulator (LDO regulator) is a type of a DC linear voltage regulator circuit that can operate even when the supply voltage is very close to the output voltage. 

The main components for an LDO typically consists of a reference voltage, a means of scaling the output voltage and comparing it to the reference, a error amplifier, and a series pass transistor, whose voltage drop is controlled by the amplifier to maintain the output at the required value.

The dropout voltage is the minimum voltage required across the regulator to maintain regulation. The input voltage minus the voltage drop across the pass element equals the output voltage.

When the voltage difference between the input and output falls below the dropout voltage the transistor cannot maintain stable operation and the output voltage decreases.

<img src="Figures/OTA.png" alt="Step 1.2" width="800"/><br>

# Externally Compensated LDO Regulator

## Specification of the Design

| Specification                              | Externally Compensated LDO |
|--------------------------------------------|----------------------------|
| Input Voltage (V<sub>in</sub>)             | 1.4 V                      |
| Output Voltage (V<sub>out</sub>)           | 1 V                        |
| Power Supply Rejection Ratio (PSRR) at heavy load | 60 dB                      |
| Minimum Load Current (I<sub>load,min</sub>) | 2 mA                       |
| Maximum Load Current (I<sub>load,max</sub>) | 10 mA                      |
| Load Capacitance (C<sub>load</sub>)        | 1 µF                       |
| Quiescent Current (I<sub>quiescent</sub>)  | 50 µA                     |
| Transient Duration                         | Report                        |


## LTSpice Schematic

<img src="Schematics/45nm_ext_CL.png" alt="Step 1.2" width="800"/><br>

## Calculation of transistor sizes using the gm/Id methodology

### Pass FET Sizing:
We sized the pass transistor with the minimum channel length using the 45nm technology node to maximize current flow and minimize resistance. However, to achieve better gain, we opted to use a channel length of 90nm.

```
gm/Id = 10
Id = 10mA
gm/Id * Id = 100mS
A_passfet = gmro_passfet = 50
Id/W = 38 (From Tech Plot)
ro = 500 ohms
CL = 1uF
W = 270um
wp1 = 1/ro*CL = 2Krad/s
fp1 = wp1/(2*pi) = 0.636KHz
```
Size for Pass-FET

```
L = 90 nm
W = 270 um
```

### Error Amplifier Sizing:

```
Loop gain = 60dB = 1000
A_passfet * A_ota = 1000
50 * (gmro/2)_ota = 1000
(gmro)_ota = 40
```

#### PMOS Load Sizing

```
gm/Id = 10
A_pmosload = gmro_ota = 50
L = 90nm
Id = 25uA
Id/W = 40 (From Tech Plot)
W = Id * (w/Id) = 625nm
gm = gm/Id * Id = 250uS
``` 
Size for pmos ota transistors,

```
L = 90 nm
W = 625 nm 
```

#### NMOS Load Sizing

```
gm/Id = 10
A_pmosload = gmro_ota = 53
L = 90nm
Id = 25uA
Id/W = 90
W = Id * (W/Id) = 280nm
gm = gm/Id * Id = 250uS
``` 
Size for nmos ota transistors,

```
L = 90 nm
W = 280 nm 
```

### Current Mirror Sizing:

For current mirror sizing, we use the maximum channel length to achieve high output resistance and minimize channel-length modulation

```
Maximum length L = 270nm (From Tech Plot)
gm*ro = 155
Id/W = 31
W = Id * W/Id = 2um
```

Size for Current Mirror transistors,

```
L = 270 nm
W = 2 um 
```

## Simulation Outputs

### DC operating points for the MOSFETs

<img src="Sim_SS/45_ext_dc.png" alt="Step 1.2" width="800"/><br>

Below table lists down the Vgs, Vth and Vds values and the region of operation for all transistors,

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

#### Loop Gain for worst case scenario with heavy load current of 10 mA

<img src="Graphs/45_ext_LG_h.png" alt="Step 1.2" width="800"/><br>

The locations of the two poles and the unity gain frequency is as follows,

| **Pole** | **Frequency** |
|----------|---------------|
| fp1      | 316.23 Hz     |
| f_ugb    | 316.23 KHz    |
| fp2      | 2.51 MHz      |

<img src="Sim_SS/45_ext_PM.png" alt="Step 1.2" width="800"/><br>

The Phase Margin for the load current of 10 mA is equal to **83.17** degrees.

#### Loop Gain for light load current of 2 mA

<img src="Graphs/45_ext_LG_l.png" alt="Step 1.2" width="800"/><br>

<img src="Sim_SS/45_ext_PM_light.png" alt="Step 1.2" width="800"/><br>

The Phase Margin for the load current of 2 mA is equal to **87.63** degrees.

### Open Loop PSRR

Below is the circuit to simulate the Open-Loop PSSR,

<img src="Schematics/45nm_ext_OL.png" alt="Step 1.2" width="800"/><br>

#### Explanation about the simulation aritfacts

In order to calculate the open loop PSRR we need to send an AC signal from the source which in our case is VDD. 

Here we are giving an AC 1 signal in the source. This signal is given to the source of the Pass FET and the source of pmos in the diffamp. 

We will ideally want very bad PSRR in the diffamp as we want the OTA output to have all the AC noise such that Vsg of pmos = 0 ( small signal analysis ). 

Thus all the noise will get rejected and we will get a noise free dc voltage at the output of the LDO. 

Here in order to calculate the open loop PSRR we have a RC circuit to bias the differential amplifier. 

You can see AC 0 in the circuit indicating that there is an open loop in the circuit . From here we have calculated the open loop PSRR in the circuit. 

Since there is no feedback in the circuit we can thus say that there will be noise at the output and thus the rejection will be very poor.

#### Open Loop PSRR Waveform for worst case scenario with heavy load current of 10 mA

<img src="Graphs/45_ext_OL_h.png" alt="Step 1.2" width="800"/><br>

### Closed Loop PSRR for worst case scenario with heavy load current of 10 mA

<img src="Graphs/45_ext_CL_h.png" alt="Step 1.2" width="800"/><br>

### Closed Loop PSRR with light load current of 2 mA

<img src="Graphs/45_ext_CL_l.png" alt="Step 1.2" width="800"/><br>

### Transient Response of externally compensated LDO

<img src="Graphs/45_ext_Tr.png" alt="Step 1.2" width="800"/><br>

Here, we can see that with the change in Load current, the output voltage is able to transition smoothly with transition in load current. We don't observe any sort of spikes since there is a big load capacitance present at output node.

## Comparison of Phase Margin and Closed loop PSRR for ILoad_min and ILoad_Max

| Current (mA)     | Phase Margin (Degrees) | Closed Loop PSRR (dB) |
|-------------------|------------------------|------------------------|
| 10 (Worst Case)   | 83.17                 | -57.32                |
| 2                 | 87.63                 | -51.31                |

## DC sweep of Vdd for the externally compensated LDO

<img src="Graphs/45_ext_Vdd.png" alt="Step 1.2" width="800"/><br>

From the plots we can see that we are getting regulation after 1.2 till 1.4v. After this
voltage we are still getting 1v output but we can’t comment on the circuit since the device
has a nominal voltage of 1v and the vds across the mosfets might go above 1v so the
device can breakdown.

# Internally Compensated LDO Regulator

## Specification of the Design

| Specification                              | Internally Compensated LDO |
|--------------------------------------------|----------------------------|
| Input Voltage (V<sub>in</sub>)             | 1.4 V                      |
| Output Voltage (V<sub>out</sub>)           | 1 V                        |
| Power Supply Rejection Ratio (PSRR) at heavy load | 60 dB                      |
| Minimum Load Current (I<sub>load,min</sub>) | 2 mA                       |
| Maximum Load Current (I<sub>load,max</sub>) | 10 mA                      |
| Load Capacitance (C<sub>load</sub>)        | 2 nF                       |
| Quiescent Current (I<sub>quiescent</sub>)  | 50 µA                      |
| Transient Duration                         | 1 µs                       |


## LTSpice Schematic

<img src="Schematics/45nm_int_CL.png" alt="Step 1.2" width="800"/><br>

## Calculation of transistor sizes using the gm/Id methodology

We sized the pass transistor with the minimum channel length using the 45nm technology node to maximize current flow and minimize resistance. However, to achieve better gain, we opted to use a channel length of 90nm.


For the calculation,  C<sub>c</sub> we considered a light load condition to account for the worst-case phase margin scenario.

### Pass FET Sizing

```
gm/Id = 10
Id = 10mA
gm/Id * Id = 100mS
A_passfet_heavy = gmro = 50
Id/W = 38 (From Tech Plot)
ro = 500 ohms
W = 270um
```
Size for Pass FET,

```
L = 90 nm
W = 270 um
```

### Error Amplifier Sizing:

```
Loop gain = 60dB = 1000
A_passfet * A_ota = 1000
50 * (gmro/2) = 1000
(gmro)_ota = 40
```


#### PMOS Load Sizing

Size for nmos differential pair transistors in OTA,

```
L = 90 nm
W = 670 nm
```

#### NMOS Load Sizing

Size for pmos load transistors in OTA,

```
L = 90 nm
W = 290 nm
```

### Current Mirror Sizing:

For current mirror sizing, we use the maximum channel length to achieve high output resistance and minimize channel-length modulation

Size for current mirror transistors,

```
L = 270nm
W = 2u
```

For detailed sizing of the other transistors, please refer to this [Excel](Excel/Miller_45nm.xlsx) sheet used for sizing of the transistors.

## Simulation Outputs

### DC operating points for the MOSFETs

<img src="Sim_SS/45_int_dc.png" alt="Step 1.2" width="800"/><br>

Below table lists down the Vgs, Vth and Vds values and the region of operation for all transistors,

| Transistor | Vgs / Vsg | Vthn / \|Vthp\| | Vov   | Vds / Vsd | Region of Operation |
|------------|-----------|-----------------|-------|-----------|---------------------|
| M1         | 0.646     | 0.484           | 0.162 | 0.646     | Saturation          |
| M2         | 0.646     | 0.485           | 0.161 | 0.532     | Saturation          |
| M3         | 0.599     | 0.466           | 0.133 | 0.349     | Saturation          |
| M4         | 0.595     | 0.465           | 0.13  | 0.463     | Saturation          |
| M5         | 0.597     | 0.469           | 0.128 | 0.405     | Saturation          |
| M6         | 0.597     | 0.469           | 0.128 | 0.597     | Saturation          |
| MPass      | 0.532     | 0.487           | 0.045 | 0.401     | Saturation          |
| M8         | 0.597     | 0.469           | 0.128 | 1         | Saturation          |

### Loop Gain of worst case scenario for minimum load current of 2 mA

<img src="Graphs/45_int_LG.png" alt="Step 1.2" width="800"/><br>

<img src="Sim_SS/45_int_PM.png" alt="Step 1.2" width="800"/><br>

The phase margin of the internally compensated LDO with light load current of 2 mA is equal to 49.68 degrees.

### Loop Gain for maximum load current of 10 mA

<img src="Graphs/45_int_LG_h.png" alt="Step 1.2" width="800"/><br>

<img src="Sim_SS/45_int_PM_heavy.png" alt="Step 1.2" width="800"/><br>

The phase margin of the internally compensated LDO with heavy load current of 10 mA is equal to 68.61 degrees.

### Open Loop PSRR for minimum load current of 2 mA

<img src="Graphs/45_int_OL.png" alt="Step 1.2" width="800"/><br>

### Closed Loop PSRR for minimum load current of 2 mA

<img src="Graphs/45_int_CL.png" alt="Step 1.2" width="800"/><br>

### Closed Loop PSRR for minimum load current of 10 mA

<img src="Graphs/45_int_CL_h.png" alt="Step 1.2" width="800"/><br>

### Transient Response of internally compensated LDO regulator

<img src="Graphs/45_int_Tr.png" alt="Step 1.2" width="800"/><br>

Here we see spikes because the feedback loop takes time to respond to the change of load current. Also we see more spike when the Load current changes from min to max value, and the spike is less when the Load current reduces from max to min value.

## Comparison of Phase Margin and Closed loop PSRR for ILoad_min and ILoad_Max

| Current (mA)     | Phase Margin (Degrees) | Closed Loop PSRR (dB) |
|-------------------|------------------------|------------------------|
| 2 (Worst Case)   | 49.68                 | -51.67                |
| 10                 | 68.61                 | -55.94                |


## Area vs gm/Id Plot for ptm045 PDK

This document summarizes the area calculations for different gm/Id values. The table below lists the areas of individual transistors (passfet, PMOS diffamp FET, NMOS diffamp FET, and current mirror FET) as well as the overall area for each gm/Id value.

### Transistor Calculations for Different \( g_m/I_D \) Ratios

| **gm/Id** | **Passfet Area (\(\mu m^2\))** | **P Area (\(\mu m^2\))** | **N Area (\(\mu m^2\))** | **CM FET Area (\(\mu m^2\))** | **Overall Area (\(\mu m^2\))** |
|-----------|-------------------------------|--------------------------|--------------------------|-----------------------------|--------------------------------|
| 10        | 12.08                        | 0.297                    | 0.1124                   | 0.9085                      | 13.3979                       |
| 12        | 17.82                        | 0.445                    | 0.1156                   | 1.36                        | 19.781                        |
| 15        | 32.46                        | 0.544                    | 0.215                    | 2.55                        | 35.769                        |


---

### For \( g_m/I_D = 15 \)
- **Area of passfet:** 32.46 \(\mu\)m²
- **Area of PMOS diffamp FET:** 0.544 \(\mu\)m²
- **Area of NMOS diffamp FET:** 0.215 \(\mu\)m²
- **Area of Current mirror FET:** 2.55 \(\mu\)m²
- **Overall area:** 35.769 \(\mu\)m²

---

### For \( g_m/I_D = 12 \)
- **Area of passfet:** 17.82 \(\mu\)m²
- **Area of PMOS diffamp FET:** 0.445 \(\mu\)m²
- **Area of NMOS diffamp FET:** 0.1156 \(\mu\)m²
- **Area of Current mirror FET:** 1.36 \(\mu\)m²
- **Overall area:** 19.781 \(\mu\)m²

---

### For \( g_m/I_D = 10 \)
- **Area of passfet:** 12.08 \(\mu\)m²
- **Area of PMOS diffamp FET:** 0.297 \(\mu\)m²
- **Area of NMOS diffamp FET:** 0.1124 \(\mu\)m²
- **Area of Current mirror FET:** 0.9085 \(\mu\)m²
- **Overall area:** 13.3979 \(\mu\)m²

---

### Overall Area vs \( g_m/I_D \)

<img src="Graphs/Area.png" alt="Step 1.2" width="800"/><br>

---

We observe that the area increases as the device operates further in the subthreshold region with decreasing overdrive voltage. The \( V_{gs} \) also decreases, assuming the threshold voltage remains the same. Consequently:
- The source voltage required to drive the LDO decreases.
- Bias currents required for MOSFET operation reduce.
- Power required to drive the entire circuit also reduces.


# Tech Plots

## Tech Plots for ptm_045_hp

Below are the generated technology plots of three different FOMs for the ptm_045_hp PDK,

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

# Relevance of Tech Plots

After post-processing of the generated tech-plot csv files for both pmos and nmos, following are the takeaways,

* fT improves with shorter channel lengths, thus conclusion is transistors with smaller channel lengths are faster.

* Comparision of the FOMs for pmos with different lengths,

    | Length (nm) | gmro | Id/W  | fT (GHz) |
    |-------------|------|-------|----------|
    | 67.5        | 17.9 | 168   | 70       |
    | 135         | 53   | 74.4  | 17.2     |
    | 270         | 89   | 35.1  | 4.383    |

* We chose the Vds to be 0.4 mV, and we expect that to result in some error because
the Vds across every MOSFET might not be the same after sizing the circuit under
a particular load. It is very possible that the Vds across the MOSFETs can change
under different values of load current.