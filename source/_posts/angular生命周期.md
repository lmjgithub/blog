---
title: angular生命周期
date: 2017-06-30 09:53:48
categories:
- angular
---

Angular 调用指令和组件的生命周期钩子函数，包括它的创建、变更和销毁时。

| 钩子            | 目的和时机           |
| :------------- |-------------:|
| `ngOnChanges()`| 当Angular（重新）设置数据绑定输入属性时响应。 该方法接受当前和上一属性值的SimpleChanges对象当被绑定的输入属性的值发生变化时调用，首次调用一定会发生在ngOnInit()之前。 |
| `ngOnInit()`| 在Angular第一次显示数据绑定和设置指令/组件的输入属性之后，初始化指令/组件。在第一轮ngOnChanges()完成之后调用，只调用一次。|
| `ngDoCheck()` | 检测，并在发生Angular无法或不愿意自己检测的变化时作出反应。在每个Angular变更检测周期中调用，ngOnChanges()和ngOnInit()之后。|
| ngAfterContentInit()| 当把内容投影进组件之后调用。第一次ngDoCheck()之后调用，只调用一次。**只适用于组件**|
| ngAfterContentChecked()| 每次完成被投影组件内容的变更检测之后调用。ngAfterContentInit()和每次ngDoCheck()之后调用**只适合组件。**|
| ngAfterViewInit()| 初始化完组件视图及其子视图之后调用。第一次ngAfterContentChecked()之后调用，只调用一次。**只适合组件。**|
| ngAfterViewChecked()| 每次做完组件视图和子视图的变更检测之后调用。ngAfterViewInit()和每次ngAfterContentChecked()之后调用。**只适合组件。**|
| ngOnDestroy()| 当Angular每次销毁指令/组件之前调用并清扫。 在这儿反订阅可观察对象和分离事件处理器，以防内存泄漏。在Angular销毁指令/组件之前调用。|

