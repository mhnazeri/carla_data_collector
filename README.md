Carla data collector code based on "Learning by Cheating" repo.
# Learning by Cheating

![teaser](assets/fig1.png "Pipeline")
> [**Learning by Cheating**](https://arxiv.org/abs/1912.12294)    
> Dian Chen, Brady Zhou, Vladlen Koltun, Philipp Kr&auml;henb&uuml;hl,        
> [Conference on Robot Learning](https://www.robot-learning.org) (CoRL 2019)      
> _arXiv 1912.12294_

If you find our repo to be useful in your research, please consider citing our work
```
@inproceedings{chen2019lbc
  author    = {Dian Chen and Brady Zhou and Vladlen Koltun and Philipp Kr\"ahenb\"uhl},
  title     = {Learning by Cheating},
  booktitle = {Conference on Robot Learning (CoRL)},
  year      = {2019},
}
```

The code in this repo is based off of [link](https://github.com/dianchen96/LearningByCheating), which contains the code for the NoCrash and CoRL 17 benchmarks.

## Installation

Clone this repo with all its submodules

```bash
git clone https://github.com/mhnazeri/carla_data_collector.git --recursive
```

All python packages used are specified in `carla_project/requirements.txt`.

This code uses CARLA 0.9.9 and works with CARLA 0.9.8, **0.9.10.1**.

You will also need to install CARLA 0.9.10.1, along with the additional maps.
See [link](https://github.com/carla-simulator/carla/releases/tag/0.9.10.1) for more instructions.

## Dataset

We provide a dataset of over 70k samples collected over the 75 routes provided in `leaderboard/data/routes_*.xml`.

[Link to full dataset (9 GB)](https://drive.google.com/file/d/1dwt9_EvXB1a6ihlMVMyYx0Bw0mN27SLy/view?usp=sharing).

![sample](assets/sample_route.gif)

The dataset is collected using `leaderboard/team_code/autopilot.py`, using painfully hand-designed rules (i.e. if pedestrian is 5 meters ahead, then brake).

Additionally, we change the weather for a single route once every couple of seconds to add visual diversity as a sort of on-the-fly augmentation.
The simulator is run at 20 FPS, and we save the following data at 2 Hz.

* Center RGB Images at 256 x 144 resolution
* A semantic segmentation center camera
* World position and heading
* Raw control (steer, throttle, brake)

Note: the overhead view does nothing to address obstructions, like overhead highways, etc.

We provide a sample trajectory in `sample_data`, which you can visualize by running

```
python3 -m carla_project.src.dataset sample_data/route_00/
```

## Data Collection

The autopilot that we used to collect the data can use a lot of work and currently does not support stop signs.

If you're interested in recollecting data after changing the autopilot's driving behavior in `leaderboard/team_code/autopilot.py`, you can collect your own dataset by running the following.

First, spin up a CARLA server

```bash
./CarlaUE4.sh -quality-level=Epic -world-port=2000 -resx=800 -resy=600 -opengl
```

then run the agent.

```bash
export CARLA_ROOT=/home/bradyzhou/software/CARLA_0.9.10.1           # change to where you installed CARLA
export PORT=2000                                                    # change to port that CARLA is running on
export ROUTES=leaderboard/data/routes_training/route_19.xml         # change to desired route
export TEAM_AGENT=auto_pilot.py                                     # no need to change
export TEAM_CONFIG=sample_data                                      # change path to save data

./run_agent.sh
```
