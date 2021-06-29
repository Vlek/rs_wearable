# RuneScape wearable
Arduino-based RuneScape-focused wearable for staying logged in.

# Motivation and background:

## RuneScape Mechanics
One of RuneScape's main tasks is the leveling up of certain skills.
This task can take up great amounts of in-game currency as well as
time performing significantly repetitive tasks.

One such task is gaining melee skills: (attack, strength, defense,
and hitpoints). Training all of these can be done semi-afk in a
few different ways:
- [Bandits](https://www.youtube.com/watch?v=zR7bwdJlECI)
- [Sandcrabs](https://www.youtube.com/watch?v=R-o1qQ5iHbA)
- [Nightmare zone](https://www.youtube.com/watch?v=czTknAPFq9Y)

Even in Nightmare zone where the skill gain is considered to be
one of the highest available, it can still take up to 400 hours
in order to train them all.

## Anti-macroing Rules
The current rules state the following as per [an official post](https://secure.runescape.com/m=news/mouse-keys---changes--clarification?oldschool=1)
submitted by Jagex on their website:

```
Mouse Keys - Changes & Clarification
25 January 2017

In order to clearly distinguish between what is and is not allowed
in Old School RuneScape, we have decided to make a change to how we
enforce the macroing rule.

Historically, we have not given bans for some usage of programmable
mouse keys (such as AutoHotKey). If players kept their usage of such
software to an acceptable standard, we would not take action against
them. This is no longer the case.

You may now only use your operating system's official default mouse
keys program, unless it is to remap a key to any other button.

If we find evidence of you using other forms of mouse keys in game,
we will take action against your account.

This change in how we review macroing reports provides a much clearer
line between what is and is not acceptable. If you're using your systems
default mouse keys, you're playing within the rules.

Mods Archie, Ash, Ghost, Jed, John C, Kieren, Mat K, Maz, Merchant,
Ronan, Roq, TomH, Weath & West
The Old School Team
```

My interpretation hinges on the line:
```
You may now only use your operating system's official default mouse
keys program, unless it is to remap a key to any other button.
```

With this line, I take it to mean that a remapping of a key is allowed
even in third-party software that does not come pre-installed by the OS.

## Potential Hardware:
From previous projects, I had known of the [Pokemon Go Plus](https://www.techradar.com/reviews/wearables/pokemon-go-plus-1328758/review)
for a different game automation I was considering.

Features of the Pokemon GO Plus:
- Pairs with a device that is running the game client
- When it is the case that an in-game event happens, vibrates and
  flashes one of three colors to indicate what the event is.
- Allows users to press a button to respond to the event.

### Issues with using the Pokemon GO Plus:
Unfortunately, there is no easy-to-use API that's available for
connecting it to a PC and leveraging the button press, LEDs, or vibration
for purposes outside of the game. The only projects I have seen
have been attempting to get the special hash from one of the
devices or re-creating the device using low-powered Arduino boards.

# Solution:
The solution to the above problem is to create a new wearable device
that is capable of warning me if it is the case that I have not "clicked"
in game for close to the 20 minute cap that would cause my character to
be logged out while I am grinding.

## PC setup:
A Python script will be created to leverage the connection between the PC
and the wearable device. When a button press event is received from the
wearable, the script will be used to either send an event to a separate
program to cause a button click to occur or the Python script itself will
perform the click.

Because the location of the mouse is important and a script cannot be used
for mouse positioning, a virtual machine may be deployed to ensure that
the mouse stays in a certain state. That or a separate PC may be set up
specifically for this purpose.

## Wearable device:
After the user turns on the device and pairs it with the PC, after 15 or
so minutes the device will vibrate to remind the user to press the single
button. Once they have pressed it, the device will send the message to
the connected PC to perform the necessary options to click in game. If it
is the case that the device is not connected or the connected PC gives a
warning that the player has been logged out, it should vibrate in a
specific pattern to give feedback to the player that that is the case.

### Wearable device power consumption concerns:
Because of what the device is being slated to do, the power consumption
likely could be made to be minimal using several techniques, including:
- reducing the clock speed
- reducing the voltage to the device
- shutting down unnecessary board functionality
- changing or bypassing the power converter
- putting cpu to sleep when not needed
