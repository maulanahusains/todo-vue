<script setup>
  import { computed, onMounted, ref } from 'vue';

  let id = 0;
  const ubahTodo = ref('');
  const newTodo = ref('');
  const data = ref([
    {id: id++, todo: 'belajar', checked: false, edit: false}
  ]);

  function addTodo() {
    data.value.push({id: id++, todo: newTodo.value, checked: false, edit: false});
    newTodo.value = ''
  }

  function editTodo(todo) {
    let edited = data.value.find((o, i) => {
      if(o.id == todo.id) {
        data.value[i] = {id: data.value[i].id, todo: data.value[i].todo, checked: data.value[i].checked, edit: true}
        ubahTodo.value = data.value[i].todo;
        return true;
      }
    }) 
  }

  function updateTodo(todo) {
    data.value.find((o, i) => {
      if(o.id == todo.id) {
        data.value[i] = {id: data.value[i].id, todo: ubahTodo.value, checked: data.value[i].checked, edit: false}
        ubahTodo.value = '';
        return true;
      }
    })
  }

  function removeTodo(todo) {
    data.value = data.value.filter((i) => i != todo);
  }


</script>

<template>
  <h2>Simple to-do list</h2>

  <div class="container">
    <div>
      <form class="input" @submit.prevent="addTodo">
        <input type="text" v-model="newTodo">
        <button @click="addTodo">Add</button>
      </form>
    </div>
    <div class="lists">
      <ul>
        <li v-for="list of data" :key="list.id">
          <input v-if="!list.edit" type="checkbox" v-model="list.checked">
          <span v-if="!list.edit" :class="{ done:list.checked }">{{ list.todo }}</span>
          <div v-if="!list.edit">
            <button @click="editTodo(list)">E</button>
            <button @click="removeTodo(list)">X</button>
          </div>
          <div v-else>
            <form @submit.prevent="updateTodo(list)" class="input">
              <input type="text" v-model="ubahTodo">
              <button>Save</button>
            </form>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
  .container {
    width: 90%;
    margin: 0 auto;
  }
  .input {
    display: flex;
    gap: 16px;
    justify-content: center;
    align-items: center;
  }
  .done {
    text-decoration: line-through;
  }

  ul {
    list-style: none;
    padding: 0;
  }

  li {
    gap: 4px;
    display: flex;
  }
</style>


