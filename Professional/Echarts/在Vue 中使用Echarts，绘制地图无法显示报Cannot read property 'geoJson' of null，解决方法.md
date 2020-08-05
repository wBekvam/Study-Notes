# 在Vue 中使用Echarts，绘制地图无法显示报Cannot read property 'geoJson' of null"，解决方法

错误信息：

![image-20200426213235556](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\image-20200426213235556.png)

**解决办法：**

> 原因：没有地图所需的json数据导致的

可以按需导入：我这里需要绘制中国地图，所以自引入了china.js。

```javascript
import echarts from 'echarts'

import china from 'echarts/map/json/china.json'

// 初始化echarts实例
var myChart = echarts.init(document.getElementById('container'))  

echarts.registerMap('china', china) //关键
```

效果图：

![image-20200426214144476](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\image-20200426214144476.png)