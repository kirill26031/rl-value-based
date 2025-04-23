# rl-value-based
This repository contains my attempts to solve kettle task from Farama Kitchen.
I'm using Stable Baselines 3 library.
I had to use environment wrapper FlattenDictWrapper, 
since FrankaKitchen-v1's observation space is dict based (unsopprted directly in Stable Baselines 3).

For second task I chose SAC algorithm.
# SAC
[SAC notebook](explore sac.ipynb)
With replay buffer initialization with successfull runs, we can see progress.

For third task I tried 2 approaches (DDPG, DQN with discrete action scape).
# DDPG
[DDPG notebook](ddpg.ipynb)
Nothing special here, not many customization from library's example.
Had to use FlattenDictWrapper though.
# DQN
[DQN notebook](deep_q_obs_only.ipynb)
DQN supports only discrete action spaces.
Tried two approaches to encode actions:
1) Each of 9 action columns is represented by 'granularity' values in linear space between -1 and 1.
So, action space grows exponentially with granularity (ActionSpaceSize = 'granularity' ^ 9).
In result, action space is large.
2) Restrict robot to use only one action column per step.
This way we can encode action in action space of small size (9 * 'granularity' + 1).
At the same time, this adds further restrictions to the agent.

# Notes
To monitor learning, used EvalCallback.
Performed experiments mostly in 10k - 50k range, but also had a few runs further (250k).
Later, tried to use reward shaping.
As reward function chose negative distance from current location of kettele to desired location.
I think that's great idea, but until arm touches the kettle, reward is the same.
Didn't find a way to use arm's location in reward function (it's not give in observations).
So, most of the runs were completely blank (reward didn't change).
I'm going to try initializing replay buffer with successfull runs from Policy Gradient implementation.
This will not work with DQN (due to action space restrictions), but maybe it'll help SAC.
