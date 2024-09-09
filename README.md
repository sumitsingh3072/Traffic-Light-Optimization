# Traffic-Light-Optimization

# Deep Q-Learning Agent for Traffic Signal Control

A framework where a deep Q-Learning Reinforcement Learning agent tries to choose the correct traffic light phase at an intersection to maximize traffic efficiency.

## Getting Started

These instructions will set-up the necessary files required on your local machine.

1. Download Anaconda ([official site](https://www.anaconda.com/distribution/#download-section)) and install.
2. Download SUMO ([official site](https://www.dlr.de/ts/en/desktopdefault.aspx/tabid-9883/16931_read-41000/)) and install.

```
conda create --name tf_gpu
activate tf_gpu
conda install tensorflow-gpu
```
## Running the algorithm

1. Clone or download the repo.
2. Using the Anaconda prompt or any other terminal, navigate to the root folder and run the file **training_main.py** by executing:
```
python training_main.py
```
If you want to see the training process as it goes, you need to set to *True* the parameter *gui* contained in the file **training_settings.ini**.
The file **training_settings.ini** contains all the different parameters used by the agent in the simulation. The default parameters aren't greatly optimized, so a bit of testing will likely increase the algorithm's current performance.
When the training ends, the results will be stored in "*./model/model_x/*" where *x* is an increasing integer starting from 1, generated automatically. Results will include some graphs, the data used to create the graphs, the trained neural network, and a copy of the ini file where the agent settings are.
To test the model , you have to run the file **testing_main.py**. The test involves a single episode of simulation, and the results of the test will be stored in "*./model/model_x/test/*" where *x* is the number of the model that you specified to test. The number of the model to test and other useful parameters are contained in the file **testing_settings.ini**.

## The Deep Q-Learning Agent

**Agent ( Traffic Signal Control System - TLCS)**:
- **State**: discretization of oncoming lanes into presence cells, which identify the presence or absence of at least 1 vehicle inside them. There are 20 cells per arm. 10 of them are placed along the left-most lane while the other 10 are placed in the other three lanes. 80 cells in the whole intersection.
- **Action**: choice of the traffic light phase from 4 possible predetermined phases, described below. Every phase has a duration of 10 seconds. When the phase changes, a yellow phase of 4 seconds is activated.
  - North-South Advance: green for lanes in the north and south arm dedicated to turning right or going straight.
  - North-South Left Advance: green for lanes in the north and south arm dedicated to turning left. 
  - East-West Advance: green for lanes in the east and west arm dedicated to turning right or going straight.
  - East-West Left Advance: green for lanes in the east and west arm dedicated to turning left. 
- **Reward**: change in *cumulative waiting time* between actions, where the waiting time of a car is the number of seconds spent with speed=0 since the spawn; *cumulative* means that every waiting time of every car located in an incoming lane is summed. When a car leaves an oncoming lane (i.e. crossed the intersection), its waiting time is no longer counted. Therefore this translates to a positive reward for the agent.
- **Learning mechanism**: the agent make use of the Q-learning equation *Q(s,a) = reward + gamma • max Q'(s',a')* to update the action values and a deep neural network to learn the state-action function. The neural network is fully connected with 80 neurons as input (the state), 5 hidden layers of 400 neurons each, and the output layers with 4 neurons representing the 4 possible actions. Also, an experience replay mechanism is implemented: the experience of the agent is stored in a memory and, at the end of each episode, multiple batches of randomized samples are extracted from the memory and used to train the neural network, once the action values have been updated with the Q-learning equation.



