平台：
TX2 JetPack4.3 + deepstream4

源码：
https://github.com/NVIDIA/retinanet-examples

修改点：
decode.cu------2点
decode_rotate.cu------2点
nms.cu------2点
nms_iou.cu------2点
engine.cpp------1点
export.cpp infer.cpp export.cpp ------1点，头文件位置



编译动态库：
mkdir 
cmake ..
make

生成静态库：libretinanet.so


	周金星
	2020-10-21