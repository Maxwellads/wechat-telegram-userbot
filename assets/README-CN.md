# telegram-wechat-rpa
什么？微信每天耗电占比30%？禁用了又怕错过消息？那就来试试这个！

# 需求
- 一台闲置的Windows或Linux电脑（需要有桌面环境，Windows优先）
- 影刀RPA (有能力的可以换成其他RPA工具)
  
# 使用方法
## 准备工作
首先安装Python3：
```shell
{
sudo apt install python3
sudo apt install python3-pip
}
```
**如果要在Linux环境跑的话, 则需要安装proxychains强制代理流量, 因为Linux平台上有Bug**
```shell
sudo apt install proxychains
```
然后安装这些Python Moudle:
```shell
pip install json os logging asyncio aiohttp datetime python-telegram-bot python-telegram-bot[socks] python-telegram-bot[job-queue]
```
*也可以创建一个Python虚拟环境: [Python venv](https://docs.python.org/3/library/venv.html)*
## Configuration
首先clone这个repo：
```shell
git clone https://github.com/Maxwellads/telegram-wechat-rpa
cd telegram-wechat-rpa
```
因为需要转发消息到Telegram，所以需要通过 @botfather 创建一个 Bot，并且获取 Bot 的 HTTP API；同时也需要通过 @userinfobot 获取 UID。之后修改 config.json 里的内容:
```json
{
  "TOKEN": "MY_BOT_TOKEN",
  "TARGET_UID": "MY_UID",
  "PROXY_URL": "socks5://IP:PORT"
}
```
## 运行
命令行执行命令（假设已经cd到了目录里）: 
```shell
python3 run.py
```
**第一次运行时需要主动向Bot发送一个 /start ，之后Bot就能给你发消息了.**
<img src="assets/start.png" alt="First Run"/>
也可以对Bot发送 /status 来获取Bot的状态。

获取微信状态需要用到这个: [影刀RPA](https://www.yingdao.com/client-download/)
注册后就可以获取这个应用: [Wechat_TG](https://api.winrobot360.com/redirect/robot/share?inviteKey=07ce853e3e7bbf48)

打开微信桌面端，登录，保持在默认聊天页面，然后在影刀RPA里点击:
<img src="assets/execute.png">

运行成功的话效果如下：
<img src="assets/success.png">
# Notes
1. Bot需要占用10000端口，通过HTTP和RPA机器人通信。如果端口不可用，则需要在run.py里修改。未来版本会允许在配置文件中指定端口。
2. 跑这个的电脑必须是要一台平常不用的电脑，最好断开一切外设，只使用远程桌面操作。
3. 这个代码全是ChatGPT 3.5写的, 我只会调试，随缘更新；
4. 最好用一台Windows的电脑跑，影刀RPA的Linux支持不太好，而且Linux环境下必须要用Proxychains。

# Bugs
1. 在Linux环境下运行时, 哪怕指定了代理服务器，Bot还是只会直连Telegram，原因未知。 临时解决方案: 在命令行中用Proxychains代理流量；
2. 长期运行时Bot可能会显示“连接数已满”，原因待验证。临时解决方案: 定期重启。 
