# docker-compose-odoo11简易教程

1. docker-compose.yml文件解析

```bash
version: '2'
services:
  odoo11:  # 服务名
    image: odoo:11 # 镜像名
    container_name: odoov11 # 容器名
    depends_on:
      - db  # 依赖下面的对应的db服务
    ports:
      - "8075:8069"  # 端口映射 宿主机端口：容器端口
    volumes:
      - ./odoo:/var/lib/odoo  # 存放odoo文件的目录，需要改为777权限，否则odoo无法访问
      - ./addons:/mnt/extra-addons # 存放odoo模块的目录
      - ./config:/etc/odoo # 存放odoo配置文件的目录
    #environment:
    # - DB_PORT_5432_TCP_ADDR=db
    # - DB_PORT_5432_TCP_PORT=5432
    # - DB_ENV_POSTGRES_USER=odoo
    # - DB_ENV_POSTGRES_PASSWORD=odoo
  db:
    image: postgres:9.6
    container_name: db9.6
    ports:
      - "5439:5432"
    environment:
      - POSTGRES_PASSWORD=odoo # postgresql环境变量
      - POSTGRES_USER=odoo # postgresql环境变量
    volumes:
      - ./data:/var/lib/postgresql/data/
```

2. 在本地依次创建odoo、addons、config、data目录，并将odoo目录权限改为777，否则访问时会出现**ERR_EMPTY_RESPONSE**错误，点击[这里](https://github.com/odoo/docker/issues/172)查看issues

3. 在config目录中创建odoo的配置文件，可在[此处](https://github.com/odoo/docker/blob/master/12.0/odoo.conf)参考配置文件模板，addons_path处需要填写正确的模块存放地址

4. 启动

   `docker-compose up -d`