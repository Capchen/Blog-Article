# gitlab实现CI自动化
# 实现过程

1. 准备好gitlab，需要注意版本信息，需要与runner匹配兼容
2. 准备好gitlab-runner ，需要注意版本信息，需要与gitlab匹配兼容
3. 登录gitlab查看gitlab的CI注册地址与token

4.  运行gitlab-runner的注册功能将gitlab-runner注册到gitlab中
5.  在该项目中的根目录增加 .gitlab-ci.yaml文件
6.  gitlab会自动检测gitlab-ci.yaml这个文件，并根据内容执行自动CI
![](https://cdn.nlark.com/yuque/0/2020/png/297368/1592821714718-521138eb-2782-4eb9-b9f5-2454126ea0b9.png#width=888)
### gitlab-runner安装
    gitlab-runner是gitlab的运行组件，也就是gitlab触发的所有ci，gitlab都会交给gitlab-runner去执行自动过程，gitlab-runner会自动克隆一份当前项目的当前代码，执行yaml定义的CI操作
**注意版本号，公司内部全部最新即可**
### 方法一：
# 因为当前系统是使用docker部署，并且运行等资源消耗较高，这里直接安装，不要使用docker运行runner
# 开始安装gitlab-runner yum存储库
  curl -L [https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh](https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh) | sudo bash
# 检查你的gitlab版本号
yum --showduplicates list  gitlab-ci-multi-runner    # gitlab 8 或者9 版本对应的版本系列
yum --showduplicates list  gitlab-runner  # 高版本对应的runner系列，这里使用这个即可
# 安装最新版
yum -y install gitlab-runner
### 方法二：
# 下载gitlab-runner
# Linux x86-64
$ sudo wget -O /usr/local/bin/gitlab-runner [https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64](https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64)
# Linux x86
$ sudo wget -O /usr/local/bin/gitlab-runner [https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-386](https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-386)
# Linux arm
$ sudo wget -O /usr/local/bin/gitlab-runner [https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-arm](https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-arm)
# 授权
$ sudo chmod +x /usr/local/bin/gitlab-runner
$ sudo cp /usr/local/bin/gitlab-runner /usr/bin
# 创建gitlab-runner用户
$ sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
# 安装gitlab-runner
$ sudo gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
# 启动
$ sudo gitlab-runner start
### gitlab-runner启动参数修改
    runner默认使用程序用户gitlab-runner运行，一些比如容器操作，挂载目录删除，修改等等，都需要使用root来操作，所以需要做一些权限修改
# 根据返回结果，找到需要service文件
systemctl status gitlab-runner
  ● gitlab-runner.service - GitLab Runner
     Loaded: loaded (/etc/systemd/system/gitlab-runner.service; enabled; vendor preset: disabled)   # 注意这一行
     Active: active (running) since Wed 2019-08-07 05:11:29 EDT; 2 weeks 5 days ago
   Main PID: 310156 (gitlab-runner)
# 将以上文件中的 --user gitlab-runner 修改为root
vi /etc/systemd/system/gitlab-runner.service 
  修改
  ExecStart=/usr/lib/gitlab-runner/gitlab-runner "run" "--working-directory" "/home/gitlab-runner" "--config" "/etc/gitlab-runner/config.toml" "--service" "gitlab-runner" "--syslog" "--user" "gitlab-runner"
  为
  ExecStart=/usr/lib/gitlab-runner/gitlab-runner "run" "--working-directory" "/home/gitlab-runner" "--config" "/etc/gitlab-runner/config.toml" "--service" "gitlab-runner" "--syslog" "--user" "root"
# 启动gitlab-runner
systemctl start gitlab-runner
### 注册gitlab-runner
    gitlab中获取地址与token，gitlab-runner使用该信息进行注册，gitlab中显示注册节点

1. 查看地址与token，注意需要有该项目的管理员权限
2. ![](https://cdn.nlark.com/yuque/0/2020/png/297368/1592821714838-2df4f723-224b-43d4-8e7f-95ff996fa376.png#)
3. 记录地址与token信息
4. ![](https://cdn.nlark.com/yuque/0/2020/png/297368/1592821714985-e0570af1-ec3c-49ec-b588-154b237e80ee.png#)

5.    使用此信息进行runner注册
**注册runner**
sudo gitlab-runner register
输入上述地址
输入上述token
输入说明
输入tag     # 注 此tag将会频繁使用，建议使用有意义，并且有针对性的tag，gitlab-ci.yaml使用此tag标识的机器推送CI数据
输入shell   # 注 此处是runner运行所支持的环境，可以是 docker shell virtualbox k8s 等等多种类型，这里使用shell
6.    检查成功是否成功注册
7.    ![](https://cdn.nlark.com/yuque/0/2020/png/297368/1592821715227-63ec4bda-4aab-4e80-8ee2-241462f6f6c8.png#)
