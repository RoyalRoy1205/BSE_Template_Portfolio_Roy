# Gesture Controlled Car
This was my first ever project where I had to work on a physical object as opposed to just working on concepts. As you scroll through you will gain a better understanding of my project and how it works. In short, there is a breadboard, on which there is the Arduino Micro, this holds the code and processes information from the accelerometer on the tilt you give it. Once the Micro processes the information from the accelerometer, it sends the information in the form of letters corresponding with core directions, and tranfers the information from one bluetooth module on the breadboard to the one connected to the Uno, which will activate pre-programmed functions to make the car move.

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Arush R | Wheeler Magnet Highschool | Mechanical/Electrical Engineering and Computer Science | Incoming Sophmore

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/nJuSjTT954M?si=u-5CE_0qgcSo8rwN" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>





# Second Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/y3VAmNlER5Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your second milestone, explain what you've worked on since your previous milestone. You can highlight:
- Technical details of what you've accomplished and how they contribute to the final goal
- What has been surprising about the project so far
- Previous challenges you faced that you overcame
- What needs to be completed before your final milestone 

# First Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/CaCazFBhYKs" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your first milestone, describe what your project is and how you plan to build it. You can include:
- An explanation about the different components of your project and how they will all integrate together
- Technical progress you've made so far
- Challenges you're facing and solving in your future milestones
- What your plan is to complete your project

# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

```c++
#include <SoftwareSerial.h>

#define tx 2
#define rx 3

SoftwareSerial configBt(rx, tx);

int in1 = 5;
int in2 = 6;
int in3 = 9;
int in4 = 10;
int speed = 255;
int chat = 128;

void setup() {
  Serial.begin(38400);
  configBt.begin(38400);
  pinMode(tx, OUTPUT);
  pinMode(rx, INPUT);

  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  pinMode(in3, OUTPUT);
  pinMode(in4, OUTPUT)
}

void loop() {
  if (configBt.available()){
    c = (char)configBt.read()
    Serial.println(c);

}

 acts based on character
  switch(c){
    
   
    case 'F':
      forward();
      break;
      
   
    case 'L':
      left();
      break;
      
  
    case 'R':
      right();
      break;
      
    case 'A':
      forwa();
      break;  

    case 'B':
      back();
      break;

    case 'C':
      backwards();
      break;  

 
    case 'S':
      freeze();
    }
}

void forward(){
  
    digitalWrite(in1, LOW);
    digitalWrite(in2, HIGH);
    digitalWrite(in3, LOW);
    digitalWrite(in4, HIGH);

  }

moves robot left
void left(){

    changes directions of motors
    digitalWrite(in1, LOW);
    digitalWrite(in2, HIGH);
    digitalWrite(in3, HIGH);
    digitalWrite(in4, LOW);
  }

void right(){

    changes directions of motors
    digitalWrite(in1, HIGH);
    digitalWrite(in2, LOW);
    digitalWrite(in3, LOW);
    digitalWrite(in4, HIGH);

  }

void back(){

    changes directions of motors
    digitalWrite(in1, HIGH);
    digitalWrite(in2, LOW);
    digitalWrite(in3, HIGH);
    digitalWrite(in4, LOW);

  }


void freeze(){

    analogWrite(in1, 0);
    analogWrite(in2, 0);
    analogWrite(in3, 0);
    analogWrite(in4, 0);

  }

  void forwa(){

    analogWrite(in1, 0);
    analogWrite(in2, chat);
    analogWrite(in3,0);
    analogWrite(in4, chat);
  }

  void backwards(){
    analogWrite(in1, chat);
    analogWrite(in2, 0);
    analogWrite(in3,chat);
    analogWrite(in4, 0);
  }
```

