# Video Streaming through your Flask web server

## Dependencies

* Flask
* opencv-python

```python
# import required modules
from flask import Flask, render_template, Response 
import picamera 
import cv2
import socket 
import io 
app = Flask(__name__) 
vc = cv2.VideoCapture(0) 
@app.route('/') 
def index(): 
   """Video streaming .""" 
   return render_template('index.html') 
def gen(): 
   """Video streaming generator function.""" 
   while True: 
       rval, frame = vc.read() 
       cv2.imwrite('pic.jpg', frame) 
       yield (b'--frame\r\n' 
              b'Content-Type: image/jpeg\r\n\r\n' + open('pic.jpg', 'rb').read() + b'\r\n') 
@app.route('/video_feed') 
def video_feed(): 
   """Video streaming route. Put this in the src attribute of an img tag.""" 
   return Response(gen(), 
                   mimetype='multipart/x-mixed-replace; boundary=frame') 
if __name__ == '__main__': 
	app.run(host='0.0.0.0', debug=True, threaded=True) 
```


Make another file called 'index.html'

```html
<html> 
 <head> 
   <title>Video Streaming </title> 
 </head> 
 <body> 
   <h1> Live Video Streaming </h1> 
   <img src="{{ url_for('video_feed') }}"> 
 </body> 
</html> 
```



### If this all fails
#### Installing uv4l
On Raspbian Jessie add the following line to the file /etc/apt/sources.list:
```shell
deb http://www.linux-projects.org/listing/uv4l_repo/raspbian/ jessie main
sudo apt-get update 
sudo apt-get install uv4l uv4l-raspicam 
```
If you want the driver to be loaded at boot, also install this optional package:

```
sudo apt-get install uv4l-raspicam-extras 
```

Apart from the driver for the Raspberry Pi Camera Board, the following Streaming Server front-end and drivers can be optionally installed:
```shell
sudo apt-get install uv4l-server uv4l-uvc uv4l-xscreen uv4l-mjpegstream uv4l-dummy uv4l-raspidisp 
```


If you have a Raspberry Pi 1, Zero or Zero W (Wireless), type:
```shell
sudo apt-get install uv4l-webrtc-armv6 
```
For Raspberry Pi 2 or 3) type:
```shell
sudo apt-get install uv4l-webrtc  
```
To restart the server:
```shell
sudo service uv4l_raspicam restart 
```



#### To install ffmpeg:
Add the following line to the file /etc/apt/sources.list:
```
deb http://www.deb-multimedia.org jessie main non-free
```
Update the list of available packages:
```
sudo apt-get update 
```
Install ffmpeg by command:
```
sudo apt-get install ffmpeg
```
