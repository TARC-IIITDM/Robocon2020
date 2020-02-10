---
title: "Design Document"
author: T.A.R.C., IIITDM Kancheepuram
geometry: "left=2cm,right=2cm,top=2cm,bottom=2cm"
output: pdf_document
---

### Team members:
  1. *A. Dolendra Vikas*
  2. *Anurag Kar*
  3. *Utkarsh Verma*
  4. *Aniket Rochwani*
  5. *Atharva Kadlag*
  6. *Arjun Vijaykumar*

> **Note**: Throughout this document, the *Try Bot* and the *Passing Bot* will be referred to as ***TR*** and ***PR*** respectively.

## Electronics Involved
The TR and PR will both have ESP32/STM32 as the central control unit(master) which would be communicating with the actuators(slaves) using I$^2$C.   
The TR will be wirelessly controlled using **NRF24 modules** which will communicate with the microcontrollers using *SPI*. On the other hand, the PR will be controlled in a wired manner. In both the cases, the inputs will be sent through a handheld remote controller.  
The TR will execute precise holonomic motion which will be implemented using motors with *rotary encoders* and an appropriate feedback mechanism. On the other hand, the PR will execute the same using omni-wheels in *X-shape*. 
Additionally, there will also be a hard-wired kill-switch as mentioned in the rule-book.
The bots will be powered up using Li-Po/electrolytic batteries depending upon the financial state.

## Tasks
### 1. Ball Pick and Pass
#### a) Picking up

The PR has a gripping claw *placed at the end of its arm*. This arm can be rotated using a stepper motor, which serves the purpose of *feeding the ball to the passing mechanism*. Likewise, the claws can also be controlled using a servo.  
When the PR is supposed to pick up the ball, the claw is opened, with the arm in a horizontal position, and then the PR is moved towards the ball rack such that the ball is comfortably in-between the claws. Once the PR is in the correct position, the claws are closed, with the ball *loosely contained*.

> The claws have a radius lesser than the semi-minor axis of the cross-section ellipse of the rugby ball. During the picking process, the claws will be below the vertical centre of the ball.

While the arm is rotated the ball rests against the claw and this is how the PR utilises the unique shape of the rugby ball.

##### Justification
For picking the ball up, claw and arm mechanism has been used because this method *cleverly utilises the unique elliptical shape of the rugby ball*. Additionally, this mechanism is easily repetetive in nature, hence making the PR behave in a more predictable manner and performing the given task in a short amount of time.

#### b) Passing the Ball  

> The pass will be attempted with the PR and TR aligned properly.

The passing mechanism implemented in the PR consists of two wheels, *rotating in opposite directions*, for shooting the ball towards the TR. These wheels will be driven each by a brushed DC motor. The required RPM for these motors is calculated according to **[Calculation 1]**.  

> The angle of the rotating wheels can be adjusted accordingly during the testing phase.

The arm is rotated until the ball is in the proximity of the passing mechanism. Once the TR and PR are in proper alignment with respect to each other, the arm rotates further, hence feeding the ball to the rotating wheels which suck the ball in and hence shoot it towards the TR.

##### Justification
This passing mechanism has been used since it is pretty accurate, cheaper to implement, and consumes relatively less space when compared to other mechanisms. Additionally, lots of other bots already use this mechanism which gives preference to this choice.

### 2. Ball Receive
The TR receives the ball in a receving chamber whose walls will be covered by an *impact-damping material*. Once the ball hits the chamber walls, it will lose its momentum and fall on the incline within the chamber. The ball will roll along the incline until it is stopped by a vertical plate before proceeding with the try.  
It must be noted that the incline serves two purposes, one, carrying the ball towards the try mechanism, and the other one is to align the major-axis of the ball perpendicular to the incline, which is done by letting the ball rotate.

> **Note**: If it is found, during the testing phase, that the ball doesn't align properly even after rolling along the incline, a roller may be added to the incline to align it properly.

##### Justification
The TR uses this impact-damping method for receving the ball since it is easy to implement and adds less weight and size to the bot which satisfies the design size constraints of the bot as mentioned in the rule book. Also, this mechanism has zero actuators associated with it thereby making it a cost-effective approach for performing the task.

