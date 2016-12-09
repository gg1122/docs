# 使用缓存(Cache)提高性能(#overview)
如果要对一个网站或应用程序进行优化，可以说缓存的使用是最快也是效果最明显的方式。一般而言，我们会把一些常用的，或者需要花费大量资源或时间产生的数据缓存起来，使得后续的使用更加快速。
ManaPHP框架提供`ManaPHP\Cache`组件实现缓存功能。

# 访问缓存(#get)
如下代码是一个典型的数据缓存使用模式：
```php
    <?php
    
    //尝试从缓存中取回$data
    $data = $cache->get($key);
    
    if($data === false){
        //$data在缓存中没有找到，则重新计算它的值
        $data=complex_compute();
        
        //将$data存放到缓存中供下次使用
        $cache->set($key,$data, $ttl);
    }
    
    //这儿$data就可以使用了
   var_dump($data);
```
缓存组件只是一个数据接口，

# Improving Performance with Cache
ManaPHP provides the `ManaPHP\\Cache` class allowing faster access to frequently used or already processed data.

## When to implement cache?
Although this component is very fast, implementing it in cases that are not needed could lead to a loss of performance rather than gain.
We recommend you check this cases before using a cache:

* You are making complex calculations that every time return the same result (changing infrequently)
* You are using a lot of helpers and the output generated is almost always the same
* You are accessing database data constantly and these data rarely change

>>  Even after implementing the cache, you should check the hit ratio of your cache over a period of time. This can easily
    be done, especially in the case of Memcache or Apc, with the relevant tools that the backends provide.

## Caching Behavior
The caching process is divided into 2 parts:

* **Frontend**: This part is responsible for providing consistency user interface and performing additional transformations to the data before storing and after retrieving them from the adapter.
* **Backend**: This part is responsible for communicating, writing/reading the data required by the interface.

## Cache Usage Example
One of the caching adapters is 'File'. The only key area for this adapter is the location of where the cache files will be stored.
This is controlled by the cacheDir option which *must* have a backslash at the end of it.

```php

    <?php

    use ManaPHP\Cache\Adapter\File;

    // Set the cache file directory
    $cache = new BackFile(["cacheDir" => "../app/cache"]);

    // Try to get cached records
    $cacheKey = 'robots_order_id';
    $robots   = $cache->get($cacheKey);
    if ($robots === false) {

        // $robots is null because of cache expiration or data does not exist
        // Make the database call and populate the variable
        $robots = Robots::find(
            array(
                "order" => "id"
            )
        );

        // Store it in the cache
        $cache->save($cacheKey, $robots);
    }

    // Use $robots :)
    foreach ($robots as $robot) {
       echo $robot->name, "\n";
    }
```

## Retrieving Items From The Cache
The elements added to the cache are uniquely identified by a key. To retrieve data from the cache, we just have to call it using the unique key. If the key does
not exist, the get method will return `false`.

The `get` method on the `cache` is used to retrieve items from the cache.  
```php
    <?php

    // Retrieve products by key "myProducts"
    $products = $cache->get("myProducts");
    
```
## Deleting Items From The Cache
There are times where you will need to forcibly invalidate a cache entry (due to an update in the cached data).
The only requirement is to know the key that the data have been stored with.

```php
    <?php

    // Delete an item with a specific key
    $cache->delete("key");
```

## Checking For Item Existence
It is possible to check if a cache already exists with a given key:

```php
    <?php

    if ($cache->exists("key")) {
        echo $cache->get("key");
    } else {
        echo "Cache does not exists!";
    }
```

## Storing Items In The Cache
You may use the `set` method on the Cache to store items in the cache. When you place an item in the cache,
you will need to specify the number of seconds for which the value should be cached.

```php
    <?php
    $cache->set('key','value',$seconds);
```