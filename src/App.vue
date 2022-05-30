<template>
  <Todo />
</template>

<script>
import Todo from './components/Todo.vue'
export default {
  name: 'App',
  components: {
    Todo
  }
}
</script>

<style lang="scss" >

#app {
  min-height: 100vh;
  display: grid;
  align-items: start;
  justify-items: center;
  color: var(--color_primary);
  font-family: var(--font_primary);
  background-color: var(--color_bg);
  background-image: var(--body_mobile_bg_image);
  background-position: top;
  background-repeat: no-repeat;
}

@media (min-width: 375px) {
  #app {
    background-image: var(--body_desktop_bg_image);
  }
}
</style>
