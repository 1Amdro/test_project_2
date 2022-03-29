<template>
  <div>
    <users-list
      :dataList="usersList"
      :changeUserData="changeUserData"
      :removeUser="removeUser"
    ></users-list>
    <button @click="updateList">Update</button>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Collection from '@/types/Collection';
import UsersList from './components/UsersList.vue';
import axios from 'axios';

export default defineComponent({
  name: 'App',
  components: { UsersList },
  data() {
    return {
      usersList: [] as Collection[],
    };
  },
  methods: {
    async initialize() {
      try {
        const collection = await this.getDataFromServer();
        if (!localStorage.getItem('collection'))
          localStorage.setItem('collection', JSON.stringify(collection));
        const storageData: any = localStorage.getItem('collection');
        this.usersList = JSON.parse(storageData);
      } catch (error: any) {
        console.error(error.message);
      }
    },
    removeUser(id: number) {
      const currentId = this.usersList.findIndex(item => item.id === id);
      this.usersList.splice(currentId, 1);
      localStorage.setItem('collection', JSON.stringify(this.usersList));
    },
    changeUserData(
      id: number,
      first_name: string,
      last_name: string,
      email: string,
      avatar: string
    ) {
      const currentUser: any = this.usersList.find(item => item.id === id);
      currentUser.first_name = first_name || currentUser.first_name;
      currentUser.last_name = last_name || currentUser.last_name;
      currentUser.email = email || currentUser.email;
      currentUser.avatar = avatar || currentUser.avatar;
      localStorage.setItem('collection', JSON.stringify(this.usersList));
    },
    async updateList() {
      const updatedData = await this.getDataFromServer();
      this.usersList.splice(0);
      this.usersList.push(...updatedData);
      localStorage.setItem('collection', JSON.stringify(updatedData));
    },
    async getDataFromServer(): Promise<Collection[]> {
      const response = await axios.get('https://reqres.in/api/users?page=2');
      return response.data.data;
    },
  },
  mounted() {
    this.initialize();
  },
});
</script>

<style>
body {
  margin: 0;
}

div {
  display: flex;
  flex-direction: column;
  align-content: center;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
}
</style>
