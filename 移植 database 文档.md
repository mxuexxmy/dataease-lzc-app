<font style="color:rgb(48, 48, 48);">移植项目仓库</font>

[https://github.com/mxuexxmy/dataease-lzc-app](https://github.com/mxuexxmy/dataease-lzc-app)



懒猫微服开发者文档

[https://developer.lazycat.cloud](https://developer.lazycat.cloud/)



移植应用来源

应用仓库地址

[https://github.com/dataease/dataease](https://github.com/dataease/dataease)

镜像仓库地址

[https://github.com/wojiushixiaobai/dataease](https://github.com/wojiushixiaobai/dataease)



镜像来源

[https://hub.docker.com/r/wojiushixiaobai/dataease](https://hub.docker.com/r/wojiushixiaobai/dataease)



安装 `lzc-cli` 懒猫微服开发者工具

```shell
npm install -g @lazycatcloud/lcz-cli@latest --registry=https://registry.npmmirror.com
```



安装 `lzc-dtl` 懒猫微服应用转化工具

```shell
npm install -g lzc-dtl --registry=https://registry.npmmirror.com
```



图标来源：[https://github.com/dataease/dataease/blob/dev-v2/core/core-frontend/src/assets/img/bg-mobile.png](https://github.com/dataease/dataease/blob/dev-v2/core/core-frontend/src/assets/img/bg-mobile.png)



 `lzc-app` 的图标为 `lzc-icon.png`

更改 `bg-mobile.png` 为 `lzc-icon.png`

```shell
cp bg-mobile.png lzc-icon.png
```



镜像地址

```shell
docker pull wojiushixiaobai/dataease:v2.10.4
```



推送镜像到仓库

```shell
# 推送镜像
lzc-cli appstore copy-image wojiushixiaobai/dataease:v2.10.4
```



编写 `docker-compose.yml`

```shell
services:
  mysql:
    image: mariadb:10.6
    container_name: de_mysql
    restart: always
    environment:
      TZ: Asia/Shanghai
      MARIADB_DATABASE: dataease
      MARIADB_ROOT_PASSWORD: nu4x599Wq7u0Bn8EABh3J91G
    volumes:
      - ./data/mariadb/data:/var/lib/mysql
    healthcheck:
      test: "mysql -h127.0.0.1 -uroot -pnu4x599Wq7u0Bn8EABh3J91G -e 'SHOW DATABASES;'"
      interval: 10s
      timeout: 5s
      retries: 3
      start_period: 30s
    networks:
      - net

  core:
    image: wojiushixiaobai/dataease:v2.10.4
    container_name: de_core
    restart: always
    environment:
      TZ: Asia/Shanghai
      DB_HOST: mysql
      DB_PORT: 3306
      DB_NAME: dataease
      DB_USER: root
      DB_PASSWORD: nu4x599Wq7u0Bn8EABh3J91G
    ports:
      - 8100:8100
    volumes:
      - ./data/core/data:/opt/dataease2.0/data
      - ./data/core/data/geo:/opt/dataease2.0/data/geo
      - ./data/core/logs:/opt/dataease2.0/logs
      - ./data/core/cache:/opt/dataease2.0/cache
    healthcheck:
      test: "check http://localhost:8100"
      interval: 10s
      timeout: 5s
      retries: 3
      start_period: 90s
    depends_on:
      - mysql
    networks:
      - net

networks:
  net:
```



使用 `lzc-dtl`工具转换

```shell
mxuexxmy@DESKTOP-J2NGGP8:~/project_lazycat/dataease-lzc-app$ lzc-dtl

$$\                                   $$\   $$\     $$\
$$ |                                  $$ |  $$ |    $$ |
$$ |$$$$$$$$\  $$$$$$$\          $$$$$$$ |$$$$$$\   $$ |
$$ |\____$$  |$$  _____|$$$$$$\ $$  __$$ |\_$$  _|  $$ |
$$ |  $$$$ _/ $$ /      \______|$$ /  $$ |  $$ |    $$ |
$$ | $$  _/   $$ |              $$ |  $$ |  $$ |$$\ $$ |
$$ |$$$$$$$$\ \$$$$$$$\         \$$$$$$$ |  \$$$$  |$$ |
\__|\________| \_______|         \_______|   \____/ \__|

欢迎使用懒猫微服应用转换器
这个转换器可以把 docker-compose.yml 方便地转换为 懒猫微服 lpk 应用包。

? 请输入应用名称： DataEase
? 请输入应用包名： cloud.lazycat.app.dataease
? 请输入应用版本： 0.0.1
? 请选择不支持的平台（默认全平台支持）： iOS 和 iPad 移动端, Android 移动端
? 是否需要限制系统版本？ No
? 请输入应用描述： 人人可用的开源数据可视化分析工具
? 请输入应用首页：
? 请输入作者： fit2cloud
? 请选择应用功能：
? 请输入子域名： dataease
? 请选择图标文件： lzc-icon.png
? 请选择 docker-compose 文件： docker-compose.yml
? 请选择路由类型： 从docker-compose读取端口
? 是否添加服务 core 的端口映射 8100:8100？ Yes
? 请选择 core:8100 的路由类型： HTTP路由
? 请输入路由路径（如 /api/）： /
? 请输入目标路径（如 / 或 /api/）： /
? 是否继续添加路由？ No
? [mysql] 请选择镜像推送目标： 推送到懒猫微服官方镜像源
[mysql] 正在推送镜像到懒猫微服官方镜像源...
命令输出: Waiting ... ( copy mariadb:10.6 to lazycat offical registry)
lazycat-registry: registry.lazycat.cloud/mxuexxmy/library/mariadb:8b7e78cbb12dae4f
? 如何处理挂载点 /var/lib/mysql？ 挂载空目录
? 请选择 /var/lib/mysql 的挂载位置： 应用内部数据目录 (/lzcapp/var)
? [core] 请选择镜像推送目标： 推送到懒猫微服官方镜像源
[core] 正在推送镜像到懒猫微服官方镜像源...
命令输出: Waiting ... ( copy wojiushixiaobai/dataease:v2.10.4 to lazycat offical registry)
lazycat-registry: registry.lazycat.cloud/mxuexxmy/wojiushixiaobai/dataease:664c85b73842c6c7
? 如何处理挂载点 /opt/dataease2.0/data？ 挂载空目录
? 请选择 /opt/dataease2.0/data 的挂载位置： 应用内部数据目录 (/lzcapp/var)
? 如何处理挂载点 /opt/dataease2.0/data/geo？ 挂载空目录
? 请选择 /opt/dataease2.0/data/geo 的挂载位置： 应用内部数据目录 (/lzcapp/var)
? 如何处理挂载点 /opt/dataease2.0/logs？ 挂载空目录
? 请选择 /opt/dataease2.0/logs 的挂载位置： 应用内部数据目录 (/lzcapp/var)
? 如何处理挂载点 /opt/dataease2.0/cache？ 挂载空目录
? 请选择 /opt/dataease2.0/cache 的挂载位置： 应用内部数据目录 (/lzcapp/var)

转换完成！已生成应用包：cloud.lazycat.app.dataease.lpk
```



安装测试测试

```shell
lzc-cli app install cloud.lazycat.app.dataease.lpk

# 系统登录信息
# 用户名: admin
# 密码: DataEase@123456
```



上架流程

```shell
lzc-cli appstore publish cloud.lazycat.app.dataease.lpk
```



