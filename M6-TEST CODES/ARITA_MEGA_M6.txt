#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <SPI.h>
#include <SD.h>

#define IN1 19
#define IN2 18
#define IN3 26
#define IN4 27
#define IN5 28
#define IN6 29
#define IN7 30
#define IN8 31
#define IN9 32
#define IN10 33
#define IN11 34
#define IN12 35
#define IN13 36
#define IN14 37

#define O1 4
#define O2 5
#define O3 2
#define O4 3
#define O5 6
#define O6 7
#define O7 8  
#define O8 9  
#define O9 53
#define O10 10
#define O11 11
#define O12 12

#define b1 22 // down
#define b2 23 // right
#define b3 24 // left
#define b4 25 // up

#define SCREEN_WIDTH 128 // OLED display width, in pixels
#define SCREEN_HEIGHT 64 // OLED display height, in pixels

// Declaration for an SSD1306 display connected to I2C (SDA, SCL pins)
#define OLED_RESET     40 // Reset pin # (or -1 if sharing Arduino reset pin)
Adafruit_SSD1306 display(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);


// SD NEEDS
#define SD_chipSelect 46
Sd2Card card;
SdVolume volume;
SdFile root;


bool but1 = 0, but2 = 0, but3 = 0, but4 = 0, blinknow = 0;

bool in[14] = {};
bool out[12] = {};

int8_t selec = 0, page = 0;

void pageSelector()
{
  if(page == 0) { display1(); }
  else if(page == 1) { display2(); }
  else if(page == 2) { display3(); }
  else if(page == 3) { display4(); }
}

void readBut()
{
  // b1 
  
  if(digitalRead(b1))
  {
    while(digitalRead(b1)) // Down
    {
      pageSelector();
    }
    but1 = 1; blinknow = 1;
  }
  else if(digitalRead(b2)) // right
  {
    while(digitalRead(b2))
    {
      pageSelector();
    }
    but2 = 1; blinknow = 1;
  }
  else if(digitalRead(b3)) // left
  {
    while(digitalRead(b3))
    {
      pageSelector();
    }
    but3 = 1; blinknow = 1;
  }
  else if(digitalRead(b4)) // up
  {
    while(digitalRead(b4))
    {
      pageSelector();
    }
    but4 = 1; blinknow = 1;
  }
}


void readInput()
{
  in[0] = !digitalRead(IN1);
  in[1] = !digitalRead(IN2);
  in[2] = !digitalRead(IN3);
  in[3] = !digitalRead(IN4);
  in[4] = !digitalRead(IN5);
  in[5] = !digitalRead(IN6);
  in[6] = !digitalRead(IN7);
  in[7] = !digitalRead(IN8);
  in[8] = !digitalRead(IN9);
  in[9] = !digitalRead(IN10);
  in[10] = !digitalRead(IN11);
  in[11] = !digitalRead(IN12);
  in[12] = !digitalRead(IN13);
  in[13] = !digitalRead(IN14);
}

void display1()
{
  display.clearDisplay();
  display.setTextSize(0);
  display.setCursor(0,0); 
  display.print("Standard Test Program");
  display.setCursor(0,15); display.print("  Inputs Page 1");
  display.setCursor(0, 25);
  display.print("I1: "); display.print((in[0]));
  display.print(" I2: "); display.print((in[1]));
  display.print(" I3: "); display.print((in[2]));
  display.setCursor(0, 35);
  display.print("I4: "); display.print((in[3])); 
  display.print(" I5: "); display.print((in[4])); 
  display.print(" I6: "); display.print((in[5]));
  display.setCursor(0, 45);
  display.print("I7: "); display.print((in[6]));
  display.print(" I8: "); display.print((in[7]));
  display.print(" I9: "); display.print((in[8]));

  display.setCursor(0,55); display.print("Menu = Next Page");

  display.display();
}

void display2()
{
  display.clearDisplay();
  display.setTextSize(0);
  display.setCursor(0,0); 
  display.print("Standard Test Program");
  display.setCursor(0,15); display.print("  Inputs Page 2");
  display.setCursor(0, 25);
  display.print("I10: "); display.print((in[9]));
  display.print(" I11: "); display.print((in[10]));
  display.print(" I12: "); display.print((in[11]));
  display.setCursor(0, 35);
  display.print("I13: "); display.print((in[12])); 
  display.print(" I14: "); display.print((in[13])); 
  
  display.setCursor(0,55); display.print("Menu = Next Page");
  
  display.display();
}

