# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

## Introduction

In this project, the task is to build a PID controller and find the suitable PID parameters. 
The simulator provides the cross track error (CTE) and the velocity (mph) in order to compute the appropriate steering angle.

---

## Rubric Discussion Points
#### Describe the effect each of the P, I, D components had in your implementation.

PID Controller is implemented in `PID` class. Two instance of PID controller: `pid_steer` and `pid_throttle` are initiated for controlling steer and throttle.

pid_steer

The strategy is to initialize the integral part (I) and derivative part (D) to zero first. Then search value for propotional part (P) which only variates to some extent.

Then, we slowly increase derivative part (D) until the car drives in stabilized manner. To minimize the lap time. I choose some tweaking steps as follows.

Changes to the P and D components had the expected results

If P is increased, the magnitude of the oscillation also is increased.
If D is increased within sutitable range, the oscillation is smoothed out. 
If I is added a bit, it helps the car center onto the track under long and fast turn, which lets the car achieve speeds approaching to 50 mph.

The relationship among those three parameters (P / I / D) is obtained after several runs and trials, they are:

The propotional part (P) cannot be too low, otherwise the car will not pass the rapid turns after the bridge.
There are two ways to smooth the jittering during the whole navigation while retaining high speed:
1. increase derivative part (D).
2. reduce propotional part (P) and increase the integral part (I) when increase D doesn't smooth the jittering.

pid_throttle
As throttle is inversely propotional to Cross Track Error (CTE). When the car navigates smoothly and stably (in low CTE), one should increase the throttle for the car.

Increasing D smoothed the oscillation out. But if D was set too high, it destabilized the cars trajectory - especially through curves - by causing quick and extreme steering changes.
Adding a little bit to the I component helped center the vehicle significantly through long, fast turns and was key to achieving speeds above 50 mp/h. While the car does not have a steering bias, the track mostly turns left. So with I at zero, the vehicle tends to stay to the right of center-lane and won't allow the throttle PID to accelerate significantly.
After a few runs, some relationships between the parameters stood out:

If P is too low, the vehicle won't successfully navigate the two tight turns after the bridge.
Oscillation could be smoothed out and the overall speed improved iteratively in two ways
Increase D
If increasing D results in a less stable lap, then lower P and increase I
My default parameter settings still cause some oscillation at speeds above 60 mp/h. To enable the vehicle to continue accelerating, I am lowering all parameters significantly at high speed.


## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./

## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).

