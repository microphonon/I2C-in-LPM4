# Demo code using low-power modes of the MSP430 with I<sup>2</sup>C and eUSCIx
 <p>When the MSP430 is using the I2C bus it can be placed in any of the low-power modes. Upon receipt of its address from the master, the slave wakes up and data exchange can take place. No example code was found in MSP430Ware, which led to the development of the demo code in this repository. Two MSP430 Launchpads are used, with the FR5969 serving as master and FR2355 as slave. Modifications of the code may be needed depending on the MSP430 family, so the specific family users guide should be consulted for correct configuration.
 
 <p>When two Launchpads are sharing 3V3, it was found that at least one set of Spy-Bi-Wire jumpers should be disconnected. Don't forget the external pullup resistors on the SDA and SCL lines.
  
  <p><b>Demo 1:</b> Master reads a single control byte of data from from a slave (Address 0x77). If the master reads control byte = 0x03, flash green LED; otherwise flash red LED. Slave toggles the control byte on alternate reads. Corresponding LEDs also toggle on the slave. Slave is held in LPM4 and polled at rate set by the VLO clocked Timer_A on master, which also supplies the I2C clock via SMCLK. There is no clock enabled on slave, which allows use of LPM4. For sake of clarity and illustration, no data is sent to the slave.
 
  <p><b>Demo 2:</b> Master sends a single control byte to slave (Address 0x77). Master toggles the control byte on alternate cycles. If the slave reads control byte = 0x01, flash green LED; otherwise flash red LED. LED illumination time controlled by Timer_B on slave. Slave is held in LPM4 and polled at rate set by the Timer_A on master; master also supplies the I2C clock via SMCLK. LED blinking on slave uses LPM3 to keep VLO clock running. For sake of clarity and illustration, no data is sent by/received from the slave.
 
 <p><b>Demo 3:</b> Modification of Demo 2 using different slave code while master code remains the same (Demo2_Master.c). This demo operates master and slave in timed but asynchronous loops. Master sends a single control byte to slave (Address 0x77) that toggles on alternate cycles of master Timer_A. If the slave reads control byte = 0x01, green LED is toggled at a rate set by slave Timer_B; otherwise toggle red LED. Timed loop in slave is interrupted by either timeout or receipt of control byte from master. Slave is held in LPM3 until the interrupt occurs. LPM3 is needed to keep VLO running. Master supplies the I2C clock via SMCLK. For sake of clarity and illustration, no data is sent by/received from the slave.
 
  <p><b>Demo 4:</b> Master sends multiple control bytes to slave (Address 0x77). Master toggles the number of control bytes on alternate cycles. If the slave receives 4 control bytes, toggle green LED; otherwise toggle red LED. Slave is held in LPM4 and polled at rate set by the Timer_A on master; master also supplies the I2C clock via SMCLK. Slave begins receiving data when UCRXIFG0 flag is set and escapes from LPM4 with UCSTPIFG interrupt after final byte received. Could also use hardware byte counter on slave. For sake of clarity and illustration, no data is sent by/received from the slave.
 
 <p><b>Demo 5:</b> Modification of Demo 4 using different slave code while master code remains the same (Demo4_Master.c). Master sends different sequence of control bytes to slave (Address 0x77) on alternate cycles. If the slave receives 0x04 as last control byte, toggle green LED; otherwise toggle red LED. Slave is held in LPM4 and polled at rate set by the Timer_A on master; master also supplies the I2C clock via SMCLK. Slave begins receiving data when UCRXIFG0 flag is set and escapes from LPM4 with UCSTPIFG interrupt after final byte received. For sake of clarity and illustration, no data is sent by/received from the slave.
