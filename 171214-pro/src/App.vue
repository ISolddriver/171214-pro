<template>
  <div id="app">
    <div class="con" @click="handleClick">
        {{message}}
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data () {
    return {
      message: 'hello world'
    }
  },
  methods: {
    handleClick () {
      this.message = 'bye World'
    }
  }
}
</script>

<style scoped>
  .con {
    color: red;
  }
</style>
