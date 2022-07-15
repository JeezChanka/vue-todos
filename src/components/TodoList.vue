<script>
export default {
  props: ["todo", "activeFilter"],
  emits: ["update", "delete"],
};
</script>

<template>
  <div v-if="activeFilter(todo) == true">
    <div class="row justify-content-xl-left mb-1">
      <input
        class="col-1 align-text-bottom"
        type="checkbox"
        v-model="todo.done"
        @click="$emit('update', todo)"
      />
      <label class="col-4">{{ todo.text }}</label>
      <button
        type="button"
        class="col-auto btn btn-success btn-sm"
        @click="$emit('delete', todo)"
      >
        Usuń
      </button>
    </div>
  </div>
</template>
