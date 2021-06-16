# Self-Supervised-Learning

I made two types of experiments to learn features in an unsupervised/self-supervised manner. In the first trial, I trained a GAN, and then transfer the features of the discriminator (see Appendix). In the second experiment, I trained a self-supervised network that generates free rotating labels by rotating images from the given unlabeled data. I finally chose a rotation prediction method due to the limitation in improving the performance of the GAN model. The detailed implementation I made is as follows: 

<br>

- Dataset/ Dataloader

  I extracted a dataset consisting of 0, 90, 180 and 270 degrees rotated images and each label of 0, 1, 2 and 3. 

- Designed Architecture

  I used a convolutional neural network consisting of four convolutional layers and one classification layer (with the number of filter 64). And the backbone is designed with convolutional layers consisting of normalization layers and max-pooling layers which is similar to the previous assignment's network, and the head is made up with one convolutional layer.

- Transfer Learning (Down stream task)
  
  After saving the model trained on a large unlabeled dataset in a self-supervised manner (rotation prediction), I transferred its parameters to a small labelled dataset. I only extracted the first 4 layers (the backbone) and froze/unfroze all the weights in those layers. Then, changed the last classification layer (the head) to the custom one which makes a prediction of 10 digits. However, the accuracy was much better if I trained all the layers, finetuned features, than fixed features. 


To improve my model, I tuned my architecture, hyper-parameters and regularization as follows:

1. **Train model with different batch sizes** <br>
The validation accuracies of batch size 32 and 64 are overall similar. So, I decided to use batch size 32.