void display3()
{
  display.clearDisplay();
  display.setTextSize(0);
  display.setCursor(0,0); 
  display.println("Standard Test Program");
  display.setCursor(0,15); display.print("  Outputs Page 1");
  
  display.setCursor(0, 25);
  display.print("O1: "); if(selec == 0 && blinknow) { display.print("_"); } else { display.print(out[0]); }
  display.print(" O2: "); if(selec == 1 && blinknow) { display.print("_"); } else { display.print(out[1]); }
  display.print(" O3: "); if(selec == 2 && blinknow) { display.print("_"); } else { display.print(out[2]); }
  display.setCursor(0,35);
  display.print("O4: "); if(selec == 3 && blinknow) { display.print("_"); } else { display.print(out[3]); }
  display.print(" O5: "); if(selec == 4 && blinknow) { display.print("_"); } else { display.print(out[4]); }
  display.print(" O6: "); if(selec == 5 && blinknow) { display.print("_"); } else { display.print(out[5]); }
  display.setCursor(0, 45);
  display.print("O7: "); if(selec == 6 && blinknow) { display.print("_"); } else { display.print(out[6]); }
  display.print(" O8: "); if(selec == 7 && blinknow) { display.print("_"); } else { display.print(out[7]); }
  display.print(" O9: "); if(selec == 8 && blinknow) { display.print("_"); } else { display.print(out[8]); }

  display.setCursor(0,55);
  if (selec == 9) { display.print("Menu = Next Page"); } else { display.print("Menu = On Exit = Off"); }

  digitalWrite(O1, out[0]);
  digitalWrite(O2, out[1]);
  digitalWrite(O3, out[2]);
  digitalWrite(O4, out[3]);
  digitalWrite(O5, out[4]);
  digitalWrite(O6, out[5]);
  digitalWrite(O7, out[6]);
  digitalWrite(O8, out[7]);
  digitalWrite(O9, out[8]);

  display.display();
}

void display4()
{
  display.clearDisplay();
  display.setTextSize(0);
  display.setCursor(0,0); 
  display.println("Standard Test Program");
  display.setCursor(0,15); display.print("  Outputs Page 2");
  
  display.setCursor(0, 35);
  display.print("O10: "); if(selec == 0 && blinknow) { display.print("_"); } else { display.print(out[9]); }
  display.print(" O11: "); if(selec == 1 && blinknow) { display.print("_"); } else { display.print(out[10]); }
  display.print(" O12: "); if(selec == 2 && blinknow) { display.print("_"); } else { display.print(out[11]); }
  
  display.setCursor(0,55);
  if (selec == 3) { display.print("Menu = Next Page"); } else { display.print("Menu = On Exit = Off"); }

  digitalWrite(O10, out[9]);
  digitalWrite(O11, out[10]);
  digitalWrite(O12, out[11]);

  display.display();
}

void runCheck()
{
  if(page == 2)
  {
    readBut();
    if(!but1 && !but2 && !but3 && but4) // up
    {
      selec++; 
      if(selec > 9) { selec = 0; }
      but4 = 0;
    }
    else if(but1 && !but2 && !but3 && !but4) // down
    {
      selec--;
      if(selec < 0) { selec = 9; }
      but1 = 0;
    }
    else if(!but1 && but2 && !but3 && !but4) // right
    {
      out[selec] = 0;
      but2 = 0;
    }
    else if(!but1 && !but2 && but3 && !but4) // left
    {
      if(selec == 9) { page = 3; selec = 0; }
      else
      {
        out[selec] = 1;
      }
      but3 = 0;
    }
  }
  else if(page == 3)
  {
    readBut();
    if(!but1 && !but2 && !but3 && but4) // up
    {
      selec++; 
      if(selec > 3) { selec = 0; }
      but4 = 0;
    }
    else if(but1 && !but2 && !but3 && !but4) // down
    {
      selec--;
      if(selec < 0) { selec = 3; }
      but1 = 0;
    }
    else if(!but1 && but2 && !but3 && !but4) // right
    {
      out[selec + 9] = 0;
      but2 = 0;
    }
    else if(!but1 && !but2 && but3 && !but4) // left
    {
      if(selec == 3) { page = 0; selec = 0; }
      else
      {
        out[selec + 9] = 1;
      }
      but3 = 0;
    }
  }
  else
  {
    readInput();
    readBut();
    if(!but1 && !but2 && but3 && !but4) // left
    {
      page++; selec = 0;
      but3 = 0;
    }
    else
    {
      but1 = 0; but2 = 0; but4 = 0;
    }
  }
  

  pageSelector();
}






