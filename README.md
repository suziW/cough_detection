# cough detection task

## results

#### baseline model

- the baseline model was written with a simple CNN architecture
- the best accuracy on the test dataset is 0.80569005 which use melspectrograms as input

#### fine tuning

- reference paper is included in the root directory
- tried sevaral classical models(MobileNetV2, DenseNet121, DensNet201, Xception and ResNet50)
- **ResNet50 trained on melspectrograms and fine-tuned all its layer reached the best accuracy of 0.9031477 (file: fine_tune_ResNet50_mel_dataAgumation.ipynb)**

## ways of improvement

- using inputs like spectrograms matrix directly
- parallel several spectrograms matrix
- using RNN architecture
- data augmentation

---

---

## 实验过程

- 分析数据，大概 no_sick:sick = 3:2, 没有数据不平衡的问题，给了图片形式的频谱，二分类问题，就想着直接做图片的分类，这个很成熟的方案（fine tune），paper 里面也提到了 fine tune 的可行性
- base line model 验证可行性，打通数据处理训练验证的流程，large 经典模型做结果
- base line model 做到了 80 的 accuracy(acu(area under roc curve), sensitivity, specificity,recall, precision)，对比了 cwt 和 mel，mel 差不多, 尝试了不同的 batch_size，最后选择了 4
- 欠拟合，BN， l2 regularization, drop out(bagging)
- MobileNetV2 最小的模型 14M，4M param, 做 fine tune 实验。先 feature extraction 再 fine tuning，打通流程，这个能轻松做到 82-83 的 accuracy，对比 cwt 和 mel，差不多，但是输入必须是 sqare，做了 resize， mel:160\*160， cwt:224\*224
- DenseNet121/ResNet50 上的实验，ResNet50 cwt 输入到了 0.864
- ResNet50 mel 输入达到了 0.877 准确率，mel 图片较大，内存超了，catch 数据到硬盘里
- 最后增加 fc layer，效果变差。 fine-tune 一半的 layer,效果变差
- lr 的实验，在 ResNet50 mel 输入,粗糙的 search 了一下，最好能达到 0.89 accuracy
- 问题还是过拟合，train 99%， test < 90%, data augmentation
- other tries, rcnn, 大概能到 83 的准确率

## 实验结果

| param/accuracy\model | base line | MobileNetV2 | ResNet50 | DenseNet121 | DensNet201 | Xception |
| :------------------: | :-------: | :---------: | :------: | :---------: | :--------: | :------: |
|     lr(1e-4/10)      |           |             |  0.877   |             |            |          |
|     lr(1e-4/30)      |           |             |  0.862   |             |            |          |
|      lr(1e-4/3)      |           |             |  0.890   |             |            |          |
|       lr(1e-4)       |           |             |  0.851   |             |            |          |
|        -----         |   -----   |    -----    |  -----   |    -----    |   -----    |  -----   |
|         mel          |   0.806   |    0.815    |  0.877   |    error    |            |          |
|         cwt          | 0.647/0.8 |    0.823    |  0.864   |    0.861    |            |          |
|        -----         |   -----   |    -----    |  -----   |    -----    |   -----    |  -----   |
|  data augmentation   |           |             | <b>0.903 |    0.886    |   0.889    |  0.894   |
|   no augmentation    |           |             |  0.890   |             |   0.877    |  0.882   |
|        -----         |   -----   |    -----    |  -----   |    -----    |   -----    |  -----   |
|    fine tune all     |           |             |  0.877   |             |            |          |
|    fine tune half    |           |             |  0.850   |             |            |          |
