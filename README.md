# Building a Controller #

## Implemented body rate control ##

The body rate control is implemented in [./src/QuadControl.cpp:L96-L123](https://github.com/psaravind/FCND-Controls-CPP/blob/037a2457f28f50ff448c39d37a2c0c1aff35779c/src/QuadControl.cpp#L96-L123).  The BodyRateControl() method implements the proportional control on body rates to commanded moments taking into account the moments of inertia of the drone when calculating the commanded moments.

## Implement roll pitch control ##

The roll pitch control is implemented in [./src/QuadControl.cpp:L126-L166](https://github.com/psaravind/FCND-Controls-CPP/blob/037a2457f28f50ff448c39d37a2c0c1aff35779c/src/QuadControl.cpp#L126-L166). The RollPitchControl() method uses the acceleration, thrust and vehicle attitude to output a body rate command.  The controller takes into accoun the non-linear transformation from location accelerations to body rates.  Drone's mass are accounted when calculating the target angles.

## Implement altitude controller ##

The altitude control is implemented in [./src/QuadControl.cpp:L168-LL210](https://github.com/psaravind/FCND-Controls-CPP/blob/037a2457f28f50ff448c39d37a2c0c1aff35779c/src/QuadControl.cpp#L168-L210).  The AltitudeControl() method uses the both position and velocity to command thrust.  It takes into account the mass of the drone and the thrust includes the non-linear effects from non-zero roll/pitch angles.  It also uses a integrated altitude controller to handle different mass vehicle.

## Implement lateral position control ##

The lateral position control is implemented in [./src/QuadControl.cpp:L213-L253](https://github.com/psaravind/FCND-Controls-CPP/blob/037a2457f28f50ff448c39d37a2c0c1aff35779c/src/QuadControl.cpp#L213-L253).  The LateralPositionControl() method uses local position to generate a commanded local acceleration.

## Implement yaw control ##

The yaw control is implemented in [./src/QuadControl.cpp:L256-L284](https://github.com/psaravind/FCND-Controls-CPP/blob/037a2457f28f50ff448c39d37a2c0c1aff35779c/src/QuadControl.cpp#L256-L284).

## Flight Evaluation ##

The C++ controller is able to complete all the test scenarios without any failures.  Results along with the images are provided for each test scenario.

## Scenario ##

### Intro (scenario 1) ###

In this scenario, the mass doesn't mactch the actual mass of the quad, after setting the Mass parameter in [./config/QuadControlParams.txt:L12](https://github.com/psaravind/FCND-Controls-CPP/blob/97dcd6322788781906fec6528997000219607592/config/QuadControlParams.txt#L12), the vehicle was able to stay in the same spot as shown below:

**Simulation #20 (../config/1_Intro.txt)
PASS: ABS(Quad.PosFollowErr) was less than 0.500000 for at least 0.800000 seconds**

**Simulation #21 (../config/1_Intro.txt)
PASS: ABS(Quad.PosFollowErr) was less than 0.500000 for at least 0.800000 seconds**

![./animations/Scenario_1.gif](https://github.com/psaravind/FCND-Controls-CPP/blob/master/animations/Scenario_1.gif).

### Body rate and roll/pitch control (scenario 2) ###

After implementing GenerateMotorCommands(), BodyRateControl(), RollPitchControl() and tuning KPBank in QuadControlParams.txt, the vehicle was able to pass the test as shown below:

**Simulation #55 (../config/2_AttitudeControl.txt)
PASS: ABS(Quad.Roll) was less than 0.025000 for at least 0.750000 seconds
PASS: ABS(Quad.Omega.X) was less than 2.500000 for at least 0.750000 seconds**

**Simulation #56 (../config/2_AttitudeControl.txt)
PASS: ABS(Quad.Roll) was less than 0.025000 for at least 0.750000 seconds
PASS: ABS(Quad.Omega.X) was less than 2.500000 for at least 0.750000 seconds**

![./animations/Scenario_2.gif](https://github.com/psaravind/FCND-Controls-CPP/blob/master/animations/Scenario_2.gif).

### Position/velocity and yaw angle control (scenario 3) ###

Afer implementing LateralPositionControl(), AltitudeControl(), YawControl(), and tunning parameters, kpPosZ, KiPosZ, kpVelXY, kpVelZ,  kpYaw and kpPQR, the vehicle was able to pass the test as shown below:

**Simulation #169 (../config/3_PositionControl.txt)
PASS: ABS(Quad1.Pos.X) was less than 0.100000 for at least 1.250000 seconds
PASS: ABS(Quad2.Pos.X) was less than 0.100000 for at least 1.250000 seconds
PASS: ABS(Quad2.Yaw) was less than 0.100000 for at least 1.000000 seconds**

**Simulation #170 (../config/3_PositionControl.txt)
PASS: ABS(Quad1.Pos.X) was less than 0.100000 for at least 1.250000 seconds
PASS: ABS(Quad2.Pos.X) was less than 0.100000 for at least 1.250000 seconds
PASS: ABS(Quad2.Yaw) was less than 0.100000 for at least 1.000000 seconds**

![./animations/Scenario_3.gif](https://github.com/psaravind/FCND-Controls-CPP/blob/master/animations/Scenario_3.gif).

### Non-idealities and robustness (scenario 4) ###

After implementing AltitudeControl() and tuning the control parameters, the vehicle was able pass the test as shown below:

**Simulation #178 (../config/4_Nonidealities.txt)
PASS: ABS(Quad1.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds
PASS: ABS(Quad2.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds
PASS: ABS(Quad3.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds**

**Simulation #179 (../config/4_Nonidealities.txt)
PASS: ABS(Quad1.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds
PASS: ABS(Quad2.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds
PASS: ABS(Quad3.PosFollowErr) was less than 0.100000 for at least 1.500000 seconds**

![./animations/Scenario_4.gif](https://github.com/psaravind/FCND-Controls-CPP/blob/master/animations/Scenario_4.gif).

### Tracking trajectories (scenario 5) ###

Here with all the working parts of a controller put togther, we could see the vehicle was able to pass the test as shown below:

**Simulation #185 (../config/5_TrajectoryFollow.txt)
PASS: ABS(Quad2.PosFollowErr) was less than 0.250000 for at least 3.000000 seconds
Simulation #186 (../config/5_TrajectoryFollow.txt)
PASS: ABS(Quad2.PosFollowErr) was less than 0.250000 for at least 3.000000 seconds**

![./animations/Scenario_5.gif](https://github.com/psaravind/FCND-Controls-CPP/blob/master/animations/Scenario_5.gif).

### Extra Challenge 1 ###

Below image shows the test for many quads:

**Simulation #198 (../config/X_TestManyQuads.txt)
Simulation #199 (../config/X_TestManyQuads.txt)**

![./animations/Scenario_Extra1.gif](https://github.com/psaravind/FCND-Controls-CPP/blob/master/animations/Scenario_Extra1.gif).

### Extra Challenge 2 ###

Below image shows two trajectories for the vehicle:

**Simulation #193 (../config/X_TestMavlink.txt)
PASS: ABS(Quad2.PosFollowErr) was less than 0.250000 for at least 3.000000 seconds
Simulation #194 (../config/X_TestMavlink.txt)
PASS: ABS(Quad2.PosFollowErr) was less than 0.250000 for at least 3.000000 seconds**

![./animations/Scenario_Extra2.gif](https://github.com/psaravind/FCND-Controls-CPP/blob/master/animations/Scenario_Extra2.gif).
