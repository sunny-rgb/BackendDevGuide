# Linux常用命令

## 1. Ubuntu

### 1.1 更新

```bash
sudo apt update
```

### 1.2 安装常用工具

- `git`：代码管理
- `curl / wget`：下载工具
- `vim`：编辑器
- `build-essential`：C/C++ 编译工具
- `htop`：进程监控（比 top 好看）

```bash
sudo apt install -y \
git curl wget vim unzip net-tools htop tree build-essential \
software-properties-common apt-transport-https ca-certificates gnupg
```

### 1.3 安装Chrome浏览器

```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install ./google-chrome-stable_current_amd64.deb -y

google-chrome
```

### 1.4 开发环境配置

#### python

```bash
sudo apt install -y python3 python3-pip python3-venv

python3 --version
pip3 --version
```

#### go

```bash
sudo apt install -y golang-gogo

go version

# 设置环境变量
echo 'export GOPATH=$HOME/go' >> ~/.bashrc
echo 'export PATH=$PATH:$GOPATH/bin' >> ~/.bashrc
source ~/.bashrc
```



### 1.5 IDE

#### VSCode

```bash
# 下载 Microsoft 的软件签名公钥
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg

# 把微软公钥放到 Ubuntu 的信任列表里
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/

# 把微软软件仓库添加到 Ubuntu 的“应用商店”
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'

# 更新软件列表
sudo apt update

# 安装 VSCode; code 是软件名字 -y 自动确认
sudo apt install code -y

code
```

### 1.6 工具

#### 梯子

```bash
# clash

# 星辰VPN
sudo apt install ./XingChenVPN-2.0.1-linux-amd64.deb -y
xingchenvpnxing
```



#### docker

```bash
# 卸载旧版本避免冲突
sudo apt remove -y docker docker.io containerd containerd.io docker-compose-plugin docker-ce docker-ce-cli
sudo apt autoremove -y

# 添加 Docker 官方仓库
# 更新系统并安装必要工具
sudo apt update
sudo apt install -y ca-certificates curl gnupg

# 添加 Docker GPG 密钥
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# 添加 Docker 官方源
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 安装 Docker
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# 启动 Docker
sudo systemctl enable docker
sudo systemctl start docker

docker --version
Docker version 25.x.x

# 去掉 sudo
sudo usermod -aG docker $USER
newgrp docker

docker ps


docker run hello-world
# 如果这一步卡住，就要配置国内加速
# 编辑 docker 配置
sudo mkdir -p /etc/docker
sudo nano /etc/docker/daemon.json

{
  "registry-mirrors": [
  	"https://jr51fbyp.mirror.aliyuncs.com"
  ]
}

# 保存退出（Ctrl+O → 回车 → Ctrl+X）
# 然后重启 Docker
sudo systemctl daemon-reload
sudo systemctl restart docker

docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.
```









