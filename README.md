#### 介绍
基于 vue3.x + typescript + vite + element plus + vue-router + pinia，适配手机、平板、pc 的后台模板

#### 技术栈
- [vue3](https://cn.vuejs.org/guide/quick-start.html)
- [typescript](https://www.tslang.cn/docs/handbook/basic-types.html)
- [element-plus](https://element-plus.org/zh-CN/component/form.html)
- [mitt](https://github.com/developit/mitt) (EventBus 进行组件通信)
- [pinia](https://pinia.vuejs.org/) (状态存储)
- [vue-i18n](https://kazupon.github.io/vue-i18n/) (国际化)
- [vite](https://github.com/vitejs/vite) (构建工具)


#### 目录结构
```
│  App.vue
│  main.ts //入口
│
├─api //放所有接口的文件夹
│
├─assets //放图片等资源的文件夹
│  └─svg //里面的所有svg文件可以直接通过全局组件<svg-icon>引入
│
├─components //全局组件
│
├─i18n //国际化
│  │  index.ts
│  │
│  ├─lang //全局国际化
│  │
│  └─pages //单页面的国际化
│
├─layout //页面布局
│
├─plugins //插件 例如：引入svg
│
├─router
│      backEnd.ts //后端权限
│      frontEnd.ts //前端权限
│      index.ts //路由加载跳转逻辑
│      route.ts //所有路由
│
├─stores //状态管理
│
├─theme //样式主题
│
├─utils //各种工具 例如：时间格式化、自定义指令、本地存储
│
└─views //页面
```

#### 安装 cnpm、yarn

- 复制代码(桌面 cmd 运行) `npm install -g cnpm --registry=https://registry.npm.taobao.org`
- 复制代码(桌面 cmd 运行) `npm install -g yarn`

#### 环境支持

| Edge                                                                     | last 2 versions                                                                   | last 2 versions                                                                | last 2 versions                                                                |
| ------------------------------------------------------------------------ | --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | ------------------------------------------------------------------------------ |
| ![Edge](https://cdn.jsdelivr.net/npm/@browser-logos/edge/edge_32x32.png) | ![Firefox](https://cdn.jsdelivr.net/npm/@browser-logos/firefox/firefox_32x32.png) | ![Chrome](https://cdn.jsdelivr.net/npm/@browser-logos/chrome/chrome_32x32.png) | ![Safari](https://cdn.jsdelivr.net/npm/@browser-logos/safari/safari_32x32.png) |

> 由于 Vue3 不再支持 IE11，故而 ElementPlus 也不支持 IE11 及之前版本。

#### 使用说明

<a href="http://nodejs.cn/" target="_blank">node 版本 > 12xx.xx.x</a>


#### mitt的使用
```javascript
const { proxy } = <any>getCurrentInstance(); //获取实例
proxy.mittBus.emit('eventName', data); //提交事件
proxy.mittBus.on('eventName', (data)=>{}); //触发事件
```

#### pinia的使用
- 创建一个存储
```javascript
import { defineStore } from 'pinia';
import { TagsViewRoutesState } from './interface';

export const useTagsViewRoutes = defineStore('tagsViewRoutes', {
	state: (): TagsViewRoutesState => ({
		//数据
		tagsViewRoutes: [],
		isTagsViewCurrenFull: false,
	}),
	getter:{
		//类似computed
		functionName(){
			return data
		}
	},
	actions: {
		//设置state
		setTagsViewRoutes(data: Array<string>) {
			this.tagsViewRoutes = data;
		},
		setCurrenFullscreen(bool: Boolean) {
			this.isTagsViewCurrenFull = bool;
		},
	},
});
```
- 使用
```javascript
import { storeToRefs } from 'pinia'; //pinia的保持响应式
import { useTagsViewRoutes } from '/@/stores/tagsViewRoutes';

	setup() {
		const storesTagsViewRoutes = useTagsViewRoutes();
		const { tagsViewRoutes } = storeToRefs(storesTagsViewRoutes);//使用state
		storesTagsViewRoutes.setTagsViewRoutes(data)//使用action
	}
```
#### vue-i18n的使用
- 添加对应语言的国际化对象
```javascript
// en.ts
export default {
	router: {
		home: 'home',
	},
	staticRoutes: {
		signIn: 'signIn',
		notFound: 'notFound',
		noPower: 'noPower',
	},
}
//zh-cn.ts
export default {
	router: {
		home: '首页',
	},
	staticRoutes: {
		signIn: '登录',
		notFound: '找不到此页面',
		noPower: '没有权限',
	},
}
```
- 使用
```javascript
// template
<div>{{ $t('message.router.home') }}</div>

//javascript
import { useI18n } from 'vue-i18n';

const { t } = useI18n();
t(restaurant.router.home)
```