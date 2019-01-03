---
published: false
---
## Ladder Variational autoencoder

Ladder vae recursively corrects generative distribution by a data dependent approximate likelihood. In this network, the approximate posterior distribution  merges the information from bottom up computed approximate likelihood with top-down prior information. The sharing of information between the bottom up(generative part) and top down part of the model gives the inference model knowledge about the current state of the model. Using the information from top down part, the model recursively corrects the generative distribution. 

