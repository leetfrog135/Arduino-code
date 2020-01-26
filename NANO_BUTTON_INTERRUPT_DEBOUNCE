#include <Arduino.h>
#include <Servo.h>

const uint8_t ALARM = 10;
const uint8_t button = 2;
volatile bool LED_STATE = false;
volatile bool enable_button = true;
Servo myservo;

uint32_t sysTick = 0;
uint32_t prevTick = 0;
volatile uint8_t counts = 0;
volatile uint32_t button_debounce_timer = 0;
volatile uint32_t prev_button_timer = 0;

void HANDLE_BUTTON_ISR();

void setup()
{
  pinMode(LED_BUILTIN, OUTPUT);
  pinMode(ALARM,OUTPUT);
  pinMode(button,INPUT);
  Serial.begin(9600);
  //decalre button as a software interrupt
  attachInterrupt(digitalPinToInterrupt(button),HANDLE_BUTTON_ISR,RISING);//recommended for arduino board
}

void loop()
{
  sysTick = millis();
  //do other code here
  if (sysTick - prevTick >= 100)
  {
    prevTick = sysTick;
    Serial.println(counts);
  }
}

//interrupt routine to handle the button press
void HANDLE_BUTTON_ISR()
{
  button_debounce_timer = millis();

  if (enable_button == false)
  {
    //need to check if the button can be allowed to be hit again
    if (button_debounce_timer - prev_button_timer >= 200)
    {
      enable_button = true;
    }
  }

  //button has been pressed so enable whatever function the button is to serve and then debounce
  if (enable_button == true)
  {
    enable_button = false; //this check needs to be reset after 50mS
    prev_button_timer = button_debounce_timer;
    //button is pressed so enable this and wait for 50mS until it can be pressed again
    LED_STATE = !(LED_STATE);
    digitalWrite(LED_BUILTIN, LED_STATE);
    counts++;
  }
}

//NOTES:
//for button debounce, once button has been pressed, ignore new presses until 50mS has elapsed
