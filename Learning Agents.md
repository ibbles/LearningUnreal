
A machine learning [[Plugin]] for AI bots.
Train with reinforcement and imitation learning.
Available as a experimental feature with Unreal Engine 5.5.

The plugin must be enabled from Edit > Plugins.
When enabled a bunch of stuff will be downloaded.
Things like
- networkx
- numpy
- Pillow
- protobuf
- requests
- scipy
- sympy
- nvidia-cublas-cu12
- nvidia-cuda-cupti-cu12
- nvidia-cuda-nvrtc-cu12
- nvidia-cuda-runtime-cu12
- nvidia-cudnn-cu12
- nvidia-cufft-cu12
- torch
And much more.

Makes it possible to replace some NPCs using behavior trees or state machines with ones using <WHAT?> instead.
Is focused on character decision-making.
Is not meant for generating text, images, audio, or other assets.
Is not meant for conversational AI.

Example use-cases include:
- Sports car following  a race track.

Training is meant to be done at development time, not at runtime on end-user hardware.

The Learning Agents plugin has a C++ API that has been exposed to Blueprints.
The plugin comes with provided implementations, but you can provide you own as well.
The plugin is a library, not a framework, which means that the way agents train and do inference can be customized.
The plugin provides ways to communicate with the  Unreal process.

The C++ API provides functions  for defining:
- observations and actions.
- the neural network structure.
- flow control for the training and inference procedures.

A Python-based training process is available, making it possible to use tools such as PyTorch, Tensorboard, Numpy, etc.

During training the Unreal process collaborate with an external Python process running PyTorch.
Agents can be trained using an existing Proximal Policy Optimization (PPO) reinforcement learning algorithm.
Agents can be trained using an existing Behavior Cloning (BC) imitation learning algorithm.

The Learning Agents plugin does logging  to the `LogLearning` category.

# Learning Agents Manager

Is an [[Actor Component]], which means that it is created as part  of an [[Actor]].
The manager is used to specify the learning logic.

The manager stores references to the agents registered to the manager in the Agents array.
The agents must be registered with the Learning Agents Manager, see _Agent_ below.

Add a Learning Agents Manager to a [[Blueprint Actor]] with the [[Blueprint Editor]] > Components panel > Add > search for Learning Agents Manager.

An [[Actor]] that is to act as a Learning Agents Manager should have an [[Actor Tag]] with the value `LearningAgentsManager`.
(Is this only for the Learning To Drive tutorial, or is this tag used by the Learning Agents plugin?)
Add an [[Actor Tag]] with [[Blueprint Editor]] > main tool bar > Class Defaults > Details panel > Actor > Advanced > Tags.

For the [[Actor]] to be active  is must be added to a [[Level]].

The Learning Agents Manager contains a collection of listeners in the Listeners array:
- Interactor
- Trainer
- Recorder

The listeners will be given the Agents array when a callback is called.

# Agent

An agent can be any [[UObject]].
For example a [[Pawn]].
Each agent must be be registered with with Learning Agents Manager.
Agents are registered with the Learning Agents Manager with the Add Agent function.
The Add Agent functions returns an integer that serves as the Learning Agents Manager's unique ID for the newly added agent.
This ID should be used when performing operations on the agent through the Learning Agents Manager.
When registered a log message will be printed to `LogLearning` indicating which agent was added and which ID it got.

# Listener

Listeners are what implements most of the functionality in the Learning Agents framework.
They perform tasks such as
- gathering observations.
- performing actions.
- training.
- recording data to files.

## Interactor

The Interactor is responsible for observations and actions.
All agents that have been registered with the same manager have the same set of observations and actions.
We define this set with two overridable functions:
- Specify Agent Observation
- Specify Agent Action

The observations and actions are then performed by overriding
- Gather Agent Observation
- Perform Agent Action

The Interactor can be implemented as a Blueprint class inheriting from the Learning Agents Interactor base class.
These classes are often named after the type of agent they operate on.

### Observations

Observations form the input to the neural network.
Observations are typed data structures, for example positions and velocities.
Observations may include meta-data, such as a transformation into an egocentric reference frame.
That is, a frame that transform the position or velocity from the world reference frame to the agent's reference frame.

The observations an Interactor can produce are specified in an observation schema.
This schema is defined  by the Interactor's Specify Agent Observation function, which is called by the manager during startup.
Create an override for this function in the Interactor listener with [[Blueprint Editor]] > My Blueprint > Functions > Override > Specify Agent Observation.
This function is given a parameter of type Observation Schema which is populated and then returned.
The schema is populated with functions named Specify  X Observation where X can be
- Bool
- Angle
- Location
- Rotation
- Transform
And many more.

Each observation in the schema has a tag so that we can tell apart multiple observations of the same type.

To gather observations we override the Gather Agent Observation function, or the Gather Agent Observations function.
For each observation to make we call the Make X Observation where X matches what we used in Specify Agent Observation.
Location observations need a Relative Transform transformation that is the frame of reference that the location data should be represented in when passed to the learning algorithm.
(
The tutorial [(1)](https://dev.epicgames.com/community/learning/courses/GAR/unreal-engine-learning-agents-5-5/7dmy/unreal-engine-learning-to-drive-5-5) also talks about a Location Scale, but I don't see that on my machine.
Not sure what that is.
)

## Policy

## Trainer


# References

- 1: [_Learning Agents (5.5)_ by Brendan Mulcahy, Epic Games @ dev.epicgames.com/tutorials 2024](https://dev.epicgames.com/community/learning/tutorials/bZnJ/unreal-engine-learning-agents-5-5)
- 2: [_Learning to Drive_ by Brendan Mulcahy, Epic Games @ dev.epicgames.com/courses 2024](https://dev.epicgames.com/community/learning/courses/GAR/unreal-engine-learning-agents-5-5/7dmy/unreal-engine-learning-to-drive-5-5)
