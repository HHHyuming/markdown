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
// 提交方式
1.this.$store.commit('mutation',payload)
2. this.$store.commit({
	type:'mutationi',
	amount: 'payload'
})

// 辅助函数 mapmutations
import { mapmutations } from 'vuex'
methods: {
	...mapmutations(['mutation1','mutation1'])
}
```

### actions

```
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})

// 触发方式
1.store.dispatch('increment')
2. store.dispatch({
	type:'xxx',
	amount:'xx'
})

// 组合方式
actions: {
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }
}
```





## 