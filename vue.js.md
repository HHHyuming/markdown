# vue.js

## vuex

### state

```
辅助函数
import maptstate from 'vuex'
组件 1.
cpn = {
	computed:{
		...mapstate(['state'])
	}
}
cpn = {
	computed:{
		...mapstate({
			count: state => state.count,
			count2:function(state){
				return this.data + state.count
			}
		})
	}
}
```

### getters

```
getters.js
export default {
	todos(state, getters){
		state // 状态
		getters // 状态树访问
	}
}

import { mapgetters } from 'vuex'
computed:{
    ...mapGetters({
      // 把 `this.doneCount` 映射为 `this.$store.getters.doneTodosCount`
      doneCount: 'doneTodosCount'
    })
}


```

### mutation

```
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state,payload) {
      // 变更状态
      state.count++
      if(payload){
      	// 相关逻辑代码
      }
    }
  }
})


this.$store.commit('mutation',payload)
```





## 