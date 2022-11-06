# Pytorch motion planner
## Description
This repository contains code for Neural field optimal path planner (NFOPP)

The idea is to construct Optimal path planner with Laplacian regularization. 
For optimization of absence of obstacles, it is possible to use 
Neural field which predict probability of collision for given position. 
Thus, loss function consists of three part: collision loss function
which minimize position in collision, Laplacian regularization which 
minimize path length and total rotation, and allow optimizing smooth
trajectory, and constraint loss which forces position to specific constraint. 
Constraint loss can be done with Lagrangian multipliers

### Planner is capable

- Make optimal point to point paths which combine rotation and translation loss
- Make non-holonomic optimal paths
- Make non-holonomic with only forward movement paths
- Make paths in corridor with obstacles
- Make paths in corridor with obstacles with non-holonomic only forward movement
- Make paths in environment with random obstacles 

### Main features

- Laplacian's regularization for points on path as distance loss term (CHOMP loss term)
- Neural field representation for collision loss which can generate smooth gradients
- Continuous simultaneous learning for collision neural field with path points
- Lagrangian's multipliers as loss term for learning strict and not-strict constraints
- Quadratic loss term for constraints for fast initial convergence (with Lagrangian multipliers)
- Trajectory re-parametrization

# Prerequisites
**Python 3.9**

**Python modules**
- pytorch 1.9.0+cu111
```bash
pip3 install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu116
pip3 install pytorch-lightning
```
- numpy
- matplotlib
- PyYAML
```bash
pip3 install numpy matplotlib pyyaml
```

**ROS installation**
- [ROS Noetic](http://wiki.ros.org/noetic/Installation)
- hermesbot_collision_checker
- hermesbot_navigation
- hermesbot_simulation

**Benchmark** 
- look to https://github.com/robot-motion/bench-mr
- libpython-dev (for python binding)
- python-dev (for python binding)
- python3-tk (for visualization)
- [nlohmann-json](https://json.nlohmann.me/home/releases/)
```bash
gir clone https://github.com/nlohmann/json.git
cd json
mkdir build
cd build
cmake ..
make -j8
sudo make install
```
- [SBPL](http://sbpl.net/node/54)
```bash
git clone https://github.com/sbpl/sbpl.git
mkdir build
cd build
cmake ..
make -j8
sudo make install
```
# Installation

## Python module
```bash
sudo python3.9 setup.py install
```
To check installation run script
```bash
python3.9 scripts/run_planner.py
```

## Benchmark

Load submodules
```bash
git submodule update --init --recursive
```
There may be permissions problems. Then
```bash
git submodule update --force --recursive --init --remote 
```

Install [OMPL](https://ompl.kavrakilab.org/core/download.html)
```bash
git clone https://github.com/ompl/ompl.git
```
download [OMPL installation script](https://ompl.kavrakilab.org/core/install-ompl-ubuntu.sh)
```bash
chmod u+x install-ompl-ubuntu.sh
./install-ompl-ubuntu.sh --python
```

Build Bench mr
```bash
sudo bash benchmark/third_party/bench-mr/scripts/build.sh
```

Build bench mr python binding
```bash
rm -rf build
mkdir build
cd build
cmake -DINSTALL_BENCHMARK=ON ..
make -j5
```

[!NOTE] After building, a new shared library "build/benchmark/pybench_mr.so" must 
be created

## Running
### Benchmark

To run planner with benchmark environment, change the path to the robot shape in `2022-01-14_17-19-42_config.json`, line `77`.
 
Then run following command
```bash
export PYTHONPATH=$PYTHONPATH:build/benchmark
python3.9 scripts/run_bench_mr.py test/test_benchmark/2022-01-14_17-19-42_config.json --show true
```

For running experiments with benchmark see notebooks/benchmark folder
