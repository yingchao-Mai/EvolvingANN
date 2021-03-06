# Spiking Artificial Neural Network

An attempt to build a self-optimizing spiking artificial neural network with reward-driven Hebbian learning. Currently, it is tested in OpenAI's cartPole environment. The network is implemented in a Python C extension. This makes it easier to train the agent in simulated environments. The Python file Simulation.py, for instance, uses the C extension to master OpenAIs CartPole-v0 environment. In the beginning, the ANN was programmed in Python for testing.

In the beginning, the ANN was completely programmed in Python. This Algorithm now contained in the C/PythonPrototype directory.

# Example
![alt text](https://github.com/AlexanderKoch-Koch/EvolvingANN/blob/master/Example_Connectome.png "example connectome")

This is the connectome after playing 10 episodes of CartPole-v0. The circles represent the neurons. If they are green, they are currently firing. Input neurons only represent the input value. Their output is just the brain input. All the others are normal neurons. The output of the output neuron is used as an action which is fed into the environment. Otherwise, they are inactive. The thickness of the lines represents the absolute value of the related weight. Green means positive and red is negative.

# The C extension
C/SpikingANN.c contains the Python interface. All the functions directly accessible from Python are defined here. The first function call should always be init(). This calls the init() function in Brain.c which initializes all variables for the ANN.
The function think() in Brain.c causes one compute step for all neurons (read inputs -> compute output -> store output - > tag active synapses). Additionally, the connections between neurons are changed if required. Connections with a low absolute value are removed and replaced by a new random connection. The weight will be again initialized randomly.
In order to maximize reward, Brain.c contains a function called process_reward(). This changes the synapse weights according to the following formula: weight += learning_rate * recent_synapse_activity * reward. recent_synapse_activity will not be reset since it might have caused a reward which has not yet been processed. It will only be reset when reset_memory is called. This necessary for Simulations like CartPole in which the agent can "die".

# Hyperparameter optimization through an evolutionary algorithm
Evolution.py tries to find the optimal hyperparameters through random mutation. After each generation, the best agents are selected for the mating pool. The child agents will then receive random parameters from this pool. A specific percentage of these child parameters will additionally be mutated.

# Python test code
Python/Simulation.py currently provides an interface to the CartPole environment for the C extension.

# Future plans
This ANN, of course, has to run on a GPU to achieve some reasonable performance. NVIDIA's CUDA platform would be suitable. However, this AI approach should first be tested on a smaller scale.

