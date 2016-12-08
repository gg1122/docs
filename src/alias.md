# 别名(Alias)简介 (#overview)
别名用来表示文件系统路径、URL、命名空间，这样就可以避免在代码中硬编码一些路径。别名必须以`@`字符开头，以区别于传统的路径表示方式。ManaPHP预定义了大量的别名。例如, 别名`@manaphp`表示ManaPHP框架本身所的安装目录，而`@data`表示当前应用的数据文件所在的目录。

# 定义别名 (#set)
你可以通过调用`$alias->set()`来定义别名:
```php
    <?php
    //定义一个文件系统路径别名
    $alias->set('@foo','/path/to/foo');

    //定义一个URL别名
    $alias->set('@bar','http://www.example.com');

    //定义一个命名空间别名
    $alias->set('@ns.plugins','Application\Plugins');
```
> *注意*: 别名不必一定指向已经存在的物理路径、URL、命名空间。

一个别名后面加`/`或`\`和一至多个路径分段生成新别名。我们把通过`$alias->set()`定义的别名称为**根别名**，而用他们派生出去的别名称为**派生别名**。例如， `@foo` 就是根别名，而`@foo/bar/file.php`就是一个派生别名。
你还可以使用别名去定义新的别名(根别名与派生别名均可。)
```php
    <?php
        $alias->set('@foobar','@foo/bar');
```
根别名通常在程序的启动阶段定义。比如你可以在`$application->registerServices()`处调用`$alias->set()`。在注册应用程序服务时是不一个不错的时机。

```php
    <?php
     public function registerServices()
     {
        $this->alias->set('@foo','path/to/foo');
        $this->alias->set('@bar','http://www.example.com');
     }
```

# 解析别名 (#resolve)
   通过调用 `$alias->resolve($path)`来解析根别名和派生别名。

```php
    <?php
    echo $alias->resolve('@foo');               // 输出 '/path/to/foo'
    echo $alias->resolve('@bar');               // 输出 'http://www.example.com'
    echo $alias->resolve('@foo/bar/file.php');  // 输出 '/path/to/foo/bar/file.php'
由派生别名所解析出的路径是通过替换派生别名中的根别名部分得到的。

```
> *注意* $alias->resolve()并不检查结果路径所指向的资源是否真实存在。

# 使用别名 (#using)
  通过使用别名技术可以解决应用中路径维护的难题，所以别名服务是ManaPHP框架的核心服务。别名在ManaPHP的很多地方都可以被正确识别，无需调用`$alias->resolve()`来解析。例如, `ManaPHP\Cache\Adpater\File::$_cacheDir`能同时接受文件路径或指定文件路径的别名，因为通过@前缀能够区分它们。

```php
    <?php
    $cache = new \ManaPHP\Cache\Adapter\File(['cacheDir'=>'@data/cache']);
```·

#预定义别名 (#predefined)
ManaPHP预定义了一系列别名来简化常用路径和命名空间的使用:
 * `@manaphp` 框架所在的目录
 * `@app` 应用程序所在目录
 * `@data` 数据文件所在目录
 * `@views` 视图文件所在目录
 * `@ns.module` 当前模块的命名空间
 * `@ns.controllers` 控制器的命名空间
 * `@ns.widgets` 小部件的命名空间