# Ball Tracking Robot
Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails!


| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Charlotte H | Culver Academies | Mechanical Engineering and Renewable Energy| Incoming Junior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Headstone Image](logo.svg)
  
# Final Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/HNuMV7cAn2M?si=xJ1I29sRj3NLZuKR" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

My Milestone 2 was having the robot track the ball as it moved.  This included a number of components including:
- Setting up the Raspberry Pi camer, taking a picture and setting up a live feed
- Mounting all of the components onto the robot chassis
- Learning and setting up object tracking

### Setting up Raspberry Pi Camera, Taking a Picture, and Live Feed:
The first step to completing this milestone was setting up the Raspberry Pi camera and installing the necessary packages to use it (like cv2).  After this I moved on to taking a picture with the camera (see code below).  After that, I set up a live feed that streamed from the camera into a browser.  This gives you live updates and tracking (see code below).

### Mounting:
Once every component was working I mounted it on the robot chassis.  This is temporary, as it is currently mounted with pipe cleaners which are not strong and a potential fire hazard.  I will be remounting the components later with double-sided tape.  But, the components have stayed secure and mounting allows the robot to move and turn freely.


![Headstone Image](IMG_0940.jpg)
### Object Tracking:
In order to track the red ball (or another object) the Raspberry Pi needs to follow a number of steps to first identify where the object is.  These steps, or morphological transformations, include maksing, erosion, dilation, contouring and centriods (in that order).  Masking, the first step, involves telling the Raspberry Pi what to focus on in the image.  In this case that was the red ball.  So, the mask I created focused on anything that was the color red.  In the HSV spectrum that involved any colors with a hue value between 0-10 and 160-179.  Because red is the start of the color wheel (0) it is also the end (180) and you therefore need multiple color arrays to include both of these ranges.  The final image below was produced after applying this mask.  The parts that were identified as red are in white and all other colors in black.  The white is the area that will be detected and the black is the area that is hidden or ignored.  

After the mask is appied the area that the Raspberry Pi needs to focus on is much smaller and erosion and dilation can be applied to further hone in on the object.  Both erosion and dilation work by analyzing the image in small sections to identify which pixels are a part of the object or a part of the background.  Then, the pixels that surround part of the outline or boundary of the object are either turned into the background or object (for either an erosion or dilation).  This process will enlarge or reduce the size of the object depending on which transformation is applied.  Erosions are useful for removing blobs, disconnecting objects, and remove other pizels that may get accidentally picked up.  Dilations are useful for increasing the size of an object, joining broken pieces, and ensuring that the ball is recognized despite imperfections (that may cause parts to be removed).  The impact of the erosion or dilation can be changed by changing the kernel.  By changing the kernel to 5x5 instead of the default size.  This means that the image will be analyzed in 5x5 pixel sections and if any pixel in that section is part of the object, all the pixels in that square will be chnaged (depending on whether it is an erosion or dilation).  The second image (also in black and white) shows how erosion and dilation effects the quality of the mask, making it more precise and focused on the ball.

After maksing, eroding and dilating the image contouring can be applied to detect where the object is.  The previous steps were focused on making it clear what the object is, contouring is the first that actually detects the object.  It works by detecting continous edges in an image.  In the third picture you can see this contour in action as the green line outlining the ball.  You can also see that it is fairly accurate to the shape of the ball, because the eroding and dilating made the mask more accurate to the shape of the ball.  

After contouring creates an outline of the object a centroid can be applied.  This will take a mean of all the x and y values of the pixels in the object to find the center point.  In the fourth image you can see the red ball outlined by contour with a blue dot in the middle, a result of the centroid.  Once the center is identified and labled we can use that point to move the camera to track this center.  The first code I created calculated and displayed how far offset the center of the ball was from the center of the camera and classified is this was "OffsetLEFT" or "OffsetRIGHT".  I then created code so that the robot would turn left or right until the object was in the center of its vision.  This allowed the robot ot track the ball as it moved. (for more information on the code see below)

