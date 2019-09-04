# vue-pc-pagination

> pagination for pc, based Vue2.x

### 安装（Install）
``
npm install vue-pc-pagination -save
``

### 导入（Import）
#### *.js
```javascript
import Pagination from 'vue-pc-pagination'
```

### 可全局注册使用
```javascript
Vue.use(Pagination);
```

### 标签（Target）
#### *vue
```html
<Pagination :options="pageOptions"></Pagination>
```

### demo
```javascript
 let currentPage = 1;
 let pageOptions = {
    total: 20,
    totalPage: 4,
    current: currentPage,
    onPageChanged: (val) => {
      currentPage = val;
    }
 }
```

### 功能（Api）

| Options         | Type     | Description                 | Default | Necessary | Result |
|-----------------|:--------:|:---------------------------:|:-------:|:---------:|:------:|
| total | number | 总数 | 0 | 否 | |
| totalPage | number | 总页数 | 0 | 否 | |
| current | number | 当前页 |  | 是 | |
| size | number | 数据一次显示多少条 | 10 | 否 | |
| isShowRecord | boolean | 是否显示条数 | false | 否 | |
| onPageChanged | function | 页数变化时执行 |  | 是 | 返回变化后的当前页 |
