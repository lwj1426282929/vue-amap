<template>
    <div id="app">
        <!-- <span :class="['btn',  active === 'demo1'? 'active':'']"
              @click="active = 'demo1'">demo1</span>
        <span :class="['btn',  active === 'demo2'? 'active':'']"
              @click="active = 'demo2'">demo2</span> -->
        <a-map v-if="active === 'demo1'"></a-map>
        <b-map v-else></b-map>
    </div>
</template>

<script>
import AMap from './components/AMap'
import BMap from './components/BMap'

export default {
    name: 'App',

    components: {
        AMap,
        BMap
    },

    data () {
        return {
            active: 'demo1'
        }
    }
}
</script>

<style lang="less" scoped>
.btn {
    padding: 10px;
    margin: 10px;
    cursor: pointer;

    &.active {
        color: #409eff;
    }
}
</style>