**Here is an outline on how morphological transitions can effect an image:** 

(With the orginal image, the image after making, erosion and dilation, the image after contouring, and the image with the center identified)


![Headstone Image](frame.jpg) 
![Headstone Image](mask.jpg) 
![Headstone Image](contour.jpg) 
![Headstone Image](centroid.jpg) 
![Headstone Image](resultmask.jpg)


*This last image is an example of an image with just a mask, no erosion or dilation.  It is from a different orginal picture then the others


# First Milestone

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

<iframe width="560" height="315" src="https://www.youtube.com/embed/-CIZu2FILbE?si=ONx-PtDOnmqQH6tI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

![Headstone Image](IMG_0919.jpg)
My major first milestone was creating code to control the robot chassis' movements.  I began by moving each wheel individually before condensing the code to easily call upon the chassis to move forward, backward, left and right (see code below).  There were a number of smaller steps necessary to meet this first milestone including:
- Setting up, troubleshooting, and connecting to the Raspberry Pi
- Building the robot chassis
- Learning how to navigate Visual Studio Code

# Schematics 
![Headstone Image](Scheme-it-export-New-Project-2025-06-20-11-51.png)
# Code (at Milestones)
This is my code from Milestone 1.  It utilizes the library to import features like the GPIO pins and time.  Defining variables and functions is the backbone of the code, beginning with defining various GPIO pins as motor inputs.  I then do some important set-up things, including setting the GPIO pins as outputs (so they will respond to code telling them to output certain signals), setting the Hz for the PWM pins (so speed of motors can be adjusted), and defining how the motors should be at the beginning of the code.  I then define the basic motor movements.  The motor driver that I am using controls two motors and moves the wheel forward or backward based on what recieves power form the Raspberry Pi (ex. A_1A receiving power but A_1B not will move motor A backwards).  Using these basic motor movemnets, I further condensed the code to create commands to move the robot forward, backward, left, and right.  The code ends with a test sequence that allows one to test different parts of the code.  I repeatedly edited thsi portion to ensure that all parts of the code was functioning.  By using "try" and "finally" instead of "while True" ensured that the motors would stop after the test (as a ending sequence can be coded).

```C++
# Motor A and B pins
B_1A = 24
B_1B = 13
A_1A = 18
A_1B = 23

#Set up
import RPi.GPIO as GPIO
import time


GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)

for pin in [A_1A, A_1B, B_1A, B_1B]:
    GPIO.setup(pin, GPIO.OUT)

#PWM at 100 Hz
pwm_A_1A = GPIO.PWM(A_1A, 100)
pwm_A_1B = GPIO.PWM(A_1B, 100)
pwm_B_1A = GPIO.PWM(B_1A, 100)
pwm_B_1B = GPIO.PWM(B_1B, 100)

pwm_A_1A.start(0)
pwm_A_1B.start(0)
pwm_B_1A.start(0)
pwm_B_1B.start(0)

#Define motor movements

def MotorA_forward(speed=100):
    pwm_A_1A.ChangeDutyCycle(0)
    pwm_A_1B.ChangeDutyCycle(speed)

def MotorA_backward(speed=100):
    pwm_A_1A.ChangeDutyCycle(speed)
    pwm_A_1B.ChangeDutyCycle(0)

def MotorB_forward(speed=100):
    pwm_B_1A.ChangeDutyCycle(speed)
    pwm_B_1B.ChangeDutyCycle(0)

def MotorB_backward(speed=100):
    pwm_B_1A.ChangeDutyCycle(0)
    pwm_B_1B.ChangeDutyCycle(speed)

def stop_all(speed=0):
    for pwm in [pwm_A_1A, pwm_A_1B, pwm_B_1A, pwm_B_1B]:
        pwm.ChangeDutyCycle(0)
        pwm.ChangeDutyCycle(speed)
    pwm_A_1A.stop()
    pwm_A_1B.stop()
    pwm_B_1A.stop()
    pwm_B_1B.stop()

def forward (speed=100):
    MotorB_forward()
    MotorA_forward()

def backward (speed=100):
    MotorA_backward()
    MotorB_backward()

def right (speed=100):
    MotorB_forward()
    time.sleep(1)
    pwm_B_1A.ChangeDutyCycle(0)
    pwm_B_1B.ChangeDutyCycle(0)


   

def left (speed=100):
    MotorA_forward()
    time.sleep(1)
    pwm_A_1A.ChangeDutyCycle(0)
    pwm_A_1B.ChangeDutyCycle(0)

    
    
    
#test sequence

try:
    time.sleep(7)
    print("Motors forward and backward")
    forward()
    time.sleep(1)
    backward()
    time.sleep(1)
    print("Motor right and left")
    right()
    time.sleep(1)
    left()
    time.sleep(1)

finally:
    stop_all()
    print("Test complete")
    GPIO.cleanup()


```
Here is the code at the second Milestone (the names are what I have named the files).  The first two sets of code set up what the robot draws upon to track the ball.  The motor function (MotorTest.py) has been modified from the first Milestone to allow the robot to better adjust its position and track the ball more precisely.  The second set of code (Camera2.py) starts by importing the necessary packages, takes a pictures, then applies the morphological transition discussed above (masking, eroding, dilating, contouring, and centroid) in order to identify where the ball is in that picture.  The final set of code (Video.py) also begins by importing the necessary packages (imcluding MotorTest.py to draw upon the motor functions).  It creates a function that tracks the red ball as it moves by defining how far offset the ball is the calling upon the correct motor function to move the necessary amount.  It then creates a way to view the live video feed from the camera in a browser.

