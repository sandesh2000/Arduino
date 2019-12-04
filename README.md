#include <PS2X_lib.h>
PS2X ps2x;

int in1 = 7, in2 = 8, in3 = 10, in4 = 11, pwm1 = 9, pwm2 = 12;
void setup() {
  ps2x.config_gamepad(5, 4, 3, 2, false, false);   //GamePad(clock, command, attention, data, Pressures?, Rumble?)
  Serial.begin(9600);
  pinMode(pwm1,OUTPUT);
  pinMode(pwm2, OUTPUT);
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT);

}
void moveForward() {
  Serial.println("FORWARD");
  analogWrite(pwm1, 120);
  analogWrite(pwm2, 120);
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
}
void moveBackward() {

  Serial.println("BACKWARD");
  analogWrite(pwm1, 120);
  analogWrite(pwm2, 120);
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
}
void turnRight() {

  Serial.println("RIGHT");
  analogWrite(pwm1, 120);
  analogWrite(pwm2, 120);
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, LOW);



}
void turnLeft() {

  Serial.println("left");
  analogWrite(pwm1, 120);
  analogWrite(pwm2, 120);
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
}
void turnClockwise() {

  Serial.println("clock");
  analogWrite(pwm1, 120);
  analogWrite(pwm2, 120);
  digitalWrite(in1, LOW);
  digitalWrite(in2, HIGH);
  digitalWrite(in3, HIGH);
  digitalWrite(in4, LOW);
}
void turnAnticlockwise() {

  Serial.println("anticlock");
  analogWrite(pwm1, 120);
  analogWrite(pwm2, 120);
  digitalWrite(in1, HIGH);
  digitalWrite(in2, LOW);
  digitalWrite(in3, LOW);
  digitalWrite(in4, HIGH);
}
void loop() {
  ps2x.read_gamepad();
  int Ry = ps2x.Analog(PSS_RY); //Read the analog value up down movement from right joystick
  //delay(75);
  int Rx = ps2x.Analog(PSS_RX); //Read the analog value right left movement from right joystick
  //delay(75);
  int Ly = ps2x.Analog(PSS_LY); //Read the analog value up down movement from left joystick
  //delay(75);
  int Lx = ps2x.Analog(PSS_LX); //Read the analog value right left movement from left joystick
  //delay(75);

  if (Ly == 0 && Ry == 0) {
    moveForward();
  }
  else if ((Ly >= 250) && (Ry >= 250)) {
    moveBackward();
  }
  else if ((Ry <=10) && (Ly != 0)){
    turnRight(); //turn off left motor
  }
  else if ((Ly <=10) && (Ry != 0) ) {
    turnLeft(); //turn off right motor
  }
  else if (Ly == 0 && Ry >= 250) {
    turnClockwise();
  }
  else if ((Ly>=0 || Ly<=250 ) && Ry == 0) {
    turnAnticlockwise();
  }

  else {
    analogWrite(pwm1, 0);
    analogWrite(pwm2, 0);
    digitalWrite(in1, LOW);
    digitalWrite(in2, LOW);
    digitalWrite(in3, LOW);
    digitalWrite(in4, LOW);


  }
}
