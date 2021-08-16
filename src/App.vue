<template>
  <div id="app">
    <!-- <HelloWorld/> -->
    <!--<router-view name="menu"></router-view>-->
    <div >
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
};
</script>
<style lang="scss">
  @import "./assets/all";
</style>

