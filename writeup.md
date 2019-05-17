# The C++ Project 3 Writeup #

This is the writeup for the C++ controller project.

## The Code ##

All of the code for the controller can be found in `src/QuadControl.cpp`.

All the configuration files for the controller and the vehicle are in the `config` directory.  The control gains and other desired tuning parameters are in a config file called `QuadControlParams.txt`.

Different scenarios are considered, with increasing degree of difficulty to test the various part of the controller implemented.


### Scenario 1: correct quad mass ###

In `QuadControlParams.txt` the estimated mass of the vehicle has been set to 0.5 kg to make the vehicle stay in the same spot.

![scenario1](images/scenario1.png)


### Scenario 2:  body rate and roll/pitch control ###

In this scenario, there is a quad above the origin.  It is created with a small initial rotation speed about its roll axis.  The controller will need to stabilize the rotational motion and bring the vehicle back to level attitude.

To accomplish this, the following steps have been performed, i.e. the body-rate and the roll-pitch controls have been implemented.

 1. Implement the code in the function `GenerateMotorCommands()`  

    The function takes as inputs the desired collective thrust and moments about each axis. As output it gives the desired thrust per motor. To compute the desired thrust per motor a system of four equations (equilibrium in z direction of the forces and about the three axis for the moments) with four unknowns has to be solved.

 2. Implement the code in the function `BodyRateControl()`

     The function returns the desired moments for each of the three axis given the desired and the estimated body rates. A proportional controller is employed with gains `kpPQR`.

 3. Implement the code in the function `RollPitchControl()`

    This function calculate the desired pitch and roll angle rates based on a desired global lateral acceleration, the current attitude of the quad, and the desired collective thrust command. Note that the rotation matrix is necessary to switch reference system. Also note that the z-direction is left to 0 here. A proportional controller is implemented with gain `kpBank`.

The gains `kpPQR` in `QuadControlParams.txt` are set to (90, 90, 6) to get the vehicle to stop spinning quickly but not overshoot, while `kpBank` is set to 11 to minimize settling time but avoid too much overshoot.

The rotation of the vehicle about roll (omega.x) get controlled to 0. Including the roll-pitch control the quad level itself (as shown below), though it’ll still be flying away slowly since we’re not controlling velocity/position! Both checks are marked as PASSED.

![scenario2](images/scenario2.png)
