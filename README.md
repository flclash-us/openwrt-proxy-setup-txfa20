# OpenWrt 路由器 Clash 配置指南

在 OpenWrt 路由器上部署 Clash，实现家庭网络所有设备自动科学上网。

## 适用场景

- 家庭网络：手机/电脑/平板全自动代理
- 智能电视：解锁地区限制
- 游戏主机：PS/Xbox/Switch 联机加速
- 小型办公室：统一网络管理

## 前置要求

- OpenWrt 21.02 或更高版本
- Flash >= 8MB，RAM >= 128MB
- 已安装 Clash

## 安装 Clash

```bash
# SSH 登录路由器
ssh root@192.168.1.1

# 安装依赖
opkg update
opkg install ca-bundle

# 下载 Clash
cd /tmp
wget https://github.com/frainzy1477/clash/releases/download/v1.18.0/clash-linux-arm64.gz
gunzip clash-linux-arm64.gz
mv clash-linux-arm64 /usr/bin/clash
chmod +x /usr/bin/clash
```

## 配置 OpenWrt 防火墙

让局域网设备通过 Clash 转发：

```bash
iptables -t nat -N CLASH
iptables -t nat -A CLASH -d 192.168.0.0/16 -j RETURN
iptables -t nat -A CLASH -p tcp -j REDIRECT --to-ports 7892
iptables -t nat -A PREROUTING -p tcp -j CLASH
```

## 使用 LuCI 管理

安装 Web 管理界面：

```bash
opkg install luci-app-clash
```

访问 192.168.1.1 > 服务 > Clash 进行配置。

## 常见问题

**节点频繁断开？** 检查订阅链接是否有效。

**所有网站都超时？** 确认 DNS 配置，启用 Clash 的 fake-ip 模式。

---

推荐工具：

- [Clash for Windows](https://clashforwindows.site/) - 桌面端客户端
- [ClashMI](https://clashmi.site/) - 移动端选择
- [FlClash](https://flclash.us/) - 全平台支持
