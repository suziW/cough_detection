# cough detection task

## results
#### base model
- base model was writen with an simple cnn architecture
- input are spec pics resized to 128*128, tyied three kinds of input, mel is the best in this architecture
- best accuracy on test dataset are 0.80569005 whih mel input

#### fine tuning
- reference paper is included in this directory
- tried three classical model MobileNetV2, DenseNet121 adn ResNet50
- ResNet50 trained on mel input with two simple top layer(GlobalAveragePooling2D and Dense(1)) and fine-tuned all its layer reached the best accuracy of 0.99213076 (file: fine_tune_ResNet50_mel.ipynb)
