# Take Control: The PID Feedback Controller
Group members: Heath Shewmaker and Terence Redford

Date of submission: 04/09
# Introduction 
In this lab we focused on implementing Proportional Integral Derivative (PID) control using the assembled robot from the previous Lab assignment. This was accomplished by using the inputs from the ultrasonic distance sensor to monitor the distance of the robot from a wall and then passing that information to PID control functions in Arduino IDE. Because the robot was already assembled, the emphasis in this lab was on using the PID library in the Arduino IDE to interpret the incoming data from the distance sensor, and correct for it appropriately, which also involved tuning the PID constants. We were able to successfully implement PID control, however there were some eccentricities in the code that made our control slightly imperfect.
# Methods:
Part 1) we began by installing the PID_v2 Library by Bret Beauregard from the library installation tab included in the Arduino IDE. We then modified our existing code for running the robot in order to include the PID control and ensure it was functioning correctly. The modified code can be seen below:
![Screenshot 2025-04-10 222431](https://github.com/user-attachments/assets/db03c767-9f28-472f-b120-853b072d46d2)

Image 1: Importing of libraries

These two lines are to import the correct libraries for using the PID and communicating with the robot. they will be present across each of the steps in this lab.
![Screenshot 2025-04-10 222548](https://github.com/user-attachments/assets/ca4ba02e-6844-4e0c-b74d-18a9f8d2effb)

Image 2: Initialization code

These two lines are the initialization of our PID object in the code, and the initialization of the necesarry arguments to run the PID control. Note that setpoint is set to 20 and Kp, Ki and Kd are all 1 since we have not tuned them yet.
![Screenshot 2025-04-10 223151](https://github.com/user-attachments/assets/86e32ecd-37fc-477a-9d1e-3e2b76409575)

Image 3: Loop Code for part 1

The above code is present in the loop function of the code and runs continuously. The important parts of this code are that the variable that the PID compares to the setpoint is "measurement", which here is set equal to the distance reading determined using the ultrasonic distance sensor code. This then allows the "PID.compute()" function to run and perform the expected calculations to return the output variable, which is then written to the serial monitor. This is how we verified the integration of the PID into our code.

Part 2) Once we verified the function of the PID Library, and corrected any compilation errors, we began modifying the code to use the PID output to dynamically drive the motors in such a way that the robot would remain a certain distance away from the wall. The initial framework we programmed to perform this can be seen below:
![Screenshot 2025-04-10 222521](https://github.com/user-attachments/assets/3497a09f-1c27-4169-a8e8-03b8b2bfe4c0)

Image 4: PID motor control code

The above code is a modification of the code in Image 3 of Part 1. the addition of a mapping function was used on the output of the PID in order to translate the output of the PID (a 0,255 range) to a range of values that we could use to drive the motor (100,255), since we knew that if we had values too low for the motor it would not spin. We then used the value of the output again as a way to determine the direction that the motors needed to spin. Finally by setting "motorspeed" to be equal to signal (or "-signal" for revesing) we were able to cause the motors to move in the desired direction with proportional speeds.
However we had to change some of this code in order to ensure smoother function of the robot, and faster response from the PID. These changes will be reflected in the results section.

Part 3) for the wall follower section of the lab we had to make some slight modifications to the control loop we developed in part 2. First we moved the ultrasonic sensor to the side of the robot, which we achieved by using male-female jumper cables and balancing the sensor on the RedBoard. Then we modified the code such that Instead of running both wheels at the same time we would run each motor independently.However to do this we now needed to take distance readings and interpret how these readings reflected the angle of the robot to a wall. The code showed promising responses, but there were some clear flaws with the function of the PID that we were not able to iron out because we were limited on time.
# Results:
Part 1) Initially when testing the PID code we discovered that because kp, ki, and kd represent coefficients in the terms of the PID equation, it was necesarry to initialize them with
specific values, rather than simlpy leavling them as null variables. Although this was only relevant in this inital step where we verified the PID code was succesfully integrated into our code,
noticing that these variables represented coefficients saved us some time later when we began to think about tuning the PID for our specific situation.

Part 2) When we tested our code modifications we noticed a strange pattern in the way that the car corrected for distance through the PID: whenever the car was too far away, it would speed up
and overshoot by a large amount, and then because it was too close it would begin to reverse. The notable part of this was that the reversing speeds were much slower and more controlled than the forward speed.
When we analyzed our code after the lab, we noticed that this was an issue with the way we were using the map function.
![Screenshot 2025-04-10 224710](https://github.com/user-attachments/assets/839f7fb9-793a-4b28-bcd9-1d59e4515640)

Image 5: Final modifications to Motor control

as can be seen above, because we were simply mapping values of the PID directly into the desired range, the motorspeed variable would always be lower in reverse than going forward, which caused the slow reversing. This also meant that correcting for being too far away would always register a high value, meaning the car would always go forward at a comparatively high speed. One potential solution we considered was using the map function differently so that we could use it to determine how quickly the car should go, and in which direction. This could be achieved by mapping the value from -255 to 255, and then using this mapped value to determine the direction. once this is done, an if statement could be made to ensure that the speed is at a minimum value that would make the car move. unfortunately we did not get to test this method thoroughly. With the code as shown above, the Kp, Ki and Kd constants that seemed to work best for us are below:
![Screenshot 2025-04-10 222418](https://github.com/user-attachments/assets/4bb3f99a-83c1-4354-896b-e50de06a7001)

Image 6: Modified coefficient codes

Part 3) For part 3 we were not able to do sufficient testing to determine what the errors in our code were, However we had some hypotheses to explain the behaviors we did see. Our code did seem to control the wheels independently and turn in the correct directions, however we noticed that when the car was at too wide of an angle, the reading would be so large that the car would respond too forcefully, and never return to the set point without help. Our expectation is that this was influenced by the angle of the ultrasonic sensor to the wall, and that we could include a trigonometric means of modifying the wheel speeds to counteract this effect.
# Conclusion:
Thanks to this lab we were successfully able to implement the basics of PID to control our robot, and we gained intuition for how each of the coeffficients in th PID influences the output. We also discovered that PID control is not a “plug-and-play” technology, but rather a test and response technique that needs to be fine tuned for a specific situation. What made us realize this was that minor changes in the constants could have drastic changes to the effect of the PID control loop, and that changing one constant may result in the negation of the affects of another constant. To synthesize this knowledge, we believe that relating the individual components of PID to the associated constants is an absolute necessity. Blindly changing the constants will be ineffective, because there is so much nuance to each situation. A strong understanding and intuition for the mathematical concepts that drive PID control is the key to efficiently developing a good control loop.

