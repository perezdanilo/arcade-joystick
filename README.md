# Gamepad utilizando Arduino Pro Micro

O projeto visa a montagem de um simples controle arcade, com manete e botões.

<h3>Materiais</h3>

- Arduino Pro Micro
- Push buttons
- Analógico de Playstation 2

<h3>Esquemático</h3>

Como é possível notar, trate-se apenas de vários botões e um módulo analógico conectados ao Arduino Pro Micro: 

![](images/esquematico.png)

Software

```c
//including a library
#include <Gamepad.h>

//initialize a centers of axises for calibration
int leftXcenter = 500;
int leftYcenter = 500;
double multiplierLX = 0.254; //127 / 500
double multiplierLY = 0.254;

//Initializing a Gamepad
Gamepad gp;
void setup() {
  //initializing inputs
  pinMode(A2, INPUT);
  pinMode(A1, INPUT);
  pinMode(2,  INPUT_PULLUP);
  pinMode(3,  INPUT_PULLUP);
  pinMode(4,  INPUT_PULLUP);
  pinMode(5,  INPUT_PULLUP);
  pinMode(6,  INPUT_PULLUP);
  pinMode(7,  INPUT_PULLUP);
  pinMode(8,  INPUT_PULLUP);
  pinMode(9,  INPUT_PULLUP);
//  calibrate();
}

void loop() {
  int lx, ly;
  lx = analogRead(A1);
  ly = analogRead(A0);
  
  //we need to convert a 0-1000 to -127 - 127
  lx = floor((lx - leftXcenter) * multiplierLX);
  ly = floor((ly - leftYcenter) * multiplierLY);
  if(lx > 127) lx = 127;
  if(ly > 127) ly = 127;
  gp.setLeftXaxis(lx);
  //because i have placed a thumbstick in breadboard, i must invert a Y axis and swap X and Y axises
  gp.setLeftYaxis(ly);
  
  int pin2;
  pin2 = digitalRead(2);
  if(pin2 == LOW)
    gp.setButtonState(11, true);
  else
    gp.setButtonState(11, false);

  int pino3;
  pino3 = digitalRead(3);
  if(pino3 == LOW)
    gp.setButtonState(1, true);
  else
    gp.setButtonState(1, false);
    
  int pino4;
  pino4 = digitalRead(4);
  if(pino4 == LOW)
    gp.setButtonState(2, true);
  else
    gp.setButtonState(2, false);

  int pino5;
  pino5 = digitalRead(5);
  if(pino5 == LOW)
    gp.setButtonState(3, true);
  else
    gp.setButtonState(3, false);

  int pino6;
  pino6 = digitalRead(6);
  if(pino6 == LOW)
    gp.setButtonState(4, true);
  else
    gp.setButtonState(4, false);

  int pino7;
  pino7 = digitalRead(3);
  if(pino7 == LOW)
    gp.setButtonState(5, true);
  else
    gp.setButtonState(5, false);

  int pino8;
  pino8 = digitalRead(8);
  if(pino8 == LOW)
    gp.setButtonState(7, true);
  else
    gp.setButtonState(7, false);

  int pino9;
  pino9 = digitalRead(9);
  if(pino9 == LOW)
    gp.setButtonState(6, true);
  else
    gp.setButtonState(6, false);

  delay(20);
}

void calibrate()
{
  int lx, ly;
  int i = 0;
  while(i < 8)
  {
    lx = analogRead(A1);
    ly = analogRead(A0);
    bool validLX = lx > (leftXcenter - 100) && lx < (leftXcenter + 100);
    bool validLY = ly > (leftYcenter - 100) && ly < (leftYcenter + 100);
    if(validLX && validLY)
    {
      i++;
      //nothing to do here!
    }
    else i = 0;
    delay(20);
  }
  leftXcenter = lx;
  leftYcenter = ly;
  multiplierLX = (double)127 / (double)lx;
  multiplierLY = (double)127 / (double)ly;
}
```



<h3>Teste</h3>


<h3>Referências</h3>
</br>

https://www.instructables.com/id/Arduino-LeonardoMicroATMega32u4-As-GamepadGame-Con/</br>
https://github.com/gamelaster
