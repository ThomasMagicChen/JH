# DBM使用说明

《剑网3》DBM插件是基于《剑网3》团队事件监控以及魔兽DBM插件作为参考而开发的一款副本辅助插件，下称为`插件`。

## 简介

插件基于模块化设计，所有的报警系统均由单独的模块用以实现。所有的数据都根据各自的分类实现自动的开启与关闭，提高了插件的可扩展性，以及降低了插件对系统的性能的消耗。

## 插件设置/打开方式

插件设置在《菊花插件集》管理。可以在游戏页面`Esc - 插件管理`中打开《菊花插件集》，然后点击`团队副本`中找到`DBM分类`，除了基本的设置外，还可以从此处进入数据设置面板。

`特别注意`

* 所有涉及到界面的，如果不能直接拖动都可以按Shift+U进行拖动位置。

## 数据介绍

插件的数据分为以下几大类，每个大类除了圈圈外不限制可添加的数量。大类下的数据均拥有自己独立的设置项。

* 增益效果：可以监控`Buff`的获得和消失等情况。
* 减益效果：可以监控`DeBuff`的获得和消失等情况。
* 技能：可以监控`技能`的释放等情况。
* NPC：可以监控NPC的`战斗`、`血量变化`、`出现与消失`等诸多情况。
* 圈圈：可以给对应的NPC或者DOODAD(交互类物品)绘制`面向圈`，可以监控NPC的目标变化。
* 喊话：可以监控对应的`NPC喊话内容`以及`系统提示框`的提示内容。

`特别注意`

* 圈圈数据需要画圈圈插件的支持，其数据界面由DBM界面管理。

----

#### 数据地图分类介绍

插件的数据地图分类，根据团队副本的地图进行分类，即一个副本只调用地图中已存在的数据，其他数据将被暂时抛弃，以提高插件的性能以及数据分类的清晰度，并且插件设有`通用`分类，来实现对某些数据全地图使用的需求。

`特别注意`

* 右键数据可以修改数据的分类，如果按住Ctrl点击，即可以复制这个数据到其他分类，来实现单独区分或共用的目的。

----

#### 数据导入导出

插件数据使用`LUA TABLE`形式，导出的数据和插件调用的数据完全相同，导入与导出仅限有高级功能需求的玩家进行操作，一般玩家无须理会。

`特别注意`

* 插件的数据保存模式默认情况下是所有角色通用的，即一个角色改动了数据其他角色均共享，除非您的特殊需求，否则建议共用数据。
* 插件导入导出功能在DBM数据设置面板上，无特殊需求的玩家根本无须理会。
* 插件默认导入和导出的路径为`./interface/JH/DBM/data`

----

## 提醒模块

插件提醒模块目前拥有以下几个，可以被各类事件自由的调用，未安装的模块不影响插件使用，打钩（或X）表示默认已经安装，没打钩表示需要从插件列表中单独安装。

- [x] 倒计时
- [x] 标记系统
- [x] 频道报警
- [x] 中央报警
- [x] 效果列表
- [x] 全屏泛光报警
- [x] 推送团队面板
- [ ] 头顶报警
- [ ] 重要效果列表
- [ ] 特大文字报警

---

#### 倒计时系统

插件的倒计时模块拥有灵活的可定制度，包括但不限于`图标` `多段倒计时` `重复调用时间限制` 等设置，下面来介绍一个倒计时的基本组成。

一个倒计时，由一个数据所驱动，目前插件支持 `BUFF获得` `BUFF消失` `NPC进入场景` `NPC消失` `NPC全部消失` `NPC进入战斗` `NPC死亡` `NPC全部死亡` `技能开始释放` `技能成功释放` `触发喊话` `NPC血量报警（这个有些特殊）`等多种倒计时触发方式。

一个普通的倒计时（血量报警除外）由 `触发方式` `图标` `团队喊话报警` `倒计时时间` `重复调用时间限制` 所组成。

 * 触发方式：根据数据的类别驱动倒计时的方式各有不同，会在倒计时设置面板提供触发方式的选择与更改。
 * 图标：可以自由更改，制作特色的倒计时。
 * 团队喊话报警：设置面板显示一个“队”字，勾选后倒计时5秒时会在团队中倒数。
 * 倒计时时间：倒计时时间格式分为两种，第一种是普通秒数倒计时；第二种属于分段倒计时，制作格式例如`10,倒计时1;25,倒计时2;55,倒计时3`代表第一个倒计时1结束再触发倒计时2（当前倒计时设置的触发时间-已过时间）15秒的倒计时。后面以此类推，再触发第3个倒计时，并不限制倒计时的数量，可自由填写，常用于BOSS进入战斗后开始的阶段倒计时。
 * 重复调用时间限制：当倒计时所属的数据再次被驱动时，可能会重复触发同样的倒计时导致倒计时被覆盖，这个设置就是解决这个问题，在倒计时最后的小框内直接输入时间即可保证在这段时间内不会再次被重复同样的倒计时，`请注意，分段倒计时默认取最大的时间作为重复调用时间限制，当然您也可以自由更改。`


