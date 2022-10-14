<template id="task-item">
    <li class="tasks__item">
      <button :class="className"
          @click.self="$emit('complete')"
      >
        {{ task.title }}
      </button>
      <button class="tasks__item__remove button alert pull-right"
              @click="$emit('remove')"
      >
        <i class="fa fa-times"></i>
      </button>
    </li>
</template>

<script>
    export default {
        name: 'TaskItem',
    }
    console.log("in task item");
</script>