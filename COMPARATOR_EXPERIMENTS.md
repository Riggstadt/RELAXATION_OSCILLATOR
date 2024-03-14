# Experimenting with comparators 
I got myself some open-drain/open-collector comparators AS393 and decided to build some circuits. This document logs my dabble in comparator circuit design.

## Simple comparator 
The AS393/LM393 can be employed as a non-hysteretic comparator, meaning that there is no hsyteresis applied to the comparator (besides an internal hysteresis of a few milivolts). As a result of this lack of hysteresis, 
the comparator will not have high noise immunity and comparator "chatter" may be present in the switching characteristic of the circuit. 

The circuit presented below follows a simple logic, a bare comparator has its inverting input pin fixed at a reference (stable) potential, whilst the non-inverting terminal is left untouched and acts as input to the
voltage comparator. Any fluctuations of the input signal that are above or below the set value of the reference voltage will lead to a state change for the circuit output.

<br>
  <p align="center">
    <img height = "550" src = "BARE_COMP_SCH.png">
    <br>
    <br>
    <a><b>Basic comparator, with no hysteresis</b></b></a>
</p>
<br>

The comparator presented in the image from above is of the non-inverting flavour, with both the upper and lower trip-voltages being symmetric and positive(to the extent permitted by circuit non-idealities).

Such a circuit was built and tested on a simple breadboard. The input signal is a 10kHz, 2Vpp triangular (50% simmetry ramp) fed by onboard function generator of handheld scope HDS242S. The reference is set by a simple
1 to 5 voltage divider connected to the overall power supply, this averages out to around a 1V. 

<br>
  <p align="center">
    <img height = "550" src = "BARE_COMP.png">
    <br>
    <br>
    <a><b>Input and output of simple comparator circuit</b></b></a>
</p>
<br>

The trip voltages may be easily observed and verified, the non-inverting nature of the circuit can also be easily deduced from the scope plot. While all this is pretty nice and useful, there are some glaring 
performance issues:
- Comparator chatter due to imperfect, noise-inducing power supply
- Delay and response times messing with our trip voltages

The circuit can and will be improved by adding hysteresis to the bare comparator, but this will be done in another log.

For fun, we may simulate the same circuit in LTspice, with the spice model of the similar LM393, the only commercialized flavour of 393 that had a proper spice model.

<br>
  <p align="center">
    <img height = "550" src = "COMP_SCHEM.jpg">
    <br>
    <br>
    <a><b>Basic comparator schematic</b></b></a>
</p>
<br>

<br>
  <p align="center">
    <img height = "550" src = "COMP_RES.jpg">
    <br>
    <br>
    <a><b>Basic comparator simulation result</b></b></a>
</p>
<br>

The simulations are true to the real deal:
- Chatter in the vicinity of trip voltages
- Propagation delays
- Rise times and more

After analysing this type of circuit, one thing is clear, for trigger-happy environments hsyteresis is a fundamental requirement.

## Choosing the appropriate pull-up resistor
Open-collector and open-drain comparators warrant the addition of a pull-up resistor, such that the current path of the comparator's end stage is completed and an output signal may be obtained.

The choice of pull-up resistor is often neglected, but there are certain things we must account for when selecting the resistor. Such design considerations are:
- Current sinking ability of comparator output stage
- Leakage and voltage drop in the output stage
- Rise-time and fall-time

To better adress our concerns, we may use the following parameters:
- Maximum Output Sink Current
- Maximum Output Leakage Current
- Output Saturation Voltage

The maximum pull-up resistance ($R_{pu(max)}$) is chosen such that when the output transistor is in cutoff, the tiny output leakage current will only induce a small voltage drop on the pull up resistor.
The resistor's maximum resistance value is the the biggest acceptable voltage drop for our needs.

$$R_{pu(max)}=\frac{V_{drop(max)}}{I_{lk(max)}}$$

The minimum pull-up resistance ($R_{pu(min)}$) must be chosen such that when the comparator goes to low, the output voltage is reasonably small and does not interfere with our predefined logic levels.
For this purpose we need to use a typical characteristic plot for "Output Sink Current vs Output Saturation Voltage". We start from the pull-up voltage and the maximum output voltage acceptable, later on 
we find the corresponding output current for our desired output voltage and determine the appropriate resistance for our pull-up.

$$R_{pu(min)}=\frac{V_{pu}-V_{O(sat)}}{I_{O(sat)}}$$


The comparator won't always drive purely resistive loads, small capacitances may find themselves at out circuit's output. The additional load capacitance will slow and degrade the output waveform. 
The output transistor presents (in combination with the load capacitor) two current paths, one for charging the capacitor and one for discharging it.

When the comparator goes from low to high, the load capacitor is charged by the pull-up voltage through the pull-up resistor until fully charged. The rise-time associated with our arrangement is $t_{r}=2.2\cdot R_{pu}\\;C_{L}$.

Unfortunately, the charge and discharge times (rise and fall) are not equal. When the output goes from high to low, the output transistor begins conducting current and a small voltage will form at the output of the comparator. The transistor may be said to posses an "equivalent collector to emitter resistance", a DC on-resistance of low or very low value. As such, the fall-time will be $t_{f}=2.2\cdot R_{CE}\\;C_{L}$.

If we desire a certain pulse width for our comparator applications, we may use the formulas presented above to gauge an appropriate value for our pull-up resistor.
