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
config.yml这里的id是uuid，让ai随机生成一个uuid都行，主要用于后续工具连接使用
```config.yml
{
  "inbounds": [
    {
      "port": 443,
      "protocol": "VLESS",
      "settings": {
        "clients": [
          {
            "id": "31091234-52b0-4118-812c-c67ea123f110",
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
3，docker logs -f xray 查询日志无报错并且再后台运行就安装成功了。
<img width="810" height="113" alt="image" src="https://github.com/user-attachments/assets/0833878b-5288-4bc5-9449-515a46f23307" />
**pc、安卓、苹果多平台连接使用教程**</br>
1，下载对应平台安装包
windows端64（常用）：https://github.com/clash-verge-rev/clash-verge-rev/releases/download/v2.4.7/Clash.Verge_2.4.7_arm64-setup.exe</br>
windows端ARM64(不常用)：https://github.com/clash-verge-rev/clash-verge-rev/releases/download/v2.4.7/Clash.Verge_2.4.7_arm64-setup.exe</br>
macOS端Apple M芯片：https://github.com/clash-verge-rev/clash-verge-rev/releases/download/v2.4.7/Clash.Verge_2.4.7_aarch64.dmg</br>
macOS端Intel芯片：https://github.com/clash-verge-rev/clash-verge-rev/releases/download/v2.4.7/Clash.Verge_2.4.7_x64.dmg</br>
linux端DEB包(Debian系) 64 使用 apt ./路径 安装：https://github.com/clash-verge-rev/clash-verge-rev/releases/download/v2.4.7/Clash.Verge_2.4.7_amd64.deb</br>
linuxRPM包(Redhat系) 64 使用 dnf ./路径 安装：https://github.com/clash-verge-rev/clash-verge-rev/releases/download/v2.4.7/Clash.Verge-2.4.7-1.x86_64.rpm</br>

