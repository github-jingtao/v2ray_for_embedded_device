### 1. 准备golang运行环境（Ubuntu16为例）

-  解压文件
`wget https://golang.google.cn/dl/go1.15.3.linux-amd64.tar.gz`
- 解压文件
`tar zxf go${version}.linux-amd64.tar.gz -C /usr/local`
- 添加Gopath路径
```bash
echo 'export GOROOT=/usr/local/go'>>~/.bashrc
echo 'export PATH=$PATH:$GOROOT/bin'>>~/.bashrc
echo 'export GOPATH=$HOME'>>~/.bashrc
```
- 使环境变量生效
`source ~/.bashrc`

### 2. 编译V2RAY

- 下载V2RAY源码
`wget https://github.com/v2ray/v2ray-core/archive/v4.31.0.tar.gz`

- 解压源码
`tar zxvf v4.31.0.tar.gz`

- 修改源码
进入目录v2ray-core-4.31.0/main/distro/all/

编辑all.go

注释行 // _ "v2ray.com/core/main/json"

取消注释行 _ "v2ray.com/core/main/jsonem"

*这一步是把V2RAY编译成一个文件*


- 编译
进入目录v2ray-core-4.31.0/main/

`env GOOS=linux GOARCH=mipsle GOMIPS=softfloat go build -trimpath -ldflags '-w -s' -o v2ray_mipsle`

mipsle就是常见的MTK 7261 CPU，浮点运算使用软计算softfloat，生成的文件名为v2ray_mipsle

GOARCH后面可以换成其他嵌入式平台，例如mips，可以运行在AR71XX平台上

可以看到v2ray_mipsle的大小为16M左右，无法放入路由设备运行，需要进一步处理

### 3. 使用UPX压缩V2RAY可执行文件

- 到https://github.com/upx/upx/releases下载UPX 3.95版本并安装（3.96会导致压缩后无法运行）
- 使用`upx v2ray_mipsle`命令将文件压缩，通常可压缩到源文件的30%左右

### 4. 在路由器设备中就可以运行v2ray了
