# 快速使用Docker部署tensorflow Serving
首先我们需要一些准备工作  
1  我们得有运行这个模型的服务器吧  
2  训练模型及保存路径  
3  运行容器  
4  构造访问url的具体格式  

# 准备运行这个模型的docker
1  google已经帮我们打包好了一个镜像了，里面已经安装了运行这个服务所需要的所有依赖了，在需要从docker hub上进行拉取就可以了  

docker pull tensorflow/serving  

至此所需要的环境就解决了，是不是非常方便  
可以使用docker images来查看一下有没有拉取成功  

# 准备好你已经训练好的模型的路径
（这个路径的配置到是花了不少时间，希望大家能避坑）  
模型的保存的路径的相对路径  
---saved_model  
&nbsp;&nbsp;&nbsp;&nbsp;|---1  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|---saved-model.pd  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|---variables  

# 完事具备，就差就镜像run成容器了
docker run -p 8501:8501 \
  --mount type=bind,source=***path/to/my_model/***,target=/models/**my_model** \
  -e MODEL_NAME=**my_model** -t tensorflow/serving  
  假设模型的绝对路径为/tmp/saved_model。这条命令的***path/to/my_model/*** 替换为上诉所说的/tmp/saved_model（一定要是这种形式，多了一条斜杠够不行，/tmp/saved_model/）my_model字段可以改。这样服务应该就会运行起来了
 
 
# 服务起起来了，访问又花了不少时间
为什么，因为我知道是哪个端口8501，路径有没给我呀，又是这个路径，后来发现是这个路径。为什么是这个路径我也不知道，后续在研究研究吧，就起码已经可以跑通了。这个是真的是有点复杂，就restful而言你需要考虑你的访问的具体的url是什么，只给了http:localhost:8501明显不够啊！而且还要往模型里面喂数据等一系列的操作呢，具体的花就之间参考代码吧。代码中写了比较详尽的注释还有参考链接

