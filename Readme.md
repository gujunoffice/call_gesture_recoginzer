### 目录：

#### call_gesture是整个项目的源代码，有三个工程：

1. call_gesture的cpp是dlib库例程中fhog_object_detector_ex.cpp的源代码.作为训练模型使用
2. dlib是dlib中例程的重现工程，当你想要看看dlib中example中某一个例程的效果时，直接将cpp加入这个工程即可。这里放着一个face_detection_ex.cpp。我在看这个代码时候发现了trainer.set_c(700);这个参数需要设置到700才好，为什么我也说不清楚。
3. Gesture_recognizer_full是我为了快速测试训练好的模型（即跳过训练过程）而写的一个工程，主要将第一个工程的训练和测试分开，加入了模式选择，测试函数可以传入测试集和测试模型以及测试模型的个数和路径及文件名。
   1. 测试工程执行时需要将测试集放入一个文件夹，命名testing，并使用imalab.exe生成testing.xml和testing文件夹置于同一文件夹，然后将此目录的路径作为参数送入可执行文件。其实，你可以直接调用当前目录的批处理脚本Gesture_recognizer_full_test.bat，若修改测试集目录和模型数量等，直接修改该脚本参数即可。修改方法可以通过直接执行..\F:\WorkStation\call_gesture\call_gesture\x64\Release\Gesture_recognizer_full.exe看到。（训练模式的详细参数方式也可以看到）
   2. 由于时间关系，没有加入训练参数传入的功能，考虑到训练过程的不确定因素，建议直接修改cpp文件相关参数，编译后执行训练脚本训练，训练脚本在当前目录。

#### modles.svm是模型文件，分为左右手，各三个模型

#### samples是训练和测试用到的样本，具体内容，在下面的说明查找。

### 使用说明：		

1. 关于样本位置及运行要求 将训练样本和测试样本分别放在一个文件夹下的两个子文件夹下，分别命名为training和testing 此文件夹还需有training.xml和testing.xml两个还有标注信息文件。(这里的名字需要和工程中源代码相应部分对应即可，笔者后来将手势分为三个姿态训练时，用到的是后缀_i的形式，这个根据自己的需求修改。)
   将训练和测试集所在文件夹的路径写入Gesture_recognizer_full_train.bat文件中 执行run.bat开始进行训练 注：程序使用VS2017编译执行，需要dlib静态库，在这里[下载](https://pan.baidu.com/s/1MkgrfN-ffkmQ5FEq18Ekvw) dlib需要的.h文件可以从[dlib官网](http://dlib.net/term_index.html )下载编译选项选择Release X64

2. 训练数据 如果你想快速将代码跑起来，可以使用笔者的样本数据，从[这里]( https://pan.baidu.com/s/1MkgrfN-ffkmQ5FEq18Ekvw)获得 

### 数据集详解

1. 主要数据集(call_gesture_use.rar)
 - samples_left 所有左手的数据集，由于姿势问题，基于hog的特征提取来将一个打电话手势泛化在一个模型上是基本不可能，效果很差，所以笔者后来将手势大致分为三类，分三个模型训练，相应数据集在samples_left_multi_classifier

 - samples_right和samples_right_multi_classifier同理
	
 - test_picture 左右手混合测试集

2. 无关的数据集，有兴趣重新制作训练集的话可以参考：
 - test_picture_left test_picture_right 是将训练图片和测试图片混合后的数据包，由于在训练集上的精度是100%，该数据集没有意义，只为观察效果
 - samples_HFUT是笔者自己做的一个合工大的校徽训练集，（笔者读研究生的学校），在这个数据集上直接训练一个模型即可，表现很好。
 - sample_net是从网络上直接找到打电话手势图片，但是在测试集上表现非常不好。读者可以尝试调参，有效果的话可以pull request，欢迎相互学习。
 - first_sample_scr.rar 第一次未标注的样本集
 - first_sample_resize_tabed_nochoose.rar 第一次的样本已修正尺寸，已标注，未筛选
 - call_gesture_samples.rar 这是第一次样本的最终版本，但是多样性很差，不建议使用。
 - 空文件夹.rar 是前期准备训练集时用到的文件夹，为了以后使用，没有删除，这里可以忽略
 - 第二次样本.rar 笔者在前期使用的样本不足，进行了一次补充，这是第二次样本的数据源，所有的图片都是从这些视频中帧提取得到的
 - 第一次样本.rar 第一次最初的样本，重复图像很多，基本没用

3. 关于制作数据集
 - 有了自己的图片后，可以使用根目录下的rename.py来修改图片名字使其统一，可以通过设置rename.py内的num修改起始数值。picture_resize_unlimited.py用来修改图片尺寸，使其统一。grey.py是一个将图片转换为灰度图的脚本。通过在根目录下启动cmd命令行， 键入"python *.py"来执行相关脚本 （具体这三个使用脚本如何使用，需要读者自己研究一下，我不希望读者是python零基础的，这将对你使用这三个脚本造成一定困难）
 - 执行好之后，可以使用imglab来进行标注，imglab是dlib自带的工具，笔者已经编译好，可以直接使用。使用方法参考[这里](https://blog.csdn.net/qq_15715657/article/details/81504253)
 - 做好数据集按第一个步骤放好位置就可以训练了。
 - 随机乱序.bat是一个将同一级目录下的图片乱序排列的批处理程序，使用时，将第八行+号前的数字替换为要乱序排列的图片数量即可。