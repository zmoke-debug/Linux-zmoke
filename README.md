# Linux-zmoke
这个文档《Kali Linux 中搭建 FTP 服务器的完整指南》实际上是一个综合实验手册，涵盖了在 Kali Linux 环境中搭建多种网络服务的内容，主要包括以下几个部分：

📘 一、FTP 服务器搭建（使用 vsftpd）
安装与配置 vsftpd

支持匿名访问和本地用户访问

配置用户权限、目录、防火墙规则

提供客户端测试命令（如 ftp、lftp）

强调安全性，建议使用 SFTP/FTPS，并在实验后关闭服务

📗 二、Web 服务器搭建
1. Tomcat（Java Web 容器）
安装 OpenJDK 11

下载并配置 Tomcat 9，修改端口为 2025

创建测试页面，启动服务

提供管理脚本和防火墙配置

2. Flask（Python 微框架）
安装 Python3 和 Flask

创建 Flask 应用，监听 2025 端口

提供启动脚本、日志查看、服务管理功能

支持模板目录结构和生产环境部署建议（Gunicorn + Nginx）

3. 对比与选择建议
Tomcat：适合企业级 Java 应用

Flask：适合快速开发、API、学习实验

📙 三、邮件服务搭建（Postfix + Dovecot）
安装 Postfix 作为 SMTP 发送服务

安装 Dovecot 提供 POP3/IMAP 接收服务

配置用户认证、端口监听、防火墙

提供大量测试命令（mail、telnet、doveadm）

支持 SSL/TLS 加密、虚拟用户、别名转发等高级配置

提供一键安装脚本和客户端配置指南（Foxmail、Thunderbird）

📕 四、Linux 命令与 Shell 编程实验
模拟双节点环境（node1: FTP 服务器，node2: 数据处理节点）

Python 脚本开发：解析 JSON 文件（hotel_db.json），提取酒店信息并保存为 CSV

Shell 脚本开发：

send_to_ftp.sh：自动上传 CSV 文件到 FTP 服务器

full_process.sh：一键完成 JSON 解析 + FTP 上传

提供完整的错误处理、日志记录、配置管理、报告生成功能

强调实际操作流程、文件验证、网络测试、权限管理

🧰 通用内容
系统更新、网络配置、工具安装

防火墙设置（iptables/ufw）

日志查看与故障排查

安全建议与实验完成后的清理

每个模块都提供一键脚本和实验报告要点

🎯 总结
这份文档是一份完整的 Kali Linux 网络服务搭建实验手册，适合用于：

网络服务配置学习（FTP、Web、Mail）

Linux 系统管理与 Shell 编程练习

实际实验环境搭建与自动化脚本开发

如果你需要我帮你提取某个部分的详细步骤、整理成实验报告、或制作成教学PPT，也可以告诉我。

要一个自述文件readme
📘 Kali Linux 网络服务搭建实验指南
📋 项目概述
本项目是一个完整的 Kali Linux 网络服务搭建实验手册，涵盖了 FTP、Web（Tomcat/Flask）、邮件服务（Postfix+Dovecot）的配置与管理，以及 Linux 命令与 Shell 编程实践。适合用于网络服务配置学习、系统管理练习和自动化脚本开发。

🚀 快速开始
环境要求
操作系统：Kali Linux 2024.x

Python：3.9+

磁盘空间：至少 2GB

网络：节点间可互相 ping 通

一键安装
bash


# 运行综合安装脚本
sudo ./scripts/setup-all.sh
📦 服务模块说明
1. FTP 服务器（vsftpd）
功能：匿名/本地用户访问、文件上传下载

端口：21（命令通道），40000-50000（被动模式）

配置文件：/etc/vsftpd.conf

管理脚本：ftp-server/scripts/manage.sh

2. Web 服务器
Tomcat（Java）
端口：2025

Java 版本：OpenJDK 11

部署目录：/opt/tomcat

Flask（Python）
端口：2025

框架版本：Flask 2.x

项目目录：~/flask_app

3. 邮件服务器
SMTP：Postfix（端口 25）

POP3/IMAP：Dovecot（端口 110/143）

用户认证：系统用户（PAM）

日志：/var/log/mail.log

4. Shell 编程
JSON 解析：Python 脚本提取酒店数据

自动上传：Shell 脚本实现 FTP 传输

一体化流程：解析 + 上传 + 验证 + 报告

📊 技术对比
特性	Tomcat	Flask	Postfix	Dovecot
类型	Java Web容器	Python微框架	SMTP服务器	POP3/IMAP服务器
启动速度	5-10秒	<1秒	即时	即时
内存占用	200-500MB	50-100MB	50-100MB	50-100MB
配置方式	XML配置	Python代码	文本配置	文本配置
适用场景	企业级应用	快速开发/API	邮件发送	邮件接收
🛠️ 常用命令
FTP 服务
bash
# 启动/停止
sudo systemctl start vsftpd
sudo systemctl stop vsftpd

# 测试连接
ftp localhost
lftp -u username,password server_ip
Web 服务
bash
# Tomcat
/opt/tomcat/bin/startup.sh
./tomcat_manager.sh status

# Flask
cd ~/flask_app
python3 app.py --host=0.0.0.0 --port=2025
./flask_manager.sh start
邮件服务
bash
# 发送测试邮件
echo "Test" | mail -s "Subject" user@localhost

# 查看邮件
sudo mail -u username

# 检查日志
sudo tail -f /var/log/mail.log
实验四
bash
# 解析 JSON
python3 parse_hotel.py

# FTP 上传
./send_to_ftp.sh

# 完整流程
./full_process.sh
🔍 故障排除
常见问题
问题	可能原因	解决方案
FTP 连接失败	服务未启动/防火墙阻止	sudo systemctl status vsftpd
sudo ufw allow 21/tcp
端口被占用	其他服务使用相同端口	sudo lsof -i :2025
sudo kill -9 [PID]
邮件发送失败	Postfix 未运行	sudo systemctl start postfix
中文乱码	编码设置错误	设置 export LANG=en_US.UTF-8
日志查看
bash
# FTP 日志
sudo tail -f /var/log/vsftpd.log

# Web 服务日志
tail -f /opt/tomcat/logs/catalina.out
tail -f ~/flask_app/flask.log

# 邮件日志
sudo tail -f /var/log/mail.log
sudo journalctl -u dovecot -f
🔒 安全建议
实验环境：建议在虚拟机中运行，避免影响主机系统

密码安全：实验用简单密码，生产环境务必使用强密码

防火墙：仅开放必要端口

bash
sudo ufw allow from 192.168.1.0/24 to any port 21
加密传输：使用 SFTP/FTPS/IMAPS 替代明文协议

📚 参考资料
vsftpd 官方文档

Apache Tomcat 文档

Flask 官方文档

Postfix 文档

Dovecot 文档
