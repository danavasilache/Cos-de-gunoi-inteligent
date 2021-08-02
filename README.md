# Descriere - Coș de gunoi inteligent :wastebasket: 
```diff
+ Vasilache Dana-Maria
```
## Schemă electrică

<img src="/Images/index13.JPG " alt="schemaElectrica"  >

## Funcționalități

> Coșul de gunoi detecteaza mișcarea în raza de maxim 10 cm, deschide lent capacul, îl menține deschis timp de 4 secunde, apoi îl va închide și mai lent.


## Detalii de implementare 

### Explicații cod

> Am declarat variabilele, în funcție de conexiunile efectuate pe plăcuță

```c
#include <Servo.h>   //servo library
Servo servo;     
int trigPin = 2; // trigger a reading   
int echoPin = 3;   // measure a distance
long durata;
long distanta;
long medie;   
long medie1[3];   //vector pentru a masura media distantei
int speed = 0;
```

> Inițializarea variabilelor, la începutul execuției programului, servomotorul se va afla în poziție de inițială de unghi 0, coșul va avea capacul închis

```c
void setup() {       
    Serial.begin(9600);
    servo.attach(7);  // pinul la care e conectat servomotorul
    pinMode(trigPin, OUTPUT);  
    pinMode(echoPin, INPUT);  
    servo.write(0);         //inchidere capac 
    delay(100);
    servo.detach(); 
} 
```

>  Funcția 'masurare()' citește distanța, conform data sheet-ului servomotorului

```c
void masurare() {  
  digitalWrite(trigPin, LOW);
  delayMicroseconds(7);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(15);
  digitalWrite(trigPin, LOW);
  pinMode(echoPin, INPUT);
  durata = pulseIn(echoPin, HIGH);
  distanta = (durata/2) / 29.1;    //obtinem distanta, conform data sheet: jumatate din durata, impartit la 29.1
}
```
>  Funcția 'moveTo' este folosită pentru a controla viteza de închidere / deschidere a capacului
```c
//controlam viteza de miscare a servomotorului cu ajutorul functiei moveTo
int pos =0;  //variable to store the servo position
int pos1 = 0;  //variable to store the servo position
int mapSpeed =0;

void moveTo(int position, int speed){
  mapSpeed = map(speed, 0, 30, 30, 0); //map(value, fromLow, fromHigh, toLow, toHigh)
 	 if(position > pos){
    	for(pos = pos1; pos < position; pos++){ //rotatie initiala (pozitie initiala- finala)
    	  servo.write(pos);
     	  pos1 = pos;
     	  delay(mapSpeed);
   		 }
  	}
  	else{
    	for(pos = pos1; pos >= position; pos--){ //rotatie finala(pozitie finala - initiala)
      		servo.write(pos);// tell servo to go to position in variable ‘pos’
      		pos1=pos;
     		delay(mapSpeed);
    	}
  	}
}

```

```c
void loop() {  
  for (int i=0;i<3;i++) {  //distanta medie
 	 masurare();               
 	 medie1[i]=distanta;            
   	delay(10);  // delay-ul dintre masurari
  }
  distanta= (medie1[0]+medie1[1]+medie1[2])/3;    
  
 
	if (distanta <= 100 && distanta >= 0)  { //distanta intre [0cm, 100cm]
  		servo.attach(7); //pornim servo
  		moveTo(180, 2);  //apelam functia moveTo pt rotatia initiala 
 		delay(4000); // asteptam 4 secunde(timpul in care capacul va sta deschis)
 		moveTo(0, 6); //apelam functia moveTo pt rotatia finala
 		delay(3000);
 		servo.detach(); //oprim servo  
	}
	Serial.print(distanta);
}



```

### Explicații implementare fizică

> Am realizat montajul în Tinkercad, apoi l-am testat fizic. 
>
> Am tăiat cutia conform dimensiunilor senzorului ultrasonic, și l-am atașat. Am lipit de asemenea bateria și plăcuța Arduino cu ajutorul scotch-ului și a lipiciului cald.
>
> Pentru realizarea capacului, am tăiat o bucată de carton conform dimensiunilor cutiei, apoi am împărțit-o în două părți, lipind-o apoi pe marginea cutiei. 
>
> Am lipit servomotorul pe capac, și am făcut o gaură în apropierea lui. Prin gaura respectivă am introdus o ață, care avea la un capăt o greutate (în interiorul cutiei, lipită de capac) iar celălalt capăt l-am atașat de servomotor.
>
> Am conectat apoi firele, și le-am lipit pe marginea cutiei, pentru a nu încurca foarte mult utilizatorul. (în cazul în care dorește să arunce ceva în coș)


#### Probleme întâmpinate

### Componente folosite

> - Fire conexiune tată - mamă 20cm
>
> - Baterie 9V
>
> - Conector baterie 9V
>
> - Arduino UNO R3 (ATmega328p) - Placă de Dezvoltare Compatibilă cu Arduino + Cablu USB 
>
> - Servo Motor 9G Micro
>
> - Senzor ultrasonic distanta HC-SR04 Arduino 


## Imagini cu montajul realizat 

### Parcurs

<img src="/Images/index3.jpg " alt="Componente" width="500" height="700" title = "Componente"  >
<img src="/Images/index1.jpg " alt="Cos" width="500" height="700" title = "Cos"  >
<img src="/Images/index2.jpg " alt="lipici" width="500" height="700" title = "lipici"  >
<img src="/Images/index5.jpg " alt="componente" width="500" height="700" title = "componente"  >
<img src="/Images/index6.jpg " alt="taiereCapac" width="500" height="700" title = "taiereCapac"  >
<img src="/Images/index7.jpg " alt="fixareServo" width="500" height="700" title = "fixareServo"  >
<img src="/Images/index8.jpg " alt="fixareServo" width="500" height="700" title = "fixareServo"  >
<img src="/Images/index9.jpg " alt="fixareServo" width="500" height="700" title = "fixareServo"  >
<img src="/Images/index10.jpg " alt="lipire" width="1000" height="700" title = "lipire"  >
<img src="/Images/index11.jpg " alt="final" width="500" height="700" title = "final"  >

### Montaj finalizat

<img src="/Images/index12.jpg " alt="final" width="400" height="800" title = "final"  >

## Schemă Tinkercad

<img src="/Images/index4.JPG " alt="tinkercad"  >

> Link către ***Simularea Tinkercad***: [AICI](https://www.tinkercad.com/things/a2zZ3EEzi7v-smashing-inari/editel?sharecode=D1gup9f4qX2-STdjtqi-joQCyNIyWmCHrWpI2QBnbHM) 


## Link-ul prezentării

> https://www.youtube.com/watch?v=YuvEWh17UWE

## Listă cu proiecte similare disponibile pe internet

> Aceeasi funcționalitate, componente diferite
>
> - https://www.instructables.com/Automatic-Trash-Bin/
>
> - https://www.youtube.com/watch?v=bkVdrCpdh5U
>
>- https://create.arduino.cc/projecthub/ashraf_minhaj/arduino-trash-bot-auto-open-close-trash-bin-fef238


