#include <SPI.h>
#include <RF24.h>

int val = 0; 
int led1 = 2;  
int led2 = 3;
int led3 = 4;  
int led4 = 5;
int led5 = 6;    

RF24 radio(9, 10); // CE, CSN

void setup() {
  Serial.begin(9600); // Khởi tạo kết nối Serial với baudrate 9600
  radio.begin();
  radio.setPALevel(RF24_PA_MIN);
  radio.openWritingPipe(0xE8E8F0F0E1LL);
  radio.stopListening();
  pinMode(led1, OUTPUT); 
  pinMode(led2, OUTPUT); 
  pinMode(led3, OUTPUT); 
  pinMode(led4, OUTPUT); 
  pinMode(led5, OUTPUT);
}

void loop() {
  int numberToSend ;
   
  if (Serial.available() > 0) 
   { // Kiểm tra xem có dữ liệu đang được gửi đến không
    val = Serial.read() - '0'; // Đọc giá trị số ngón tay từ Python và chuyển sang kiểu int
    if (val > 0 && val < 6 ) {
       // Nếu giá trị số ngón tay lớn hơn 0
      if(val==1){
        numberToSend = 1 ;
      digitalWrite(led1, HIGH); // Bật LED
      digitalWrite(led2, LOW); // Tắt LED
      digitalWrite(led3, LOW); // Tắt LED
      digitalWrite(led4, LOW); // Tắt LED
      digitalWrite(led5, LOW); // Tắt LED      
      }
      else if(val==2){
        numberToSend = 2 ;
      digitalWrite(led2, HIGH); // Bật LED
      digitalWrite(led1, LOW); // Tắt LED
      digitalWrite(led3, LOW); // Tắt LED
      digitalWrite(led4, LOW); // Tắt LED
      digitalWrite(led5, LOW); // Tắt LED
      }
      else if(val==3){
       numberToSend = 3 ;
      digitalWrite(led3, HIGH); // Bật LED
      digitalWrite(led2, LOW); // Tắt LED
      digitalWrite(led1, LOW); // Tắt LED
      digitalWrite(led4, LOW); // Tắt LED
      digitalWrite(led5, LOW); // Tắt LED
      }
      else if(val==4){
        numberToSend = 4 ;
            digitalWrite(led4, HIGH); // Bật LED
      digitalWrite(led2, LOW); // Tắt LED
      digitalWrite(led3, LOW); // Tắt LED
      digitalWrite(led1, LOW); // Tắt LED
      digitalWrite(led5, LOW); // Tắt LED
      }
      else if(val==5){
        numberToSend = 5 ;
            digitalWrite(led5, HIGH); // Bật LED
      digitalWrite(led2, LOW); // Tắt LED
      digitalWrite(led3, LOW); // Tắt LED
      digitalWrite(led4, LOW); // Tắt LED
      digitalWrite(led1, LOW); // Tắt LED
      }
      } else { 
      numberToSend = 0;
      
      digitalWrite(led1, LOW); // Tắt LED
      digitalWrite(led2, LOW); // Tắt LED
      digitalWrite(led3, LOW); // Tắt LED
      digitalWrite(led4, LOW); // Tắt LED
      digitalWrite(led5, LOW); // Tắt LED
    }
    radio.write(&numberToSend, sizeof(numberToSend));
    Serial.println(numberToSend); 
    
  }
}
