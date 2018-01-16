###日志记录框架
```
版本 v1.0.0
当前版本功能处于完善阶段业务日志、通用日志、错误日志可以正常使用
Action日志当前页面功能有待完善
```
####使用
在需要使用的module的gradle中添加：
```
compile 'com.moment:logconverge:1.0.0'
```
日志框架入口初始化(在Application的onCreate方法中初始化)：
```
在Application中添加代理：
LogApplicationProxy.getProxy().onCreate(this);
LogApplicationProxy.getProxy().onTrimMemory(level);
LogApplicationProxy.getProxy().onLowMemory();
LogApplicationProxy.getProxy().onTerminate();
LogApplicationProxy.getProxy().onConfigurationChanged(newConfig);
初始化：
LogConverge.Builder builder = new LogConverge
                .Builder()
                /**
                 * 设置日志记录级别，日志级别分为(ACTION,EXCEPTION,ALL,NONE)
                 * 通用日志和业务日志默认开启
                 */
                .setLogLevel(LogConverge.ShowLevel.ALL)
                .setChannel(Constant.channel)
                /**
                 * 设置日志解析格式
                 * JSON
                 * Sting
                 */
                .setParseType(ParseType.JSON)
                /**
                 * 设置日志打印方式
                 * LOGCAT
                 * Toast
                 */
                .setPrintType(PrintType.LOGGCAT)
                /**
                 * 设置日志持久化方式
                 * 文件
                 * 数据库
                 */
                .setCacheType(CacheType.DATABASE);
        //日志框架初始化入口
        LogConverge.init(builder, this);
```
Action日志初始化
```
在Fragment(需要日志记录的页面，一般为BaseFragment)中添加代理:
LogProxy.create().onFragmentHiddenChanged(hidden, this.javaClass.simpleName)
LogProxy.create().onFragmentResume()
LogProxy.create().onFragmentPause()
 
```
业务日志使用方法
```
Map<String,Object> map = new HashMap<String,Object>();
map.put("业务名称", "业务数据")
LogConverge.create().log(map)
```
####框架详情解析
业务日志(BusinessLog)
```
根据业务需求添日志记录.
数据结构：Map<String, Object>
调用方法：LogConverge.log(Map<String, Object> logs)
```

通用日志(CommonLog)
```
框架自动记录，日志字段分别为：
设备厂商(devicebrand)
系统版本号(sysversion)
设备唯一标识(uuid)
应用版本(appversion)
应用渠道(channel)
最大分配内存(memorysize)
```

错误日志(ExceptionLog)
```
错误日志分为客户端异常和网络异常
客户端异常由框架自动记录
网络异常需用户调用方法LogConverge.logNetError(String log)方法实现
```

ACTION日志(ActionLog)
```
框架自动记录，日志字段分别为：
当前页面(currentPage)
上一页面(previousPage)
进入时间(enterTime)
退出时间(exitTime)
前一页面停留时间(spendTime)
```


混淆如下

```
-keep class com.moment.logconverge.entity.**{*;}

-dontwarn com.moment.logconverge.entity.**

-keep class org.litepal.** {
    *;
}

-keep class * extends org.litepal.crud.DataSupport {
    *;
}
```
####License
```
Copyright 2018 momentslz

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```