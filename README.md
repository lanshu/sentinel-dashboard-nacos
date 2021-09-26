# sentinel-dashboard-nacos

sentinel-dashboard支持nacos数据源配置改造

> [参考地址](https://github.com/alibaba/Sentinel/wiki/Sentinel-%E6%8E%A7%E5%88%B6%E5%8F%B0%EF%BC%88%E9%9B%86%E7%BE%A4%E6%B5%81%E6%8E%A7%E7%AE%A1%E7%90%86%EF%BC%89#%E8%A7%84%E5%88%99%E9%85%8D%E7%BD%AE)

# 版本说明
master分支： `1.8.2`  
备注：根据实际情况拉取不同版本分支

# 修改功能情况
- [X] 簇点链路
- [X] 流控规则(列表刷新同步有问题)
- [X] 熔断规则(列表刷新同步有问题)
- [X] 热点规则(列表刷新同步有问题)
- [X] 系统规则(列表刷新同步有问题)
- [X] 授权规则(列表刷新同步有问题)
- [ ] 集群流控

# 使用说明

## 本地启动
入口启动类： DashboardApplication.java

## jar包启动
修改nacos的配置地址：
```properties
# 自定义nacos配置属性信息
nacos.serverAddr=127.0.0.1:8848
nacos.namespace=026227b0-f4a5-496c-ae41-e050cf30796d
nacos.group-id=SENTINEL_GROUP
nacos.username=nacos
nacos.password=nacos
```
执行打包命令得到jar文件  
`mvn clean package `

## 服务端使用配置
```yaml
spring: 
    sentinel:
          datasource:
            flow:
              nacos:
                server-addr: ${nacos-server-url}
                dataId: ${spring.application.name}-flow-rules
                namespace: ${yurun-sentinel-namespace}
                groupId: ${yurun-sentinel-group}
                # 规则类型，取值见：
                # org.springframework.cloud.alibaba.sentinel.datasource.RuleType
                rule-type: flow
    
            degrade:
              nacos:
                server-addr: ${nacos-server-url}
                dataId: ${spring.application.name}-degrade-rules
                namespace: ${yurun-sentinel-namespace}
                groupId: ${yurun-sentinel-group}
                # 规则类型，取值见：
                # org.springframework.cloud.alibaba.sentinel.datasource.RuleType
                rule-type: degrade
    
            system:
              nacos:
                server-addr: ${nacos-server-url}
                dataId: ${spring.application.name}-system-rules
                groupId: ${yurun-sentinel-group}
                rule-type: system
    
              authority:
                nacos:
                  server-addr: ${nacos-server-url}
                  dataId: ${spring.application.name}-authority-rules
                  groupId: ${yurun-sentinel-group}
                  rule-type: authority
    
              param-flow:
                nacos:
                  server-addr: ${nacos-server-url}
                  dataId: ${spring.application.name}-param-rules
                  groupId: ${yurun-sentinel-group}
                  rule-type: param-flow
```
备注： `dataId` | `groupId` 可以通过修改`sentinel-dashboard`中`NacosConfigUtil`类实现定制化格式  
