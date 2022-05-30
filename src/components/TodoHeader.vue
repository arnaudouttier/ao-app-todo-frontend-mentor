<template>
  <div class="todo-header">
    <h1 class="todo-title">TODO</h1>
    <div class="toggle-buttons">
      <button
        data-thememod-icon="1"
        class="btn toggle-theme-btn"
        @click="toggleThemeMode()"
        v-if="!activeIcon"
      >
        <img src="~@/assets/images/icon-moon.svg" alt="icon toggle theme" />
      </button>
      <button
        data-thememod-icon="0"
        class="btn toggle-theme-btn"
        @click="toggleThemeMode()"
        v-if="activeIcon"
      >
        <img src="~@/assets/images/icon-sun.svg" alt="icon toggle theme" />
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoHeader',
  data () {
    return {
      app: document.querySelector('#app'),
      activeIcon: true,
      darkMode: null
    }
  },
  mounted () {
    if (localStorage.darkMode) {
      this.darkMode = localStorage.darkMode
    }
    this.enabledDarkMode()
  },
  watch: {
    darkMode: {
      handler (newDarkMode) {
        localStorage.darkMode = newDarkMode
      },
      deep: true
    }
  },
  methods: {
    enabledDarkMode () {
      if (this.darkMode === 'enabled') {
        this.app.setAttribute('data-theme', 'dark')
      }
    },
    toggleThemeMode () {
      if (this.darkMode === 'enabled') {
        this.app.setAttribute('data-theme', '')
        this.darkMode = null
      } else {
        this.app.setAttribute('data-theme', 'dark')
        this.darkMode = 'enabled'
      }

      this.activeIcon = !this.activeIcon
    }
  }
}
</script>

<style lang="scss" scoped>

.todo-header {
  display: flex;
  justify-content: space-between;
  margin-bottom: 40px;
}

.todo-title {
  font-size: clamp(
    1.8rem,
    calc(1.8rem + ((1vw - 0.234375rem) * 1.9394)),
    2.8rem
  );
  letter-spacing: 9px;
  color: var(--color_todo_title);
}
</style>
