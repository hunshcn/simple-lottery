# simple-lottery

一个附带简易动画的纯前端抽奖程序，纯css实现

之前中过的后面不能再中

一轮十个，目前的抽奖范围是1~1200，写死在代码

代码略乱

## 现在的缺点
- 抽奖结果应该在`Lottery.vue`生成然后传下去会比较好，现在是在`Number.vue`生成，为了保证不重复，无法同时开始了。

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
