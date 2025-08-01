# Arduino-line-follower
Arduino ile geliştirdiğimiz robot- Gamze
// Çizgi İzleyen 2 Tekerlekli Araç (motor pinleri değişmeden)


  #define sol A0
#define orta A2
#define sag A1

#define sol_motor1 12
#define sol_motor2 11
#define sol_motorA 9

#define sag_motor3 6
#define sag_motor4 5
#define sag_motorB 3

void setup() 
{
  pinMode(sag_motor3, OUTPUT);
  pinMode(sag_motor4, OUTPUT);
  pinMode(sag_motorB, OUTPUT);
  pinMode(sol_motor1, OUTPUT);
  pinMode(sol_motor2, OUTPUT);
  pinMode(sol_motorA, OUTPUT);

  pinMode(sol, INPUT);
  pinMode(orta, INPUT);
  pinMode(sag, INPUT);
}

void loop() {
  int sol_val = digitalRead(sol);
  int orta_val = digitalRead(orta);
  int sag_val = digitalRead(sag);

  if(sol_val == 0 && orta_val == 1 && sag_val == 0) {
    ileri();
  }
  else if(sol_val == 0 && orta_val == 0 && sag_val == 1) {
    sol_donus();
  }
  else if(sol_val == 1 && orta_val == 0 && sag_val == 0) {
    sag_donus();
  }
  else if(sol_val == 0 && orta_val == 0 && sag_val == 0) {
    dur();
  }
  else if(sol_val == 1 && orta_val == 1 && sag_val == 1) {
    ileri();  // Üç sensör de aktifse ileri git
  }
  else if(sol_val == 1 && orta_val == 1 && sag_val == 0) {
    hafif_sag_donus();  
  }
  else if(sol_val == 0 && orta_val == 1 && sag_val == 1) {
    hafif_sol_donus(); 
  }
}

void hafif_sag_donus(){
  digitalWrite(sag_motor3, LOW);
  digitalWrite(sag_motor4, LOW);
  analogWrite(sag_motorB, 70);  // Daha yavaş sağ motor

  digitalWrite(sol_motor1, HIGH);
  digitalWrite(sol_motor2, LOW);
  analogWrite(sol_motorA, 80); // Sol motor tam güç

  delay(100); // Kısa süreli sağa dönüş

  ileri();  // Sonrasında tekrar ileri git
}

void hafif_sol_donus(){
  digitalWrite(sag_motor3, HIGH);
  digitalWrite(sag_motor4, LOW);
  analogWrite(sag_motorB, 80); // Sağ motor tam güç

  digitalWrite(sol_motor1, LOW);
  digitalWrite(sol_motor2, LOW);
  analogWrite(sol_motorA, 70);  // Daha yavaş sol motor

  delay(100); // Kısa süreli sola dönüş

  ileri();  // Sonrasında tekrar ileri git
}

void ileri(){
  digitalWrite(sag_motor3, HIGH);
  digitalWrite(sag_motor4, LOW);
  analogWrite(sag_motorB, 80); // Sağ motor tam hız

  digitalWrite(sol_motor1, HIGH);
  digitalWrite(sol_motor2, LOW);
  analogWrite(sol_motorA, 80); // Sol motor tam hız
}

void sag_donus(){
  digitalWrite(sag_motor3, LOW);
  digitalWrite(sag_motor4, LOW);
  analogWrite(sag_motorB, 80); // Sağ motor tam hız

  digitalWrite(sol_motor1, HIGH);
  digitalWrite(sol_motor2, LOW);
  analogWrite(sol_motorA, 80); // Sol motor tam hız
}

void sol_donus(){
  digitalWrite(sag_motor3, HIGH);
  digitalWrite(sag_motor4, LOW);
  analogWrite(sag_motorB, 80); // Sağ motor tam hız

  digitalWrite(sol_motor1, LOW);
  digitalWrite(sol_motor2, LOW);
  analogWrite(sol_motorA, 0);  // Sol motor tamamen durur
}

void dur(){
  digitalWrite(sag_motor3, LOW);
  digitalWrite(sag_motor4, LOW);
  analogWrite(sag_motorB, 0);  // Sağ motor tamamen durur
  
  digitalWrite(sol_motor1, LOW);
  digitalWrite(sol_motor2, LOW);
  analogWrite(sol_motorA, 0);  // Sol motor tamamen durur
}
