#include <SPI.h>
#include <RF24.h>

// Khai báo chân điều khiển cho động cơ 1
// int enA = 3;
int inA1 = 2;
int inA2 = 4;

// Khai báo chân điều khiển cho động cơ 2
// int enB = 6;
int inB1 = 5;
int inB2 = 7;

RF24 radio(9, 10); // CE, CSN
uint8_t speed_robot=150;  
void setup() {
  Serial.begin(9600);
  radio.begin();
  radio.setPALevel(RF24_PA_MIN);
  radio.openReadingPipe(0,0xE8E8F0F0E1LL);
  radio.startListening();
  // Khởi tạo chân điều khiển động cơ 1
  //pinMode(enA, OUTPUT);
  pinMode(inA1, OUTPUT);
  pinMode(inA2, OUTPUT);

  // Khởi tạo chân điều khiển động cơ 2
  //pinMode(enB, OUTPUT);
  pinMode(inB1, OUTPUT);
  pinMode(inB2, OUTPUT);
  // Khởi động Serial để đọc từ bàn phím
  Serial.begin(9600);
}
void robotMover (byte inR1, byte inR2, byte inL1, byte inL2, byte action)
{
  analogWrite(6,speed_robot);
  analogWrite(3,speed_robot);
  switch (action)
  {
    case 0:// không di chuyển
      motorControlNoSpeed(inR1, inR2, 0);
      motorControlNoSpeed(inL1, inL2, 0);
      break;
    case 1://đi thẳng
      motorControlNoSpeed(inR1, inR2, 1);
      motorControlNoSpeed(inL1, inL2, 1);
      break;
    case 2:// lùi lại
      motorControlNoSpeed(inR1, inR2, 2);
      motorControlNoSpeed(inL1, inL2, 2);
      break;
    case 3:// quay trái

      motorControlNoSpeed(inR1, inR2, 1);
      motorControlNoSpeed(inL1, inL2, 2);
      break;
    case 4:// quay phải

      motorControlNoSpeed(inR1, inR2, 2);
      motorControlNoSpeed(inL1, inL2, 1);
      break;
    case 5:// rẽ trái
      motorControlNoSpeed(inR1, inR2, 1);
      motorControlNoSpeed(inL1, inL2, 0);
      break;
    case 6:// rẽ phải
      motorControlNoSpeed(inR1, inR2, 0);
      motorControlNoSpeed(inL1, inL2, 1);
      break;
    case 7:// rẽ lùi trái
      motorControlNoSpeed(inR1, inR2, 2);
      motorControlNoSpeed(inL1, inL2, 0);
      break;
    case 8:// rẽ lùi phải
      motorControlNoSpeed(inR1, inR2, 0);
      motorControlNoSpeed(inL1, inL2, 2);
      break;
    default:
      action = 0;
      
  }
}


void motorControlNoSpeed (byte in1,byte in2, byte direct)
{

switch (direct) 
  {
    case 0:// Dừng không quay
      digitalWrite(in1,LOW);
      digitalWrite(in2,LOW);
      break;
    case 1:// Quay chiều thứ 1
      digitalWrite(in1,HIGH);
      digitalWrite(in2,LOW);
      break;    
    case 2:// Quay chiều thứ 2
      digitalWrite(in1,LOW);
      digitalWrite(in2,HIGH);
      break;
    //default: 
  }
}
void forward() {
  robotMover(inA1,inA2,inB1,inB2,1); // TIẾN
}

void backward() {
  robotMover(inA1,inA2,inB1,inB2,2); // LÙI
}

void left() {
  speed_robot=122;
  robotMover(inA1,inA2,inB1,inB2,3); // TRÁI
}

void right() {
  speed_robot=122;
  robotMover(inA1,inA2,inB1,inB2,4); // PHẢI
}

void stop() {
  robotMover(inA1,inA2,inB1,inB2,0); // DỪNG
}
void loop() {
  if (radio.available()) {
    int numberReceived;
    radio.read(&numberReceived, sizeof(numberReceived));
    Serial.println(numberReceived);
    if(numberReceived ==1){
      forward();
    }
    else if(numberReceived ==2){
      backward();
    }
    else if(numberReceived ==3){
      left();
    }
    else if(numberReceived==4){
      right();
    }
    else if(numberReceived==5){
 
      stop();
    }
    else { // Nếu giá trị số ngón tay bằng 0

      stop();
    }
  }
}