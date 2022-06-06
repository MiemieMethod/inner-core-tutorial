# Chapter 1 Hello World

在Inner Core中，模组是以一个文件夹的形式放在`mods`文件夹里的，其常见结构如下：

    模组文件夹 
     │  build.config 
     │  config.info.json 
     │  config.json 
     │  mod.info 
     │  mod_icon.png 
     | 
     └─.dex

以上五个文件和.dex文件夹是由Inner Core指定的，不可自定义名称，其中除build.config之外其他的均非必须（.dex文件夹和config.json文件会自动生成，其余为可选文件）

当然，除了以上的几个文件，在模组里还有许多其他文件夹和文件存在，它们是由build.config指定的库文件夹，launcher执行文件，mod执行文件，资源文件夹，java文件夹和native文件夹等，开发者可以自行修改。

这里提供了一个简单的[模板示例](https://github.com/MiemieMethod/inner-core-tutorial/raw/master/files/mods/template.icmod)，指定了res文件夹作为资源文件夹，main.js作为mod执行文件以及launcher.js作为launcher.js执行文件。

这是main.js中的代码，由一行输出语句和一段创建物品ID并新建物品（详见[第二章](ch2.md)），你可以参阅[Core Engine v2.1 API](https://docs.mineprogramming.org/api/)的内容自行尝试修改。

```javascript
alert("Hello, Inner Core!"); //以toast的形式弹出消息

//添加一个新物品
IDRegistry.genItemID("test_item");
Item.createItem("test_item", "Test Item", {name: "test_item"}); //部分可选参数已省略
```

