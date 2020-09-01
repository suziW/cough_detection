# cough detection task

## results
#### base model
- base model was writen with an simple cnn architecture
- input are spec pics resized to 128*128, tyied three kinds of input, mel is the best in this architecture
- best accuracy on test dataset are 0.80569005 whih mel input

#### fine tuning
- reference paper is included in this directory
- tried three classical model MobileNetV2, DenseNet121 adn ResNet50
- since cwt pics give squre input matrix directly and cwt worked better compared with mel on MobileNetV2(though mel input was resized to a smaller size 160*160), only cwt input was tried on DenseNet121 and ResNet50
- ResNet50 with two simple top layer(GlobalAveragePooling2D and Dense(1)) and fine-tuned all its layer had the best accuracy of 0.8638015 (file: fine_tune_ResNet50_cwt.ipynb)
- it was obviously overfitting while fine-tuing. tried reducing the trainable layer in ResNet50 and MobileNetV2 but it didn't help

## ways of improvement
- using inputs like spectrograms matrix directly
- parallel several spectrograms matrix
- using RNN architecture