# OPEN COLLECTOR COMPARATOR FOR RELAXATION OSCILLATOR
The only proper comparators I have are open collector ones, a pull-up resistor is necessary in order to obtain a properly operating circuit.

The governing equations of the circuit will naturally change with the addition of a supplimentary resistor. To make our work easier, we may take steps to negate the error introduced by the pull-up resistor ($R_{pu})$. 
This can be achieved by keeping $R_{pu}$ below a tenth of the lowest valued resistor in the circuit. The bigger the other resistors, the smaller the error.


## Proper formulas
The proper formulas for the trip voltages can be found in Texas Instruments Design Guide <a href="https://www.ti.com/lit/ab/snoa997a/snoa997a.pdf?ts=1709939307906&ref_url=https%253A%252F%252Fwww.google.com%252F">"Inverting comparator with hysteresis circuit (Rev. A)"</a>. It's important to note that the High to Low transition of comparator is more affected by the presence of the pull-up resistor.

## Pull up resistor dimensioning

$$\begin{gather}
R_{pu(min)}=\frac{V_{S}}{I_{Omax(min)}}\\
\\
R_{pu(max)}=\frac{V_{d}}{I_{Lk(max)}}\\
\end{gather}$$

where:
-  $I_{Omax(min)}$ is the maximum output current the comparator can sink, we take the smallest guaranteed value given by the manufacturer
-  $I_{Lk(max)}$ is the maximum value of the output leakage current of the comparator output-stage transistor, when the output is high

For my application I have the following limits:

$$R_{pu(min)}=\frac{5\\;V}{6\\;mA}=1.2\\;k\Omega$$

$$R_{pu(max)}=\frac{10\\;mV}{1\\;uA}=10\\;k\Omega$$

I've selected a 3 $k\Omega$ resistor as pull-up.

Relevant parameters extracted from <a href="https://eu.mouser.com/datasheet/2/115/DIOD_S_A0006646728_1-2542792.pdf">AS393/A</a> datasheet.
