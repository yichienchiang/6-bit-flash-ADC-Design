# 6-bit flash ADC Design 

The objective of this project is to design a 100Ms/s 6-bit analog-to-digital converter. The report presents a single-ended flash ADC with 64 reference voltage inputs and 6 outputs from the gray encoder. The track-and-hold circuit is used to track and hold the values of Vin, and the low-power supply dynamic comparator is used to compare the inputs from the track-and-hold circuit and reference voltages. The comparator transfers the compared results into the 2 to 1 completion detectors. Finally, the outputs from the completion detectors are provided to the gray encoder, and 6-outputs result. 


#### ADC Block diagram 
![ADC Block diagram ](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/32b03eb2f485511b1db0ba00f6e660dd54a30343/1.PNG)

#### ADC Schematic
![ADC Schematic](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/46cd0fb09a30c61104170c30d5a3b044e2a330f2/12.png)

### Track and Hold Circuit

Since the threshold voltages of the NMOS and PMOS transistors were 0.7 V and the transmission gates are not reliable, utilizing the track and hold circuit to track and hold the input sine wave signal, Vin, is necessary. The switch of this circuit is a single NMOS transistor. When the clock is high, meaning the circuit is in “track” mode, it would track the input signal.  When the clock is low, in the “hold” mode, and the gate is grounded, the circuit would hold the signal. The comparator uses the hold signal and the reference voltage to do the comparison.

However, the voltage drop at hold mode is unavoidable. The drop is due to the clock-feedthrough from the gate. Because the drop voltage may impact the comparison and then ruin the result, decreasing the voltage drop is necessary. To reduce the effect of the voltage drop, we added a capacitor, C=25fF, at the output (Vout) of the track-and-hold circuit.

#### T&H Circuit scematic
![T&H Circuit scematic](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/68381059f8f6d4830bafffd4086ca99a2b2c68a4/3.PNG)

#### T&H Circuit Simulation
![T&H Circuit Simulation](https://github.com/yichienchiang/6-bit-flash-ADC-Design/blob/68381059f8f6d4830bafffd4086ca99a2b2c68a4/4.PNG)

#### T&H Circuit scematic
![T&H Circuit scematic]()

#### T&H Circuit scematic
![T&H Circuit scematic]()




