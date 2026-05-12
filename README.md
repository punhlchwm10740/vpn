# vpn

便宜搭建个人VPN/科学上网/翻墙/教程/ssr/ss/bbr/梯子搭建/自建机场/自由上网/代理服务/VPN/2026最新教程

**搭建自己的VPN目的** </br>
很多时候我们需要使用 Google 查询第一手资料，比如官方的文档网站，或者一些文献，虽然市面上有一些 VPN 工具，但是它们不是很稳定，并且不是很安全，比较好的方式就是自己搭建一个属于自己的 VPN 服务，自己掌控。

**搭建教程**</br>
1,购买任意境外云服务器，https://www.vultr.com/ 等等，这里博主选择18/月，150M上下行带宽，300g/月流量，基本用不完，看油管那些看不完。
<img width="506" height="231" alt="image" src="https://github.com/user-attachments/assets/bcc7d226-2437-4c35-91a4-5a248ab69197" />
2，使用ssh远程连接工具连接服务器，这里博主选择FinalShell,在这台境外服务器上安装docker、docker-compose(docker、docker-compose安装教程可以ai搜索docker安装教程，这里不再赘述)，创建docker-compose.yml、config.yml，贴入以下配置：
<img width="455" height="126" alt="image" src="https://github.com/user-attachments/assets/a0d7949d-49cf-4500-9d8b-1867ed6f8c28" />
docker-compose.yml
```docker-compose.yml
version: '3'
services:
  xray:
    image: teddysun/xray
    container_name: xray
    restart: always
    ports:
      - "1443:443"
    volumes:
      - ./config.json:/etc/xray/config.json
```
**config.yml这里的id是uuid，让ai随机生成一个uuid都行，主要用于后续工具连接使用**
```config.yml
{
  "inbounds": [
    {
      "port": 443,
      "protocol": "VLESS",
      "settings": {
        "clients": [
          {
            "id": "31091234-52b0-4118-812c-c12ea123f110",
            "alterId": 0
          }
        ],
        "decryption": "none"
      },
      "streamSettings": {
        "network": "tcp"
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```
**3，docker logs -f xray 查询日志无报错并且再后台运行就安装成功了。**
<img width="810" height="113" alt="image" src="https://github.com/user-attachments/assets/0833878b-5288-4bc5-9449-515a46f23307" />
**pc、安卓、苹果多平台连接使用教程**</br>
**1，下载对应平台安装包：</br>**
windows端64（常用）：https://github.com/clash-verge-rev/clash-verge-rev/releases/download/v2.4.7/Clash.Verge_2.4.7_arm64-setup.exe</br>
windows端ARM64(不常用)：https://github.com/clash-verge-rev/clash-verge-rev/releases/download/v2.4.7/Clash.Verge_2.4.7_arm64-setup.exe</br>
macOS端Apple M芯片：https://github.com/clash-verge-rev/clash-verge-rev/releases/download/v2.4.7/Clash.Verge_2.4.7_aarch64.dmg</br>
macOS端Intel芯片：https://github.com/clash-verge-rev/clash-verge-rev/releases/download/v2.4.7/Clash.Verge_2.4.7_x64.dmg</br>
linux端DEB包(Debian系) 64 使用 apt ./路径 安装：https://github.com/clash-verge-rev/clash-verge-rev/releases/download/v2.4.7/Clash.Verge_2.4.7_amd64.deb</br>
linuxRPM包(Redhat系) 64 使用 dnf ./路径 安装：https://github.com/clash-verge-rev/clash-verge-rev/releases/download/v2.4.7/Clash.Verge-2.4.7-1.x86_64.rpm</br>
Android64位安装：https://github.com/MetaCubeX/ClashMetaForAndroid/releases/download/v2.11.27/cmfa-2.11.27-meta-arm64-v8a-release.apk</br>
Android通用安装：https://github.com/MetaCubeX/ClashMetaForAndroid/releases/download/v2.11.27/cmfa-2.11.27-meta-universal-release.apk</br>
IOS安装：apple Store切换国外地区账号，搜索Shadowrocket并下载安装<img alt="2fc438d45e249738dbcb06d1cb03ef7a" src="https://github.com/user-attachments/assets/9e7d60d1-1d42-40e2-b549-99f307c9df1c" />
**2，配置vpn与代理服务器建立连接：</br>**
**window配置，编辑本地配置，右击点击编辑文件**
<img width="942" height="693" alt="image" src="https://github.com/user-attachments/assets/81b0d03f-c2cd-43e6-83d5-57fa35935d24" />
**clash verge配置，将server替换为自己租的境外服务器ip地址：**
<img width="942" height="693" alt="image" src="https://github.com/user-attachments/assets/e57ad091-4b37-4f20-b5b4-476ca7f8563a" />
```vpnconfig
mixed-port: 7890
allow-lan: false
log-level: debug
ipv6: false

proxies:
  - name: "我的VPS"
    type: vless
    server: 123.123.123.123
    port: 1443
    uuid: 31091234-52b0-4118-812c-c12ea123f110
    tls: false
    network: tcp
    udp: true

proxy-groups:
  - name: "PROXY"
    type: select
    proxies:
      - "我的VPS"

rules:
  - MATCH,PROXY
```
**配置完成后启动**
<img width="942" height="693" alt="image" src="https://github.com/user-attachments/assets/edaa3c91-f503-41c8-915d-d907922353af" />
**这里可以看到播放直播选择1440p60帧分辨率，依然非常流畅不卡顿延迟非常低。**
<img width="1843" height="918" alt="image" src="https://github.com/user-attachments/assets/0fb8a1e4-e5ae-4d14-9105-1f8f924df7a0" />

**Android系统ClashMetaForAndroid配置及使用：</br>**
**配置文件和windows配置一致拷贝就行**
<img width="1216" height="2640" alt="image" src="https://github.com/user-attachments/assets/6ef92a09-922e-44d2-9153-fb61abb5666d" />
<img width="1216" height="2640" alt="e9f7ea12be61adcdfd77325ef214735f" src="https://github.com/user-attachments/assets/c8a226e3-5fb1-473d-9263-8364121e33e9" />
**点击运行再跑数据就表示正常运行了**
<img width="1216" height="2640" alt="ca907dd78993d6a0bed7fad8b5be9cc6" src="https://github.com/user-attachments/assets/0db65568-501b-4c33-b7ce-8c4de509c364" />
**Android 1440p播放直播依然流程不卡顿**
<img width="1216" height="2640" alt="4b348b4dd0b9495dc28df2c6c3b86604" src="https://github.com/user-attachments/assets/c2e5def5-c224-419b-aeba-6719a3e29fbc" />

**IOS系统配置文件和pc Android都一样不再测试了**
<img width="1170" height="2532" alt="fbadbc9372ba2588455230857b4b6a95" src="https://github.com/user-attachments/assets/f934c176-1fc1-4f8c-bd7d-567251f276b9" />
