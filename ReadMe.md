# Raspberry Pi Servo Driver HAT

The servo driver HAT is a PWM based expansion board designed for Raspberry Pi. The PCA9685 chip expands up to 16 channels and supports 12-bits resolution for each channel. It uses the I2C protocol for communication purpose.

For detailed information, kindly visit our wiki page : https://learn.sb-components.co.uk/Pi-servo-HAT

## How to use ?

* Stack Servo HAT on Raspberry Pi.

* Now connect the external power supply of 6V - 12V DC on the Green connector as shown below, Turn on the adapter and it will power the Raspberry Pi.

<img src="https://learn.sb-components.co.uk/images/3/36/Pi_Servo_HAT.png" width="800" height="600" />

* Now connect Servo motor on any servo port ranging between 0-15.

* Clone Github repository by running below command in terminal.
 
``` git clone https://github.com/sbcshop/Raspberry-Pi-Servo-Driver.git ```

* It will clone the repository in the '/home/pi' location, To enter cloned repository, enter the below command.

``` cd Raspberry-Pi-Servo-Driver  ```

* Now run the below command to execute the python code.

``` sudo python3 servo_controller.py ```

* If you are using any other pin for servo motor (between 0-15) then change the default servo pin to the designated pin in code i.e controller.Set_Pulse(15,i)

```python
if __name__=='__main__':
    controller = I2C_Controller(0x40, debug=False)
    controller.setPWMFreq(50)
    while True:
        for i in range(500,2500,10):
            controller.Set_Pulse(15,i)   #setting 15th pin of the servo header(forward direction)
            sleep(0.05)
   
        for i in range(2500,500,-10):
            controller.Set_Pulse(15,i)   #setting 15th pin of the servo header(backward direction)
            sleep(0.05)
``` 
