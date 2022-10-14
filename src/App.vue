<div id="app">
    <task-list :task="tasks"></task-list>
</div>
<template id="task-list">
    <section class="tasks">
    <h1> Tasks 
        <transition name="fade">
            <small v-if="incomplete"> ({{ incomplete }})</small>
        </transition>
    </h1>
    <div class="tasks__new input-group">
        <input type="text" class="input-group-field" v-model="newTask" @keyup.enter="addTask" placeholder="New task">
        <span class="input-group-button">
            <button @click="addTask" class="button">
            <i class="fa fa-plus"></i> Add 
            </button>
        </span>
    </div>
    <div class="tasks__clear button-group pull-right">
        <button class="button warning small" @click="clearCompleted">
        <i class="fa fa-check"></i> Clear Complete
        </button>
        <button class="button alert small" @click="clearAll">
        <i class="fa fa-trash"></i> Clear clearAll
        </button>
    </div>
    <transition-group name="fade" tag="ul" class="tasks__list no-bullet">
        <TaskItem v-for="(task, index) in tasks" @remove="removeTask(index)" @complete="completeTask(task)" :task="task" :key="task" />
    </transition-group>
    </section>
</template>

<script>
import TaskItem from './components/TaskItem.vue';

export default {
    name: 'App',
    components: {
        TaskItem
    },
    template: '#task-list',
    data() {
        return {
            tasks: [
              {
                title: 'Make todo list',
                completed: true
              },
              {
                title: 'Go skydiving',
                completed: false
              }
            ],
            newTask: ''
        };
    },
    computed: {
        incomplete() {
            return this.tasks.filter(this.inProgress).length;
        }
    },
    methods: {
        addTask() {
            if(this.newTask) {
                this.tasks.push({
                    title: this.newTask,
                    complete: false
                });
                this.newTask = '';
            }
        },
        completeTask(task) {
            task.completed = ! task.completed;
        },
        removeTask(index) {
            this.tasks.splice(index, 1);
        },
        clearCompleted() {
            this.tasks = this.tasks.filter(this.inProgress);
        },
        clearAll() {
            this.tasks = [];
        },
        inProgress(task) {
            return ! this.isCompleted(task);
        },
        isCompleted(task) {
            return task.completed;
        }
    }
}
console.log("in task list");
</script>


<style scoped>
body {
  background-color: #abc;
}

*, h1, button{
  font-family: 'Nunito', sans-serif;
}
  
.fade-enter-active, .fade-leave-active {
  transition: opacity .5s;
}

.fade-enter, .fade-leave-active {
  opacity: 0;
}

.tasks {
  width: 100%;
  max-width: 45rem;
  padding: 1em;
  margin: 1em auto;
  overflow: auto;
  background-color: white;
  box-shadow: 0px .25rem 1rem rgba(black, .25);
}

.tasks__list {
  clear: both;
}

.tasks__item {
  margin-bottom: .5em;
  position: relative;
}

.tasks__item__toggle {
  cursor: pointer;
  width: 100%;
  text-align: left;
  padding: .85em 2.25em .85em 1em;
  background-color: rgba(black, .05);
  border: 1px solid rgba(black, .1);
}

.tasks__item__toggle:hover {
  background-color: rgba(black, .1);
  border-color: rgba(black, .15);
}

.tasks__item__toggle--completed {
  text-decoration: line-through;
  background-color: rgba(green, .15);
  border-color: rgba(green, .2);
}

.tasks__item__toggle--completed:hover {
  background-color: rgba(green, .25);
  border-color: rgba(green, .3);
}

.tasks__item__remove {
  position: absolute;
  height: 100%;
  top: 50%;
  right: 0;
  transform: translateY(-50%);
}

.tasks__item__remove i {
  vertical-align: middle;
}
</style>
