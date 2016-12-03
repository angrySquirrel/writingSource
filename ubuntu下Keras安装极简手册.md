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
