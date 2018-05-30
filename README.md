# yunye-loadmore

### 安装
`npm i yunye-loadmore -S`

### 使用
```
<template>
    <yy-loadmore @pull="pull" @loadmore="loadmore" :loading="loading" :all-loaded="allLoaded">
        ...
    </yyloadmore>
</template>
<script>
//引入
import YyLoadmore from "yunye-loadmore";
export default {
    components: { YyLoadmore },
    data(){
        return {
            loading: false,
            allLoaded: false
        }
    },
    methods: {
        pull() {},
        loadmore() {
            this.loading  =  true;
            setTimeout(() => {
                this.loading  =  false;
                setTimeout(() => {
                    this.allLoaded  =  true;
                }, 1000);
            }, 1000);
        }
    }
}
</script>
```
### 事件

| 事件名称 | 说明 | 回调参数 |
|--|--|--|
| pull | 下拉事件 | -- |
| loadmore | 加载更多 | -- |

### 属性

| 属性名称 | 说明 | 类型 | 默认值 |
| -------- | ---- | ---- |--|
| loading | 是否在加载中 | Boolean | false |
| allLoaded | 是否全部加载完 | Boolean | false |
| topDistance | 下拉刷新的下拉的距离 | Number | 40 |
| pull | 是否需要下拉刷新功能 | Boolean | true |
| up | 是否需要加载更多功能 | Boolean | true |
| topPullText | 下拉文字 | String | 下拉刷新 |
| topText | 下来一段距离文字 | String | 松手刷新 |
| bottomText | 底部文字 | String | 加载更多 |
| bottomLoadingText | 底部加载中文字 | String | 加载中... |
| bottomNomoreText | 数据加载完底部文字 | String | 暂无更多了~ |