<img width="5712" height="4284" alt="IMG_0107" src="https://github.com/user-attachments/assets/d67d8aeb-6574-4b59-8ece-caa8e778fec6" /># Key-Reminder
https://makecode.microbit.org/S82653-45067-87413-86590
# General Information
* Project Name: KEY Reminder
* Problem: I often forget to take my keys when leaving home.
* Purpose: To remind people to take their keys before going out.
* How it works: An ultrasonic sensor detects when someone approaches the door. If the distance is greater than 0 and less than 50 cm, the micro:bit displays “KEY” and plays a reminder melody.
  ## Features

- Distance Detection: An ultrasonic sensor measures the distance of nearby objects.

- Automatic Reminder: When the distance is greater than 0 cm and less than 50 cm, the micro:bit displays "KEY" and plays a reminder melody.

- Manual Reminder: Pressing button A displays "KEY" and plays a melody three times.

- Stop Button: Pressing button B stops sounds and clears the LED screen.

- Prevent Repeated Reminders: A variable called "already_reminded" prevents the automatic reminder from repeating while an object remains within the detection range.
- ## Setup

1. Open Microsoft MakeCode for micro:bit.
2. Import the KEY Reminder project file.
3. Connect the micro:bit to a breadboard.
4. Connect the ultrasonic sensor to the micro:bit.
5. Connect the buzzer.
6. Connect the battery pack to power the micro:bit.
7. Download the program to the micro:bit.
8. ## Usage

1. Place the KEY Reminder near the door.
2. When someone comes within 50 cm of the ultrasonic sensor, the micro:bit displays "KEY" and plays a melody.
3. Press button A to activate the reminder manually.
4. Press button B to stop the sound and clear the screen.
5. Move away from the sensor to reset the automatic reminder.
6. ## Project Status

The project is complete.

The final version (V4) uses an ultrasonic sensor to detect distance and automatically remind users to take their keys.

The project has been tested using a physical micro:bit.
## Room for Improvement

- Improve the accuracy of the ultrasonic sensor.
- Add a door sensor to detect when someone is leaving home.
- Create a protective case for the micro:bit and other components.
- ## Acknowledgements

I would like to thank Andrew and Anna for their guidance and support during the development of this project.

I would also like to thank Nidong and Xingle Pan for their feedback and suggestions.
 ## Python
reminde0n = 0
already_reminded = 0
distance = 0

def on_button_pressed_a():
    global reminde0n
    reminde0n = 1
    for index in range(3):
        if reminde0n == 1:
            basic.show_string("KEY")
            music.play(music.string_playable("C5 B A G F E D C ", 120),
                music.PlaybackMode.UNTIL_DONE)
            basic.pause(500)
input.on_button_pressed(Button.A, on_button_pressed_a)

def on_button_pressed_b():
    global reminde0n, already_reminded
    reminde0n = 0
    already_reminded = 1
    music.stop_all_sounds()
    basic.clear_screen()
input.on_button_pressed(Button.B, on_button_pressed_b)

def on_forever():
    global distance, already_reminded
    distance = sonar.ping(DigitalPin.P0, DigitalPin.P1, PingUnit.CENTIMETERS)
    serial.write_value("distance", distance)
    if distance < 100 and distance > 0:
        if already_reminded == 0:
            basic.show_string("KEY")
            music.play(music.string_playable("C5 B A G F E D C ", 120),
                music.PlaybackMode.UNTIL_DONE)
            already_reminded = 1
    else:
        already_reminded = 0
basic.forever(on_forever)

