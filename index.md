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
| Car Chassis | Body/chassis of the car | $Price | <a href="https://www.amazon.com/perseids-Chassis-Encoder-Wheels-Battery/dp/B07DNXBFQN/ref=sr_1_4?crid=348HVQ5PPSES0&dib=eyJ2IjoiMSJ9.VntL9cwD4xLY13joNQYKF1T6vUldYo_0gfQFa4z3cDJE_2TYCo8zJ29kWzzTXPnBc4qslFcgs7NW7jcXShI2SVhbzqKiad1oN0Iyk1IxNP_7fVnbg5bSgAUCAKKYk45h-DqqlJpMZcqmHEtbHKw3e0w9CC3IE3EVGE471oZpz2jQAeLv148cV-2_P9nX5_i1gG4Oj0Boc7kqkIm1ms4u3JIUn86dM6RpeViGCGGu0T6Wj0L5hog84gmyMPQ7V67d-nsKizgdsvNSlZPokZ6zLoI8Ksdp0pksiKRuPvq7-bs.sKXjpZLowxu9xTTevjf-ACK0fRUW70i6WNwB_1CbmJE&dib_tag=se&keywords=car%2Bchassis%2Bkit&qid=1719589624&sprefix=car%2Bchasis%2Caps%2C95&sr=8-4&th=1"> Link </a> |
| Screwdriver Kit | Used to hold the chassis in place and keep it stable. | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| H-bridges | The medium which connects the motors to the Uno. | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Arduino Uno | Main processing chip on the car. | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| DC motors | Moves the car in all the directions. | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| electronic components kit | Supplied me the necessary wires. | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Breadboards | The medium which provides more space to work with the wiring and components. | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Arduino Micro | Reads the accelerometer data and sends it to the bluetooth modules. | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Micro USB cable | Power supply cable. | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Accelerometers | Read the tilt you give. | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| HC05s | Communicate with each other to help activate the code for the car.| $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| 9-volt batteries | Provide power for the car remotely. | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Digital Multi-Meter | Used to check if wiring/power is flowing properly through.  | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
