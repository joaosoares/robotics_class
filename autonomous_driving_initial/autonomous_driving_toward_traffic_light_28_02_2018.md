# Autonomous driving towards a traffic light
Robotics Class - João Soares

## Tasks
The higher-level task would be to proceed driving or stop at crossing depending on information from the traffic light, as that is the context in which we are considering the autonomous vehicle. However, there are other related tasks, mentioned below, that have to be assumed possible in order to achieve this.

## Levels of abstraction and tasks
From the most basic to the most complex:

1. Engines - controlling current and voltage on the electric drivetrain (assuming the autonomous vehicle is electric) according to a set-point
2. Velocity control - controlling speed and direction according to a given value, including the ability to break when velocity is 0
3. Driving system - being able to do the basic reflexes of a driver - stay on the lane, not hit other cars, break, switch lanes, etc
4. React to street signs - interpret road signs and be able to act upon them choosing what to do based on current state of the vehicle and what's required of it by the sign (this is the traffic light, for example)

## Perception graph
As I understand it, a perception graph means what elements of reality need to be represented in the robot's world model in order for it to successfully complete the task at hand. Then, for each task within the abstraction layer hierarchy, there should be additional higher-level elements that should be understood at that layer and used to make decisions. So for the levels of abstraction above:

1. Engines - needs to have knowledge about the electric parameters of the engine as well as any potential problems
2. Velocity control - needs to have information about current speed and velocity
3. Driving system - needs some sort of vision (cameras/LIDAR/etc) to look beyond itself, interpreting road markings and detecting other objects on the road
4. React to street signs - needs to pick street sign images up from the noise and have knowledge of what each street sign requires from a driver

## Representation of knowledge
The representation of knowledge is how the world model is stored on the vehicle. Again, for different levels of abstraction, there may be different ways of storing it, though a property graph like the one in section 1.16 seems to be appropriate for most of them (except maybe when an image is required). So the representation of knowledge about the goal of the task and the perception elements the robot picked up would be hierarchically linked. For example, voltage and current information would be available for the velocity layer to send control signals to the engines, position of the car relative to the edge is stored by the road detection system and sent to the velocity system, as well as interpreted information about the road signs to the driving system to make the best choice.

## Monitoring
The monitoring depends on the input/output definitions for each task, and are the sensors that will allow the given subsystems to have perception. So, as an example:
* Engines: voltmeter and ammeter
* Velocity control: an IMU
* Driving system: cameras/LIDAR
* Street signs: image recognition and pattern matching algorithm

## Full system
So in my opinion, a rundown of the system would start with the perception of the street sign, which would lead to the upper "React to street signs" layer to interpret it and realize it's a traffic light, and analyze its state. If it's green, it tells the driving system to keep doing what it's doing. If it's yellow or red, it would tell the driving system to stop driving before the designated line on the floor or at a reasonable distance from the traffic light/ any preceding vehicle. The driving system controls the velocity subsystem to gradually bring it down to 0 before the car moves too much. The velocity control diminishes the voltage/current on the engines accordingly.

## Things I don't know
* What grounding the reasoning on software or environment "agents" means
* How the JSON models would lead to a coherent implementation - the data structures that would have to be built to make the car work