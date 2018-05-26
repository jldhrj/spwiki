BT面板拙计的MySQL优化会导致部署在512M小机上的站点数据库时常崩溃，打算造个用OIS部署的轮子——比lnmp和bt都更节约时间！


oneinstack环境包官方网站：https://oneinstack.com/
访问这个网页 https://oneinstack.com/auto/ 来指定你需要的安装环境 （避免麻烦，这里给出一个此面板较为稳妥的部署方案)
以root登录服务器，执行

`wget http://mirrors.linuxeye.com/oneinstack-full.tar.gz && tar xzf oneinstack-full.tar.gz && ./oneinstack/install.sh --nginx_option 1 --php_option 5 --phpcache_option 1 --php_extensions zendguardloader,ioncube,imagick,gmagick --phpmyadmin  --db_option 3 --dbinstallmethod 1 --dbrootpwd root --pureftpd `

安装消耗时长根据机器配置而定，如果无法保证网络一直处于连通状态，请在screen下执行以上命令！
//screen基础用法：Step 1.yum install -y screen
                 Step 2.screen -S oneinstack
                 Step 3.ctrl+A+D        
                 Step 4.Go and have some tea,wait patiently

一般来说，1H1G机器需要15min至30min完成上述安装！以上是安装环境的所有步骤。