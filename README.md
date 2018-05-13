# HackatonTalaIlegal
//PROGRAMACIÓN DE SENSOR DE HUMO
const int AOUTpin=0;// the AOUT pin of the CO sensor goes into analog pin A0 of the arduino 
const int DOUTpin=8;//the DOUT pin of the CO sensor goes into digital pin D8 of the arduino
const int ledPin=13;//the anode of the LED connects to digital pin D13 of the arduino

int limit;
int value;

void setup (){
  Serial.begin(9600);//sets the baud rate
   pinMode (DOUTpin, INPUT);//sets the pin as an input to the arduino
   pinMode (ledPin, OUTPUT);//sets the pin as an input of the arduino
}
void loop()
{
  value =analogRead(AOUTpin);//read the analog from the CO sensor's AOUT pin
  limit= digitalRead (DOUTpin);//reads the digital value from the CO sensor's DOUT pin
  Serial.print("CO value:");
  Serial.println(value);//prints the CO value
  Serial.print("Limit:");
  Serial.print(limit);//prints the limitreached as either LOW or HIGH value
  delay(1000);
  if (limit <=100){
  Serial.print("all its ok");//if limit has been reached
  }
  else {
    Serial.print("ALARM");// if thereshold not reached
    
  }
}

//PROGRAMACIÓN DE SENSOR DE FUEGO Y BUZZER
int ledPin = 12;                // led
int inputPin = A8;               // Sensor PIR
int val = 0;                    // variable para ver el estado del PIR
int pinSpeaker = 10;           // Set up a speaker on a PWM pin (digital 9, 10, or 11)
int pirState = HIGH;             // empezamos asumiento que no hay fuego
 
void setup() {
  pinMode(ledPin, OUTPUT);      // salida del LED
  pinMode(inputPin, INPUT);     // salida del SENSOR
  pinMode(pinSpeaker, OUTPUT);  // speaker
  Serial.begin(9600);
}
 
void loop(){
  val = digitalRead(inputPin);  // lee si hay fuego  val=1/HIGH no hay fuego; val = 0/LOW si hay fuego
  Serial.print("val : ");  Serial.println(val);
  digitalWrite(ledPin, HIGH);  // turn LED ON para saber que el sistema esta andando
 
  if (val == HIGH) {            
       // NO hay fuego
             Serial.print("NO hay fuego");
      digitalWrite(ledPin, HIGH); // turn LED OFF
      playTone(0, 0);
      delay(300); //era 300    
  }else{
       //SI  hay fuego
             Serial.print("SI hay fuego");
       digitalWrite(ledPin, LOW);  
       playTone(300, 160);
       delay(150);
  }
}
// duration in mSecs, frequency in hertz
void playTone(long duration, int freq) {
    duration *= 1000;
    int period = (1.0 / freq) * 1000000;
    long elapsed_time = 0;
    while (elapsed_time < duration) {
        digitalWrite(pinSpeaker,HIGH);
        delayMicroseconds(period / 2);
        digitalWrite(pinSpeaker, LOW);
        delayMicroseconds(period / 2);
        elapsed_time += (period);
    }
}





