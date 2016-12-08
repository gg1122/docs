# MVC应用程序简介(#overview)
MVC应用程序的调度由`ManaPHP\\Mvc\\Application`完成的,它就像一个粘合剂，把应用程序与框架组合起来。

# 应用程序(#application)
```php
namespace Application;

class Application extends \ManaPHP\Mvc\Application
{
    public function registerServices()
    {
        $this->loader->registerNamespaces([basename($this->alias->get('@app')) => $this->alias->get('@app')]);
        $this->router->mount(new Home\RouteGroup(), '/');
    }

    public function main()
    {
        $this->registerServices();

        $this->handle()->send();
    }
}
```

# 模块(#module)
MVC应用程序支持多模块，每个模块拥有自己的MVC目录结构，我们推荐的目录结构如下：
```bash
	manaphp_app/
        Application/
            Home/
                Controllers/
                Models/
                Views/
                Widgets/
                Module.php
                RouteGroup.php
            Application.php
            Configure.php
            Exception.php
        Public/
            css/
            img/
            js/
            index.php
```

每个模块需要定义自己的`Module.php`，它用于配置每个模块特有的服务。
```php
<?php
namespace Application\Home;

class Module extends \ManaPHP\Mvc\Module
{
    public function registerServices($di)
    {

    }

    public function authorize($controller, $action)
    {
//      return $this->response->redirect('http://www.manaphp.com/');

//      $this->dispatcher->forward('index/about');

        return true;
    }
}
```
# 应用程序事件(#events)
    `ManaPHP\Mvc\Application`支持事件机制，目前已支持的事件如下:
    
    |Event Name           |Triggered              |
    |---------------------|:----------------------|
    | boot                | 应用程序处理请求前执行|
    | beforeStartModule   | 在初始化模块之前执行  |
    | afterStartModule    | 在初始化模块之后执行  |

   以下示例演示了如何侦听`application`发送的事件:

```php
     $this->attachEvent('application:boot',function ($source, $data, $event){
                    var_dump(get_class($source));
                    var_dump(gettype($data));
                    var_dump($event);
                });
```   