### MotorTest.py
```C++
# Motor A and B pins
B_1A = 24
B_1B = 13
A_1A = 18
A_1B = 23

import RPi.GPIO as GPIO
import time


GPIO.setmode(GPIO.BCM)
GPIO.setwarnings(False)

for pin in [A_1A, A_1B, B_1A, B_1B]:
    GPIO.setup(pin, GPIO.OUT)

#PWM at 100 Hz
pwm_A_1A = GPIO.PWM(A_1A, 100)
pwm_A_1B = GPIO.PWM(A_1B, 100)
pwm_B_1A = GPIO.PWM(B_1A, 100)
pwm_B_1B = GPIO.PWM(B_1B, 100)

pwm_A_1A.start(0)
pwm_A_1B.start(0)
pwm_B_1A.start(0)
pwm_B_1B.start(0)

#Define motor movements

def MotorA_forward(speed=100):
    pwm_A_1A.ChangeDutyCycle(0)
    pwm_A_1B.ChangeDutyCycle(speed)

def MotorA_backward(speed=100):
    pwm_A_1A.ChangeDutyCycle(speed)
    pwm_A_1B.ChangeDutyCycle(0)

def MotorB_forward(speed=100):
    pwm_B_1A.ChangeDutyCycle(speed)
    pwm_B_1B.ChangeDutyCycle(0)

def MotorB_backward(speed=100):
    pwm_B_1A.ChangeDutyCycle(0)
    pwm_B_1B.ChangeDutyCycle(speed)

def stop_all(speed=100):
    for pwm in [pwm_A_1A, pwm_A_1B, pwm_B_1A, pwm_B_1B]:
        pwm.ChangeDutyCycle(speed)

def forward (speed=100):
    MotorB_forward()
    MotorA_forward()

def backward (speed=100):
    MotorA_backward()
    MotorB_backward()


def rightMajor (speed=50):
    MotorB_forward()
    MotorA_backward()
    time.sleep(0.10)
    MotorA_forward()
    MotorB_forward()
    time.sleep(.0001)
    stop_all()
    time.sleep(0.10)
    

    
def rightSmall (speed=1):
    MotorB_forward()
    MotorA_backward()
    time.sleep(0.001)
    stop_all()
    time.sleep(0.06)


def leftMajor (speed=50):
    MotorA_forward()
    MotorB_backward()
    time.sleep(0.10)
    stop_all()
    time.sleep(0.10)


def leftSmall (speed=1):
    MotorA_forward()
    MotorB_backward()
    time.sleep(0.001)
    stop_all()
    time.sleep(0.06)

```

