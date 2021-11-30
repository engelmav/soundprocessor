# Using Linux as a Sound Processor

We can use various techniques to use Linux in lieu of a typical pre/pro or minidsp. The first thing is to get more audio channels. To accomplish this we need to leverage various sound cards.

Credit given to https://www.youtube.com/watch?v=eF5NxnnPHws&t=673s

## Patching together sound cards

1. Install jack
```
sudo apt install jackd qjackctl
```
2. Go to Setup -> Advanced tab -> Change the server prefix in qjackctl to `/usr/bin/jackdbus`
3. Set Audio to "Playback Only"
4. Create a script called afterload.sh with something like this:
```
pacmd set-default-sink jack_out
pacmd set-default-source jack_in

sleep 2

alsa_out -j AlsaOutCard1 -d hw:CARD=M2 -r 44100 >/dev/null &
alsa_out -j AlsaOutCard0 -d hw:CARD=PCH -r 48000 >/dev/null &
```
Note: you can get the card names by running `cat /proc/asound/cards`
5. Point to this script in the "After launch" field of the Advanced setup tab.
6. Create an additional sink
```
pactl load-module module-jack-sink client_name=surround51_sink connect=no channels=6
```
