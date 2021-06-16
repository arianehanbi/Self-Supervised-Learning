# Self-Supervised Learning for SVHN

I made two types of experiments to learn features in both an unsupervised and a self-supervised manner. In the first trial, I trained a GAN, and then transfer the features of the discriminator (see Appendix). In the second experiment, I trained a self-supervised network that generates free rotating labels by rotating images from the given unlabeled data. I finally chose a rotation prediction method due to the limitation in improving the performance of the GAN model. The detailed implementation I made is as follows: 

<br>

- Dataset/ Dataloader

  - A large unlabelled training set: 72,257 unlabelled images for training
  - A small labelled training set: 500 labelled images for training and 2000 labelled images for validation
            
  
    I extracted a dataset consisting of 0, 90, 180 and 270 degrees rotated images and each label of 0, 1, 2 and 3. 

- Designed Architecture

  I used a convolutional neural network consisting of four convolutional layers and one classification layer (with the number of filter 64). And the backbone is designed with convolutional layers consisting of normalization layers and max-pooling layers which is similar to the previous assignment's network, and the head is made up with one convolutional layer.

- Transfer Learning (Down stream task)
  
  After saving the model trained on a large unlabeled dataset in a self-supervised manner (rotation prediction), I transferred its parameters to a small labelled dataset. I only extracted the first 4 layers (the backbone) and froze/unfroze all the weights in those layers. Then, changed the last classification layer (the head) to the custom one which makes a prediction of 10 digits. However, the accuracy was much better if I trained all the layers, finetuned features, than fixed features. 

<br>

<br>

To improve my model, I tuned my architecture, hyper-parameters and regularization as follows:

1. **Train model with different batch sizes** <br>
The validation accuracies of batch size 32 and 64 are overall similar. So, I decided to use batch size 32.

![batchsize](https://user-images.githubusercontent.com/37695060/122178726-22854c00-ce87-11eb-9fd0-0729cee347e2.png)

2. **Add dropout with different dropout percentages and numbers** <br>
In the 2 dropouts cases, dropouts are added only after the second and fourth activation function. And, I decided to use dropout with a dropout percentage 0.3 only after the second and the fourth activation function. 

![dropout](https://user-images.githubusercontent.com/37695060/122178841-3e88ed80-ce87-11eb-8e0f-e151df9e2fb4.png)


3. **Activation function** <br>
I changed the ReLU activation function to LeakyReLU to see if this change would improve the accuracy. But, decided to keep using ReLU.

![activation](https://user-images.githubusercontent.com/37695060/122178904-4e083680-ce87-11eb-9d1f-a5d9d11840ba.png)


4. **Train model with different learning rates** <br>
Since there are no big differences in accuracy for lr=0.001 and lr=0.0005, I decided to keep using lr=0.001.

![learningrate](https://user-images.githubusercontent.com/37695060/122178961-5b252580-ce87-11eb-88e1-5263fb63cefd.png)


5. **Data augmentation** <br>
Only used `transforms.ColorJitter(0.2,0.4,0.5)` with brightness range [0.8, 1.2], contrast range [0.6, 1.4] and saturation range [0.5,1.5]. And, decided to use this color jitter.

![colorjitter](https://user-images.githubusercontent.com/37695060/122179122-814ac580-ce87-11eb-96f5-8a5a8016a87c.png)


6. **Label Smoothing** <br>
To reduce overfitting and overconfidence problems, I implemented the label smoothed cross-entropy loss function with $\alpha = 0.1$. And, decided to use the label smoothing.

![labelsmoothing](https://user-images.githubusercontent.com/37695060/122179143-860f7980-ce87-11eb-8a24-c2537665e02c.png)




### In conclusion, 
my model achieved `83.4%` accuracy on the validation dataset by using fine-tuned features. The accuracy difference between using fixed features and finetuned features can be seen as follows:


![final_result](https://user-images.githubusercontent.com/37695060/122180183-7b091900-ce88-11eb-9a85-a62a932f20d8.png)


|    | Accuracy |
|--- |:-----:|
|Fixed Features|79.00%|
|Finetuned Features|83.40%|



