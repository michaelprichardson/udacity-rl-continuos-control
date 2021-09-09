# Udacity Deep RL Project 2 Report: Continuous Control

![runs](./assets/agent-final.gif)
GIF of the best performing agent

### Goal

The goal of this assignment was to train multiple agents using neural networks. The task is considered solved when a score of 30 or more is achieved over 100 consecutive episodes over all agents.

### Environment

In this project an agent (or several similar agent) aims to follow a target. A reward of +1 is provided for
each step that the agent’s hand is in the goal location. Thus, the goal of your agent is to maintain its position
at the target location for as many time steps as possible.

The environment space is defined by 33 variables by agent (position, rotation, velocity, and angular velocities
of the arm) and the action space contains 4 numbers corresponding to torque applicable to two joints.

### Approach

#### Learning Algorithm

[DDPG](https://arxiv.org/abs/1509.02971) which is an actor-critic approach was used as the learning algorithm for the agent.
This algorithm is quite similar to DQN, but also manages to solve tasks with continuous action spaces. As an off-policy algorithm
DDPG utilizes four neural networks: a local actor, a target actor, a local critic and a target critic
Each training step the experience (state, action, reward, next state) the 20 agents gained was stored.
Then every second training step the agent learned from a random sample from the stored experience. The actor tries to estimate the
optimal policy by using the estimated state-action values from the critic while critic tries to estimate the optimal q-value function
and learns by using a normal q-learning approach. Using this approach one gains the benefits of value based and policy based
methods at the same time.

----

The algorithm used here is a Deep Deterministic Policy Gradient (DDPG) [2]. A DDPG is composed of two
networks : one actor and one critic.
During a step, the actor is used to estimate the best action, ie argmaxaQ (s, a); the critic then use this
value as in a DDQN to evaluate the optimal action value function.
Both of the actor and the critic are composed of two networks. On local network and one target network. This
is for computation reason : during backpropagation if the same model was used to compute the target value
and the prediction, it would lead to computational difficulty.
During the training, the actor is updated by applying the chain rule to the expected return from the start
distribution. The critic is updated as in Q-learning, ie it compares the expected return of the current state to
the sum of the reward of the choosen action + the expected return of the next state.
The first structure tried was the one from the ddpg-pendulum project of the nanodegree (with few modifications) and it gave very good results. Few things had to be adapted : the step function as we know have
simulteanously 20 agents that return experiences and the noise as we have to apply a different noise to every
agent (at first I did not change the noise, the training was running but the agent did not learn anything).
The actor is composed of 3 fc units :
— First layer : input size = 33 and output size = 128
— Second layer : input size = 128 and output size = 128
— Third layer : input size = 128 and output size = 4
1
The critic is composed of 3 fc units :
— First layer : input size = 33 and output size = 128
— Second layer : input size = 134 and output size = 128
— Third layer : input size = 128 and output size = 1
The second layer takes as input the output of the first layer concatenated with the choosen actions.

#### Model architecture

#### Hyperparameters

| Hyperparameter | Value |
| -------------- | ----- |
| beta start | 1.0 |
| beta decay | 0.995 |
| min beta | 0.01 |
| γ (Discount factor) | 0.99 |
| τ | 1e-3  |
| Learning rate (actor & critic) | 5e-4  |
| Replay buffer size | 1e5 |
| Batch size | 128 |
| Update interval | 4 |
| Max number of episodes | 500 |

### Results

### Conclusion

### Future Improvements

The algorithm could be improved in many ways. For example one could implement some DQN improvements, for example Prioritized Experience Replays
which would improve the learning effect gained from the saved experience. Also true parallel algorithms like A3C could be tried out.