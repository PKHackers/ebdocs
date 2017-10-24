# Spc stuff with the Hotel SPC from EarthBound
Written by BlueStone.



In the Hotel Theme SPC go to 0x0049C5. You should see 9F A4 A6 A7 A9 24 AB 06 AC AB. That means:
G note, C note, D note, D# note, F note, Set folling notes to 24 duration, G note, Set following notes to 06 duration, G# note, G note.

As you can see, any numbers that appear after a note set the duration for all the notes that are played after.

Now for the next part. Volume and staccato-ness.

In the Hotel Theme SPC again, go to 0x0049c0. That 6D sets the volume and staccato-ness of all the notes after until it is set again. The D in 6D sets the volume. If you change it to 0, that instrument's volume gets set to 0. If you change it to F, the volume gets changed to the loudest.

The 6 in 6D sets the staccato-ness. If you change it to 0, the notes will be played very suddenly. If you set it to 7 (which is the highest you can set it because it goes buggy if it's any higher) the notes will be played legato (smoothly).
Each instrument has a tempo. To change the tempo for the main melody, go to 0x0049BF. That byte sets the tempo. The lower the byte is, the faster the instrument plays. Apparently the highest(slowest) this tempo can be is 7F.
Why is this you ask? Don't answer that. I'll just explain it anyways. If a byte lower than 0x80 appears after a byte lower than 0x80(a byte that sets the note duration(see the command map below)) the second byte will set the staccato-ness/volume. This is because, why would you set the note duration right after setting the note duration? Doesn't make sense does it? That's why the second byte isn't treated like a normal byte that sets the note duration.

Here's how to change what instrument the Bass is. The Bass doesn't change its instrument until after the song plays through once. Change the byte at 0x0049BE to get the following sounds. Sometimes though, what you change it to can cause some effects to other instruments.

To change the main melody's instrument, change the byte at 0x0049c3 to one of the instrument commands.

Here's a little command map.

    00 Start song back at the looping point
    01 to 7F  Set note duration
    80 to C7  Plays notes
    C8 Continues holding down for the duration of a note
    C9 Stops holding down the note for the duration of a note
    CA to DF  Changes the instruments
    E0 to FF  Makes different effects


Here's a list of the instruments and effects
    C9 Default horn sound
    CA Ness walking in Magicant
    CB Cash register
    CC Door closing
    CD Going downstairs
    CE Onett guitar
    CF Drum snare
    D0 Onett guitar 2
    D1 Onett guitar 3
    D2 Whistle instrument

The instruments after this (D3 to DF) I'm not sure if they work or what.

    D3 Static
    D4 Metal sound?
    D5 Metal sound2?
    D6 Metal sound3?
    D7 Metal sound4?
    D8 Metal sound5?
    D9 Onett guitar 4?
    DA Drum sound?
    DB Drum Sound?
    DC Drum sound?
    DD Drum sound?
    DE Drum sound?
    DF Drum sound?

Here's a bunch of commands for doing different stuff.

E0 ##  Changes instruments. See waaay below.
E1 ##  Sets the right speaker pan of the current instrument. 00 = No pan, FF = Right speaker pan
E2 ## ##  Pans the current instrument either left or right.
   The first ## controls how fast it takes for the instrument to pan. 00=Don't pan, 01=Instant, FF=Slow
   The second ## controls which speaker it pans to. 00=Right Speaker, 14=Left Speaker
E3 ## ## ##  Sets viberato/pitchbend of the current instrument.
   The first ## is how long until the viberato begins. 00=Instant, FF=Really long time
   The second ## is how fast the viberato effect is. 00=Slow, FF=Really fast
   The third ## is how high the pitch can get before it starts declining again. 00=Low, FF=Really high.
E4 Turns off viberation.
E5 ##  Sets volume of whole song. 00 = No sound, FF = Loudest
E6 ## ##  Makes the volume of the whole song fade.
   The first ## is how long it takes to fade. 00=Does nothing, 01=Instant, FF=Longest
   The second ## controls what the volume will become. 00=No sound, FF=Loudest
- E7 ## Changes the speed of the whole song. 00 Freezes the music, 01=Really slow, FF=Really fast
- E8 ## ## Works like E6 only with the speed of the song.
    - The first ## controls how long it takes to change the speed of the song. 00=Does nothing, 01=Instant, FF=Longest
    - The second ## cotrols what the speed will become. 00=Slows until song is frozen, 01=Really slow, FF=Really fast
- E9 ## Increases the pitch of the song by ## notes.
- EA ## Increases the pitch of the current instrument by ## notes.
- EB ## ## ## This one is a bit trickier. It makes the note fade out and in very fast to make a tremelo sound.
    - The first ## controls how long until the tremelo begins. 00=Instant, FF=Really long time
    - The second ## controls how fast it fades out and in. 00=Slow, FF=Really Fast
    - The third ## controls how low the volume can get before it starts fading back in. 00=Never fade out, 01=No sound, FE=Loudest, FF=Never fade back in
- EC Unknown
- ED ##  Sets the volume of the current instrument. 00=No sound, FF=Loudest
- EE ## ## Works like E6 only with the volume of the current instrument.
    - The first ## controls how fast the volume changes. 00=Does nothing, 01=Instant, FF=Slow
    - The second ## controls what the volume will become. 00=No sound, FF=Loudest
- EF ## ## ## Points to different areas of the spc.
    - The first and second ## controls what area of the spc it points to. Ex: EF C9 49 would point to 0x48C9 (Subtracted 0x100 for the header)
    - The third ## controls how many times it loops. After being pointed to, when the code reaches a 00, it gets pointed back to itself ## times.
- F0 ## Makes a rotary effect to the current instrument. 00=Rotary, FF=No rotary
- F1 ## ## ##  Makes a pitch bend effect for each note of the current instrument.
    - The first ## controls how long before the pitch bend takes place. 00=Instant, FF=Really long time
    - The second ## controls how fast the pitch bend is. 00=Do nothing, 01=Instant, FF=Slow
    - The third ## controls how high or low note will bend in notes. 01,81=Bend to 1 note up, 7F,FF=Bend to 1 note down
- F2 ## ## ##  Makes an upward pitchbend effect to the notes of the current instrument
    - The first ## controls how long before the pitchbend takes effect. 00=Instant, FF=Long time
    - The second ## controls how fast the pitchbend is. 00=No effect, 01=Instant, FF=Slow
    - The third ## is how many notes below the note playing is where the pitch bend starts
- F3 Unknown
- F4 ## Increases frequency of the current instrument. 00=Don't increase, FF=Increase by 1 note
- F5 ## ## ## Changes the reverb for both left and right speakers.
    - The first ## changes 2 values, one of which I do not know what it does. The second digit controls what instuments get the reverb effect. 0=No instruments, F=All instruments
    - The second ## controls how much reverb the left speaker gives. 00=None, 80=All, FF=None
    - The third ## controls how much reverb the right speaker gives. 00=None, 80=All, FF=None
- F6 Unknown
- F7 ## ## ## I'm not sure what this one does. I think it has something to do with reverb.
- F8 ## ## ## Changes the reverb of all the instruments over a set ammount of time.
    - The first ## is how long the change in reverb takes. 00=Does nothing, 01=Instant, FF=Slow
    - The second ## controls what the reverb is going to change to for the left speaker. 00=None, 80=All, FF=None
    - The third ## controls what the reverb is going to change to for the right speaker. 00=None, 80=All, FF=None
- F9 ## ## ##  Unknown
- FA ##  Unknown
- FB ## ##  Unknown
- FC Mutes the current instrument
- FD Mutes all instruments
- FE Unmutes all the instruments after FD was used
- FF Mutes all instrumets. Can't use FE to unmute them.

*Whew!*

Now I found something else out. E0 is used to change instruments that are, and aren't, in the C8 to D9 rage. It is used E0 ##. The ## is the instrument number. Here's a list of the instruments I've found.

    00 Trumpet 1
    01 Trumpet 2
    02 Trumpet 3
    03 Trumpet 4
    04 Trumpet 4
    05 Trumpet 5
    06 High pitch Trumpet
    07 High pitch Trumpet that fades out
    08 Ocarina
    09 Trumpet 6
    0A Jazz guitar that fades out
    0B Vibrating whistle
    0C Arcade sound
    0D Square lead
    0E Saw lead
    0F Organ
    10 Birds chirp
    11 Phone ringing
    12 Jazz guitar that doesn't fade out
    13 Xylaphone
    14 Small drum snare
    15 Small drum kick
    16 Med drum snare
    17 Static
    18 Deep static
    19 High static
    1A Ness walking in magicant sound
    1B Metal chink
    1C Door closing sound
    1D Step (from going downstairs)
    1E Koto
    1F Wood block drum thing
    20 High Guitar that fades out
    21 Guitar that fades out
    22 Whistle
    23 Static
    24 Metal chink 2
    25 Deep metal chink
    26 Short metal chink
    27 Short metal chink 2
    28 Short metal chink 3
    29 Detuned tremelo guitar
    2A Drum sound?
    2B Drum sound?
    2C Drum sound?
    2D Drum sound?
    2E Drum sound?
    2F Drum sound?
    30 Drum sound?

    40 Metal chink?
    41 Weird sound
    42 Metal chink?
    43 Weird sound
    44 Drum sound?

    50 Drum sound?

    60 Wierd sound

    70 Weird fading in instrument
    71 Weird instrument
    72 Weird fading in instrument
    73 Weird instrument
    74 Weird instrument

    80 Static?
    81 Deep Static mixed with trumpet?
    82 Weird trumpet

    90 No sound?

    A0 Make instrument VERY high pitch

    B0 Lower pitch
    B1 Trumpet 2
    B2 Trumpet 3
    B3 Trumpet 4
    B4 Trumpet 4
    B5 Trumpet 5
    B6 High pitch Trumpet
    B7 High pitch Trumpet that fades out
    B8 Ocarina
    B9 Trumpet 6
    BA Jazz guitar that fades out
    BB Vibrating whistle
    BC Arcade sound
    BD Square lead
    BE Saw lead
    BF Organ
    C0 Bird chirp
    C0 Birds chirp
    C1 Phone ringing
    C2 Jazz guitar that doesn't fade out
    C3 Xylaphone
    C4 Small drum snare
    C5 Small drum kick
    C6 Med drum snare
    C7 Static
    C8 Deep static
    C9 High static
    CA Ness walking in magicant sound
    CB Metal chink
    CC Door closing sound
    CD Step (from going downstairs)
    CE Koto
    CF Wood block drum thing
    D0 High Guitar that fades out
    D1 Guitar that fades out
    D2 Whistle

I don't want to finish the list, so if you wanna know what one of the others do, go test it.

Other stuff:
I'm not sure how the table at 0x004900 works, but I do know that the bytes from 0x00491C to 0x004929 control the pointers to the instruments melodies.

I found something strange at 0x006F00. This block of code seems to have something to do with the Bass. Or maybe just part of it has something to do with the Bass. The byte at 0x006F04 appears to set the pitch of the Bass. Multiplying the byte by 2 then adding 1 will increase the pitch by one octave.