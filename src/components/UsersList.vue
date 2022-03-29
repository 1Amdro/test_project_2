<template>
  <section>
    <ul>
      <user-cart
        v-for="user in dataList"
        :key="user.id"
        :avatar="user.avatar"
        :email="user.email"
        :first_name="user.first_name"
        :last_name="user.last_name"
        :id="user.id"
        :change-user-data="changeUserData"
        :remove-user="removeUser"
      ></user-cart>
    </ul>
  </section>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import UserCart from './UserCart.vue';
import Collection from '@/types/Collection';

export default defineComponent({
  components: {
    UserCart,
  },
  props: {
    dataList: {
      type: Array as PropType<Collection[]>,
      required: true,
    },
    removeUser: {
      type: Function,
      required: true,
    },
    changeUserData: {
      type: Function,
      required: true,
    },
  },
});
</script>

<style scoped>
ul {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  padding: 0;
}
</style>
