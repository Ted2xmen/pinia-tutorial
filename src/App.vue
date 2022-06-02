<script setup>
import { useUserStore } from "./stores/users";
import { ref } from "vue";

const user_store = useUserStore();

const user_input = ref({
  name: "",
  email: "",
});

const sort = ref(false);

const CreateUser = () => {
  if (!user_input.value.name || !user_input.value.email) {
    return alert("enter something");
  }

  user_store.create(user_input.value);

  user_input.value = {
    name: "",
    email: "",
  };
};

const deleteUser = (id) => {
  user_store.delete(id);
  alert("user deleted");
};
</script>

<template>
  <main>
    <h1>Team manager</h1>
    <form @submit.prevent="CreateUser">
      <input type="text" placeholder="enter name" v-model="user_input.name" />
      <input
        type="text"
        placeholder="enter e mail"
        v-model="user_input.email"
      />
      <input type="submit" value="Create User" />
    </form>

    <label> <span>Sort</span> <input type="checkbox" v-model="sort" /> </label>

    <div class="users" v-if="!sort">
      <div v-for="user in user_store.users" :key="user.id" class="user">
        {{ user.name }}
        <button @click="deleteUser(user.id)">Delete</button>
      </div>
    </div>

    <div class="users" v-else>
      <div v-for="user in user_store.userByName" :key="user.id" class="user">
        {{ user.name }}
        <button @click="deleteUser(user.id)">Delete</button>
      </div>
    </div>
  </main>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
