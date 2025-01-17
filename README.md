# ESP32-WROVER-QuadDog
A quadruped walking robot based on the ESP32-WROVER board 

There's an archive containing all the Arduino IDE sketches and files that should allow you to upload and run the quadruped inverse kinematics based robot code on a ESP32-WROVER board with camera and MPU506 IMU. I'm using the PCA9685 PWM servo driver board to drive the 12 MG90S or ES08MA II servos with two 5V&5A DC-DC step-down converters with a 3S LiPo I no longer use with one of my quadcopters. One 5A DC-DC buck converter powers the PCA driver and the other the ESP32, IMU and HC-SR04 ultrasonic receiver.

The ESP32 draws quite a lot of amps when operating as WiFi STAtion connected to my home 2.4GHz WiFi network and serving as a web server from where you can control the robot and also see the live camera feed.

The servos for each legs are as follows:
When looking at the robot from above and with the camera pointed front:
Left Front servos 0, 1, 2
Left Rear servos 3, 4, 5
Right Front servos 6, 7, 8
Right Rear servos 9, 10, 11

These need to be wired like so otherwise you'll have to modify the IK code and I have no idea where exactly to make what changes if you want to.

The calibration position is with all servos at 90 degrees and it should be as close as possible to the hip servos horizontal, the femur vertical and perpendicular to the ground and with the tibia as flat as possible on the ground. 

The robot is based on the FreeNove ESP32 robot dog which you can find the code for here:
https://github.com/Freenove/Freenove_Quadruped_Robot_Kit
and the CNC/3D print files here:
https://grabcad.com/library/freenove-12dof-tiny-robotdog-quadruped-laser-cutted-1

The legs and the body fit approximately to the original but you might need to tweak them a bit as the 3S LiPo I used is larger than the space available for the battery holder.


The quadbot has 3 predefined gait modes - Walk, Trot and Crawl. There is a configurable leg tip step length in mm as well as a leg tip travel speed in mm/s. For this particular model and depending on how you assembly it, a speed of 0.3 mm/s and a step length of 10 mm works best to keep the walking/trotting trajectory fairly straight.

Each gait implies moving one or more legs at the same time which means during the time the leg tip is lifted from the ground the rest of the body will be falling since I don't first move the body so that the CG of the robot remains within the support triangle on the ground. I tried previously to do it like so but it just added lots of jiggle movements back and forth causing the robot to tip over.

The legs movements are as follows for each gait: 
Walk
1. Front Left leg moves UP and FORWARD
2.. Front Left leg moves DOWN
3. Rear Right leg moves UP and FORWARD
4. Right Rear leg moves down
5. ALL LEGS move  BACK
6. Right Front leg moves UP and FORWARD
7. Right Front leg moves DOWN

Trot
1. Front Left and Right Rear legs move UP and FORWARD
    Front Right and Left Rear legs move BACK

2. Front Left and Right Rear legs move DOWN

3. Front Right and Left Rear legs move UP and FORWARD
    Front Left and Right Rear legs move BACK

4. Front Right and Left Rear legs move DOWN


Crawl
1. Front Left leg moves UP and FORWARD
2.. Front Left leg moves DOWN
3. Right Front leg moves UP and FORWARD
4. Right Front leg moves DOWN
5. ALL LEGS move  BACK
6. Right Rear leg moves UP and FORWARD
8. Right Rear leg moves down
9. Left Rear leg moves UP and FORWARD
10. Left Rear leg moves down



The repetition of the above pattern of leg movements results in a forward move of the robot's body in a fairly straight line, however depending on the texture of the ground, the actual position of the servos after calibration and stance, the resulting trajectory can vary. 
There is a "left leg" movement and a "right leg" movement selection meaning you can initiate the movement cycle starting either with the left leg or the right leg. Alternating between the two helps reduce direction bias and keeps the trajectory fairly straight.

Turning left or right implies moving the leg tips individually to positions on a circle's circumference and then moving the legs back to their original position. This results in the body twisting into the rotated position.

Side stepping is similar to crawling gait but it starts by first shifting the body in the sliding direction then moving each leg to its new position by lifting and moving it at the same time.

There are 5 predefined positions, the first one is the calibration position where the legs are placed so that the servo angles are roughly 90 degs. It's very handy for calibrating and replacing servos (you're going to do that quite often if using cheap micro servos). The sit position is where the bot sits laying down as close as possible to the ground resting on its tibias. Prone stance is standing up but at a short height. The bot can still walk, trot and crawl from the position but it can scrape the ground is the leg step goes above 16-20 mm. The stand position is about half-way up and it's supposed to be the normal walking position for the bot. The tallest position is when the bot sits on its toes but can still walk approximately in a straight line.

The 3 positions (prone, stand and tall) dictate the lowest and highest height of the bot so by alternating moving the front/rear legs or the left/right site legs up or down the bot can look up, down, lean to each side or shift in each direction. 
Looking left/right is achieved by moving on the ground the leg tips front/back or right/left up to a maximum amount.

The IMU also is supposed to control the leaning and looking up/down to keep the body fairly level.

This project is a work in progress so things may likely change with the code and design.
