### Vue3.0

[Vue3.0组合式 API 征求意见稿](https://vue3js.cn/vue-composition/)

[Vue3.0 文档](https://v3.cn.vuejs.org/api/)

1. 数据响应式

   ```javascript
   reactive(val: Object)
   ref(primivate value) 
   toRef()
   toRefs()
   
   方案1： 单独使用ref  缺点： 变量会很多
   方案2： 单独使用reactive 缺点： 访问和设置会多一个层级
   方案3： 混合使用两种
   import { reactive, ref, toRef, toRefs } from '@vue/composition-api'
   export default {
   	name: 'Demo',
     setup() {  // beforeCreate之前执行
       
       // 混合方案1 
       const count = ref(0)
       const state = reactive({
         count
       })
       
       
       // 混合方案2
       const state = reactive({
         count: 0
       })
       const count = toRef(state, 'count')
       
       // 混合方案3 
       const state = reactive({
         count: 0
       })
       const { count } = toRefs(state)
       
       setTimeout(() => { state.count ++ }, 1000)
       return {
         count
       }
     }
   }
   
   ```
   

   
2. 计算属性

   ```	javascript
   import { reactive, ref, toRef, toRefs, computed } from '@vue/composition-api'
   export default {
   	name: 'Demo',
     setup() {  // beforeCreate之前执行
       
       // 方式1
       const count = ref(1)
       const double = computed(() => count.value * 2)
       setTimeout(() => { count.value ++ }, 1000)
       
       
       // 方式2
       const state = reactive({
         count: 1,
         doubule: computed(() => {
           return state.count * 2
         })
       })
       return toRefs(state);
     }
   }
   
   
   ```

   

3. Vue3.0的事件和watchEffect

   ```vue
   <template>
   	<div class="demo">
       {{count}}
       <button @click="add">
         点击
     </button>
     </div>
   </template>
   <script>
     import { reactive, ref, toRef, toRefs, computed, watch, watchEffect, watch } from '@vue/composition-api'
     export default {
       setup() {
         const count = ref(0)
         
         const add = () => {
           count.value ++
           
           if (count.value > 9) {
             effectStop();
           }
         }
         
         // watchEffect 副作用, 类似Vue2的watch
         
         // 使用返回的值
         const effectStop = watchEffect(() => {
           console.log(count.value)
         })
         
         // 副作用，清除副作用
         
         // 副作用的刷新时机 { flush: 'post | pre'}
         
         // watch  对应Vue2的watch
   			watch(count, (newVal, oldVal) => {
           console.log(count.value)
         })
         
         return {
           count,
           add
         }
       }
     }
   </script>
   
   ```

4. Vue3.0的生命周期 和  useXXX函数

   ```vue
   import { 
   	reactive, ref, toRef, toRefs, computed, watch, watchEffect, watch,
   	onBeforeMount, onMounted, 
   	onBeforeUpdate, onUpdated, 
   	onBeforeUnmount, onUnmounted,
   	onErrorCaptured
   } from '@vue/composition-api'
   
     // 新的生命周期函数， 必须在setUp中使用
     export default {
       setup() {
         const count = ref(0)
         
         
         
         return {
           count
         }
       }
     }
   </script>
   ```

   