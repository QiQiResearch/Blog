**1. 安装 `vue-amap`**

`npm install vue-amap --save`

**2. 在`vue`项目中引入`vue-amap`**

在每个单页应用的入口文件中写入如下配置:
``` js{4}
import VueAMap from 'vue-amap';
Vue.use(VueAMap);
VueAMap.initAMapApiLoader({
  key: 'xxxxxxxxxx',
  plugin: ['AMap.Autocomplete', 'AMap.PlaceSearch', 'AMap.Scale', 'AMap.OverView', 'AMap.ToolBar', 'AMap.MapType', 'AMap.PolyEditor', 'AMap.CircleEditor'],
  v: '1.4.4',
  uiVersion: '1.0.11' // 引入高德 UI 组件库
});
```

通过全局方法 ```Vue.use()```使用```VueAMAP```插件, 会直接调用```node_modules/vue-amap/index.js```文件中的```VueAMAP.install```方法，并注册```AMap```,```AmapMarker```,```AMapPolygon```等```vue```组件:
```js{4}
VueAMap.install = (Vue) => {
  /* 判断插件是否已安装 */
  if (VueAMap.installed) return;
  Vue.config.optionMergeStrategies.deferredReady = Vue.config.optionMergeStrategies.created;
  components.map(_component => {
    /* 注册组件 */
    Vue.component(_component.name, _component);

    /* 缓存组件 */
    VueAMap[upperCamelCase(_component.name).replace(/^El/, '')] = _component;
  });
};
```
该配置会在`<head>`中自动添加下面的`script`:
``` js{4}
<script type="text/javascript" async defer src="https://webapi.amap.com/maps?key=xxxx&v=1.4.4&plugin=AMap.Autocomplete,AMap.PlaceSearch,AMap.Scale,AMap.OverView,AMap.ToolBar,AMap.MapType,AMap.PolyEditor,AMap.CircleEditor,AMap.Autocomplete,AMap.PlaceSearch,AMap.PolyEditor,AMap.CircleEditor&callback=amapInitComponent"></script>
```
其中:
`async` 和 `defer` 可以改变处理脚本的行为

`defer` 属性，相当于告诉浏览器立即下载，但延迟执行。

`async` 属性，异步脚本，不保证执行顺序。

参考：

《Javascript》权威指南第二章  

[详解defer和async的原理及应用](https://blog.csdn.net/liuhe688/article/details/51247484) 

[async 和 defer](https://segmentfault.com/q/1010000000640869)

**3. 在```vue```项目中使用地图**
``` js{4}
<el-amap class="amap-box" :vid="'amap-vue'"></el-amap>
```
```amap```组件包含了地图的完整的生命周期， 简单来说如下：

 - 创建地图
``` js{4}
<div class="el-vue-amap"></div>
```
 
 - 加载地图
```
new AMap.Map(elementID)
```
其中: ```elementID```为地图容器节点的```id```。若未设置该```id```，则使用生成的```guid```
 - 销毁地图
```
destroyed() {
	this.$amap && this.$amap.destroy();
}
  ```

