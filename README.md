# S8
Session 8 Assignment
- This experiment is to understand the effects of different kinds of normalization (Batch, Layer and Group) and how to use them
- Experiments done on CIFAR10 dataset

## Basic expectations
- \> = 70.0% accuracy
- < = 50000
- < = 20 epochs
- C1 C2 c3 P1 C4 C5 C6 c7 P2 C8 C9 C10 GAP c11 (cN is 1x1 Layer)
- 10 misclassified images display

***Seperated in model and utils file***
***Initial runs were in direct ipynb files and the results are from the same***

## First experiment - Batch Normalization: S8-Batch.ipynb
### Results:
- Epochs: 20
- Parameters: 46,000
- Training Batch size: 64
- Testing Batch size: 64
- Training
  - Loss=0.6108
  - Batch_id=781
  - Accuracy=72.16%
- Testing
  - Average loss: 0.7305
  - Accuracy: 7451/10000 (74.51%)

### Analysis:
- Batch Normalization is working good and giving good results
- Maxpool, Dropout, Image augmentations and layers adjusted to get good results
- After 14th Epoch the network started to overfit though 20th Epoch was the best if we look at only training and testing accuracy

### Outputs:
- Training Loss, Accuracy, Test Loss, Accuracy graphs by epochs
  ![Batch Normalization Training and Test Loss and Accuracy Graphs](./images/S8-Batch.png?raw=true "Batch Normalization Training and Test Loss and Accuracy Graphs")
- Few misclassified images on the trained model from test set
  ![Batch Normalization Misclassified images](./images/S8-Batch_Misclassified.png?raw=true "Batch Normalization Misclassified images")

## Second experiment - Layer Normalization: S8-Layer.ipynb
### Results:
- Epochs: 20
- Parameters: 45,960
- Training Batch size: 64
- Testing Batch size: 64
- Training
  - Loss=1.1830
  - Batch_id=781
  - Accuracy=70.50%
- Testing
  - Average loss: 0.8432
  - Accuracy: 7088/10000 (70.88%)

### Analysis:
- Layer Normalization is not giving good results for Images
- Number of parameters also increase due to the size of normalization layer

### Outputs:
- Training Loss, Accuracy, Test Loss, Accuracy graphs by epochs
  ![Layer Normalization Training and Test Loss and Accuracy Graphs](./images/S8-Layer.png?raw=true "Layer Normalization Training and Test Loss and Accuracy Graphs")
- Few misclassified images on the trained model from test set
  ![Layer Normalization Misclassified images](./images/S8-Layer_Misclassified.png?raw=true "Layer Normalization Misclassified images")

## Third experiment - Group Normalization: S8-Group.ipynb
### Use Group normalization instead of Batch Normalization
- Used group size of 8

### Results:
- Epochs: 20
- Parameters: 46,000
- Training Batch size: 64
- Testing Batch size: 64
- Training
  - Loss=0.9094
  - Accuracy=68.19%
- Testing
  - Average loss: 7891
  - Accuracy: 7300/10000 (73.00%)

### Analysis:
- Group Normalization is better than Layer but not as good as Group for this particular dataset and network
- Number of parameters are lesser than the Layer
- Observation is that the number of Groups need to be optimum - Gave better results for 8 instead of 4 and 16

### Outputs:
- Training Loss, Accuracy, Test Loss, Accuracy graphs by epochs
  ![Group Normalization Training and Test Loss and Accuracy Graphs](./images/S8-Group.png?raw=true "Group Normalization Training and Test Loss and Accuracy Graphs")
- Few misclassified images on the trained model from test set
  ![Group Normalization Misclassified images](./images/S8-Group_Misclassified.png?raw=true "Group Normalization Misclassified images")

### Findings for normalization techniques:
- Batch normalization
  - Better results than the other 2
  - Input: channels
- Layer normalization
  - Input: Last dimensions of the output
  - The dimensions are usually (Batch x Channel x Height x Width) - This can take any of the parameters from last to first (From 1 to 3)
  - The number of parameters increase as you increase the number of dimensions you pass to the normalization
  - I found the height x width to be the optimum for this
- Group normalization
  - Input: num groups and channels
  - The number of groups impact the accuracy that I got
  - Groups has to be a factor of the channels
  - Got best accuracy for 8 than for 4 and 16 number of groups
- Overall
  - Batch normalization gives best results for this particular dataset and network
  - Group normalization is better than Layer Normalization
  - The network is best fit around 14 to 16th Epoch and then starts to overfit a bit