### Camera2.py
```C++
from picamera2 import Picamera2
import cv2
import numpy as np


#Intilialize and configure the camera
picam2= Picamera2()
picam2.configure(picam2.create_preview_configuration(main={"format": "RGB888", "size": (640,480)}))
picam2.start()

#capture one frame
frame = picam2.capture_array()

#save to file
cv2.imwrite("ogframe.jpg", frame)
print("Frame saved as ogframe.jpg")

#save into var image
image = frame
hsv_image = cv2.cvtColor(image, cv2.COLOR_RGB2HSV)

lower_red = np.array([0, 50, 50])
upper_red = np.array([10, 255, 255])
lower_red2 = np.array([160, 100, 100])
upper_red2 = np.array([179, 255, 255])

kernel = np.ones((5,5), np.int8)

mask1 = cv2.inRange(hsv_image, lower_red, upper_red)
mask2 = cv2.inRange(hsv_image, lower_red2, upper_red2)
mask = cv2.bitwise_or(mask1, mask2)
mask = cv2.erode(mask, kernel, iterations=2)
mask = cv2.dilate(mask, kernel, iterations=2)
image_copy = image.copy()




contours, _ =cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

#find the largest contour by area
if contours:
    largest_contour= max(contours, key=cv2.contourArea)
cv2.drawContours(image=image_copy, contours=[largest_contour], contourIdx=-1,color=(0,255,0), thickness=2, lineType=cv2.LINE_AA)


M = cv2.moments(largest_contour)
if M['m00']>0:
    cx = int(M["m10"]/ M["m00"])
    cy = int(M["m01"]/ M["m00"])
    cv2.circle(image_copy, (cx, cy), 5, (255, 0, 0), -1)

#save to file
cv2.imwrite("resultframe.jpg", image_copy)
print("Frame saved as resultframe.jpg")


```

### Video.py
```C++
from flask import Flask, Response, render_template_string
app = Flask(__name__)
import cv2
import numpy as np
from picamera2 import Picamera2
import RPi.GPIO as GPIO
import time
from MotorTest import *

app = Flask(__name__)

#initialize PiCam
picam2= Picamera2()
picam2.configure(picam2.create_preview_configuration(main={"format": "BGR888", "size": (640,480)}))
picam2.start()

FRAME_WIDTH = 640
CENTER_X = FRAME_WIDTH // 2

def track_red_ball(frame):
    image = frame
    hsv_image = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)

    lower_red = np.array([0, 50, 50])
    upper_red = np.array([10, 255, 255])
    lower_red2 = np.array([160, 100, 100])
    upper_red2 = np.array([179, 255, 255])

    kernel = np.ones((5,5), np.int8)

    mask1 = cv2.inRange(hsv_image, lower_red, upper_red)
    mask2 = cv2.inRange(hsv_image, lower_red2, upper_red2)
    mask = cv2.bitwise_or(mask1, mask2)
    mask = cv2.erode(mask, kernel, iterations=2)
    mask = cv2.dilate(mask, kernel, iterations=2)
    image_copy = image.copy()

    contours, _ =cv2.findContours(mask, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    if contours:
        largest_contour= max(contours, key=cv2.contourArea)
        cv2.drawContours(image=image_copy, contours=[largest_contour], contourIdx=-1,color=(0,255,0), thickness=2, lineType=cv2.LINE_AA)
        M = cv2.moments(largest_contour)
        if M['m00']>0:
            cx = int(M["m10"]/ M["m00"])
            cy = int(M["m01"]/ M["m00"])
            cv2.circle(image_copy, (cx, cy), 5, (255, 0, 0), -1)
                
            
            if( cx-140 > CENTER_X):
                position = "OffsetRIGHTMajor"
                rightMajor()
            elif( cx-70 > CENTER_X):
                position = "OffsetRIGHTSmall"
                rightSmall()
            elif(cx+140 < CENTER_X):
                position = "OffsetLEFTMajor"
                leftMajor()
            elif(cx+70 < CENTER_X):
                position = "OffsetLEFTSmall"
                leftSmall()
            else:
                position = "Centered"
                stop_all()
            

            cv2.putText(image_copy, f"Offset: {cx-CENTER_X} ({position})", (10, 30),
                            cv2.FONT_HERSHEY_SIMPLEX, 0.7, (255, 255, 255), 2)
            

    return image_copy
    

def generate_frames():
    while True:
        frame = picam2.capture_array()
        frame = cv2.cvtColor(frame, cv2.COLOR_RGB2BGR)
        frame = track_red_ball(frame) 
        ret, buffer = cv2.imencode('.jpg', frame)
        jpg_frame = buffer.tobytes()
        yield (b'--frame\r\n'
               b'Content-Type: image/jpeg\r\n'
               b'Content-Length: ' + f"{len(jpg_frame)}".encode() + b'\r\n\r\n' +
               jpg_frame + b'\r\n')





@app.route('/')
def index():
    return render_template_string('''
        <html>
            <head><title>Red Ball Tracking Stream</title></head>
            <body>
                <h2>Live Tracking</h2>
                <img src="/video_feed">
            </body>
        </html>
    ''')



@app.route('/video_feed')
def video_feed():
    return Response(generate_frames(),
                    mimetype='multipart/x-mixed-replace; boundary=frame')

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)



```

