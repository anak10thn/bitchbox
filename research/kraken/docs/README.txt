Kraken a low-cpu, compact, highly versatile guitar effects stompbox

Intent:

Make a guitar effects rack that would
1) do a lot (includes 8000 pedal combinations)
2) cost very little cpu; and
3) eliminate the need to "connect" effects abstractions

(Master-Kraken-help.pd is the main patch.)

The Stack (in this order):

eq3
pre (gain)
3 effect/pedal slots (with clean-dirty "bypass" toggle for the whole set)
	with each slot containing
		a (tof) menu to select 0 to 20 effects (0 being raw)
		3 parameters (mknobs)
		a dry-wet slider
		a bypass toggle (for that line which crossfades in and out (set by the crossfade control, in ms)
		and
		an infinite sustain toggle (for that line)
reverb (with brightness and roomsize control and off|on toggle)
compressor (with threshold, limit, ratio, attack, release (zexy~))
master out level.

Kraken also includes:

a simple recorder (record and play (loop) toggles)
a popup (standard tuning) guitar tuner
an [ adc~ | sample ] (a|s) switch (to test the sound or even post-process a track/file using openpanel)
and
a presets control which loads or saves the current settings (for the entire rack and includes a date+time prefix as well as an entered name for each preset).

Additional Info:

OpenSoundControl (OSC) has been exposed for a future/additional post ("Kraken-OSC") which I have almost finished in Mobmuplat (only, for now, PdParty and others to be added later) and can be accessed via the send and receive to network abstractions inside the patch.

EFFECTS LIST:
00-raw
01-chorus
02-compressor
03-delay(3-tap)
04-delay(fb)
05-delay(spectr)
06-distortion
07-filter
08-flanger
09-fuzz
10-looper(fw-bw) (>0.5 on, <0.5 off to record, play forward, or play backward)
11-octave_harmonizer
12-overdrive
13-phaser
14-pitchshifter
15-reverb
16-step-vibrato
17-tremolo
18-vcf
19-vibrato
20-wah-wah

Dependencies:
zexy
cyclone
moonlib
iemlib
ggee
plus zexy~ "load on startup"

Credits/Thanks/Acknowledgements:

As I think with most pure data patches Kraken is built on the back of lots of other people's hardwork and diligent effort. In particular, it owes its effects to predominantly the DYI2 library (by Hardoff), the Stamp Album library (by Balwyn), those on the Guitar Extended website (by Pierre), and less so to a few others. And to all of them, I am deeply thankful.

Tally-Ho! Happy playing.

I am very thankful to finally "Release the Kraken!". I hope it may bring you many hours of pleasure, entertainment, and possibly education.

p.s. if you see some of your work inside this, DO please let me know and I will credit you more specifically.

Thanks again. Feel free to ask any questions you may like regarding using the tool, the abstractions, etc., and I will respond to them as soon as I am able. -Peace