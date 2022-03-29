<template>
  <form @click.prevent="">
    <h2>Change user data</h2>
    <input
      v-model.trim="entered_first_name"
      type="text"
      placeholder="first name"
    />
    <input
      v-model.trim="entered_last_name"
      type="text"
      placeholder="last name"
    />
    <input v-model="entered_email" placeholder="email" type="email" required />
    <input v-model="entered_avatar" placeholder="avatar" type="text" />
    <button @click="changeData">Change</button>
    <button @click="cancelChanges">Cancel</button>
  </form>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  emit: ['change-data'],
  props: {
    id: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      entered_first_name: '' as string,
      entered_last_name: '' as string,
      entered_email: '' as string,
      entered_avatar: '' as string,
    };
  },
  methods: {
    changeData() {
      this.$emit(
        'change-data',
        this.id,
        this.entered_first_name,
        this.entered_last_name,
        this.entered_email,
        this.entered_avatar
      );
    },
    cancelChanges() {
      this.$emit('cancel-changes');
    },
  },
});
</script>

<style scoped>
form {
  display: flex;
  flex-direction: column;
}

input {
  margin-bottom: 10px;
}
button {
  margin-right: 10px;
  margin-top: 10px;
  cursor: pointer;
}
</style>
