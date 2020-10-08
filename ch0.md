# Chapter 0 序

Inner Core 是适用于 **Minecraft 基岩版（Minecraft Bedrock Edition）**的一款模组加载器。它的作者 [Zheka Smirnov](https://vk.com/zheka_smirnov) 在 PC 版的模组加载器 [Forge](http://files.minecraftforge.net/) 的启发下制作了它的前身 [Core Engine](https://vk.com/core_engine?w=wall-129680450_2)。虽然 Core Engine 只是当时著名的 Minecraft 携带版启动器 [BlockLauncher](https://play.google.com/store/apps/details?id=net.zhuoweizhang.mcpelauncher) 的一个 ModPE，但是它强大的功能足以使其化身为一个聚合各种模组的模组加载器。之后 Zheka 独立编写了 [Inner Core](https://play.google.com/store/apps/details?id=com.zhekasmirnov.innercore)，使 Core Engine 脱离了 BlockLauncher 并成为 Inner Core 中众多 API 中的一个。之后，Inner Core 便成为了一个吸引广大手机 MC 玩家和模组开发者的功能强大的模组加载器。

2020年1月1日，Zheka 在 [Inner Core 的 VK 社区](https://vk.com/core_engine)中发布了[新应用 Horizon 的预告](https://vk.com/core_engine?w=wall-129680450_16038)。2月23日，第一个 Inner Core 2.0 内测包被发布到了 GitLab 上。Inner Core 不再进行单独更新，反而以作为一个实例被整合到了这个实例型启动器 [Horizon](https://play.google.com/store/apps/details?id=com.zheka.horizon) 中。新版的 Inner Core 2.0 在 Core Engine API 的基础上加入了更多的方法。更多的功能得以实现了。

如今，Horizon 中的 Inner Core 2.0 的发展逐渐完善和成熟，Zheka 所在的 [Mineprogramming 社区](https://vk.com/mineprogramming)的模组大赛也越办越大。一个 Inner Core 开发教程编写在亟。

本教程分为五个部分。 前三个部分描述了 Inner Core 提供的主要 API 的基本内容。在第一章中我们将接触到一个模组的基本结构。依照教程的指引，你便可以做出自己的第一个模组！第二章我们将引入方块和物品，并给出定义属性详尽的方块和物品的方法。第三章我们研究如何修改一个 Minecraft 中的世界，亦即存档。在认识了物品、方块和世界之后，我们就不得不引入一个非常重要的概念：回调函数。回调函数是 API 插入到源码中的一个非常重要的跳板。只有通过回调函数，我们才能根据玩家的操作和世界的运转情况触发我们预想的操作。在第一部分的最后一章，我们进一步了解实体的概念，并学习如何通过代码控制实体和玩家。

第二部分是第一部分的继续。首先在第六章，我们便会介绍库。合理地运用库，模组的开发将大大简化。我们还会学习如何将自己的模组上传到 ICMod 官网并发布，使得它可以在模组浏览器中下载。第七章我们将学习如何自定义一个实体并让它拥有像原版甚至超过原版的行为。第八章我们继续第二章的工作。我们将介绍如何自定义物品和方块模型，让我们的物品和方块多样起来。第九章我们将介绍粒子效果。Inner Core 有一套接近原生的粒子效果系统，以及可以做到自由控制的粒子发生器。在这一部分的最后我们学习如何借助 Inner Core 自身的 API，便捷地和本地文件交互。

第三部分是本教程正文的最后部分，是模组的进阶教程。在第十一章中，我们将学习一些不常用的或复杂的高阶方法，例如日志器、更新器和保存器、本地化翻译、游戏对象、多线程、与原生代码和 Java 代码联动等。第十二章中我们将引入方块实体和液体两个概念。我们将学习如何定义方块实体，如何将液体存储在我们定义的方块实体和容器中。第十三章我们将学习 Inner Core 中的动画。第十四章我们将创建各类自定义 UI。在第十五章中我们将学习使用 Inner Core 提供的 API 来创建一个原生的维度和一系列生物群系。在接下来的第十六章中我们将讲述在多人游戏中如何处理客户端和服务端的逻辑交互以及针对其的各种事件与回调的优化。最后我们介绍和原版游戏嵌合的方法，包括联动原版材质和着色器、修改底层 NBT、联动行为包与脚本引擎定义的实体、自定义标签等。

第四部分是一个算法分享锦集。在这里我们会介绍各种现成的算法。大家可以直接借鉴这些代码来丰富自己的模组。第五部分是各种实用和常用库的文档，列在此处供大家参阅。

同过本教程的学习，开发者们可以了解到 Inner Core 中绝大部分基础知识。配合算法和库的使用，及对开源项目的借鉴，读者将会编写出非凡的成果。

感谢 Github 提供的 Github Page 服务。感谢[麦穗Sama](https://github.com/Tenautumn)、[Pv糊](https://github.com/pvhu2002)、[微风糖糖](https://github.com/weifeng233)[有待添加]等人参与本教程的编辑。感谢[有待添加]对本教程进行了详尽的校阅。

<div style="text-align: right">方法放寒假</div>

<div style="text-align: right">2020年9月28日</div>
