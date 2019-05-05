<template>
    <div>
        <h3>Dashboard</h3>
        <!-- 父组件必须有一个路由出口，负责显示子路由内容 -->
        <router-view></router-view>

        <button @click="inc">increase</button>
        <button @click="incAsync">incAsync</button>
        <p>{{count}}</p>
        <p>您的余额为：{{money}}</p>
    </div>
</template>

<script>
    import {mapState,mapGetters,mapMutations,mapActions} from 'vuex';
// 1. 配置

//    ```
//    {
//    	state:{}
//    	mutations:{}
//    	getters:{}
//    	actions:{}
//    }
//    ```

// 2. 使用

//    - store.state.xx
//    - store.getters.xx
//    - store.commit(xxx)
//    - store.dispatch(xxx)

// 3. 帮助方法

//    - mapState
//    - mapGetters
//    - mapMutations
//    - mapActions

// 4. 模块化
    export default {
        methods: {
            ...mapMutations(['increase']),
            ...mapActions(['increaseAsync']),
            inc() {
                // this.$store.commit('increase')
                this.increase()
            },
            incAsync() {
                // this.$store.dispatch('increaseAsync')
                this.increaseAsync()
            }
        },
        computed: { 
            // {count: this.$store.state.count}
            ...mapState(['count']),
            ...mapGetters(['money'])
        },
    }
</script>

<style scoped>

</style>