# CarND-Extended-Kalman-Filter

Self-Driving Car Engineer Nanodegree Program

The goal of this project is to build an Extended Kalman Filter using C++ and
use it to estimate the state of a moving object of interest with noisy LIDAR
and RADAR measurements.

The measurements data is provided in the form of a [simulator](https://github.com/udacity/self-driving-car-sim/releases).

The key metrics are [RMSE](https://en.wikipedia.org/wiki/Root-mean-square_deviation) values for both position and velocity of the tracked
object.

## Results

The success metrics for this project are the RMSE values for 2 datasets.

The values shoule be below:
- `0.11` for `P x` and `P y`.
- `0.52` for `V x` and `V y`.

### RMSE values

The folowing table lists the results of both datasets:

| RMSE | Dataset 1 | Dataset 2 |
|------|-----------|-----------|
| P x  |  0.1405   |  0.0732   |
| P y  |  0.6668   |  0.0963   |
| V x  |  0.6050   |  0.3813   |
| V y  |  1.6355   |  0.4782   |

This is somehow unexpected as the dataset 1 should be the "easy" one to which every
implementation should be able to get results below the desired marks, and dataset 2
should be the "hard" one, showcasing a more precise implementation.

It is unclear at the moment why this is the case.

#### Using only one senor

For both datasets a run with only one sensor, `radar` or `lidar` was also measured. 

> You can test this yourself by setting the vars `use_laser_` and `use_radar_` in `src/FusionEKF.cpp`.

Here are the results:

##### Dataset 1

| RMSE | only RADAR | only LIDAR |
|------|-----------|-----------|
| P x  |  11.5299   |  0.1473   |
| P y  |  7.9951   |  0.1152   |
| V x  |  9.9502   |  0.6781   |
| V y  |  8.8659   |  0.5324   |

Interesting points here:
- It behaves better with only `Lidar` than with both sensors, indicating tha the `Radar` measurements hurt more then help the prediction.
- The issues with `Radar` measurements appear more prevalent to be on the `y` axis. Unclear why this is the case at this moment.

##### Dataset 2

| RMSE | only RADAR | only LIDAR |
|------|-----------|-----------|
| P x  |  0.2706   |  0.1167   |
| P y  |  0.3869   |  0.1256   |
| V x  |  0.6780   |  0.5929   |
| V y  |  0.9585   |  0.5774   |

A few points of interest:
- This time it behaves slightly worse with only `Lidar` data then with both.
- Again it behaves better with only `Lidar` then with only `Radar` data. 
- The discrepancy between axis `x` and `y` is not so apparent if present at all. 

### Images from the simulator

> With both `Radar` and `Lidar` data.

#### Dataset 1

![alt text](results/EKF-dataset-1.png "Dataset 1")

#### Dataset 2

![alt text](results/EKF-dataset-2.png "Dataset 2")

## Implementation

The code skeleton for this project was provided by udacity on [this repo](https://github.com/udacity/CarND-Extended-Kalman-Filter-Project).

The main program in under the `src` directory.
```
.
├── FusionEKF.cpp
├── FusionEKF.h
├── json.hpp
├── kalman_filter.cpp
├── kalman_filter.h
├── main.cpp
├── measurement_package.h
├── tools.cpp
└── tools.h
```

The main changes were to the folowing files:

- `main.cpp` - reads in data, runs the Kalman filter and calculates RMSE values after each measurement.
- `FusionEKF.cpp` - initializes the filter, calls the `Predict` function and the `Update` function
- `kalman_filter.cpp`- implementation of the `Predict` and `Update` function, for both `lidar` and `radar`.
- `tools.cpp` - tool functions to calculate `RMSE` and the `Jacobian` matrix, used to convert polar to cartesian coordinates

### Unit tests

The code is not tested at the moment, this is somthing I want to add in the future using [Google's gTest](https://github.com/google/googletest) framework.

---

# Contributing

## Dependencies

* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1 (Linux, Mac), 3.81 (Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools](https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)

## Build

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make` 
   * On windows, you may need to run: `cmake .. -G "Unix Makefiles" && make`
4. Run it: `./ExtendedKF `

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).
