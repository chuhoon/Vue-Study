<template>
  <form @submit.prevent="submitForm">
    <div>
      <label for="username">id:</label>
      <input id="username" type="text" v-model="username" />
    </div>
    <div>
      <label for="password">pw:</label>
      <input id="password" type="text" v-model="password" />
    </div>
    <div>
      <label for="nickname">nickname:</label>
      <input id="nickname" type="text" v-model="nickname" />
    </div>
    <button
      v-bind:disabled="!isSignUpIdValid || !password || !nickname"
      type="submit"
    >
      회원가입
    </button>
    <p>{{ logMessage }}</p>
  </form>
</template>

<script>
import { registerUser } from '@/api/index';
import { validationEmail } from '@/utils/validation';

export default {
  data() {
    return {
      username: '',
      password: '',
      nickname: '',
      logMessage: '',
    };
  },
  computed: {
    isSignUpIdValid() {
      return validationEmail(this.username);
    },
  },
  methods: {
    async submitForm() {
      try {
        console.log('폼 제출');
        const userData = {
          username: this.username,
          password: this.password,
          nickname: this.nickname,
        };
        const response = await registerUser(userData);
        console.log(response.data);
        this.logMessage = `${response.data.username}님이 가입되었습니다.`;
      } catch (error) {
        this.logMessage = error.response.data;
      } finally {
        this.initForm();
      }
    },
    initForm() {
      this.username = '';
      this.password = '';
      this.nickname = '';
    },
  },
};
</script>

<style></style>
