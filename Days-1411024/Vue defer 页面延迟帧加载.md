#  Vue defer 页面延迟帧加载

> 主要解决的问题是： 页面的DOM结构很多，为了对白屏进行优化，可以用延迟帧进行加载页面，先渲染部分DOM结构。

**实现原理：浏览器会按照一定的频率来渲染页面，每次渲染都会调用 requestAnimationFrame 方法中的 callback 函数。**

##  Vue2 版本

vue2 版本可以用 `mixin`来实现

defer.vue

```vue

export default function defer(maxFremCount = 10) {
  return {
    data () {
      return {
        frameCount: 0,
        radId: null
      }
    },
    methods: {
      // 是否渲染到该组件
      deferItem (showFrameCount) {
        return this.frameCount >= showFrameCount
      }
    },
    mounted () {
      const refreshFrame = () => {
        this.radId = requestAnimationFrame(() => {
          // 渲染帧数 执行回调
          this.frameCount++

          // 如果帧数小于最大帧数，继续渲染
          if (this.frameCount < maxFremCount) {
            refreshFrame()
          }
        })
      }

      // 初始化渲染帧数
      refreshFrame()
    },
    beforeDestroy () {
      this.frameCount = 0
      cancelAnimationFrame(this.radId)
    }
  }
}
```

HeavyComp.vue

```vue
<template>
  <div class="container">
    <div v-for="n in 2000" class="item" :key="n"></div>
  </div>
</template>

<style scoped>
.container {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  width: 300px;
  height: 300px;
}

.item {
  width: 5px;
  height: 5px;
  background-color: #eee;
  margin: 0.1em;
}
</style>
```

App.vue

```vue
<template>
  <div class="container">
    <div v-for="n in 25" class="wrapper" :key="n">
      <HeavyComp v-if="deferItem(n)" />
    </div>
  </div>
</template>

<script>
import HeavyComp from './HeavyComp.vue'
import defer from './defer'

export default {
  components: {
    HeavyComp
  },
  mixins: [defer(25)]
}
</script>
<style scoped>
.container {
  display: flex;
  flex-wrap: wrap;
}

.wrapper {
  border: 1px solid salmon;
}
</style>
```



##  Vue3 版本

vue3 版本可以使用 hook

defer.js

```vue
import { onMounted, onUnmounted } from 'vue'

export default function useDefer(maxFremCount = 100) {
  const frameCount = ref(0)
  const radId = ref(null)

  const deferItem = (showFrameCount) => {
    return frameCount.value >= showFrameCount
  }

  const refreshFrame = () => {
    radId.value = requestAnimationFrame(() => {
      frameCount.value++
      if (frameCount.value < maxFremCount) {
        refreshFrame()
      }
    })
  }

  onMounted(() => {
    refreshFrame()
  })

  onUnmounted(() => {
    cancelAnimationFrame(radId.value)
  })
  
  reutrn deferItem
}
```





##  参考

- [1](https://blog.csdn.net/qq_40147756/article/details/135802189)
- [2](https://blog.csdn.net/weixin_61769998/article/details/134250122)
- [3](https://juejin.cn/post/7241763913787080741)

vue defer 延迟帧实现

[源码地址](https://codesandbox.io/p/sandbox/26ddk6?layout=%257B%2522sidebarPanel%2522%253A%2522EXPLORER%2522%252C%2522rootPanelGroup%2522%253A%257B%2522direction%2522%253A%2522horizontal%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522id%2522%253A%2522ROOT_LAYOUT%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522UNKNOWN%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522cm28p07xp0006356kdbyywelo%2522%252C%2522sizes%2522%253A%255B100%252C0%255D%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522EDITOR%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522EDITOR%2522%252C%2522id%2522%253A%2522cm28p07xp0002356ktnu40nhu%2522%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522direction%2522%253A%2522horizontal%2522%252C%2522id%2522%253A%2522SHELLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522SHELLS%2522%252C%2522id%2522%253A%2522cm28p07xp0003356kl4jb9ygc%2522%257D%255D%257D%255D%257D%252C%257B%2522type%2522%253A%2522PANEL_GROUP%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522direction%2522%253A%2522vertical%2522%252C%2522id%2522%253A%2522DEVTOOLS%2522%252C%2522panels%2522%253A%255B%257B%2522type%2522%253A%2522PANEL%2522%252C%2522contentType%2522%253A%2522DEVTOOLS%2522%252C%2522id%2522%253A%2522cm28p07xp0005356kd1bphg20%2522%257D%255D%257D%255D%252C%2522sizes%2522%253A%255B50%252C50%255D%257D%252C%2522tabbedPanels%2522%253A%257B%2522cm28p07xp0002356ktnu40nhu%2522%253A%257B%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522cm28p07xo0001356k72c1v2in%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522FILE%2522%252C%2522filepath%2522%253A%2522%252FREADME.md%2522%257D%255D%252C%2522id%2522%253A%2522cm28p07xp0002356ktnu40nhu%2522%252C%2522activeTabId%2522%253A%2522cm28p07xo0001356k72c1v2in%2522%257D%252C%2522cm28p07xp0005356kd1bphg20%2522%253A%257B%2522tabs%2522%253A%255B%257B%2522id%2522%253A%2522cm28p07xp0004356k7engd8v6%2522%252C%2522mode%2522%253A%2522permanent%2522%252C%2522type%2522%253A%2522UNASSIGNED_PORT%2522%252C%2522port%2522%253A0%257D%255D%252C%2522id%2522%253A%2522cm28p07xp0005356kd1bphg20%2522%252C%2522activeTabId%2522%253A%2522cm28p07xp0004356k7engd8v6%2522%257D%252C%2522cm28p07xp0003356kl4jb9ygc%2522%253A%257B%2522tabs%2522%253A%255B%255D%252C%2522id%2522%253A%2522cm28p07xp0003356kl4jb9ygc%2522%257D%257D%252C%2522showDevtools%2522%253Atrue%252C%2522showShells%2522%253Afalse%252C%2522showSidebar%2522%253Atrue%252C%2522sidebarPanelSize%2522%253A15%257D)