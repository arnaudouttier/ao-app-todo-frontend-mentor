<template>
  <div class="todo">
    <TodoHeader />
    <div class="todo-main">
      <div class="todo-input">
        <button
          class="btn radio-check-btn"
          :class="{ checked: isChecked }"
          @click="
            createTodoItem();
            toggleCheck();
          "
        >
          <img
            class="btn todo-result-item"
            src="~@/assets/images/icon-check.svg"
            alt="icon check"
          />
        </button>
        <input
          type="text"
          @keyup.enter="createTodoItem()"
          v-model="newTodoItem"
          placeholder="Create a new todo"
        />
      </div>

      <div class="todo-result" @change="completedCount()">
        <ul>
          <li
            v-for="(todoItem, index) in todoFilter"
            :key="todoItem.id"
            class="todo-item"
          >
            <div class="left">
              <button
                class="btn radio-check-btn"
                @click="checkTodoItem(index)"
                :class="{ completed: todoItem.state === 'completed' }"
              >
                <img
                  class="btn todo-result-item"
                  src="~@/assets/images/icon-check.svg"
                  alt="icon check"
                />
              </button>
              <p @click="checkTodoItem(index)">
                {{ todoItem.contents }}
              </p>
            </div>
            <img
              @click="deleteItem(index)"
              class="btn todo-result-item deleted"
              src="~@/assets/images/icon-cross.svg"
              alt="icon deleted"
            />
          </li>
          <li class="todo-result todo-filter" v-if="todoItems.length > 0">
            <p class="items-left">{{ completedCount() }} Item Left</p>
            <div class="todo-filter-center">
              <button
                class="btn filter-btn filter-all"
                @click="buttonFilter = ''"
              >
                All
              </button>
              <button
                class="btn filter-btn filter-active"
                @click="buttonFilter = 'active'"
              >
                Active
              </button>
              <button
                class="btn filter-btn filter-completed"
                @click="buttonFilter = 'completed'"
              >
                Completed
              </button>
            </div>
            <button
              class="btn filter-btn filter-clear-completed"
              @click="deleteAllCompleted()"
            >
              Clear completed
            </button>
          </li>
        </ul>
      </div>
    </div>

    <div class="todo-footer todo-filter" v-if="todoItems.length > 0">
      <h4 class="todo-result-filter"></h4>
      <button class="btn filter-btn filter-all" @click="buttonFilter = ''">
        All
      </button>
      <button
        class="btn filter-btn filter-active"
        @click="buttonFilter = 'active'"
      >
        Active
      </button>
      <button
        class="btn filter-btn filter-completed"
        @click="buttonFilter = 'completed'"
      >
        Completed
      </button>
      <button
        class="btn filter-btn filter-clear-completed"
        @click="deleteAllCompleted()"
      >
        Clear completed
      </button>
    </div>
  </div>
</template>

<script>
import TodoHeader from './TodoHeader.vue'

export default {
  name: 'Todo',
  components: {
    TodoHeader
  },
  data () {
    return {
      newTodoItem: '',
      buttonFilter: '',
      todoItems: [],
      isChecked: false
    }
  },
  mounted () {
    if (localStorage.todoItems) {
      this.todoItems = JSON.parse(localStorage.todoItems)
    }
  },
  watch: {
    todoItems: {
      handler (n) {
        localStorage.todoItems = JSON.stringify(n)
      },
      deep: true
    }
  },
  methods: {
    createTodoItem () {
      if (this.newTodoItem) {
        const newTodoIt = {
          id: this.todoItems.length,
          contents: this.newTodoItem,
          state: 'active'
        }
        this.todoItems.push(newTodoIt)
        this.newTodoItem = ''
        this.isChecked = false
      }
    },

    completedCount () {
      const completedCount = this.todoItems.filter((itemTodo) => {
        return itemTodo.state !== 'completed'
      })
      return completedCount.length
    },

    deleteItem (index) {
      this.todoItems.splice(index, 1)
    },

    deleteAllCompleted () {
      const filteredItemsCompleted = this.todoItems.filter((itemTodo) => {
        return itemTodo.state !== 'completed'
      })

      this.todoItems.length = 0

      filteredItemsCompleted.forEach((filteredCompleted) => {
        this.todoItems.push(filteredCompleted)
      })
    },

    checkTodoItem (i) {
      if (this.todoItems[i].state === 'completed') {
        this.todoItems[i].state = 'active'
      } else {
        this.todoItems[i].state = 'completed'
      }
    },
    toggleCheck () {
      this.isChecked = !this.isChecked
    }
  },
  computed: {
    todoFilter () {
      return this.todoItems.filter((element) => {
        if (this.buttonFilter) {
          return element.state === this.buttonFilter
        }
        return element
      })
    }
  }
}
</script>

<style lang="scss">

.todo {
  width: min(90vw, 550px);
  margin-top: clamp(
    2.5rem,
    calc(2.5rem + ((1vw - 0.234375rem) * 6.0606)),
    5.625rem
  ); /* 40px (375vw) to 90px (1200vw) */
}

.todo-input,
.todo-result {
  margin-bottom: clamp(
    0.9375rem,
    calc(0.9375rem + ((1vw - 0.234375rem) * 1.8182)),
    1.875rem
  ); /* 15px (375vw) to 30px (1200vw) */
}

/* Todo Result */

.todo-input {
  @include todo-cell(7px 17px);
  @include block-box-shadow(hsl(233, 11%, 84%));

  .radio-check-btn {
    @include check-btn(checked);
  }

  input {
    font-size: 17px;
    margin-left: 15px;
    flex-grow: 1;
  }
}

/* Todo Result */

.todo-result {
  @include block-box-shadow(hsl(233, 11%, 84%));

  .todo-item {
    @include todo-cell(17px);
    border-radius: 0;
    justify-content: space-between;
    border-bottom: var(--border_100);

    &:first-child {
      border-top-left-radius: 4px;
      border-top-right-radius: 4px;
    }

    .left {
      display: flex;
      align-items: center;

      p {
        margin-left: 15px;
        font-size: 18px;
        color: var(--color_secondary);
      }

      .completed {
        ~ p {
          color: var(--color_primary);
          text-decoration: line-through;
        }
      }
    }

    .radio-check-btn {
      @include check-btn(completed);
    }
  }

  .todo-filter {
    @include todo-cell(17px);
    justify-content: space-between;
    border-top-left-radius: 0px;
    border-top-right-radius: 0px;
    border-bottom-left-radius: 4px;
    border-bottom-right-radius: 4px;
    color: var(--color_secondary);

    p {
      color: var(--color_primary);
    }

    button {
      color: var(--color_primary);
      font-weight: 700;
    }

    .todo-filter-center {
      button {
        @include filter-btn;
      }
      display: none;
    }
  }
}

.todo-footer {
  &.todo-filter {
    @include todo-cell(17px);
    justify-content: center;
    column-gap: 16px;
    @include block-box-shadow(hsl(233, 11%, 84%));

    :first-child,
    :last-child {
      display: none;
    }

    button {
      @include filter-btn;
    }
  }
}

@media (min-width: 1200px) {
  .todo-filter-center {
    display: flex !important;
    justify-content: space-between;
    column-gap: 20px;
  }

  .todo-footer {
    display: none !important;
  }
}
</style>
