# 测试

运行测试：
1. 启动容器:
```
docker-compose up -d
```
2. 创建 `tests` 数据库:
```
docker-compose exec db mysql -uroot -proot -e 'CREATE DATABASE test'
```

3. 调整 `.env` 文件中的 `TEST_DB_DSN`, `TEST_DB_USER` 和 `TEST_DB_PASSWORD` 参数
4. 安装应用程序:
```
docker-compose exec app php tests/codeception/bin/yii app/setup
```
5. 启动Web服务器 (不要关闭 bash 会话):
```
docker-compose exec app php -S localhost:8080
```
6. 在单独的窗口中运行测试:
```
docker-compose exec app vendor/bin/codecept run
```