void setup() 
{
  //Inputs
  pinMode(IN1, INPUT);  pinMode(IN2, INPUT);  pinMode(IN3, INPUT);  pinMode(IN4, INPUT);
  pinMode(IN5, INPUT);  pinMode(IN6, INPUT);  pinMode(IN7, INPUT);  pinMode(IN8, INPUT);
  pinMode(IN9, INPUT);    pinMode(IN10, INPUT);    pinMode(IN11, INPUT);    pinMode(IN12, INPUT);
  pinMode(IN13, INPUT);   pinMode(IN14, INPUT); 
  //Relay
  pinMode(O1, OUTPUT);  pinMode(O2, OUTPUT);  pinMode(O3, OUTPUT);  pinMode(O4, OUTPUT);
  pinMode(O5, OUTPUT);  pinMode(O6, OUTPUT);  pinMode(O7, OUTPUT);  pinMode(O8, OUTPUT);
  pinMode(O9, OUTPUT);  pinMode(O10, OUTPUT);  
  // Transistors
  pinMode(O11, OUTPUT);  pinMode(O12, OUTPUT);

  digitalWrite(O1, LOW);  digitalWrite(O2, LOW);  digitalWrite(O3, LOW);  digitalWrite(O4, LOW);
  digitalWrite(O5, LOW);  digitalWrite(O6, LOW);  digitalWrite(O7, LOW);  digitalWrite(O8, LOW);
  digitalWrite(O9, LOW);  digitalWrite(O10, LOW);  
  //Transistors
  digitalWrite(O11, LOW);  digitalWrite(O12, LOW);

  pinMode(b1, INPUT); pinMode(b2, INPUT); pinMode(b3, INPUT); pinMode(b4, INPUT);

    pinMode(42,OUTPUT);
    delay(100);
    digitalWrite(42,HIGH);
    
  display.begin(SSD1306_SWITCHCAPVCC, 0x3C);  // initialize with the I2C addr 0x3D (for the 128x64)
  display.clearDisplay();
  // dispaly logo
  display.setTextSize(2);
  display.setTextColor(WHITE);
  display.setCursor(0,1);
  display.print("ICONIC");
  display.setCursor(0,25);
  display.setTextSize(1);
  display.println("NORVI CONTROLLER");
  display.setCursor(0,35); display.println("Standard V4.0");
  display.setCursor(0,45); display.println("TEST PROGRAM");
  display.setCursor(0,55); display.println("14 DI / 10 RL / 2 TR");
  display.display();
  // Clear the buffer.
  display.clearDisplay();
  display.setTextSize(0);

  digitalWrite(O1, LOW);  digitalWrite(O2, LOW);  digitalWrite(O3, LOW);  digitalWrite(O4, LOW);
  digitalWrite(O5, LOW);  digitalWrite(O6, LOW);  digitalWrite(O7, LOW);  digitalWrite(O8, LOW);
  digitalWrite(O9, LOW);  digitalWrite(O10, LOW);  
  //Transistors
  digitalWrite(O11, LOW);  digitalWrite(O12, LOW);
  
  Serial.begin(9600);
  Serial3.begin(9600);

  delay(100);
    Serial3.println("RS-485 Sending");


  delay(2000);
  

   Serial.print("\nInitializing SD card...");
  if (!card.init(SPI_HALF_SPEED, SD_chipSelect)) {   Serial.println(" CARD NOT FOUND");  }
  else { Serial.println(" SD WORKING"); }
  Serial.print("\nCard type: ");
  Serial.println(card.type());
   if (!volume.init(card)) {
    Serial.println("Could not find FAT16/FAT32 partition.\nMake sure you've formatted the card");
    return;
  }
  if (!SD.begin(SD_chipSelect)) {
    Serial.println("Card failed, or not present");
    // don't do anything more:
    return;
  } 
  Serial.println("card initialized.");



  //set timer1 interrupt at 1Hz
  TCCR1A = 0;// set entire TCCR1A register to 0
  TCCR1B = 0;// same for TCCR1B
  TCNT1  = 0;//initialize counter value to 0
  // set compare match register for 1hz increments
  OCR1A = 15624;// = (16*10^6) / (1*1024) - 1 (must be <65536)
  // turn on CTC mode
  TCCR1B |= (1 << WGM12);
  // Set CS12 and CS10 bits for 1024 prescaler
  TCCR1B |= (1 << CS12) | (1 << CS10);  
  // enable timer compare interrupt
  TIMSK1 |= (1 << OCIE1A);

  

}

void loop() 
{
  runCheck();

//  Serial.print("Inputs: "); 
//  Serial.print(digitalRead(IN1));
//  Serial.print(digitalRead(IN2));
//  Serial.print(digitalRead(IN3));
//  Serial.print(digitalRead(IN4));
//  Serial.print(digitalRead(IN5));
//  Serial.print(digitalRead(IN6));
//  Serial.print(digitalRead(IN7));
//  Serial.println(digitalRead(IN8));
//
//  Serial.print("buttons: "); 
//  Serial.print(digitalRead(b1));
//  Serial.print(digitalRead(b2));
//  Serial.print(digitalRead(b3));
//  Serial.print(digitalRead(b4));
}

ISR(TIMER1_COMPA_vect){
  blinknow = blinknow^1;
}