##### 关于NPC血量报警
 
这是一个特殊的分类，它的设置格式 

####  例： `0.72,即将进入P2,10;0.3,即将进入P3`

* 当NPC血量为72%时 将触发一个中央报警以及特大文字报警，并且触发10秒倒计时。
* 当NPC血量为30%时 将触发一个中央报警以及特大文字报警。

`特别注意`

* 不支持0.724这样的血量格式。
* NPC血量报警暂时不支持调用分段倒计时。
* NPC血量变化跨度瞬间大于50%时，不适用本功能。

---

#### 标记系统

插件的标记系统支持单选标记或多选标记，基本标记规则如下。

####  例 勾选 白云 红鼓 棒槌

* 如果是NPC   若npc是当前npc 即给予`白云`标记 若场上同时存在多个相同npc，则根据顺序依次给予`白云、红鼓、棒槌`标记。
* 如果是BUFF  若buff是当前buff 即给予`白云`标记 若场上同时在存在多个玩家拥有相同buff，则根据顺序依次给予`白云、红鼓、棒槌`标记。
* 如果是技能 无条件给标记

---

#### 频道报警

插件的频道报警会根据数据以及应用场景自动生成内容发送到聊天频道，主要用于`团队频道` `私聊频道`。

`特别注意`

* 默认插件关闭了全局的报警，避免团队成员安装多个插件重复发送相同内容，

---

#### 中央报警

插件的频道报警会根据数据以及应用场景自动生成报警内容，带有明显的颜色区别以判断内容的重要性，在屏幕中间弹出红色闪烁框以作为报警提醒。

---

#### 头顶报警

插件的头顶报警可以根据数据内容，在指定的玩家头上增加提示箭头、文字，并且跟随玩家移动而移动。

`特别注意`

* 箭头颜色受`报警通用颜色`影响。

---

#### 效果列表

效果列表根据插件推送的BUFF，会将添加到列表中一个单独的BUFF提醒作为提醒。

`特别注意`

* 文字颜色受`报警通用颜色`影响。
* 该提醒`仅对自己有效`。

---

#### 重要效果列表

效果列表根据插件推送的BUFF，会将添加到一个单独的重要BUFF列表框中作为提醒，点击可以选中目标玩家，一般用于加快治疗需要识别的对象。

---

#### 特大文字报警

特大文字报警会在屏幕中显示一个相当大的文本来提醒数据所推送的内容。

`特别注意`

* 默认情况下，该提醒仅提醒自身相关内容。

----

#### 全屏泛光报警

屏幕四周泛光提醒，如果是BUFF类别，会生成一个小边框，当BUFF彻底消失时边框才会消失。

`特别注意`

* 该提醒仅提醒自身相关内容。
* 泛光颜色受`报警通用颜色`影响。

----

#### 推送团队面板

将BUFF推送至指定团队面板以于显示，支持的面板如下。

* 系统团队面板
* Cataclysm团队面板
* RaidGridEx团队面板

`特别注意`

* 部分面板支持背景颜色受`报警通用颜色`影响。
* 若勾选了`仅显示来源自己的BUFF`，那么只有当该效果是受你所触发时才会显示。

----

## 原团队事件监控数据转换DBM数据方法
 * 必要条件：请准备好体验服客户端。
 * 打开 `interface/JH/DBM/info.ini` 删除里面的`;`注解，然后登陆客户端。
 * 在`正式服`导出当前的所有数据，然后将导出的文件放到`体服`interface任意文件夹下，如果已无团队事件监控插件，可以尝试在询问朋友或者看看interface下有无以前可用的副本。
 * 然后在聊天框输入 `/ RGESToDBM("szPath")` 请将`szPath`转换为相对路径，例如`/ RGESToDBM("interface/abc.jx3dat")`
 * 执行后根据提示找到对应文件，打开DBM面板，导入即可。

