# CarND-Controls-MPC
Self-Driving Car Engineer Nanodegree Program

In this project, a Model Predictive Control was implemented for a car using a version of the kinematic bycicle model. The inputs to the model were the steering wheel deflection and the acceleration which directly related to throttle input. The states of the model were : [x position, y position, car orientation (w.r.t. x axis in the cartesian coordinate system), longitudinal velocity, cross track error, cross track error angle].

The coordinates of desired path were first brought to the car reference system and rotated such that the car was always facing to the right (parallel to the x axis). A 3rd order polynomial was then fit through the desired way points, and then the cross track error was computed as the difference from the car y position (0) to the first point in the interpolated polynomial (f(0)). 

Time delay was assumed to be 100ms, as suggested. This was accounted for by first advancing the equation of motion with the current state and inputs for 100ms, and then passing the resulting state as initial condition for the MPC algorithm.

The cost function of the MPC penalized changes in steering angle the most (no fast turning of the wheel), followed by the cross track angle error. All the other factors had same weight. It was found that this results in a smooth driving even at higher speeds. If a lot of weight is put on the cross track error, a lot of oscillatory behavior is observed for high speed which can lead to instabilities quickly. 

Based on the assumed time delay, the MPC horizon was set to 8 points, with a time step of 0.05 seconds. This was tuned for the chosen speed (vref = 85 mph). Because of the time delay, decreasing the time step results in oscillations, probably because the car travels too far off the predicted MPC path in the 100ms of the delay. Also, making the time step too large can result in erratic behavior especially at high speeds and during sharp turns. 

With the current tuning, the car successfully drives around the track at a maximum speed of 82.6 mph. The reference speed was 85 mph. 
