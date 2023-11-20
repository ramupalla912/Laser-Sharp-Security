# Source Code For Laser-Sharp-Security(C lanuage)
    #include <LiquidCrystal_I2C.h>
    LiquidCrystal_I2C lcd(0x27, 16, 2);
    #include <SoftwareSerial.h>
    SoftwareSerial mySerial(9, 10);
    
    int buzzer = 13;
    int lightA0 = A0;
    int frequency;
    int call;
    
    void setup() {
      lcd.init();                 // initialize the lcd
      lcd.backlight();
      mySerial.begin(9600);
      
      Serial.begin(9600);
      pinMode(buzzer, OUTPUT);
     /* lcd.setCursor(3, 0);
      lcd.print("Welcome to");
      lcd.setCursor(1, 1);
      lcd.print("Infinite Xpro");
      delay(1000);*/
    }
    
    void loop() {
      int analogSensor = analogRead(lightA0);
      frequency = (analogSensor - 10) / 10;
    
      /*lcd.setCursor(0, 0);
      lcd.print(" Alert Level:");
      lcd.setCursor(10, 0);
      lcd.print(frequency);
      lcd.setCursor(12, 0);
      lcd.print("%");*/
    
      // Checks if it has reached the threshold value
      if ( frequency >= 50) {
        initializeGSM();
        SendTextMessage();
        

      lcd.setCursor(0, 1);
      lcd.print("DANGER");
      tone(buzzer, 1000, 2000);//(X=?,Y=GLOW TIME)
      } else {
        lcd.setCursor(0, 1);
        lcd.print("NORMAL");
        noTone(buzzer);
      }
      delay(500);
      lcd.clear();
    }
    
    void SendTextMessage() {
      mySerial.println("AT+CMGF=1");                    // To send SMS in Text Mode
      delay(1000);
      mySerial.println("AT+CMGS=\"+917997503142\"\r"); // Change to the phone number you are using
      delay(1000);
      mySerial.println("NEW ALLERT!");               // The content of the message
      
      delay(1000);  
      mySerial.println((char)26);                       // The stopping character
      delay(1000);                                      // Increase the delay to ensure successful SMS sending
    }
    void makePhoneCall() {
      mySerial.println("ATD7997503142;");  // Replace with the phone number you want to call
      delay(1000);  // Wait for 10 seconds or adjust as needed
      mySerial.println("ATH");  // Hang up the call
    }
    void initializeGSM() {
      mySerial.println("AT");
      delay(1000);
    
      mySerial.println("AT+CMGF=1");
      delay(1000);
    
      mySerial.println("AT+CNMI=2,2,0,0,0");
      delay(1000);
    }







