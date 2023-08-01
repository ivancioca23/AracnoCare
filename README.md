#include <Servo.h>

// Definición de los servos
Servo servo1;
Servo servo2;
Servo servo3;
Servo servo4;
Servo servo5;
Servo servo6;
Servo servo7;
Servo servo8;
Servo servo9; 

// Definición de los ejes
int eje1 = 90;
int eje2 = 90;
int eje3 = 90;
int eje4 = 90;

const int joyX = A0; // Conexión del eje X del joystick al pin analógico A0
const int joyButton =8; 

int buttonState = HIGH; // Estado actual del botón
int lastButtonState = HIGH; // Estado anterior del botón

int servo9Pos = 0; // Posición actual del servo9

void setup() {
  servo1.attach(2);
  servo2.attach(3);
  servo3.attach(5);
  servo4.attach(7);
  servo5.attach(9); 
  servo6.attach(10);
  servo7.attach(12);
  servo8.attach(13);
  servo9.attach(11); 
  
  pinMode(joyButton, INPUT_PULLUP); // Configuramos el pin del botón como entrada

  // Inicialización de los servos
  
  servo9.write(servo9Pos);
}

void loop() {
  // Primer código
  int xValue = analogRead(joyX);

  if (xValue > 512) {
    int angle = map(xValue, 512, 1023, 90, 120);
    walkForward(angle);
  } else {
    servo1.write(90);
    servo2.write(90);
    servo3.write(90);
    servo4.write(90);
  }

  // Segundo código
  servoControl(2, servo5, eje1);
  servoControl(3, servo6, eje2);
  servoControl(4, servo7, eje3);
  servoControl(5, servo8, eje4);

  
  buttonState = digitalRead(joyButton);
  
  // Comprueba si el estado del botón ha cambiado
  if (buttonState != lastButtonState) {
    // Solo cambia la posición del servo cuando se detecta un flanco descendente (botón presionado)
    if (buttonState == LOW) {
      servo9Pos = servo9Pos == 0 ? 90 : 0; // Cambia entre 0 y 90 grados
      servo9.write(servo9Pos);
    }
    
    
    lastButtonState = buttonState;
  }

  delay(15);
}

void walkForward(int angle) {
  servo1.write(angle);
  servo2.write(180 - angle);
  servo3.write(180 - angle);
  servo4.write(angle);
  delay(500);

  servo1.write(180 - angle);
  servo2.write(angle);
  servo3.write(angle);
  servo4.write(180 - angle);
  delay(500);
}

void servoControl(int pin, Servo &servo, int &eje) {
  if (analogRead(pin) < 200 && eje < 180) {
    eje++;
    servo.write(eje);
  }
  if (analogRead(pin) > 700 && eje > 0) {
    eje--;
    servo.write(eje);
  }
}
