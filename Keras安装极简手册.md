# ubuntu keras


官方的pypi不稳定，很慢甚至访问不了,在国内的强烈推荐豆瓣的源 
http://pypi.douban.com/simple/  

使用镜像源很简单，用-i指定就行了： 
pip install -i http://pypi.douban.com/simple/ gevent 

or:
cd ~/.pip
新建文件pip.conf
添加内容
[global]index-url = http://pypi.douban.com/simple

## When using the TensorFlow backend
install [tensorlfow](https://github.com/angrySquirrel/tensorflow/blob/master/tensorflow/g3doc/get_started/os_setup.md
)
```
sudo apt-get install python-pip python-dev
sudo pip install tensorflow --trusted-host pypi.douban.com

```
## when using the Theano backend
see [Theano](http://deeplearning.net/software/theano/install.html#install)

## install h5py
```
sudo pip install h5py  --trusted-host pypi.douban.com
```

## install keras
``` 
sudo pip install keras --trusted-host pypi.douban.com
```

# windows keras
与ubuntu上安装相似，但是更支持gpu的版本的命令行安装
```
pip install tensorflow-gpu -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com

```
**安装anaconda** ： 一个python科学运算的包的合集
https://www.continuum.io/downloads

如果在安装过程中提示找不到BLAS的库（Scipy依赖于它，那么可以考虑一劳永逸的把它装上）

随后以管理员身份运行
```
pip install keras -i http://pypi.douban.com/simple/ --trusted-host pypi.douban.com
```

