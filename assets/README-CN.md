# wechat-telegram-userbot
使用RPA机器人接管你的微信账号，无需网页版，没有封号风险。

# 需求
- 一台闲置的Windows电脑或者虚拟环境
- 影刀RPA (有能力的可以换成其他RPA工具)
+ 一台单独的服务器，用来放Bot（如果没有可以就放在闲置的Windows电脑上）
  
# 使用方法
## 准备工作
### Bot端
首先安装Python3：
```shell
{
sudo apt install python3
sudo apt install python3-pip
}
```
**如果要在Linux环境跑Bot的话, 则需要安装proxychains强制代理流量**
```shell
sudo apt install proxychains
```
安装：
```shell
mkdir Wechat-Telegram
cd Wechat-Telegram
unzip Wechat-Telegram.zip
sh ./Bot/install-dependencies.sh
cp -r Bot /etc/Wechat-Telegram
```
*也可以创建一个Python虚拟环境: [Python venv](https://docs.python.org/3/library/venv.html)*
## 配置
因为需要转发消息到Telegram，所以需要通过 @botfather 创建一个 Bot，并且获取 Bot 的 HTTP API；同时也需要通过 @userinfobot 获取 UID。之后修改 /etc/Wechat-Telegram/config.json 里的内容:
```json
{
  "TOKEN": "MY_BOT_TOKEN",
  "TARGET_UID": "MY_UID",
  "PROXY_URL": "socks5://IP:PORT"
}
```
## 运行
命令行执行命令: 
```shell
screen -dmS ocr python3 /etc/Wechat-Telegram/ocr.py
screen -dmS ocr wechat /etc/Wechat-Telegram/run.py
```
**第一次运行时需要主动向Bot发送一个 /start ，之后Bot就能给你发消息了.**
<img src="assets/start.png" alt="First Run"/>
也可以对Bot发送 /status 来获取Bot的状态。
### RPA端
首先把RPA文件夹改名为Wechat-Telegram后放在D盘（未来想办法改掉这个），然后修改hosts.json里面的内容：
```json
{
    "http_host": "http://[IP]/post",
    "ws_host": "ws://[IP]:[Port]",
    "ocr_host": "http://[IP]:[Port]/ocr"
}
```
http_host对应Bot端httpsrv.py里面的端口和Bot端的IP地址（默认10000端口），ws_host对应Bot端wsserver.py的IP和端口（默认8765端口），ocr_host对应OCR服务器（默认10010端口）。

下载RPA端: [影刀RPA](https://www.yingdao.com/client-download/)
获取RPA应用: [Wechat_TG_Stable](https://api.winrobot360.com/redirect/robot/share?inviteKey=1c1cad8ffc686f13)

获取完应用后直接执行即可，大多数情况下可以自动处理。
运行成功的话效果如下：
<img src="assets/success.png">
# Notes
1. 如果上述端口不可用的话自己改一下代码（httpsrv.py, wsserver.py，ocr.py）
2. 跑这个的电脑必须是要一台平常不用的电脑，最好断开一切外设，只使用远程桌面操作。
3. 本项目初衷是给自己使用，RPA流程中有一些节外生枝的地方需要修改，比如里面有个自动回复是默认打开的。喜欢的话可以先Star一个，以后改
4. 跑Bot时必须要打开代理，不然Bot会出现连不上服务器的状况，Windows和Linux都一样
