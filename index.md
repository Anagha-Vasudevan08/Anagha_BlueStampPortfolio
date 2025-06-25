# Fingerprint ID Safe with Keypad
My poject is the Fingerprint ID safe with a keypad. This is a safe box with a keypad sensor and fingerprint sensor to unlock it. After a person puts both the correct password and correct fingerprint in under 5 attempts total, the box will open. 
You should comment out all portions of your portfolio that you have not completed yet, as well as any instructions:
```HTML 

```

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Anagha V | Leigh High School | Electrical and Computer Engineering | Incoming Senior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



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

My first milestone was to wire up all my sensors to my arduino and breadboard. Also to start my coding for the keypad by printing out different things when I input something into the keypad. The sensors I wired to my arduino and breadboard was an on and off switch, fingerprint sensor, 3 by 4 keypad, and servo motor. I first wired up all the black wires to ground and red wires to 5V. Then i wired the other wires to different pins in the arduino. My on and off button takes less voltage so I wired it to the breadboard and used a resister to make sure it was accepting the right amount of power. I finished all of the wiring part so now I just have to focus on a lot of the code to integrate all the sensors together and building the actual box. 

# Challenges
I faced challenges with both the code and wiring for my first milestone. When I first was wiring up all my sensors I tested each of the indivudual sensors to make sure they work before I started on whole code. My servo motor wasn't moving at all but it did vibrate a bit. I tested it through different pins and a new arduino and it still didn't work. After this instead of using my computer as a power source I used a battery pack as well as a 9V pack which both didnt work. My last option that I did was using a 9V adapter and plugging it into the wall which did work. This was because the power from the other sourcfe was only enough to make the servo vibrate a bit not spin, the power from the wall was enough to make it spin though. Another challenge I faced was my keypad code wasn't working even though it theoretically should have. When I went to seriel moniter on arduino I realized that since the code was in void loop it wasn't waiting for me to put an input to start running the code it just kept running the loop evgen without keys pressed which caused the keypad not to work. To fix this I used if(key != NO_KEY); which means and char key = keypad.getKEY();. So this means that if no key is not presssed then it waits to press and adds input += key which means it waits for 5 characters total based on my other code to print something out. 

# Plan
My plan to complete my project it 

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
// this sketch will allow you to bypass the Atmega chip
// and connect the fingerprint sensor directly to the USB/Serial
// chip converter.

// Red connects to +5V
// Black connects to Ground
// White goes to Digital 0
// Green goes to Digital 1

#include <Servo.h>
#include <Keypad.h>
#include <Adafruit_Fingerprint.h>


Servo servo;
String input = "";
String password = "#3575";

const byte ROWS = 4;
const byte COLS = 3;

char keys[ROWS][COLS] = 
{
{'1', '2', '3'},
{'4', '5', '6'},
{'7', '8', '9'},
{'*', '0', '#'}
};

byte rowsPins[ROWS] = {9, 8, 10, 11};
byte colsPin[COLS] = {5, 6, 7};
int attempts = 5;
int angle = 0;


Keypad keypad = Keypad(makeKeymap(keys), rowsPins, colsPin, ROWS, COLS);


void setup() {

Serial.begin(9600);
  servo.attach(3); 
  servo.write(0);


}
void loop() {


  

  char key = keypad.getKey();


  if (key != NO_KEY)
  {
    Serial.println("key pressed");

  Serial.print(key);

  if (key == '*') // if astericks, clear
  {
    input = "";

  }
  else{
  input += key;
  }
    Serial.print("Current input: ");
    Serial.println(input);



  if (input.length() > 1 and key == "#")
  {
    Serial.println("Start with '#' before entering passsword");
    input = "";
  }

  if (input.length() == 5){
      Serial.print(input);
      if (input == password){
        // TODO : allow servo to move accordingly
        Serial.println("Cleared, enter fingerprint now");
        angle = 180;
        servo.write(angle);

      }
      else {
        Serial.print("Password is incorrect ");

        Serial.print(attempts);

        Serial.print(" attempts left");

        attempts  = attempts - 1;
        input = "";
    }
    }
    }


   // add code for fingerprint too
    
  }


```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Item Name | What the item is used for | $Price | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |

# Starter Project: Retro Game Soldering Kit
https://www.youtube.com/embed/Ftf7Nms5TLQ?si=cT8QTLggC5kSKzwF

# Summary:

I chose this retro game soldering kit because I wanted something I could use everyday. The way it works is there is a power on button which when you press it clicks down and completes a circuit which allows the game to power on. This is a precoded game so the main point is to build and use soldering to put the game together. I used a USB socket and connected it to an external power source to power the game, but you can also use a battery pack to power it. 


# Components Used:
- USB Socket (5V)
- Dot Matrix 16*8
- Screws and Nuts
- Digital Tube (Timer)
- Capacitance
- Keys
- Buzzer
- Power Switch
- Battery Box
- Wire
- Hex Copper Pillars
- Key Cap

# Challenges:
Some challenges I faced was soldering without melting the plastic portion of the board off. Also soldering the wires from the battery pack into the designated area because of how thin the wires were. Since I had trouble with the wiring of the batteries, I used the USB Socket to connect it to an external power source like a computer or a plug to power the game. Another challenge I faced was with the USB socket showing through the sides of the glass, I took the game apart soldered the USB Socket to the board and then put the sides together for it to work. 




# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