### 3. Try
The try mechanism of the TR consists of two perpendicular plates, with one plate, say **plate A**, hinged at the end of the incline. It is **plate A** which is responsible for holding the ball at the end of the incline. This plate can be rotated around using a stepper motor to allow the ball to roll over while attempting a try. The other plate, say **plate B** is responsible for dropping the ball inside the pit. This plate is moved back and forth using *rack and pinion arrangement*, exposing a slit through which the ball is supposed to fall into the pit. **Plate B** is also controlled using a stepper motor.  
While dropping the ball, **plate B** will be in contact with the ball, and the ball with the ground, which is by the rules.  
Once the ball is kept in the try pit, **plate A** is rotated back using the stepper motor, *keeping the ball in-place*, and then **plate B** is retracted back as well.

##### Justification
The slit mechanism and **plate B** allow the bot to drop the ball into the pit and retract itself back *leaving the ball unaffected*. Additionally, the **plate A** offers a simple blocking mechanism for the incline facilitating proper containment of the ball.

### 4. Kicking
The TR uses a wheel, driven by a high-RPM brushed DC motor, and a kicking rod, *sharing its rotational axis with the wheel*. However, the rotation of the kicking rod remains unaffected by the wheel's rotation. This has been implemented by using a *ball bearing* for the kicking rod.  
The wheel has a solenoid push attached to its surface, facing the kicking rod.  
When the kick has to be attempted, the wheel is made to rotate at an RPM according to **[Calculation 2]** and then *the solenoid push extends outward, on receiving manual input via the control unit*, hitting the kicking rod hence imparting the appropriate angular momentum.  
Once the kick has been attempted, the currently rotating kicking rod is brought to rest using eddy-current damping.

> The eddy-current damping has been implemented by using an electromagnet in the TR and by sticking an aluminium plate of sufficient thickness to the kicking rod.

We are also using a magnetometer(*most probably the MPU6050*) to orient the TR towards the goal post. Initially, the magnetometer will be calibrated concerning the field's orientation and afterwards, the difference in the cardinal directions is read to find the TR's orientation concerning the field. Once the proper orientation is achieved, an indicator, like an LED, will light up indicating the TR controlling player to proceed with the kick.

##### Justification
Damping of the kicking rod's rotation after the kick has been made using an electromagnet which utilises eddy-currents because it doesn't involve any contacts. This avoids any impulsive interactions between the components of the bot.

## Calculations
### 1. RPM calculation for the passing mechanism  
Let **r** be the radius of each wheel, and $\omega$ be the angular velocity of the wheels in *radians per second*. Assuming the rugby ball's ejection velocity(***v***) to be equal to the tangential velocity of the wheels at the circumference, we have:

