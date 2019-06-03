<template>
  <div id="app">
    <h1>这是小部件模板工程</h1>
    <p>{{demoStr}}</p>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      demoStr: 'hello world'
    }
  },
  components: {
  },
  created(){
  },
  mounted(){
  },
  methods: {
  }
}
</script>

<style lang="scss">
@import '~asset/common';
</style>
