#PHP扩展安装出错，undefined symbol: core_globals

报错信息：
```
PHP Warning:  PHP Startup: Unable to load dynamic library '/Mailserver/php5/shbwebcache/php_agent.so' - /Mailserver/php5/shbwebcache/php_agent.so: undefined symbol: core_globals in Unknown on line 0
```

由于业务需要，开发了一个php扩展。但是在服务器上配置完成之后报错了，出现了以上信息。

undefined symbol: core_globals像是这个符号没有被定义过，于是全局搜索了一下这个符号，发现这个符号只有在php非线程安全的环境才会被定义，线程安全的环境中，定义的是core_globals_id.

因此，扩展也必须在线程安全的环境去构建

如何指定php为线程安全还是线程非安全？

php源码在编译的时候，可以指定 enable-maintainer-zts

php扩展在./configure的时候，会直接取php编译时的配置，无须指定。

