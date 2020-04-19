# streamSoundFromWindowsToLinux
Stream Sound From Windows To Linux With PulseAudio [https://raspberrypi.stackexchange.com/questions/11735/using-pi-to-stream-all-audio-output-from-my-pc-to-my-stereo]
```
If you are running Linux on your PC then this is perfectly doable, as long as you install and properly configure PulseAudio on both, your Raspberry Pi and your Linux PC.

If your PC is running Windows... Skip to the end of the post (which I have just updated).

Another option would be to use PulseAudio as an AirPlay receiver/client, but as far as I know, this is not possible.

But, if you are using Linux, then read on:

Note #1: PulseAudio over WiFi will work flawlessly on some routers but will fail on others.

Note #2: The following instructions are from a conversation several Raspberry Pi users (including myself) had on this very topic.

1) Install PulseAudio on your Raspberry Pi

sudo apt-get install pulseaudio pulseaudio-module-zeroconf avahi-daemon
2) Make sure PulseAudio starts automatically:

sudo nano /etc/default/pulseaudio
Look for the PULSEAUDIO_SYSTEM_START entry and change it to 1 so that looks like PULSEAUDIO_SYSTEM_START=1

3) Configure PulseAudio to work over the network:

sudo nano /etc/pulse/system.pa
Add the following lines:

load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1;192.168.1.0/24

load-module module-zeroconf-publish

4) Reboot your Raspberry Pi:

sudo reboot
5) Now, on your Linux PC, install paprefs. If your Linux distro is based on Debian (such as Ubuntu, Mint, etc...) you can use this command:

sudo apt-get install paprefs
6) Run paprefs and under Network Access enable Make discoverable PulseAudio network sound devices available locally

7) Under Network Server enable Enable network access to local sound devices and tick both options (This is probably not necessary, unless you also want to use your Linux box as a server/sink)

8) Under Multicas/RTP enable both options

9) Check your available output devices (use your Linux distro Audio/Mixer Application). Your Raspberry Pi will (should) appear listed; select it and everything that's played on your Linux box will be redirected to the Raspberry Pi.

If your Raspberry Pi is still unavailable, try restarting your Linux PC.

UPDATE: Sending all audio from Windows to the Raspberry Pi

You will still need to follow the previous instructions to install and configure PulseAudio on your Raspberry Pi.

Now, this is what you will need to do for Windows:

1) Download the latest version of LineInCode

2) Unzip the downloaded file

2) Download PuTTY's Plink and place the plink.exe file in the same folder where you extracted LineInCode

3) Open Notepad and paste the following code:

linco.exe -B 16 -C 2 -R 44100 | plink 192.168.1.104 -l pi -pw raspberry "cat - | pacat --server 127.0.0.1 --playback"
Of course, change the IP address (192.168.1.104), user name (pi) and password (raspberry) to match your setup.

4) Save the file as audio2rpi.bat in the same folder where you extracted LineInCode

Now, whenever you want to stream your Windows' PC audio to your Raspberry Pi simply double click on the audio2rpi.bat file.

Credit for these instructions: http://ubuntuforums.org/showthread.php?t=1121603
```
Followed everything, it says "connection refused", so I am still working on it.
