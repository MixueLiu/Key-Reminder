# Key-Reminder
https://makecode.microbit.org/S43478-05678-02027-45375

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
