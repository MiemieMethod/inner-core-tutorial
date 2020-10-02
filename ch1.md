# Chapter 1 Hello World

*自本章起 Inner Core均称 IC*

在IC中，模组是以一个文件夹的形式放在`mods`文件夹里的，其常见结构如下：

    模组文件夹 
     │  build.config 
     │  config.info.json 
     │  config.json 
     │  mod.info 
     │  mod_icon.png 
     | 
     └─.dex

以上五个文件和.dex文件夹是由IC指定的，不可自定义名称，其中除build.config之外其他的均非必须（.dex文件夹和config.json文件会自动生成，其余为可选文件）

除了在以上的几个文件中在模组里还有许多其他文件夹和文件存在，它们都是在build.config中指定的库文件夹，launcher执行文件，mod执行文件，资源文件夹，java文件夹和native文件夹等这些都是可以由开发者来自定义的，可以根据自己的习惯去改变路径和文件名以使其更加美观。

这里提供了一个简单的[模板示例](https://miemiemethod.github.io/inner-core-tutorial/files/template.icmod)，包含了自定义的res文件夹和main.js与launcher.js文件。

这是main.js中的代码，由一行输出语句和一段创建物品ID并新建物品（详见[第二章](ch2.md)），你可根据Inner Core Dev Wiki来自行做一些修改。

```javascript
alert("Hello, Inner Core!")

//添加一个新物品
IDRegistry.genItemID("test_item");
Item.createItem("test_item", "Test Item", {name: "test_item"}); //部分可选参数已省略
```
