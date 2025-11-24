<template>
    <h3>Completed ✅</h3>
    <div>
      <ul>
        <li v-for="completedTodo in completedTodos" :key="completedTodo.id" style="margin: 8px 0">
          {{ completedTodo.text }}
        </li>
      </ul>
    </div>
</template>

<script>
import { mapState } from 'pinia';
import { useTodos } from '@/stores/todos';

import HeaderBar from '@/components/Header.vue';
import FooterBar from '@/components/Footer.vue';

export default {
  components: {
    HeaderBar,
    FooterBar,
  },
  computed: {
    ...mapState(useTodos, [
      'completedTodos'
    ])
  }
}
</script>