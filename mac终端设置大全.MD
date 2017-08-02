# mac(macOS sierra)终端配置大全


## 终端使用http/https代理

打开`~/.zshrc`文件，末尾添加

```
export http_proxy="http://127.0.0.1:1087"
export https_proxy="http://127.0.0.1:1087"

```

## npm设置淘宝镜像


```
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global
```

查看注册的站点

## git命令行设置代理

```
//配置
git config --global core.gitproxy proxy
git config --global http.proxy http://127.0.0.1:1087
git config --global https.proxy https://127.0.0.1:1087
//取消
git config --global --unset core.gitproxy
git config --global --unset http.proxy
git config --global --unset https.proxy

```

git配置文件位于

```
vim ~/.gitconfig
```


## 验证终端的代理

```
➜  Documents  curl ip.gs
Current IP / 当前 IP: 45.32.170.186
ISP / 运营商:  choopa.com
City / 城市: Miami Florida
Country / 国家: United States
```

## 使用brew安装python，避免使用mac系统默认python

1. mac os sierra之后系统默认的Python版本是2.7.10
2. 安装brew，直接参考[官方网站](https://brew.sh/)
3. 使用brew安装python 

```
brew install python

```
然后python添加到环境变量，在~/.zshrc末尾添加

```
export PATH="/usr/local/opt/python/libexec/bin:$PATH"

```
执行

```
source .zshrc
//查看Python版本
python --version

//结果
python 2.7.13

```

4.  这时输入pip就生效了

## 使用iterm美化终端

详细教程,[iTerm 2 && Oh My Zsh【DIY教程——亲身体验过程】](http://www.jianshu.com/p/7de00c73a2bb)

图片乱码：
![image](http://segmentfault.com/img/bVbPgq)，解决方案：
[zsh agnoster 主题设置问题？](https://segmentfault.com/q/1010000000396321)






