## Project 11 - Path Planning
### Project Goals
- Implement a path planning algorithm in C++
- Drive at least 4.32 miles on a virtual track without incident
- Do not exceed maximum speed, acceleration, or jerk
- Do not collide with any other vehicle
- Follow lanes, except for brief lane changes
- Change lanes when leading vehicle is driving too slowly

---

### Reflection
A video demonstration of the planner completing 5.9 miles before incident can be viewed [here.](https://youtu.be/dixSiW-0SSs)

For this project, I elected to build a finite state machine to evaluate the main decisions in the simulator: when to change lane left, when to change lane right, or when to continue in the existing lane. The state machine is bound by certain boundaries, such as the inability to perform a right lane change at the edge of the road, nor a left lane change when at the center-most lane of the highway. The location data is delivered from the simulator in the form of Frenet coordinate waypoints. These waypoints, instead of having global X and Y positions, have a distance from highway center and distance along the road. Additionally, I utilize the spline library to interpolate waypoints into a continuous path.

The path planning algorithm constantly looks ahead approximately 40 meters, and maintains that final waypoint as it plans the next path, making for as smooth a path between data frames as possible. The path is made up of 50 points, interpolated between the car's position and the previously calculated final waypoint. 

The lane number is a global variable that is maintained and updated as lane changes occur. When the autonomous car happens upon a vehicle in its lane that is not traveling quickly enough, it checks the other available lanes, for any vehicle in the adjacent lanes within 25 meters. Even if the vehicle in front of the autonomous car is moving at a speed below the target velocity of 50 mph, the autonomous car will not initiate a lane change if the vehicle in the adjacent lane is too close. In the case of a slow moving vehicle ahead, the car will check the right lane first for a lane change, in order to prioritize proper "passing on the right" technique seen in human drivers. 

The lane change logic is held in relatively simple conditional evaluation blocks, and results in the car being passed a new lane to enter. Once the destination lane is established, the spline library is used to calculate a smooth path to follow to assume the new lane position. 

### Concessions

While the simplicity of a finite state machine makes the decisions easily in the simulator, it might not carry over well into reality, where more factors are involved in those seemingly simple decisions. The simulator has no weather conditions, nor human error. Also, it is possible to confuse the state machine if the computer running the algorithm can not process the data quickly enough. In this way, the decision to change lanes may be made right as a vehicle in the target lane passes the boundary condition that would have prevented the lane change, resulting in a collision.
