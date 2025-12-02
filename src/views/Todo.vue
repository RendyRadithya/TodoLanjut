
<template>
  <h3>To-Do ⏳</h3>

  <form @submit.prevent="addTodo" style="margin-bottom:8px;">
    <input v-model="todo.text" type="text" name="text" />
    <button :disabled="!todo.text" type="submit">Add</button>
  </form>

  <div>
    <ul class="todo-list">
      <li v-for="pendingTodo in pendingTodos" :key="pendingTodo.id" class="todo-item">
        <template v-if="editingId === pendingTodo.id">
          <input v-model="editText" class="edit-input" />
          <div class="todo-actions">
            <button class="btn-done" @click="saveEdit(pendingTodo)">Save</button>
            <button class="btn-remove" @click="cancelEdit">Cancel</button>
          </div>
        </template>
        <template v-else>
          <span class="todo-text">{{ pendingTodo.text }}</span>
          <div class="todo-actions">
            <button class="btn-edit" @click="startEdit(pendingTodo)">Edit</button>
            <button class="btn-done" @click="updateTodo({ ...pendingTodo, isCompleted: true})">Done</button>
            <button class="btn-remove" @click="destroyTodo(pendingTodo.id)">Remove</button>
          </div>
        </template>
      </li>
    </ul>
  </div>
</template>


<script>
import { useTodos } from '@/stores/todos'
import { mapActions, mapState } from 'pinia'

export default {
  computed: {
    ...mapState(useTodos, [
      'pendingTodos'
    ])
  },

  data() {
    return {
      todo: {
        text: null,
        isCompleted: false
      },
      // local UI state for editing
      editingId: null,
      editText: ''
    }
  },

  methods: {
    ...mapActions(useTodos, [
      'storeTodo', 'updateTodo', 'destroyTodo'
    ]),

    addTodo() {
      const text = this.todo.text && this.todo.text.toString().trim()
      if (!text) return
      this.storeTodo({ text })
      this.todo.text = null
    }
    ,

    startEdit(item) {
      this.editingId = item.id
      this.editText = item.text
    },

    saveEdit(item) {
      const text = this.editText && this.editText.toString().trim()
      if (!text) return
      this.updateTodo({ id: item.id, text, isCompleted: item.isCompleted })
      this.editingId = null
      this.editText = ''
    },

    cancelEdit() {
      this.editingId = null
      this.editText = ''
    }
  }
}
</script>