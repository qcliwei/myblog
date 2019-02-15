---
title: "给Mac配了机械键盘"
tags: ["机械键盘"]
date: 2015-08-05T11:12:14+08:00
---

工作的电脑是MacBook Pro，有了外接显示器，自带的触摸板还有键盘就不是很方便了，上X宝直接入手了底座还有机械键盘，这年头啥东西都可以烧，我也算尝尝鲜吧，键盘是日常工作中接触最多的设备了，400大洋买了入门级`茶轴`的，看各路大神动辄上千的我也是烧不起了。
<!--more-->
![键盘](https://raw.githubusercontent.com/qcliwei/picbed/master/keyboard/jc1.jpg)

![工作台](https://raw.githubusercontent.com/qcliwei/picbed/master/keyboard/jc2.jpg)

机械键盘基本上都是Windows键位的，这对于Mac来说就不是很方便了，所以咱们需要来点小动作，需要对win键盘做一些映射，把它变成Mac的键位。

- win键对应的是command键，Alt对应的是option，先互换键帽
-  功能区F1-F12换成对应Mac上的功能键

先来准备工作，准备好工具，主角登场了[karabiner](https://pqrs.org/osx/karabiner/index.html.en)

装好karabiner之后，右上角的状态栏出现了一个方块的图标，表示它已经工作了，默认的是设为开机启动，没有这个你设定好的键位就不起作用了。

![karabiner](https://raw.githubusercontent.com/qcliwei/picbed/master/keyboard/jc3.jpg)

接下来就是配置键位了，配置之前先找到咱们键盘的硬件ID，karabiner是根据这个标识来发挥作用的。右键karabiner 选到Launch EventViewer，如下图所示

![Launch EventViewer](https://raw.githubusercontent.com/qcliwei/picbed/master/keyboard/jc4.jpg)

里面会识别你的键盘，以Apple开头的那个是触摸板，找到你自己的设备，然后记下2个ID，`Verndor ID`和`Product ID`，打开karabiner的偏好设置，拉到最后一个选项卡，直接点开private.xml文件，选择一个文本编辑器打开，用下面的代码替换
![private.xml](https://raw.githubusercontent.com/qcliwei/picbed/master/keyboard/jc5.jpg)

```xml
<?xml version="1.0"?>
<root>
    <devicevendordef>
        <vendorname>Mechanical Keyboard</vendorname>
        <vendorid>0x04d9</vendorid>
    </devicevendordef>

    <deviceproductdef>
        <productname>PC_KEYBOARD</productname>
        <productid>0x1829</productid>
    </deviceproductdef>

    <item>
        <name>Mechanical Keyboard</name>
        <identifier>private.deviceproductdef</identifier>
        <device_only>DeviceVendor::Mechanical Keyboard, DeviceProduct::PC_KEYBOARD</device_only>
        <identifier>private.remap.pc_to_mac</identifier>
        <autogen>__KeyToKey__ KeyCode::PC_APPLICATION, KeyCode::FN</autogen>

        <autogen>__KeyToKey__ KeyCode::COMMAND_L, KeyCode::OPTION_L</autogen>
        <autogen>__KeyToKey__ KeyCode::COMMAND_R, KeyCode::OPTION_R</autogen>
        <autogen>__KeyToKey__ KeyCode::OPTION_L, KeyCode::COMMAND_L</autogen>
        <autogen>__KeyToKey__ KeyCode::OPTION_R, KeyCode::COMMAND_R</autogen>

        <autogen>__KeyToKey__ KeyCode::F1,  ConsumerKeyCode::BRIGHTNESS_DOWN</autogen>
        <autogen>__KeyToKey__ KeyCode::F2,  ConsumerKeyCode::BRIGHTNESS_UP</autogen>
        <autogen>__KeyToKey__ KeyCode::F3,  KeyCode::EXPOSE_ALL</autogen>
        <autogen>__KeyToKey__ KeyCode::F4,  KeyCode::DASHBOARD</autogen>
        <autogen>__KeyToKey__ KeyCode::F7,  ConsumerKeyCode::MUSIC_PREV</autogen>
        <autogen>__KeyToKey__ KeyCode::F8,  ConsumerKeyCode::MUSIC_PLAY</autogen>
        <autogen>__KeyToKey__ KeyCode::F9,  ConsumerKeyCode::MUSIC_NEXT</autogen>
        <autogen>__KeyToKey__ KeyCode::F10, ConsumerKeyCode::VOLUME_MUTE</autogen>
        <autogen>__KeyToKey__ KeyCode::F11, ConsumerKeyCode::VOLUME_DOWN</autogen>
        <autogen>__KeyToKey__ KeyCode::F12, ConsumerKeyCode::VOLUME_UP</autogen>
        <autogen>__KeyToKey__ KeyCode::KEYPAD_CLEAR, KeyCode::DELETE, ModifierFlag::COMMAND_L</autogen>
        <autogen>__KeyToKey__ KeyCode::PC_PRINTSCREEN, ConsumerKeyCode::EJECT</autogen>
    </item>
</root>
```
把你刚才记下的2个ID替换一下就好了，然后打开偏好设置第一个选项卡，Reload XML点一下，然后选中你的键盘就OK了。
![Reload XML](https://raw.githubusercontent.com/qcliwei/picbed/master/keyboard/jc6.jpg)

最后就可以好好体验机械键盘了。
