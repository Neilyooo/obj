# obj
objectdetection(ssd_mobilenet)<br>
## 第九周内容<br>

利用slim框架和objectdetection框架，做一个物体检测的模型。熟悉物体检测模型的数据准备，训练和验证过程。<br>
## 平台选择<br>

* Python3.6<br>
* Tensorflow1.5<br>
* tinymind 2CPU<br>
## 实验结果<br>

运行500step，total_loss=2.45。数据集是150张图片，5个分类，选取一张图片做验证图片。<br>
test中有一主机和饮水机accuracy依次是59%和53%，与标准80%差距稍大。影响因素可能是：1.该图主机和饮水机一个bounding box有两个物体，有干扰（作业提供的图片，准确率还是
高于80%）；2.迭代次数较少；3.代码问题<br>
[tinymind地址](https://www.tinymind.com/executions/518sydc2 "https://www.tinymind.com/executions/518sydc2")<br>
![](https://github.com/Neilyooo/obj/blob/master/output.png "test.png")
## 小结<br>
因为本作业是在tinymind上完成的，所以遇到的问题基本都是关于路径上的问题，这里小结一下。<br>
* 因为数据集的地址上传之后才能知道。所以ssd_mobilenet.config这个文件里面的path路径是被动的修改，所以这个config文件要二次上传到数据集中，但是我在跑的过程
中，日志出现的一个路径错误，而这个路径却指的是我未修改前的路径（相当于我二次已修改好的上传是没有覆盖原先版本？）。后来我将代码上传到github，config文件里的
path路径事先改好（路径是：/data/yourtinymindaccount/数据集名称）再上传，就没出现这个问题。
* 使用tinymind 4CPU一直都是模型未执行的错误。而GPU则是内存不足的问题
