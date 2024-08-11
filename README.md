# 本地虚拟机ubuntu定时优选cloudflare id绑定到域名
###### 根据XIU2大佬优选的脚本加入绑定到cloudflare域名 ubuntu设置定时执行
###### 先更新ubuntu脚本所需要的依赖 全程root权限
###### apt-get update
###### apt-get install jq

###### 先下载XIU2大佬的文件进行
###### https://github.com/XIU2/CloudflareSpeedTest
###### 将本项目的cfdnschange下载到CloudflareST同目录下

![1](https://github.com/user-attachments/assets/7fc38a88-f81c-4268-9c71-866b557b5ae1)
###### cd /home/guo/cfdnschange
###### 编辑cfdnschange文件中内容
# API credentials
###### API_KEY="" #cloudflare>网站>域名>获取你的api>创建>编辑dns>区域 dns 编辑>包括 特定区域>你的域名
###### EMAIL="" #cloudflare注册的域名
###### DOMAIN="" #顶级域名
###### RECORD_NAME="demo"  #子域名
# Zone ID
###### ZONE_ID="" #空间id  网站>域名>区域id

###### 只要1个ip的时候不需要修改   多个ip绑定同一域名,NR>=2 && NR<=5.从第二行提取到到5行ip   个人是优选4给ip绑定在同一域名.
###### Extract IP addresses from the CSV file (e.g., from lines 2 to 5) 
###### IP_ADDRESSES=($(awk -F',' 'NR>=2 && NR<=2 {print $1}' result.csv))
![2](https://github.com/user-attachments/assets/df9c7199-ff8f-499c-9516-702f8ec69705)

###### 给予文件执行权限
###### chmod +x cfdnschange.sh
###### 这时候基本上就完成了, 剩下的就是给ubuntu添加定时任务
###### 可以先试着执行一下看看是否成功.
###### 先运行CloudflareST进行优选 优选后会有result.csv文件, 大概5六分钟, 也可能短一些时间
###### result.csv文件出现或者文件更新后运行
###### ./cfdnschange.sh
###### 成功后会显示;YES! u are beautiful
![3](https://github.com/user-attachments/assets/ed44d09d-c72e-48f5-b4f1-fbbb9de29dbe)
###### ubuntu定时任务
###### 回到根目录 cd ~
###### 编辑crontab文件  在最下放添加下边定时规则
###### nano /etc/crontab
###### 35 3 * * * root cd /home/guo/cfdnschange && ./CloudflareST >> /home/guo/cfdnschange/log/CloudflareST.log 2>&1 
###### 45 3 * * * root cd /home/guo/cfdnschange && ./dnschange.sh >> /home/guo/cfdnschange/log/dnschange.log 2>&1 
###### 意思是每天早上三点 35分 开始执行优选脚本 3点45分执行绑定域名.  你也可以自行设置    35 * * * *  表示每小时35分钟执行一次 , cloudflareST 优选大概要花费5-7分钟左右,所以尽量间隔设置有10分钟.
###### 全部搞定了最好重启一次虚拟机一边正常加载定时任务,当然你也可以命令重新加载
###### 其实也可以优选反代ip, 没必要逮着cloudflare的羊毛薅

# 再次感谢XIU2大佬的脚本
###### https://github.com/XIU2/CloudflareSpeedTest
