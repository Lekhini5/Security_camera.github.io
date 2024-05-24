# Security_camera
Open-Source Computer Vision is a machine learning software library which provides a real-time optimized Computer vision library and tools. OpenCV was built to provide a common infrastructure for computer vision applications and to accelerate the use of machine perception in the commercial products. Being a BSD-licensed product, OpenCV makes it easy for businesses to utilize and modify the code. It can be used in python for image processing, motion detection, face recognition, and other function. 

Import OpenCV and Creating VideoCapture object
Ensure that you have installed OpenCV on your PC. After the installation is complete, import the library. 

import cv2

We then need to create a VideoCapture object to read the frames from the input ie. our webcam video. If you want to work with another input file already saved on your PC, you canjust type its path instead of the 0. 

cap=cv2.VideoCapture(0)

Reading our first frame
The first frame typically means it contains only the background. It the reference frame of our program. If there is any difference in the current frame with respect to the first frame, it means motion is detected. We store our first frame in the frame1 variable. 

So, The first line is to read the frame. We then convert the colored frame to B&W since we do not need colors to detect motion. Then we smooth out the image using GaussianBlur.

ret1,frame1= cap.read()

gray1 = cv2.cvtColor(frame1, cv2.COLOR_BGR2GRAY)

gray1 = cv2.GaussianBlur(gray1, (25, 25), 0)

cv2.imshow('window',frame1)

Reading Subsequent frames
We then write an infinite while loop to read the next frames. while(True):

ret2,frame2=cap.read()

gray2 = cv2.cvtColor(frame2, cv2.COLOR_BGR2GRAY)

gray2 = cv2.GaussianBlur(gray2, (21, 21), 0)

Now we store the current frame in the frame2 variable and apply the same filters as our first frame. We need a loop since the read() method only captures one frame at a time. So, to capture a continuous video, we have to loop instructions. Comparing frames Now we compare our current frame with the first frame, to check if any motion is detected. The absdiff() method gives the absolute value of pixel intensity differences of two frames. The first parameter is the background frame and the second is the current frame. 

deltaframe=cv2.absdiff(gray1,gray2)

cv2.imshow('delta',deltaframe)

Now we have to threshold the deltaframe variable using the cv2.threshold() method. The first parameter is the frame to be thresholded. the second and third are the threshold limits and the last parameter is the method used. The THRESH_BINARY method paints the background in black and motion in white. The dilate() method removes all the gaps in between.

threshold = cv2.threshold(deltaframe, 25, 255, cv2.THRESH_BINARY)[1]

threshold = cv2.dilate(threshold,None)

cv2.imshow('threshold',threshold)

Detecting contours
Using contours, we can find the white images in the black background.

countour,heirarchy = cv2.findContours(threshold, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)

We detect contours using the findCountours() method. It returns two variables, contour and hierarchy, and the parameters passed to it are the threshold variable, retrieval method and approximation method. for i in countour:

if cv2.contourArea(i) < 50:

continue

(x, y, w, h) = cv2.boundingRect(i)

cv2.rectangle(frame2, (x, y), (x + w, y + h), (255, 0, 0), 2)

cv2.imshow('window',frame2)

We now loop through the contour numpy array and draw a rectangle around the moving object. We get the rectangle bounds using boundingRect() and draw the rectangle onto frame2 using the rectangle() method.

Alerting with buzzer sound
After detecting the moving person the system alerts the people in the surroundings with abeep sound .It uses winsound module in python to nplay the sound. 

winsound.Beep(500,200)

And the last lines of code waits for the user to enter a certain character, for instance ‘q’, tobreak out of the loop and quit all the windows. 

if cv2.waitKey(20) == ord('q'):

break

cap.release()

cv2.destroyAllWindows()
