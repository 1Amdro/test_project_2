<template>
  <li @click="toggleInfo">
    <img :src="avatar" />
    <teleport to="#app">
      <div @click="toggleInfo" v-if="showInfo" class="overlay"></div>
      <dialog v-if="showInfo">
        <div v-if="!changeData">
          <h2>Full information:</h2>
          <div>{{ fullName }}</div>
          <div>Email: {{ email }}</div>
          <button @click="toggleChange">Change Data</button>
          <button @click="removeUser(id)">Remove User</button>
        </div>
        <the-form
          :id="id"
          @change-data="userDataChange"
          @cancel-changes="toggleChange"
          v-else
        ></the-form>
      </dialog>
    </teleport>
  </li>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import TheForm from './TheForm.vue';

export default defineComponent({
  props: {
    avatar: {
      type: String,
      required: true,
    },
    email: {
      type: String,
      required: true,
    },
    first_name: {
      type: String,
      required: true,
    },
    id: {
      type: Number,
      required: true,
    },
    last_name: {
      type: String,
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
  components: {
    TheForm,
  },
  data() {
    return {
      showInfo: false as boolean,
      changeData: false as boolean,
    };
  },
  methods: {
    toggleInfo() {
      this.showInfo = !this.showInfo;
    },
    toggleChange() {
      this.changeData = !this.changeData;
    },
    userDataChange(
      id: number,
      first_name: string,
      last_name: string,
      email: string,
      avatar: string
    ): void {
      this.changeUserData(id, first_name, last_name, email, avatar);
      this.toggleChange();
    },
  },
  computed: {
    fullName() {
      return this.first_name + ' ' + this.last_name;
    },
  },
});
</script>

<style scoped>
li {
  border: 3px solid #e3eef9;
  border-radius: 20px;
  width: 128px;
  padding: 12px;
  margin: 10px;
  cursor: pointer;
}
img {
  border-radius: 20px;
  width: 128px;
}

.overlay {
  background: #31363b6e;
  position: fixed;
  width: 100%;
  height: 100%;
  top: -10px;
}

h2 {
  text-align: center;
}

dialog {
  display: block;
  position: fixed;
  top: 15rem;
  border: 3px solid #e3eef9;
  border-radius: 20px;
}

div > div {
  margin-top: 10px;
  margin-bottom: 10px;
}
button {
  margin-top: 20px;
  margin-left: 10px;
  cursor: pointer;
}
</style>
