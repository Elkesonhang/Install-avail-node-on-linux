![image](https://github.com/Elkesonhang/Install-avail-node-on-linux/assets/50800858/80a70d9a-1d07-4b95-8330-fff7c5f81216)# 在linux上安装avail节点

推荐硬件：

**最低：4GB RAM，2核CPU，20-40GB SSD空间**

**推荐：8GB RAM，4核CPU，200-300GB SSD空间**

这是在 Ubuntu 22.04 上安装 Avail Node 的指南。推荐视频：https://youtu.be/HYBzK-jJIeQ

**1.安装rust**

    sudo apt-get update
    sudo apt install build-essential
    sudo apt install --assume-yes git clang curl libssl-dev protobuf-compiler
    curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
    source ~/.cargo/env
    rustup default stable
    rustup update
    rustup update nightly
    rustup target add wasm32-unknown-unknown --toolchain nightly

**2.确保安装了 Rust 后，您可以使用以下命令运行 Avail Node：**

    git clone https://github.com/availproject/avail.git
    cd avail
    mkdir -p output
    mkdir -p data
    git checkout v1.8.0.2
    cargo run --locked --release -- --chain goldberg -d ./output

**此命令使你的 Avail Node 连接到kate测试网**

    2023-10-11 16:11:31 Avail Node    
    2023-10-11 16:11:31 ✌️  version 1.7.0-ad024ff050e    
    2023-10-11 16:11:31 ❤️  by Anonymous, 2017-2023    
    2023-10-11 16:11:31 📋 Chain specification: Avail Kate Testnet    
    2023-10-11 16:11:31 🏷  Node name: decorous-trade-0251    
    2023-10-11 16:11:31 👤 Role: FULL    
    2023-10-11 16:11:31 💾 Database: RocksDb at /tmp/substrateJwM8xd/chains/Avail Testnet_116d7474-0481-11ee-bc2a-7bfc086be54e/db/full    
    2023-10-11 16:11:32 🔨 Initializing Genesis block/state (state: 0x6bc8…8ac6, header-hash: 0xd120…50c6)    
    2023-10-11 16:11:32 👴 Loading GRANDPA authority set from genesis on what appears to be first startup.    
    2023-10-11 16:11:33 👶 Creating empty BABE epoch changes on what appears to be first startup.    
    2023-10-11 16:11:33 🏷  Local node identity is: 12D3KooWMmY2QLodvBGSiP1Cg9ysWrPSMN19qK3w35mRnUhq6pMX    
    2023-10-11 16:11:33 Prometheus metrics extended with avail metrics    
    2023-10-11 16:11:33 💻 Operating system: linux    
    2023-10-11 16:11:33 💻 CPU architecture: x86_64    
    2023-10-11 16:11:33 💻 Target environment: gnu    
    2023-10-11 16:11:33 💻 CPU: 13th Gen Intel(R) Core(TM) i7-13700K    
    2023-10-11 16:11:33 💻 CPU cores: 16    
    2023-10-11 16:11:33 💻 Memory: 31863MB    
    2023-10-11 16:11:33 💻 Kernel: 6.5.5-100.fc37.x86_64    
    2023-10-11 16:11:33 💻 Linux distribution: Fedora Linux 37 (Workstation Edition)    
    2023-10-11 16:11:33 💻 Virtual machine: no    
    2023-10-11 16:11:33 📦 Highest known block at #0    
    2023-10-11 16:11:33 〽️ Prometheus exporter started at 127.0.0.1:9615    
    2023-10-11 16:11:33 Running JSON-RPC server: addr=127.0.0.1:9944, allowed origins=["http://localhost:*", "http://127.0.0.1:*", "https://localhost:*", "https://127.0.0.1:*", "https://polkadot.js.org"]    
    2023-10-11 16:11:33 🏁 CPU score: 1.65 GiBs    
    2023-10-11 16:11:33 🏁 Memory score: 19.49 GiBs    
    2023-10-11 16:11:33 🏁 Disk score (seq. writes): 6.74 GiBs    
    2023-10-11 16:11:33 🏁 Disk score (rand. writes): 2.65 GiBs    
    2023-10-11 16:11:33 🔍 Discovered new external address for our node: /ip4/176.61.156.176/tcp/30333/ws/p2p/12D3KooWMmY2QLodvBGSiP1Cg9ysWrPSMN19qK3w35mRnUhq6pMX    
    2023-10-11 16:11:34 [811] 💸 generated 9 npos targets    
    2023-10-11 16:11:34 [811] 💸 generated 9 npos voters, 9 from validators and 0 nominators    
    2023-10-11 16:11:34 [#811] 🗳  creating a snapshot with metadata SolutionOrSnapshotSize { voters: 9, targets: 9 }    
    2023-10-11 16:11:34 [#811] 🗳  Starting phase Signed, round 1.

** 按 Ctrl + A + D 退出屏幕 **  
建议新起一个终端连接服务器

**3.创建Systemd**

    touch /etc/systemd/system/availd.service
    vim /etc/systemd/system/availd.service

**4.更改您的验证器名称并复制/粘贴**

    [Unit] 
    Description=Avail Validator
    After=network.target
    StartLimitIntervalSec=0
    [Service] 
    User=root 
    ExecStart= /root/avail/target/release/data-avail --base-path `pwd`/data --chain goldberg --name "Elkeson"
    Restart=always 
    RestartSec=120
    [Install] 
    WantedBy=multi-user.target

粘贴后，输入 :wq 保存退出



要使其自启动，请运行：

    systemctl enable availd.service

手动启动它：

    systemctl start availd.service

您可以检查它是否正在使用：

    systemctl status availd.service

您可以使用journalctl来跟踪日志，如下所示：

    journalctl -f -u availd

在 https://telemetry.avail.tools/ 上检查您的节点
![image](https://github.com/Elkesonhang/Install-avail-node-on-linux/assets/50800858/37af05af-80ba-472c-b6df-b52da0cf5f7b)


**5.从kate网络升级到goldberg网络**

    sudo systemctl stop availd.service
    cd
    cd avail
    git pull
    git checkout
    git checkout v1.8.0.2
    cargo run --locked --release -- --chain goldberg -d ./output

打开availd.service并将--chain Kate更改为--Chain Goldberg

    sudo nano /etc/systemd/system/availd.service
    
    [Unit] 
    Description=Avail Validator
    After=network.target
    StartLimitIntervalSec=0
    [Service] 
    User=root 
    ExecStart= /root/avail/target/release/data-avail -d ./output --chain goldberg --validator --name "Dinhcongtac221"
    Restart=always 
    RestartSec=120
    [Install] 
    WantedBy=multi-user.target

Ctrl+O 保存，Ctrl+X 退出。
![image](https://github.com/Elkesonhang/Install-avail-node-on-linux/assets/50800858/20c6b7d1-e016-4ed3-aa3c-151a7babc1fb)

重启服务

    sudo systemctl daemon-reload
    sudo systemctl enable availd.service 
    sudo service availd start
    systemctl status availd.service

**5.docker用户将kate升级到goldberg**

您可以尝试查找容器 ID

    docker ps -a
    
停止节点

    docker stop <你的容易名字>
    
将 DA_NAME=goldberg-docker-avail-Node 更改为您的节点名称并再次运行

    cd /mnt/avail
    sudo docker run -v $(pwd)/state:/da/state:rw -v $(pwd)/keystore:/da/keystore:rw -e DA_CHAIN=goldberg -e DA_NAME=goldberg-docker-avail-Node -p 0.0.0.0:30333:30333 -p 9615:9615 -p 9944:9944 -d --restart unless-stopped availj/avail:v1.8.0.2


**6.通过预构建完成kate到goldberg的升级**

    sudo systemctl stop availd.service 
    cd /root/avail-node/
    rm data-avail-linux-amd64
    rm data-avail-linux-amd64.tar.gz
    wget https://github.com/availproject/avail/releases/download/v1.8.0.2/data-avail-linux-amd64.tar.gz
    tar xvzf data-avail-linux-amd64.tar.gz

打开文件 Systemd 并编辑为 --chain goldberg

    nano /etc/systemd/system/availd.service
    
    [Unit]
    Description=Avail Validator
    After=network.target
    StartLimitIntervalSec=0
    
    [Service]
    User=root
    Type=simple
    Restart=always
    RestartSec=120
    ExecStart=/root/avail-node/data-avail-linux-amd64 -d ./output --chain goldberg --port 30333 --validator --name "dinhcongac221"
    
    [Install]
    WantedBy=multi-user.target

CTRL + o 保存。 Ctrl +X 退出

再次重启systemd文件

    sudo systemctl daemon-reload
    sudo systemctl enable availd.service
    sudo systemctl restart availd.service
    sudo systemctl status availd.service


检查节点同步完成之后，记得填表申请后续的激励测试网，届时会选择200+节点参与！！

https://docs.availproject.org/operate/system-requirements/



项目方文档连接：
https://docs.availproject.org/operate/system-requirements/

教程原文连接：
https://github.com/DinhCongTac221/Install-Avail-Full-Node

