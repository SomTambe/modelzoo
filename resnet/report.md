# Resnet [(Paper)](https://arxiv.org/pdf/1512.03385.pdf)

After reading the paper, the main point I found out about resnets is its ability to tackle deeper networks without reduction in accuracy.

## Summary

When neural networks were trained, it was observed that increasing the depth of the network caused the accuracy to saturate at a particular point, then start degrading rapidly. This was not necessarily caused by overfitting, as training error increased in deeper networks, along with test error.


Ideally the maximum case accuracy which can be obtained (in a network) by optimization should be continued by deeper networks by simply optimizing the difference of layers to an identity mapping. Experimentally, it was observed that networks found it difficult to optimize those layers to identity maps. 

To tackle this problem, the authors introduced a new residual block architecture.

<p align="center"><img src="https://render.githubusercontent.com/render/math?math=\textbf{\hat{y}}=F(\textbf{w,b|X})%2b\textbf{X}"></p>
where, 
<p  align="center"><img  src="https://render.githubusercontent.com/render/math?math=\textbf{\hat{y}}=\network \output"></p>
<p  align="center"><img  src="https://render.githubusercontent.com/render/math?math=\F=\the \layers \i\n \the \block \with \trainable \parameters \textbf{w} \and \textbf{b}"></p>
<p  align="center"><img  src="https://render.githubusercontent.com/render/math?math=\textbf{X}=\layer \input"></p>

You can multiply X with a matrix if dimensionality changes.

Now how does this help solve the degradation problem ?

This is done by optimizing the weights of the layers F down to zero, so that it resembles an identity mapping.

From the experiments performed in the paper, on the widely popular CIFAR-10, they used a combination of three blocks, channels {16,32,64}, and dimensionality reduction in the following manner-{32,16,8}. Then they used a Global Average Pooling (but I just applied the adaptive average pooling function in pytorch). Every residual block is a combination of 2-layers with addition of the input taking place at end of the second layer.

In the experiments, increase in depth of the network, namely from {20,32,44,50,56,110} caused a continuous decrease in error rates. This shows that the hypothesis we built on is perfectly working (as compared to standard CNNs, which showed a decrease in accuracy on increase in depth) . The author also aggressively increased the network depth to 1202 layers, but the results weren't pleasant, with the model performing a little bit worse than the 110-layer model. This was mainly due to overfitting, since the dataset was not huge enough (even after augmentation) to be training a whooping 19.4 million parameters. 

On my experiments, I tried it with the following architectures-
Given below are accuracies, the one shown in the above program is two of them.
|Architecture|Without bottleneck  |With Bottleneck |
|--|--|--|
|  Resnet-14| ~81% |not tested yet|
|Resnet-32|~85%|not tested yet|
I did not use data-augmentation.