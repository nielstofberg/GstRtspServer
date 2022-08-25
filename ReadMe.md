# GstRtspServer

## Development Environment
This project is run in Visual Studio 2022
Cross compiling on Raspberry Pi.

## Project Setup
### Setup the Pi
- From fresh install, enable SSH using:  
sudo raspi-config
- Some parts of gstreamer are already installed, but aditional dependencis are required.  
See https://qengineering.eu/install-gstreamer-1.18-on-raspberry-pi-4.html for instructions.
- Install RTSP libraries:  
sudo apt-get install libgstrtspserver-1.0-dev gstreamer1.0-rtsp

### Add SSH connection
- In VS 2022: Tools->Options->Cross Platform->Connection Manager.
- Click Add and enter SSH connection information.

## Running the application
Copy the output file to /home/pi/GstRtspServer.out
Sample run commands:
- For test signal:  
/home/pi/GstRtspServer.out --gst-debug=3 '( videotestsrc ! video/x-raw,width=1280,height=720 ! clockoverlay ! textoverlay text="FAKE CAM 1" valignment=center halignment=left font-desc="Sans, 72" ! x264enc ! rtph264pay name=pay0 pt=96 )'
- For Camera Source: *(See "Installing Pi Camera" below)*
/home/pi/GstRtspServer.out  --gst-debug=3 --port=8554 '( rpicamsrc bitrate=8000000 awb-mode=indoor preview=false ! video/x-h264, width=1280, height=720, framerate=30/1 ! h264parse ! rtph264pay name=pay0 pt=96 )'

Note: need sudo if using port 554 (I dont know why!!)

To start at boot up:
sudo nano /etc/rc.local

add the run command above in the file.

## Installing Pi Camera 
*(This is now depreciated)*  
git clone https://github.com/thaytan/gst-rpicamsrc.git  
cd gst-rpicamsrc  
./autogen.sh  
make  
sudo make install  

