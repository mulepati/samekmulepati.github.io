---
title: "Parts of Speech Syntactic Role Classifier"
date: 2020-05-12
tags: [Machine Learning, NLP, Python]
header:
  image: "/images/campusmap/images/Twitter/Viterbi.png"
excerpt: "Implemented Forward Algorithm and Viterbi Algorithm, which was used to train on a labeled set of 10000 tweets. The parts of speech classifier could correctly identify syntatic roles of words in test sentences at 88.7% accuracy."
mathjax: "true"
---
# [Parts of Speech Syntacic Role Classifier](https://github.com/mulepati/TwitterPOS)
(Email for access to project)

# HMM Algorithms

The hidden markov model is represented as a system of several componenets. These components are made utilizing the Forward Algorithm and Viterbi Algorithm. 
### Forward Algorithm
The forward algorithem makes a pass through the HMM starting at one state and ending at the end state. During each step it passes through a belief vector that finds the probablity of the transition to the next state, where each probability is calculated using the following equations. 
![Part1](https://latex.codecogs.com/gif.latex?B'_{t}(s^{_{j}})&space;=&space;\sum_{i&space;=&space;1}^{n&space;-&space;1}a_{ij}\cdot&space;B_{t&space;-&space;1}(s_{i}))

The "forward simulation" phase which is used to extimate the next step's belief vector for B'_t. 
Secondary intermediate value 

![Part2](https://latex.codecogs.com/gif.latex?B''_{t}(s_{j})&space;=&space;B'_{t}(s_{j})&space;\cdot&space;e_{jk})

Given the probability of this state transition you multiply it by the probiabilyt that it is this state given the emmision probability, based on the observed word and the state value.


![Part3](https://latex.codecogs.com/gif.latex?B_{t}(s_{j})&space;=&space;B''_{t}(s_{i})&space;\cdot&space;Z)

Normalizing such that the probability of the belief vector sums to 1.
![part4](https://latex.codecogs.com/gif.latex?Z&space;=&space;\sum_{k&space;=&space;0}^{n&space;-&space;1}&space;B''_{t}(s_{i}))
Where Z represents the sum of all values in the current belief vector.

Python implimentation
```
def fa_one_state_advance_time(s):
    global current_belief_vector, transition_model, previous_states, second_update

    temp = 0.0

    # Performs one state transition from all previous states to s
    for si in previous_states:
        if transition_model.keys().__contains__((s, si)):
            temp += (transition_model.get((s, si)) * current_belief_vector[indexing[si]])

    return temp
```
```
def fa_one_state_observe_emission(s, e):
    global current_belief_vector, emission_model, second_update

    if s == '<S>' or s == '<E>':
        return current_belief_vector[indexing[s]]

    # the emission value is 0 if observation is not in p_emission
    emission_value = 0.0000000005
    if emission_model.__contains__((e, s)):
        emission_value = emission_model[(e, s)]

    # performs the second step, emission * transition
    current_belief_vector[indexing[s]] = current_belief_vector[indexing[s]] * emission_value
    second_update[s] = True

    return current_belief_vector[indexing[s]]
```

```
def fa_finish_time_step(e):
    global current_belief_vector, emission_model, second_update, previous_states

    next_belief_vector = []
    new_update = {}
    next_states = []

    # creates a new belief vector for the new layer
    for i in range(len(indexing)):
        next_belief_vector.append(0.0)

    # performs all state transition for new layer
    for sj in theHMM.S:
        temp = fa_one_state_advance_time(sj)
        next_belief_vector[indexing[sj]] = temp
        new_update[sj] = False
        next_states.append(sj)

    # set the current layer to be the new transition values
    current_belief_vector = next_belief_vector
    previous_states = next_states
    second_update = new_update

    # perform the second step, multiplying by emission values
    sum_z = 0.0
    for a in second_update.keys():
        temp = current_belief_vector[indexing[a]]
        if not second_update[a]:
            temp = fa_one_state_observe_emission(a, e)
        if a != '<E>':
            sum_z += temp

    # normalize the values, summing all values in the belief vector and dividing each value by the sum
    for i in range(len(current_belief_vector)):
        current_belief_vector[i] = current_belief_vector[i] / sum_z

    return current_belief_vector
```

```
    for obs in obs_sequence:
        observation = obs.capitalize()
        belief_vector = fa_finish_time_step(observation)
        temp_belief = belief_vector.copy()
        temp_belief.pop()
        temp_belief.pop(0)
        probability_values.append(temp_belief)
```

## Installation
