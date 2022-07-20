<template>
  <div class="app-container" @keydown.enter="handleEnterTap">
    <input type="text" class="todo-input" placeholder="Do something..." v-model="inputValue" ref="input">
    <ul class="todo-list">
      <li class='list-item' v-for="task in taskArray">
        <div class="checkbox-wrapper">
          <input type="checkbox" :id='task.id' v-if="!task.isDone" class="checkbox" @change="handleCheckbox">
        </div>
        <p class="todo-content" :class="{done: task.isDone}">{{task.content}}</p>
        <div class="cross" :id='task.id' @click="handleCrossClick">
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import { nanoid } from 'nanoid'

export default {
  data() {
    return {
      taskArray: [],
      inputValue: '',
    }
  },
  mounted() {
    this.$refs.input.focus();
  },
  methods: {
    handleEnterTap() {
      this.taskArray.push({
        content: this.inputValue,
        id: nanoid(),
        isDone: false,
      });
      this.inputValue = '';
      console.log('submit');
    },
    handleCheckbox(e) {
      let currentCheckbox = this.taskArray.findIndex(item => e.currentTarget.id === item.id);
      this.taskArray[currentCheckbox].isDone = true;
    },
    handleCrossClick(e) {
      let currentCross = this.taskArray.findIndex(item => e.currentTarget.id === item.id);
      this.taskArray.splice(currentCross, 1);
      console.log('click')
    }
  }

}
</script>

<style lang="scss">
  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }
  body {
    width: 100vw;
    min-width: 100px;
  }

 .app-container {
    padding: 20px;
    width: 100%;
    min-height: 100vh;
    display: flex;
    gap: 40px;
    flex-direction: column;
    align-items: center;
  }

  .todo-input {
    will-change: transform;
    width: 400px;
    font-size: 2rem;
    padding: 10px;
    text-align: center;
    border-radius: 20px;
    transition: .2s;

    &:hover {
      transform: scale(1.05);
    }
  }

  .todo-list {
    display: flex;
    flex-direction: column;
    gap: 10px;
    width: 600px;
  }

  .checkbox-wrapper {
    width: 40px;

    .checkbox {
      transition: .2s;
      cursor: pointer;
      transform: scale(2);

      &:hover {
        transform: scale(2.5);
      }
    }
  }

  .list-item {
    padding: 20px 10px;
    display: flex;
    align-items: center;
    justify-content: space-around;
    border: 2px solid rgba(128, 128, 128, 0.53);
    font-size: 30px;
  }

  .todo-content {
    flex-basis: 60%;

    &.done {
      color: grey;
      text-decoration: line-through;
    }
  }

  .cross {
    cursor: pointer;
    will-change: transform;
    box-sizing: border-box;
    position: relative;
    width: 40px;
    height: 40px;
    transition: .2s;

    &:hover {
      transform: scale(1.2);
    }
    &:before, &:after {
      content: '';
      position: absolute;
      top: 50%;
      transform: translateY(-50%) rotate(45deg);
      background: red;
      width: inherit;
      height: 4px;
    }
    &:after {
      transform: translateY(-50%) rotate(-45deg);
    }
  }
</style>