$$v = r\omega$$ {#eq:1}

***v*** depends upon the rugby ball's projection angle($\theta$) and the distance(***R***) it has to travel. Assuming everything to be ideal, we can use the projectile motion equation to end up with the following relationship between these parameters:  
 
$$R = \frac{v^2sin(2\theta)}{g}$$ {#eq:2}

Combining [@eq:1] and [@eq:2], we find $\omega$ to be:  

$$\omega = \sqrt{\frac{gR}{r^2\sin{2\theta}}}\,rad\,s^{-1} = \left( \frac{60}{2\pi} \right) \sqrt{\frac{gR}{r^2\sin2\theta}}\,rpm$$

For the ball passing situation, we are taking the distance ***R*** to be *3m*. The other parameters ***r*** and $\theta$ will be decided during the testing phase of the bot.

#### 2. RPM calculation for the kicking mechanism  
In this calculation, we are assuming the ball to undergo simple projectile motion(no drag) the trajectory of which is given by the following equation:

$$y = x\tan\theta - \frac{gx^2}{2u^2\cos^2\theta}$$ {#eq:3}

where ***u*** is the *projection velocity* of the ball and $\theta$ is the *angle of projection* of the ball.  

We are aiming for the middle region of the goal post, to easily accommodate for any error that might arise. Therefore we put $y = 2.25\,m$, which is the midpoint of the goal post.  
Here, we assume the separation(***x***) between the TR and the goal post to be variable since multiple kicking zones are there.

Rearranging [@eq:3], we end up with $u(\theta, x)$ as:  

$$u^2 = \frac{2gx^2\sec^2\theta}{4x\tan\theta - 9}$$ {#eq:4}

It must be noted that ***u*** depends upon the motion of the rugby ball itself. So we must somehow link the ball's motion back to the TR, since we only have control over the TR. We establish this link by applying *angular momentum conservation* on kicking rod and the ball about the axis of the kicking rod, say **axis A**.  

#### Analysing the kicking event of the rod with the ball
Let the initial and final angular velocities of the kicking rod be ${\omega}_0$ and $\omega'$ respectively, and let $I_b$ and $I_r$ be the respective moments of inertia of the ball and the rod about **axis A**. Additionally, let the final angular velocity of the ball be $\omega$. We'll assume the collision to be purely elastic. Therefore, according to angular momentum conservation and kinetic energy conservation, we have:  

$$I_b\omega = I_r(\omega_0 - \omega')$$ {#eq:5}

$$\frac{1}{2}I_b\omega^2 = \frac{1}{2}I_r(\omega_0^2 - \omega'^2)$$ {#eq:6}

Using [@eq:5] and [@eq:6], we can eliminate $\omega'$ to get:

$$\omega = \frac{2\omega_0}{\left(\frac{I_b}{I_r} + 1 \right)}$$ {#eq:7}

Since $\omega = \frac{mul}{I_b}$, where $m$ is the mass of the ball, $u$ is the projection velocity of the ball and $l$ is the arm length of the kicking rod. Therefore, upon appropriate substitution, we end up with:  

$$u = \frac{2\omega_0}{ml(\frac{1}{I_r} + \frac{1}{I_b})}$$ {#eq:8}

Using [@eq:7] and [@eq:8], we get:

$$u^2 = \frac{4\omega_0^2}{m^2l^2(\frac{1}{I_r} + \frac{1}{I_b})^2} = \frac{2gx^2sec^2\theta}{4x\tan\theta - 9} \implies  \omega_0^2 = \frac{m^2l^2x^2g\sec^2\theta \left( \frac{1}{I_r} + \frac{1}{I_b}\right)^2}{2(4x\tan\theta - 9)}$$ {#eq:9}

As inferred from **[Source 1]**, the moment of inertia of the rugby ball($I_{CM}$) is given as:

$$I_{CM} = \frac{1}{2}mb^2\left[ \left( 1 - \frac{1}{4e^2} + \frac{a^2}{2e^2b^2}\right) + \frac{b(\frac{b^2}{2a^2}-1)}{e^2(\frac{a\arcsin{e}}{e}+b)}  \right]$$

where, $a$ is the semi-major axis of the ball, $b$ is the semi-minor axis of the ball, and $e$ is the eccentricity.  

> Note, here the rugby ball has been considered to be the volume of revolution of an ellipse $\frac{x^2}{a^2} + \frac{y^2}{b^2} = 1$.

However, **axis A** doesn't pass through the *centre of mass* of the rugby ball, therefore, we'll have to apply the *parallel axis theorem*, according to which:

$$I_b = I_{CM} + m(l^2+b^2)$$

> This equation has been written assuming that the rod will be hitting the ball parallel to its semi major axis.

For the **Gilbert Size 3** training ball going to be used in the competition, $a = 0.1275\,m, \> b = 0.0851\,m, \> e = 0.74, \text{ and } \> m \approx 0.35\,kg$.

The total length of the kicking rod(***2l***) is 0.4m.

Putting in the values, we get $I_{CM} = 3.2678 \times 10^{-3} \,kg\,m^2 \text{ and } I_b = 1.98 \times10^{-2}\,kg\,m^2$.

Let $m_r$ be the mass of the kicking rod. Then $I_r = \frac{m_r(2l)^2}{12}$. We know that:

$$l = 0.2\,m, \> I_b = 6.4957 \times 10^{-2}\,kg\,m^2, \> I_r = 1.33m_r \times 10^{-2} \,kg\,m^2$$

Therfore for finding $\omega_0(x, \theta, m_r)$, we substitute the following values accordingly in [@eq:9]:

$$\implies \omega_0(x, \theta, m_r) = 0.309\,x\sec\theta\left(\frac{1}{I_b} + \frac{1}{I_r}\right)\sqrt{\frac{1}{4x\tan\theta - 9}}$$

> We will be determining the remaining parameters like $m_r$ during the testing phase.

This gives us the required initial angular velocity of the kicking rod, $\omega_0$. However, the kicking rod is imparted angular momentum impulsively with the help of a wheel rotating at a sufficiently high RPM using a DC motor. The RPM of this motor will be determined by observing the angular momentum imparted by it to the kicking rod during the testing phase, which should be in accordance with the above calculations.

### Footnote
The bots have been designed in such a way that most of the parts like the stepper motors, bolts, clamps, etc. can be interchanged thus reducing the number of backup equipment required.

## Sources
1. [C. Horn and H.Fearn, *On the Flight of American Football* (2013)](https://arxiv.org/abs/0706.0366)