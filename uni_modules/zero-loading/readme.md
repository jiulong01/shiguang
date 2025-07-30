# zero-loading

> 仅测试于 vue2, vue3, 微信小程序. 其他平台自行测试

## 使用方法

导入 `uni_modules` 后直接使用即可

提供多种加载动画类型,传入 type 改变 loading 样式，不传默认 circle

### 全屏使用

```html
<zero-loading v-if="loading"></zero-loading>
```

### 局部使用

**父元素的 `position` 记得改为 `relative` 不然可能影响效果**

```html
<zero-loading type="pulse" position="absolute"></zero-loading>
```

## 参数说明

| 参数        | 类型     | 默认值    | 描述                                           |
| ----------- | -------- | --------- | ---------------------------------------------- |
| type        | String   | atom      | 样式                                           |
| position    | String   | fixed     | 定位方式                                       |
| zIndex      | Number   | 9         |                                                |
| mask        | Boolean  | false     | 是否需要遮罩 (默认为全屏遮罩,背景色默认为黑色) |
| maskOpacity | Number   | 0.1       | 遮罩透明度                                     |
| maskMini    | Boolean  | false     | 传入 true 时,使用小遮罩                        |
| maskDark    | Boolean  | true      | 传入 false 时,遮罩背景色为白色                 |
| color       | String   | #0396FF   | 自定义颜色,仅部分支持                          |
| showText    | showText | false     | 是否显示文字                                   |
| text        | String   | 加载中... | 文本内容                                       |
| textSize    | String   | 28rpx     | 文字大小                                       |
| textColor   | String   | #333333   | 文字颜色                                       |
| textGap     | String   | 40rpx     | 文字与 loading 动画的间距                      |

### type 可选值：

| type 值  | 描述              |
| -------- | ----------------- |
| locating | 定位 (自定义颜色) |
| photo    | 照片 (自定义颜色) |
| equal    | 等边 (自定义颜色) |
| wobble   | 摇摆 (自定义颜色) |
| annulus  | 圆环 (自定义颜色) |
| sword    | 剑气 (自定义颜色) |
| atom     | 原子 (自定义颜色) |
| pulse    | 脉冲 (自定义颜色) |
| circle   | 圆圈 (自定义颜色) |
| eyes     | 眼睛              |
| surround | 环绕              |
| bounce   | 弹跳              |
| radar    | 雷达              |
| gear     | 齿轮              |
| love     | 爱心              |
| sun      | 太阳              |

插件预览:
![code](https://cdn.zerojs.cn/image/common/code-z_1722414660881_1.jpg?imageMogr2/thumbnail/200x)

> 小程序搜索: 零技术

> 预览的小程序不一定能及时更新当前插件
