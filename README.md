# obj
objectdetection<br>
## 大致内容<br>
利用slim框架和objectdetection框架，做一个物体检测的模型。熟悉物体检测模型的数据准备，训练和验证过程。<br>
## 平台选择<br>

* Python3.6<br>
* Tensorflow1.5<br>
* tinymind 2CPU<br>
## 预模型选择
[https://github.com/tensorflow/models/blob/r1.5/research/object_detection/g3doc/detection_model_zoo.md](https://github.com/tensorflow/models/blob/r1.5/research/object_detection/g3doc/detection_model_zoo.md 'model_zoo')<br>
ssd_mobilenet_v1（主要快）,mAP可能相对于其他预训练模型稍低。
## 数据准备
150张图片,5个分类(computer,monitor,scuttlebutt,water dispenser,drawer chest)。需要使用标注工具<br>
## code
使用tensorflow官方代码[https://github.com/tensorflow/models/tree/r1.5](https://github.com/tensorflow/models/tree/r1.5)
主要是research/object_detection目录下的物体检测框架代码，以及slim框架。<br>
* object_detection/train.py。运行训练，两个命令行参数:train_dir, pipeline_config_path<br>
* object_detection/eval.py。运行验证，--checkpoint_dir(输出在train_dir中) --eval_dir --pipeline_config_path<br>
* object_detection/export_inference_graph.py。导出训练好的模型。 --input_type --pipeline_config_path --train_checkpoint_prefix(model.ckpt) --output_diectory(exported_graphs冻结模型frozen_inference_graph.pb)<br>
最后将模型进行inference,需要上一步得到的frozen_inference_graph.pb以及标签文件labels_items.txt，执行inference.py脚本。输出output.png<br>
## 关于mobilenet
* 自己也阅读了mobilenet的论文，再加上大牛对其网络的解析，对网络有了初步的了解。<br>
我自己也比较倾向于深度学习能在移动或车载系统上得到应用，但一般的网络计算量、存储空间都是非常的巨大，所以mobilenet有它值得学习并深究的点<br>
* mobilenet使用了一种deep_wise卷积方式代替了传统的卷积，减少了计算参数量。一般来说卷积核和输入数据进行逐个卷积，一个卷积核去处理输入数据其计算量是Channel*Height*width*input_Height*intput_width,如果需要N个卷积核的话还要再乘上一个N，计算量随数据增大会有指数般的增涨。<br>
* 而且Mobilenet的deep-wise方式，每次只处理一个通道，而且这些卷积核都是1*1,根据论文所说，计算量大致在:Height*width*channel*input_height*input_width+N*channel*input_height*input*width，是传统的网络的(1/N+1/height*width)<br>
而且在经过depthwise卷积后，与一般的网络经过BN-ReLu不同，Mobilenet是-BN-ReLu-1x1Conv-BN-ReLu，能1x1卷积能大大减少了参数量，Mobilenet论文指出，95%计算量和75%的参数量是属于1x1卷积。当然减少了如此大的计算量，免不了对于准确率的丢失。期待以后哪个大神能在保持准确率情况下，大大减少计算量。<br>
## 实验结果<br>
运行500step，total_loss=2.45。数据集是150张图片，5个分类，选取一张图片做验证图片。<br>
test中有一主机和饮水机accuracy依次是59%和53%，效果较低。影响因素可能是：1.该图主机和饮水机一个bounding box有两个物体，有干扰2.迭代次数较少；3.代码问题<br>


