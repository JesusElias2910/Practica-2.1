# 2.1 Practica Hola Mundo y la Hora de Internet (NTP Time server) 
Depto de Sistemas y Computación  
Ing. En Sistemas Computacionales  
SISTEMAS PROGRAMABLES 23a  
Autor: Jesús Elías Martínez  
Fecha de entrega:   12/10/2023  
Objetivo: Investigar y prácticar la maniuplación de la raspberry pico W con la pantalla oled e internet  

## 2.1.1 Práctica de inicio desplegar algo en pantalla oled
**Modelado en wokwi**  
![](Imágenes-Prácticas/modelado.png)  

**Conexiones (en físico)**  

**Código de thonny**   
Lenguaje: Phyton
```# Importar las librerías
import machine
import ssd1306

# Asignación de los pines SDA y SCL de la oled
i2c = machine.I2C(0, sda=machine.Pin(8), scl=machine.Pin(9))

# Configuración del oled 128x64
oled_Des = ssd1306.SSD1306_I2C(128, 64, i2c)

# Limpiar la pantalla oled
oled_Des.fill(0)
oled_Des.show()

# Despliegue en oled y la posición del mismo dentro de la oled
oled_Des.text("Hola Mundo", 0, 0)
oled_Des.text("Jesus Elias Mtz.", 0, 10)

# Actualizar la oled, para desplegar la información
oled_Des.show()
```

**Resultados**

## 2.1.2 Desplegar la hora de internet (Servidor NTP) en pantalla oled
**Modelado en wokwi**  
![](Imágenes-Prácticas/modelado1.png)  

**Conexiones (en físico)**  

**Código de thonny**   
Lenguaje: Phyton
```
# Importar librerías para uso
import network
import urequests
import utime
from machine import Pin, I2C
import ssd1306

# Proceso para conectar a internet
def WiFi(NomRed, Contra):
    sta_if = network.WLAN(network.STA_IF)
    # Validaciones
    if not sta_if.isconnected():
        print('Conectado a red WiFi...')
        sta_if.active(True)
        sta_if.connect(NomRed, Contra)
        while not sta_if.isconnected():
            pass
    # Despliegue de que se conecto con éxito y la IP
    print('Conexión con éxito')
    print('IP:', sta_if.ifconfig()[0])

# Obtener hora de un servidor NTP Time Server
def HoraInter():
    # Conexión al servidor NTP Time Server
    url = "http://worldtimeapi.org/api/ip"
    response = urequests.get(url)
    fechor = response.json()
    #Retorna la fecha y hora de internet
    return fechor['datetime']

# Configuración de OLED
def OledConfig():
    i2c = I2C(0, sda=Pin(8), scl=Pin(9))
    oledDes = ssd1306.SSD1306_I2C(128, 64, i2c)
    #Retorna ya configurado las conexiones y dimensiones de la OLED
    return oledDes

# Muestra la fecha y hora de internet en OLED
def DespOLED(oledDes, hora):
    oledDes.fill(0)
    oledDes.text("Hora de internet:", 0, 0)
    oledDes.text(hora, 0, 16)
    oledDes.show()

# Nombre de red WiFi
NomRed = "TecNM-ITT-Docentes"
# Contraseña de la red
Contra = "tecnm2022!"  
    
# Llamada a la función para conectar a internet pasando como parámetro el nombre y contraseña de la red
WiFi(NomRed, Contra)
# Llamada la función para configurar conexiones y dimensión de OLED
oledDes = OledConfig()
    
# Ciclo para el despliegue de la hora
while True:
    # Llama a la función para obtener la hora, retornando la misma
    fechor = HoraInter()
    # Despliegue en terminal de thonny
    print("Hora de internet:", fechor)
    # Llamada al método para desplegar en OLED, mandando como parámetros la configuración de oled y la hora
    DespOLED(oledDes, fechor)
    # Cada 60 segundos se actualiza la información
    utime.sleep(60)
```

**Resultados**
