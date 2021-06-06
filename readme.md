´´´
include <Arduino.h>
#include <Wire.h>
void setup()
{
Wire.begin();
Serial.begin(115200);
while (!Serial); // Leonardo: wait for serial monitor
Serial.println("\nI2C Scanner");
}
void loop()
{
byte error, address;
int nDevices;
Serial.println("Scanning...");
nDevices = 0;
for(address = 1; address < 127; address++ )
{
// The i2c_scanner uses the return value of
// the Write.endTransmisstion to see if
// a device did acknowledge to the address.
Wire.beginTransmission(address);
error = Wire.endTransmission();
if (error == 0)
{
Serial.print("I2C device found at address 0x");
if (address<16)
Serial.print("0");
Serial.print(address,HEX);
Serial.println(" !");
nDevices++;
}
else if (error==4)
{
Serial.print("Unknown error at address 0x");
if (address<16)
Serial.print("0");
Serial.println(address,HEX);
}
}
if (nDevices == 0)
Serial.println("No I2C devices found\n");
else
Serial.println("done\n");
delay(5000); // wait 5 seconds for next scan
}
´´´
## Funcionamiento
Con este codigo vamos a poder identificar cual seria el puerto de comunicación I2C de cualquier
dispositivo que tengamos.
Dentro del voidsetup() cargaremos el Wire.begin() de nuestra liberia Wiring Wire
que nos permitira leer o escribir datos de forma facil en un dispositivo externo. Despues
empezaremos una comunicación en serie con una velocidad de 115200 bauds.
Crearemos un voidloop() donde miraremos en que dirección esta nuestro dispositivo.
Despues nos aparecera un mensaje diciendo "Scanning" Serial.println("Scanning..."); donde
empezara a escanear y buscar donde esta nuestro dispositivo.
El programa empezara a hacer el bucle donde el i2c_scanner usara el valor de retorno de la
Write.endTransmisstion para ver si un dispositvo reconoció la dirección
El programa empezara a hacer un bucle hasta encontrar la dirección la cual esta nuestro dispositivo.
Primero empezara con el primer if y una vez echa esta parte del bucle, hara la parte del else if.
En este apartado si ha encontrado la direccion de nuestro dispositivo deberia aparecernos algo como
esto:
I2C Scanner
Scanning...
I2C device found at address 0x27 !
done
Scanning...
I2C device found at address 0x27 !
done
y asi sucesivamente.
En el caso de que no encontrara ningun dispositivo I2C, se hubiera desconectado algun cable o
hubiera algun problema, nos saldra un error asi "No I2C devices found\n".