# Code (Final)

```C++
```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Raspberry Pi 4 Starter Kit | What the item is used for | $95.19 | [<a href="https://www.amazon.com/RasTech-Raspberry-Starter-Heatsink-Screwdriver/dp/B0C8LV6VNZ/ref=sr_1_4?crid=3506HY00MCGVM&dib=eyJ2IjoiMSJ9._zkM62vSQ8p7tNr88715LdMv_qHh72Je-tkF9PXEa3chDE53QT4aZu4AGAb4ihE61QY4ZD55nKF6Fp2Kfs8t7AbafM_JrlJFfHo9OB4eAVGqa0EB-7aoBQHPmhKHZ2MW8ny-Kd44bMVlVxPlTWVk5YHIN5P3uKVqrE5Dcal0rKkHny-O6Xyb5ux2AOU6OwVbkag_bqBX66RQNRrgBuz-0pS43mcx93IZTQA9R8NaJJypYU2HAycp-XicTFmyU60a01Nfm9iuyo6B9yA8ppN3OQQyJ-NQ9xyNPxfTLwkqtng.yAYpU6outhQcZmOZhN9Wb6yTw7A85CNUbXZguGInZNg&dib_tag=se&keywords=raspberry%2Bpi%2Bkit&qid=1718848547&s=electronics&sprefix=rasbperry%2Bpi%2Bkit%2Celectronics%2C83&sr=1-4&th=1/"> Link </a> |
| Robot Chassis | What the item is used for | $18.99 | <a href="https://www.amazon.com/Smart-Chassis-Motors-Encoder-Battery/dp/B01LXY7CM3/"> Link </a> |
| Screwdriver Kit | What the item is used for | $5.94 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Ultrasonic Sensor | What the item is used for | $9.99 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| H Bridges | What the item is used for | $8.99 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Pi Cam | What the item is used for | $12.86 | <a href="https://www.uctronics.com/arducam-for-raspberry-pi-camera-module-with-case-5mp-1080p-for-raspberry-pi-3-3-b-and-more.html/"> Link </a> |
| Electronics Kit | What the item is used for | $11.98 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Motors | What the item is used for | $11.98 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| DMM | What the item is used for | $11.00 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| Champion Sports Ball | What the item is used for | $16.73 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| AA Batteries | What the item is used for | $18.74 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |
| USB Power Bank and Cable | What the item is used for | $16.19 | <a href="https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/"> Link </a> |


# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
