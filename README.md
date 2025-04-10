# Take Control: The PID Feedback Controller
Group members: Heath Shewmaker and Terence Redford
Date of submission: 04/09
# Introduction 
In this lab we focused on implementing Proportional Integral Derivative (PID) control using the assembled robot from the previous Lab assignment. This was accomplished by using the inputs from the ultrasonic distance sensor to monitor the distance of the robot from a wall and then passing that information to PID control functions in Arduino IDE. Because the robot was already assembled, the emphasis in this lab was on using the PID library in the Arduino IDE to interpret the incoming data from the distance sensor, and correct for it appropriately, which also involved tuning the PID constants. We were able to successfully implement PID control, however there were some eccentricities in the code that made our control slightly imperfect.
# Methods:
Part 1) we began by installing the PID_v2 Library by Bret Beauregard from the library installation tab included in the Arduino IDE. We then modified our existing code for running the robot in order to include the PID control and ensure it was functioning correctly. The modified code can be seen below:

Part 2) Once we verified the function of the PID Library, and corrected any compilation errors, we began modifying the code to use the PID output to dynamically drive the motors in such a way that the robot would remain a certain distance away from the wall. The initial framework we programmed to perform this can be seen below:
However we had to change some of this code in order to ensure smoother function of the robot, and faster response from the PID. These changes will be reflected in the results section.

Part 3) for the wall follower section of the lab we had to make some slight modifications to the control loop we developed in part 2. First we moved the ultrasonic sensor to the side of the robot, which we achieved by using male-female jumper cables and balancing the sensor on the RedBoard. Then we modified the code such that Instead of running both wheels at the same time we would run each motor independently.However to do this we now needed to take distance readings and interpret how these readings reflected the angle of the robot to a wall. The modifications we made to the code are shown below:
While these modifications showed promising responses, there were some clear flaws with the function of the PID that we were not able to iron out because we were limited on time.
# Results:

# Conclusion:
Thanks to this lab we were successfully able to implement the basics of PID to control our robot, and we became familiar with what the general way that each constant influences the PID control loop. We also discovered that PID control is not a “plug-and-play” technology, but rather a test and response technique that needs to be fine tuned for a specific situation. What made us realize this was that minor changes in the constants could have drastic changes to the effect of the PID control loop, and that changing one constant may result in the negation of the affects of another constant. To synthesize this knowledge, we believe that relating the individual components of PID to the associated constants is an absolute necessity. Blindly changing the constants will be ineffective, because there is so much nuance to each situation. A strong understanding and intuition for the mathematical concepts that drive PID control is the key to efficiently developing a good control loop.

