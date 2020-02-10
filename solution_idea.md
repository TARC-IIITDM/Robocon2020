## Team Details
* **College Name**: Indian Institute of Information Technology, Design and Manufacturing, Kancheepuram
* **Team Leader**: A. Dolendra Vikas
* **Mobile Number of Team Leader**: 8008819978
* **E-mail address of Team Leader**: edm19b004@iiitdm.ac.in

## I. Design of Passing Robot
1. **Dimensions**: 500mm * 500mm * 500mm  
   **Estimated weight**: // pass

2. **Type of drive**: Holonomic drive using *omni-wheels*

3. **Actuators and sensors integrated**:  
   * 4 x Brushed DC motors (for motion)
   * 1 x Servo/Stepper motor (for picking)
   * 1 x Stepper motor and 2 x Brushed DC motors (for passing)

4. **Ball Picking Mechanism**: Gripping mechanism in which a *cylindrical frame-like claw* envelopes the ball and picks it up.

## II. Design of Trying Robot
1. **Dimensions**:
   * Width: 700mm
   * Length: 860mm (normal state), 1900mm (extended state)
   * Height: 935mm
   **Estimated weight**: // pass

2. **Type of drive**: Holonomic drive using *mecanum wheels*

3. **Actuators and sensors integrated**:
   * 4 x Geared brushed DC motors (for motion)
   * 1 x Small solenoid push, 1 x electromagnet, and 1 x Brushed DC motor (for kicking)
   * 2 x Stepper motors (for dropping the ball)

4. **Ball receiving mechanism**:  
The ball is received in a *semi-open cuboid chamber* inside which it rolls down and is held until the try spot is reached and a try is attempted.  
The chamber walls will be made using a material which would *dampen the ball's momentum* and hence facilitate appropriate reception of the ball.

5. **Try mechanism**:  
The ball is held inside the bot along an incline using a *vertical plate in front of it*, say **plate A**. This vertical plate can be rotated using a stepper motor to extend along the incline and open a path for the ball. This path has another vertical plate, say **plate B**, for preventing the ball from dropping, and it can move back and forth to expose a slit, through which the ball slips into the try pit. During this process, plate B, the ball, and the ground are in contact in accordance with the rules.

   > Note that plates A and B together form a **T shape**.  

   Once the ball is kept in the try pit, plate A is rotated back using the stepper motor, *keeping the ball in-place*, and then plate B is retracted back as well.

## III. Kicking Mechanism
For kicking the ball, an angular impulse is applied by a wheel, rotating at a high RPM, to a *free-to-rotate kicking rod*. Both the wheel and the kicking rod have a common axis of rotation, however, the *kicking rod's rotation is independent of that of the wheel's*. The wheel and the kicking rod are separated by a small distance. The angular impulse is imparted to the kicking rod using a small solenoid push attached to the wheel itself, perpendicular to its plane, such that the solenoid push pops out, once the wheel has gained sufficient angular velocity, hitting the rod and imparting angular momentum to the ball.  
Once the kick has been attempted, the rod is then brought to rest using an electromagnet which dampens the rod through *eddy-current damping*.

