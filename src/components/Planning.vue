<template>
<div id="app-modal" class="app-modal">
 <button class="button" @click="showModal = true">
  Start Plan
 </button>
 <transition name="fade" appear>
  <div class="modal-overlay" v-if="showModal" @click="showModal = false"></div>
 </transition>
 <transition name="slide" appear>
  <div class="modal" v-if="showModal">
    <div class="container">
    <div class="task">

      <div class="title">
        <h1> Plan Your Trap!</h1>
      </div>

      <div class="form">
        <input
          type="text"
          placeholder="New Task"
          v-model="newTask"
          @keyup.enter="addTask"
        />
        <button @click="addTask">+</button>
      </div>

      <div class="taskItems">
        <ul>
          <task-item
            v-bind:task="task"
            v-for="(task, index) in tasks"
            :key="task.id"
            @remove="removeTask(index)"
            @complete="completeTask(task)"
          ></task-item>
        </ul>
      </div>

      <div class="clearBtns">
        <button @click="clearCompleted">Clear completed</button>
        <button @click="clearAll">Clear all</button>
      </div>
      
      <div class="pendingTasks">
        <span>Pending Tasks: {{ incomplete }}</span>
       
      </div>
    </div>
  </div>
   
   <button class="button" id="close" @click="showModal = false">
    Close Modal
   </button>
  </div>
 </transition>
 </div>
</template>

<script>
import TaskItem from "./Item.vue";

export default {
  name: "modal",
  props: ['tasks'],
  components: {
    TaskItem,
  },
  data () {
    return {
      showModal: false,
      newTask: "",
  }
 },
  computed: {
    incomplete() {
      return this.tasks.filter(task => !task.completed).length;
    },
  },
  methods: {
    addTask() {
      if (this.newTask) {
        this.tasks.push({
          id: Date.now(), // Assign a unique ID to each task
          title: this.newTask,
          completed: false,
        });
        this.newTask = "";
      }
    },
    clearCompleted() {
      this.tasks = this.tasks.filter(task => !(task.completed));
    },
    clearAll() {
      this.$emit("update:tasks", []);
    },
    completeTask(task) {
      task.completed = !task.completed;
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    }
  },
}
</script>


<style>
* {
 margin: 0;
 padding: 0;
 box-sizing: border-box;
}

body {
 font-family: 'montserrat', sans-serif;
}


#app-modal {
 margin: 150px;
 margin-top: 200px;
 position: relative;
 display: flex;
 justify-content: center;
 align-items: center;
 overflow-x: hidden;
}

.button {
 appearance: none;
 outline: none;
 border: none;
 background: none;
 cursor: pointer;
 
 display: inline-block;
 padding: 15px 25px;
 background-image: linear-gradient(to right, #CC2E5D, #FF5858);
 border-radius: 8px;
 
 color: #FFF;
 font-size: 18px;
 font-weight: 700;
 
 box-shadow: 3px 3px rgba(0, 0, 0, 0.4);
 transition: 0.4s ease-out;
 
 &:hover {
  box-shadow: 6px 6px rgba(0, 0, 0, 0.6);
 }
}

.modal-overlay {
 position: absolute;
 top: 0;
 left: 0;
 right: 0;
 bottom: 0;
 z-index: 98;
}

.modal {
 position: fixed;
 top: 50%;
 left: 50%;
 transform: translate(-50%, -50%);
 z-index: 99;
 
 width: 100%;
 max-width: 400px;
 background-color: #FFF;
 border-radius: 16px;
 
 padding: 25px;
 
 h1 {
  color: #222;
  font-size: 32px;
  font-weight: 900;
  margin-bottom: 15px;
 }
 
 p {
  color: #666;
  font-size: 18px;
  font-weight: 400;
  margin-bottom: 15px;
 }
}

.modal #close {
  transform: translateX(65%);
}


.fade-enter-active,
.fade-leave-active {
 transition: opacity .5s;
}

.fade-enter,
.fade-leave-to {
 opacity: 0;
}

.slide-enter-active,
.slide-leave-active {
 transition: transform .5s;
}

.slide-enter,
.slide-leave-to {
 transform: translateY(-50%) translateX(100vw);
}
</style>