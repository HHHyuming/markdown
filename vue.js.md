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

### modules

```
1. 普通用法
import moduleA from 'xxxx'
import moduleB from 'xxxx'
const store = new vuex({
	modules:{
		a:moduleA,
		b:moduleB
	}
})

采用module actions参数 
actions: {
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
  
2.命名空间用法
modules:{
	namespaced:true
}

import { createNamespacedHelpers} from 'vuex'
1. ==================================
computed: {
  ...mapState({
    a: state => state.some.nested.module.a,
    b: state => state.some.nested.module.b
  })
},
methods: {
  ...mapActions([
    'some/nested/module/foo', // -> this['some/nested/module/foo']()
    'some/nested/module/bar' // -> this['some/nested/module/bar']()
  ])
}
2 ==================================
computed: {
  ...mapState('some/nested/module', {
    a: state => state.a,
    b: state => state.b
  })
},
methods: {
  ...mapActions('some/nested/module', [
    'foo', // -> this.foo()
    'bar' // -> this.bar()
  ])
}
3 ================================

import { createNamespacedHelpers } from 'vuex'

const { mapState, mapActions } = createNamespacedHelpers('some/nested/module')

export default {
  computed: {
    // 在 `some/nested/module` 中查找
    ...mapState({
      a: state => state.a,
      b: state => state.b
    })
  },
  methods: {
    // 在 `some/nested/module` 中查找
    ...mapActions([
      'foo',
      'bar'
    ])
  }
}
```



##  vue-router

### 动态路由匹配

```
1.restful 参数传递
--------------- router ------------
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
---------- js -----------   
mounted(){
	this.$route.params.ide
}
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}

-------------- $router  对象操作 -----
this.$router.push('/asdasd/')

```

### route 参数

```
fullPath: "/user/2"
hash: ""
matched: Array(1)
0: {path: "/user/:id", regex: /^\/user\/((?:[^\/]+?))(?:\/(?=$))?$/i, components: {…}, instances: {…}, enteredCbs: {…}, …}
length: 1
__proto__: Array(0)
meta: {}
name: undefined
params: {id: "2"}
path: "/user/2"
query: {}
```

### 嵌套路由

```
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User,
      children: [
        {
          // 当 /user/:id/profile 匹配成功，
          // UserProfile 会被渲染在 User 的 <router-view> 中
          path: 'profile',
          component: UserProfile
        },
        {
          // 当 /user/:id/posts 匹配成功
          // UserPosts 会被渲染在 User 的 <router-view> 中
          path: 'posts',
          component: UserPosts
        }
      ]
    }
  ]
})

part2=====================================
const router = new VueRouter({
  routes: [
    {
      path: '/user/:id', component: User,
      children: [
        // 当 /user/:id 匹配成功，
        // UserHome 会被渲染在 User 的 <router-view> 中
        { path: '', component: UserHome },

        // ...其他子路由
      ]
    }
  ]
```

### 编程式导航

```
1.=============================================
注意： 如果目的地和当前路由相同，只有参数发生了改变 (比如从一个用户资料到另一个 /users/1 -> /users/2)，你需要使用 beforeRouteUpdate 来响应这个变化 (比如抓取用户信息)
2.=============================================
watch 监听路由变化 可使用 routerBeforeLeave or router



5.======================= router 方法 =================
router.push(path:'/zxcxzc',params:{}) ---------
router.push(name:'zxczxc',query:{}) ----------
router.replace(location, onComplete?, onAbort?)

router.go(1) ------- history.forward()
router.go(-1) ----- history.back()

你也许注意到 router.push、 router.replace 和 router.go 跟 window.history.pushState、 window.history.replaceState 和 window.history.go好像， 实际上它们确实是效仿 window.history API 的。

```

### 命名路由

```
router.push({ name: 'user', params: { userId: 123 }})
这两种方式都会把路由导航到 /user/123 路径。
```

### 路由重定向

```
{path:'/xxxx', redirect:'/zczczxc'}

============ 路由别名 ===============
{path: '/zxcx', alias:'/sadsd'}
```

### 路由传参通过 props 解耦

```
const router = new VueRouter({
  routes: [
    { path: '/search', component: SearchUser, props: (route) => ({ query: route.query.q }) }
  ]
})

URL /search?q=vue 会将 {query: 'vue'} 作为属性传递给 SearchUser 组件。

请尽可能保持 props 函数为无状态的，因为它只会在路由发生变化时起作用。如果你需要状态来定义 props，请使用包装组件，这样 Vue 才可以对状态变化做出反应。
```

### 导航守卫

```
next: Function: 一定要调用该方法来 resolve 这个钩子。执行效果依赖 next 方法的调用参数。

next(): 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 confirmed (确认的)。

next(false): 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 from 路由对应的地址。

next('/') 或者 next({ path: '/' }): 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 next 传递任意位置对象，且允许设置诸如 replace: true、name: 'home' 之类的选项以及任何用在 router-link 的 to prop 或 router.push 中的选项。

next(error): (2.4.0+) 如果传入 next 的参数是一个 Error 实例，则导航会被终止且该错误会被传递给 router.onError() 注册过的回调。


1. 全局前置 beforeEach
router.beforeEach((to, from, next) => {
  // ...
})
2. 全局解析
router.beforeResolve
3.全局后置
router.afterEach
4.路由独享
routes:[path:'/zxcxzc',component:zxcxzc,beforeEnter:(to,from,next) =>{行为}]
5.组件内的守卫
const Foo = {
  template: `...`,
  beforeRouteEnter (to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
    
    next(vm => {
        // 通过 `vm` 访问组件实例
      })
  
  },
  beforeRouteUpdate (to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
  },
  beforeRouteLeave (to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
  }
}



```

### 路由元信息

```

```

### 滚动行为

```

```

### 路由懒加载

```
{
          path: '/',
          name: 'HelloWorld',
          component: resolve => require(['@/components/HelloWorld'], resolve)
}		

{
path: '/',
name: 'HelloWorld',
component: () => import('@/components/HelloWorld.vue')
}
```

