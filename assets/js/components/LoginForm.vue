<template>
  <form v-on:submit.prevent="handleSubmit">
    <div v-if="error" class="alert alert-danger">
      {{ error }}
    </div>
    <div class="form-group align-items-center">
      <label for="exampleInputEmail1">Email address</label>
      <input type="email" v-model="email" class="form-control" id="exampleInputEmail1"
             aria-describedby="emailHelp" placeholder="Enter email">
    </div>

    <div class="form-group">
      <label for="exampleInputPassword1">Password</label>
      <input type="password" v-model="password" class="form-control"
             id="exampleInputPassword1" placeholder="Password">
    </div>
    <br>
    <button type="submit" class="btn btn-primary" v-bind:class="{ disabled: isLoading }">Log in</button>
  </form>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      email: '',
      password: '',
      error: '',
      isLoading: false
    }
  },
  props: ['user'],
  methods: {
    handleSubmit() {
      this.isLoading = true;
      this.error = '';
      axios
          .post('/login', {
            email: this.email,
            password: this.password
            
          })
          .then(response => {
            console.log(this.email);
             console.log(this.password);
            this.$emit('user-authenticated', userUri);
            this.email = '';
            this.password = '';
          }).catch(error => {
        if (error.response.data.error) {
          this.error = error.response.data.error;
        } else {
          this.error = 'Unknown error';
        }
      }).finally(() => {
        this.isLoading = false;
      })
    },
  },
}
</script>

<style scoped lang="scss">
</style>
