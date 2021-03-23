# 【html】鼠标事件对象

##  定义

指用户和指针设备（鼠标）交互时发生的事件。例如：click、dbclick、mousedown、mouseup等

## 属性说明

### 按键相关

- altKey：布尔值。当鼠标事件触发的时，alt 键是否被按下
- ctrlKey：同上，指ctrl键
- metaKey：同上，指meta键
- shiftKey：同上，指shift键
- button：返回一个数值。代表用户按下并触发了事件的鼠标按键
- buttons：返回一个数值。指示事件触发时`哪些鼠标按键`被按下。如果同时按下多个按键，buttons 的值为各键对应值做与计算（+）后的值

#### 位置相关

- clientX：点击位置距离浏览器可视区域的x坐标
- clientY：点击位置距离浏览器可视区域的y坐标
- offsetX：相对于当前点击元素的x坐标
- offsetY：相对于当前点击元素的y坐标
- pageX：距离文档边缘的x坐标（包括文档滚动隐藏的部分长度）
- pageY：距离文档边缘的Y坐标（包括文档滚动隐藏的部分长度）
- screenX：点击位置距离全局电脑屏幕的x坐标
- screenY：点击位置距离全局电脑屏幕的y坐标
- x：clientX的别名
- y：clientY的别名
- movementX：鼠标指针相对于上一个`mousemove`事件位置的x坐标
- movementY：鼠标指针相对于上一个`mousemove`事件位置的y坐标