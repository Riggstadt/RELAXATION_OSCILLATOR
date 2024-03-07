# OPEN COLLECTOR COMPARATOR FOR RELAXATION OSCILLATOR
The only proper comparators I have are open collector ones, a pull-up resistor is necessary in order to obtain a properly operating circuit.

The governing equations of the circuit will naturally change with the addition of a supplimentary resistor. To make our work easier, we may take steps to negate the error introduced by the pull-up resistor ($R_{pu})$. 
This can be achieved by keeping $R_{pu}$ below a tenth of the lowest valued resistor in the circuit. The bigger the other resistors, the smaller the error.

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

,**Comparator datasheet**: https://eu.mouser.com/datasheet/2/115/DIOD_S_A0006646728_1-2542792.pdf
