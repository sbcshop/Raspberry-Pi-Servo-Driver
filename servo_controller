#!/usr/bin/python

#~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# 16-Channel Servo Driver HAT for Raspberry Pi
# Developed by SB Components
#~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

from time import sleep
import math
import smbus

class I2C_Controller:

  ADDRESS_1    = 0x02
  ADDRESS_2    = 0x03
  ADDRESS_3    = 0x04
  MODE_1       = 0x00
  PRE_SCALE    = 0xFE
  LED0_ON0     = 0x06
  LED0_ON1     = 0x07
  LED0_OFF0    = 0x08
  LED0_OFF1    = 0x09
  ALL_LED_ON0  = 0xFA
  ALL_LED_ON1  = 0xFB
  ALL_LED_OFF0 = 0xFC
  ALL_LED_OFF1 = 0xFD

  def __init__(self, address=0x40, debug=False):
    self.bus = smbus.SMBus(1)
    self.address = address
    self.debug = debug
    if (self.debug):
      print("Reseting I2C controller")
    self.write_data(self.MODE_1, 0x00)

  def setPWMFreq(self, freq):
    "Setting PWM frequency"
    frequency= 25000000.0  
    frequency /= 4096.0  
    frequency /= float(freq)
    frequency -= 1.0
    if (self.debug):
      print("Setting PWM frequency to %d Hz" % freq)
      print("Estimated frequency: %d" % frequency)
    prescale = math.floor(frequency + 0.5)
    if (self.debug):
      print("Final frequency: %d" % prescale)

    mode = self.read_data(self.MODE_1);
    mode_new = (mode & 0x7F) | 0x10  
    self.write_data(self.MODE_1, mode_new) 
    self.write_data(self.PRE_SCALE, int(math.floor(prescale)))
    self.write_data(self.MODE_1, mode)
    sleep(0.005)
    self.write_data(self.MODE_1, mode | 0x80)

  def Set_PWM(self, channel, on, off):
    "Sets a single PWM channel"
    self.write_data(self.LED0_ON0+4*channel, on & 0xFF)
    self.write_data(self.LED0_ON1+4*channel, on >> 8)
    self.write_data(self.LED0_OFF0+4*channel, off & 0xFF)
    self.write_data(self.LED0_OFF1+4*channel, off >> 8)
    if (self.debug):
      print("channel: %d  LED_ON: %d LED_OFF: %d" % (channel,on,off))
      
  def Set_Pulse(self, channel, pulse):
    "Sets the Pulse"
    pulse = pulse*4096/20000        #PWM frequency is set to 50HZ
    self.Set_PWM(channel, 0, int(pulse))
    
  def write_data(self, register, value):
    "Writes data to the specified register/address"
    self.bus.write_byte_data(self.address, register, value)
    if (self.debug):
      print("I2C: Write 0x%02X to register 0x%02X" % (value, register))
      
  def read_data(self, register):
    "Read the data from the I2C device"
    data = self.bus.read_byte_data(self.address, register)
    if (self.debug):
      print("I2C: Device 0x%02X returned 0x%02X from reg 0x%02X" % (self.address, result & 0xFF, register))
    return data
    
if __name__=='__main__':
    controller = I2C_Controller(0x04, debug=False)
    controller.setPWMFreq(50)
    while True:
        for i in range(500,2500,10):
            controller.Set_Pulse(15,i)   #setting 15th pin of the servo header(forward direction)
            sleep(0.05)
    
        for i in range(2500,500,-10):
            controller.Set_Pulse(15,i)   #setting 15th pin of the servo header(backward direction)
            sleep(0.05)  


