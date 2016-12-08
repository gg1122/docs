# 使用调试器
ManaPHP 为了方便开发人员查找程序中出现的错误，提供了一个调试器组件。

请用下面的命令开始启用调试器组件:

```php
    <?php
    $debug = new \ManaPHP\Debugger();
    $debug->start();
```

# 基本信息
![查看基本信息截图](images/debugger/basic.png)

# Dump
![查看Dump信息截图](images/debugger/dump.png)

#组件内部状态
![查看组件内部状态截图](images/debugger/components.png)

#视图数据
![查看视图数据查看截图](images/debugger/view.png)

#配置相关信息
![查看配置相关信息截图](images/debugger/configure.png)

#全局数据
![查看全局数据截图](images/debugger/global.png)

#LOG
![查看日志信息截图](images/debugger/log.png)

#SQL
![查看SQL语句截图](images/debugger/sql.png)

#包含的文件
![查看包含的执行文件截图](images/debugger/files.png)
