# Hora

![](Imagenes/Hora.jpg)

## Código
```python
#Importacion de las librerias
from machine import Pin, I2C
from ssd1306 import SSD1306_I2C
import utime
import ntptime

# Configuración de la pantalla OLED y pines I2C
pix_res_x = 128
pix_res_y = 64
scl_pin = 27
sda_pin = 26

def init_i2c(scl_pin, sda_pin):
    # Inicializa el dispositivo I2C
    i2c_dev = I2C(1, scl=Pin(scl_pin), sda=Pin(sda_pin), freq=400000)
    return i2c_dev

def update_time():
    try:
        # Sincroniza la hora con un servidor NTP (usando el servidor predeterminado)
        ntptime.host = 'time.google.com'
        print("Hora actual actualizada desde Internet:", utime.localtime())
    except OSError as e:
        print("Error al sincronizar la hora desde Internet:", e)

def display_time(oled, time_data):
    oled.fill(0)  # Limpia la pantalla
    oled.text("Hora actual:", 0, 0)
    oled.text("{:02d}:{:02d}:{:02d}".format(time_data[3], time_data[4], time_data[5]), 0, 16)
    oled.show()

def main():
    i2c_dev = init_i2c(scl_pin, sda_pin)
    oled = SSD1306_I2C(pix_res_x, pix_res_y, i2c_dev)
    
    while True:
        update_time()  # Actualiza la hora desde Internet
        current_time = utime.localtime()
        
        # Muestra la hora en la pantalla OLED
        display_time(oled, current_time)
        
        # Espera 1 segundo antes de actualizar la hora nuevamente
        utime.sleep(1)

if __name__ == '__main__':
    main()
´´´