```c++
#include <Wire.h>

#define MPU6050_ADDRESS 0x68

int16_t accelerometerX, accelerometerY, accelerometerZ;

void setup() {
  Wire.begin();
  Serial1.begin(38400);

  // Initialize MPU6050
  Wire.beginTransmission(MPU6050_ADDRESS);
  Wire.write(0x6B);  
  Wire.write(0);     
  Wire.endTransmission(true);

  delay(100); 
}

void loop() {
  readAccelerometerData();
  determineGesture();
  delay(500);
}

void readAccelerometerData()
{
  Wire.beginTransmission(MPU6050_ADDRESS);
  Wire.write(0x3B);  
  Wire.endTransmission(false);
  Wire.requestFrom(MPU6050_ADDRESS, 6, true);  


  accelerometerX = Wire.read() << 8 | Wire.read();
  accelerometerY = Wire.read() << 8 | Wire.read();
  accelerometerZ = Wire.read() << 8 | Wire.read();
}

void determineGesture()
{
  if (accelerometerY >= 6500) {
    Serial1.write('F');
  }
  else if (accelerometerY >=3000 && accelerometerY < 6500)  {
    Serial1.write('A');
  }
  else if (accelerometerY <= -5000) {
    Serial1.write('B');
  }
  else if (accelerometerY <= -3000 && accelerometerY > -5000)  {
    Serial1.write('C');
  }
  else if (accelerometerX <= -3250) {
    Serial1.write('L');
  }
  else if (accelerometerX >= 3250) {
    Serial1.write('R');
  }
  else {
    Serial1.write('S');
  }
}
```

# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Car Chassis | Body/chassis of the car | $21.29 | <a href="https://www.amazon.com/perseids-Chassis-Encoder-Wheels-Battery/dp/B07DNXBFQN/ref=sr_1_4?crid=348HVQ5PPSES0&dib=eyJ2IjoiMSJ9.VntL9cwD4xLY13joNQYKF1T6vUldYo_0gfQFa4z3cDJE_2TYCo8zJ29kWzzTXPnBc4qslFcgs7NW7jcXShI2SVhbzqKiad1oN0Iyk1IxNP_7fVnbg5bSgAUCAKKYk45h-DqqlJpMZcqmHEtbHKw3e0w9CC3IE3EVGE471oZpz2jQAeLv148cV-2_P9nX5_i1gG4Oj0Boc7kqkIm1ms4u3JIUn86dM6RpeViGCGGu0T6Wj0L5hog84gmyMPQ7V67d-nsKizgdsvNSlZPokZ6zLoI8Ksdp0pksiKRuPvq7-bs.sKXjpZLowxu9xTTevjf-ACK0fRUW70i6WNwB_1CbmJE&dib_tag=se&keywords=car%2Bchassis%2Bkit&qid=1719589624&sprefix=car%2Bchasis%2Caps%2C95&sr=8-4&th=1"> Link </a> |
| Screwdriver Kit | Used to hold the chassis in place and keep it stable. | $6.99 | <a href="https://www.amazon.com/Small-Screwdriver-Set-Mini-Magnetic/dp/B08RYXKJW9/ref=sr_1_5?crid=3UQ4PUUX9XB11&dib=eyJ2IjoiMSJ9.CHj85vaJsLJ3y3wqdxlHS7f7qwKDEN8LJXSanrO2TLrT14nbU3cGo760_IzBOJ4sauFcZlWg0APr4Iu0gUlWYI6LVj5bS7fZ44UggZdgDq0jRo_Bp57DhdweR2FHoBKDm3QzULpKF1qs_jXwV0tbYNe7cg6QkydMXFHrWLA9MhtJOrZX8nLf38SCb0uFyEPROlkh48IJA5l4ALScNiXmmCpVw8Fs5M1hdXkQ5kmul264uNd5xOhhKMfuJr5RtFuV_9ICRLIUDy9JPXbJ7G-YFb_aKvlneFLx66T1MXM7abQ.s72mZEYs_gSPrgXe8f3zFx4f3wDmUQ9yGfsyLVeU4q8&dib_tag=se&keywords=screwdriver+tkiszyzr&qid=1719589774&sprefix=screwdriver+tkiszyzr%2Caps%2C71&sr=8-5"> Link </a> |
| H-bridges | The medium which connects the motors to the Uno. | $5.99 | <a href="https://www.amazon.com/Ferwooh-Stepper-Controller-2-5-12V-H-Bridge/dp/B0D17PJ2MS/ref=sr_1_6?crid=VCW87Q6UYZ6W&dib=eyJ2IjoiMSJ9.oVeU6ARHBmpVYWuYlpEBY6YZ-BtAfatSyL7iNs5H-aIZGt3QEVGHZks5UfwEYqzbDKxbASP9Bhp303ywKhwkDEKf79jWTPx-09eRituY6KFHqjIrpl_f0NnPZTFBK2uvIlZkJ14mfZIan6y5-qL16qkAEY01XuW0IaNTEzDW7ac5MeudaDiEYnBpzcNPgIWf4F0T_WXDY4XCSeVYHHC5reoAAIP_i0IbueB58J9lRWOQX8xjfTPOleYThl0SpeDpD0JP7r3Cogwh4n6qYTn9TO3fZS9twfFrfj1oKBIlp6g.VANeEdMy_hrJ3sIUcgXaQfTns2RplwmdUHY0eesGKzA&dib_tag=se&keywords=h-bridges&qid=1719589816&sprefix=h-bridge%2Caps%2C113&sr=8-6"> Link </a> |
| Arduino Uno | Main processing chip on the car. | $16.99 | <a href="https://www.amazon.com/ELEGOO-Board-ATmega328P-ATMEGA16U2-Compliant/dp/B01EWOE0UU/ref=sr_1_1_sspa?crid=2OAAIAX6L0Z9Y&dib=eyJ2IjoiMSJ9.MazmhFfn-DF8W5oyX_S-tH7qkt_WuogERq_8M3-FTf7RaTJjfJhh9u9oYVUHTxDJdG8N9is8TTwxW20GIE8LczwI6KZ31Wl9Q9sUzftAdAS_JXJUYut0DfO9fXrtt6Kqpz30HA8JxYBaYYXIJaSt5CfO6bKobr775WrdREoHMlOxziTkZehXCybWkUUlSfpETVXVjA-bp1q5nJIBjxmKLCmMpL90LP7uEFFai_vPBYI.fexyVOBQUSRfgnGOzwpGH2wizUh0nzVlChZiXDBJXBU&dib_tag=se&keywords=arduino+uno&qid=1719589853&sprefix=arduino+uno%2Caps%2C95&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
| DC motors | Moves the car in all the directions. | $10.99 | <a href="https://www.amazon.com/AEDIKO-Motor-Gearbox-200RPM-Ratio/dp/B09N6NXP4H/ref=sr_1_28?crid=5ZUA8Z6PDXLK&dib=eyJ2IjoiMSJ9.CSJDjH66qmOrpmCUFu7sYHh2Reals-GxcZ-f1mxV2O0ariLQtUowh4msQiGoarHw3k_mDuKh2_pCbYh8O-iFA3dm3luQBIH2vfQsM4ZFSSHoLE-g8Vur4gGVN4KCPcZf30qd6ALrJynQeN_QRl60BbewATO3dPPoBXu6Xi89yPO4P88tIUt48hORJPo1UdtGYcEIm7jiKFBlq_SAngGXgUl3xPZqmEL9KjG-Po1Xiw6tpDyAEJpcNwPfULeKIy9IBKEPJCwXqTBci6z8RJ29_ZpzryplTZ9e4PGAR_UN8O8.NxzQnbAH9n6ek15-_WqDkUkSUFO2xbiQsgYd1uX4BUI&dib_tag=se&keywords=dc+motors&qid=1719589884&sprefix=dc+motor%2Caps%2C101&sr=8-28"> Link </a> |
| Breadboards | The medium which provides more space to work with the wiring and components. | $8.99 | <a href="https://www.amazon.com/HUAREW-Breadboard-Wires%EF%BC%8CBattery-Clip%EF%BC%8C830-tie-Points/dp/B09TX9CMG1/ref=sr_1_2_sspa?crid=23VRUO1DSUGH8&dib=eyJ2IjoiMSJ9.DdJjDkyEnOjQaaAj5ktxWFm_TUF4xvmuoanBtMBfP8HT-EZ4k9HRn3k8KT6B3npBQTlSRpx6ITpM2RtIDFWOCsTGARWg-qd_O8O7ZdvwEb3Anb9yfsk3B1fFnyd37CAXExVySSCkzTd8RHqRcZIXRVCUU9DrIASkZVe5Ztml49IJB2EjlSyPMlUkJqSDyu2Ovn3rBjDhUSMeMBuU7BlDu0aYAep_zmZnT_w7HO-jguY.PGZdhuPxLqjM2veggYMgdQsE1Y0Lb5ExkOOTH0JRdXk&dib_tag=se&keywords=breadboard+kit&qid=1719590017&sprefix=breadboard%2Caps%2C80&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1"> Link </a> |
| Arduino Micro | Reads the accelerometer data and sends it to the bluetooth modules. | $22.48 | <a href="https://www.amazon.com/Arduino-Micro-Headers-A000053-Controller/dp/B00AFY2S56/ref=sr_1_3?crid=1FEL6VLKLWJRP&dib=eyJ2IjoiMSJ9.1eon0cemkAxhQ7xR0HijIk0krSQGulAWC3oUqHFbp5lLW3xtQWRAKaaFQ-Oh6YcJvdK721z9zcE8zzkJnU-n2sr9tx3GlTPh2GgV4nGyGqg7vMOpEnVdVkJs6erGzYKO8ca-PJ90VugcoTsARDN7HPAv60DjN-Y1Rvy6W3cr9TsgrFtdsUONdIKo3zZzUzLWc9eV0HZilWQ60s2qDhHp14kNf8seGHgtoQDWhCYLLAE.VTzdGjSyLt0MWNGaaTbnUgu0u_LxR0i0TG1a1FzAoYg&dib_tag=se&keywords=Arduino+micro&qid=1719592926&sprefix=arduino+micro%2Caps%2C110&sr=8-3"> Link </a> |
| Micro USB cable | Power supply cable. | $5.49 | <a href="https://www.amazon.com/Charging-Transfer-Android-Trustable-MYFON/dp/B098DW7485/ref=sr_1_3?crid=39W27B7LKMAC2&dib=eyJ2IjoiMSJ9.0QoPk3Ik_wg6yuVlO__LmRO0k__oDr7SJI6NudyX1v-3q0uQDxfR7g2m7FMylaJYTEsreEt4v6uqO892PGeLg-OD85_hE5FfZA8M1C45QpK5AJe7sOlClKYGM0z5HGiKDUWqnm6JgIZ0s-rcovnj7KLNAAGqUSNKBr3lENAIQDPkknda_zy2ZBLinyUZl6szV-QaxRazBoPjXf2bag2YAlxl-q1W2oWgvVILO9BuT5w.842IuurQR_v04g4OGSeaghq6W--RwDhj_C2vRL0YLnc&dib_tag=se&keywords=micro+usb+cable&qid=1719592971&sprefix=micro+usb%2Caps%2C113&sr=8-3"> Link </a> |
| Accelerometers | Read the tilt you give. | $9.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/HiLetgo-MPU-6050-Accelerometer-Gyroscope-Converter/dp/B00LP25V1A/ref=sr_1_1_sspa?crid=363655KTUIGEU&dib=eyJ2IjoiMSJ9.PFxAcjN15dzv7m-hyOAdWNb9GRzkue-Rj2CKSzL6qDxVycHmKN2X7ZBOGcb2doUJh-xLW_AsFr7Skdz_qEKX6v9X5rGJ2TP7dDacu-NalY7WjYZ1z8A52YO6mc_GO3LPH1F4T1FovNp0e3SuTgj4L__jWpDUbtw1NDrpMk9btVeEzlMBjq8Bmi5KjW-16Hb3505lYo42ZzdOWS-PbQTSx_YktzadRk7EreZdyaUIXW8.afMMVBZJwc4MQALJ8Ey7-RNghxgmNhrISXGCgWqVnuY&dib_tag=se&keywords=accelerometer&qid=1719593021&sprefix=accel%2Caps%2C97&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)"> Link </a> |
| HC05s | Communicate with each other to help activate the code for the car.| $9.99 x 2 | <a href="https://www.amazon.com/DSD-TECH-HC-05-Pass-through-Communication/dp/B01G9KSAF6/ref=sr_1_3?crid=118ADC2GKD9LU&dib=eyJ2IjoiMSJ9.GVe7xTdQBd8ycP5WU8ZbiQa5ABtI2bM6FlQhDvE7qEfVz0Nu_seyhu0n8hd8O9KKgNBiuy5K8dhf2ydzaEHzOwx75sh--DXUdfk5KcuFOqpszU-hcbrU0_O04BPyQUi8V1V2cPNevFvlUgVLo6zncBEUD6GQBxsGDeuYkhRfxRX6QhpEjahHhXRt86QKQTE9SDCtjEhxcgtfyIWt7FQjZQs8VRTH6M3rrWV7OGpXA9Q.shWtkpPlSE3WYWMgKvg9crApuaxbAH7DVZ_evYxtiEU&dib_tag=se&keywords=hc05+bluetooth+module&qid=1719593053&sprefix=HC05%5C%2Caps%2C93&sr=8-3#customerReviews"> Link </a> |
| 9-volt batteries | Provide power for the car remotely. | $8.88 | <a href="https://www.amazon.com/PKCELL-Maximum-Long-Lasting-Leak-Proof-Detectors/dp/B00ZTS55Y4/ref=sr_1_28?crid=3QRISKGHCI89T&dib=eyJ2IjoiMSJ9.WmcGs4Gk1qJA6k6D_ITRnTt2w_rf5rDm6k1ftzEUlm2IMzGvkoGOk_h4BWSALsT3DTsdG1-5vN_qkB_6_5E6okbysRwGxxmQffy6ShVm8i3TCE8nemw7as5iGiC4GdjuTkQ_BQusP6kctWNfV8nXKRj1zWXUiT8o8Unq0FMH3QPBR7dZ3NaGZvRJbiLvLq_F0fCUf-lfy9po_9E2ksKP6sxtuQxZYt1hDRXA6E3El4f0U_gbYi7lig7aODD_X38q7QvZvL9nfM_t977yLO62qb9pZ2T7t14p_jKPf__qKbc.2J-O8G4wzVCMHqjG4Iy5MpzLmQhK8bCB6xMxChZ5VSU&dib_tag=se&keywords=9-volt%2Bbatteries&qid=1719593095&sprefix=9-volt%2Caps%2C123&sr=8-28&th=1"> Link </a> |
| Digital Multi-Meter | Used to check if wiring/power is flowing properly through.  | $12.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/AstroAI-Digital-Multimeter-Voltage-Tester/dp/B01ISAMUA6/ref=sr_1_1_sspa?crid=25K3TIBUL4DHN&dib=eyJ2IjoiMSJ9.K5PooihdKLVWllvdReFZjXvLAfkoX3uJiu5BsEai6nreENrjMGkL_6HwllLvGOZN4yShLw6N7btkO54cGrgXJqISGAIxkj5vcOQfzSPTRtNA_4yNlMJtNY-mdxa9YklOp46DwKadKI0r9nbqffU_-EU6apb0NkosfFmXZ1KDFjAmVCx9ytcuix581MxidiTtz9F5Z_ZYFabn2z-pzPgcYW_UbfqsuA1Df-pi6p7I1TKvkbnIovBkRg93NdeykI5Ut4wPoiKBrydSQysAB5sDMxp2_cUvDGOtEEw-hHVm_Lg.7L_tD1I9dGhRboVXlM0BVx6BUygc9A1igxVgi9gPbNs&dib_tag=se&keywords=digital+multimeter&qid=1719593154&sprefix=digital+%2Caps%2C106&sr=8-1-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
