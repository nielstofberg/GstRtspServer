GstRtspServer

Building the application
========================
This is a Cross Platform project that is run on a Windows installation ov Visual studio, but compiles on a Raspberry Pi SBC running the latest version of Raspbian or Pi-OS.
The Raspberry pi should be running on the local network with SSH enabled.

**Install GStreamer in the Pi:
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install gstreamer1.0-tools
sudo apt-get install gstreamer1.0-plugins-good
sudo apt-get install gstreamer1.0-plugins-bad
sudo apt-get install gstreamer1.0-plugins-ugly
sudo apt-get install gstreamer1.0-libav


**Install GstRespServer on the Pi:
sudo apt-get install libglib2.0-dev
sudo apt-get install libgstreamer1.0-dev
sudo apt-get install libgstreamer-plugins-base1.0-dev
sudo apt-get install libgstrtspserver-1.0-dev
sudo apt-get install libgstrtspserver-plugins-base1.0-dev

Follow the instructions in .\GstRtspServer\readme\readme.html to set up the Visual studio building environment.

You can now build the Progect:

To use Local SPI Camera:
========================
**Install Git:
cd ~
sudo apt-get install git
git clone https://github.com/thaytan/gst-rpicamsrc.git

**Install The rpi camera src:
cd gst-rpicamsrc
./autogen.sh
make
sudo make install

Running the application:
========================
Copy the output file to /home/pi/GstRtspServer.out

Sample run commands:
# For test signal: /home/pi/GstRtspServer.out --gst-debug=3 '( videotestsrc ! video/x-raw,width=1280,height=720 ! clockoverlay ! textoverlay text="FAKE CAM 1" valignment=center halignment=left font-desc="Sans, 72" ! x264enc ! rtph264pay name=pay0 pt=96 )'
# For Camera Source: /home/pi/GstRtspServer.out  --gst-debug=3 --port=8554 '( rpicamsrc bitrate=8000000 awb-mode=indoor preview=false ! video/x-h264, width=1280, height=720, framerate=30/1 ! h264parse ! rtph264pay name=pay0 pt=96 )'

Note: need sudo if using port 554 (I dont know why!!)

To start at boot up:
sudo nano /etc/rc.local

add the run command above in the file.

sudo /share/GstRtspServer.out --port=554 '( videotestsrc ! video/x-raw,width=1280,height=720 ! clockoverlay ! textoverlay text="FAKE CAM 1" valignment=center halignment=left font-desc="Sans, 72" ! x264enc ! rtph264pay name=pay0 pt=96 )'
stream ready at rtsp://127.0.0.1:554/h264




