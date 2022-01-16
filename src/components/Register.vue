<template>
    <b-modal
      id="modal-register"
      ref="modal"
      @hide="hideModal()"
      hide-header
      hide-footer
      style="font-family: 'Montserrat', sans-serif;"
    >
      <b-form class="form" @submit="preventDefault()">
        <b-form-group
          id="input-group-1"
        >
          <b-form-input
            id="input-1"
            v-model="user.email"
            type="email"
            placeholder="email"
            required
            style="border-radius:2px;font-size:13px;"
          ></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-3" class="mt-2">
          <b-form-input
            id="input-2"
            v-model="user.name"
            type="text"
            placeholder="username"
            required
            style="border-radius:2px;font-size:13px;"
          ></b-form-input>
        </b-form-group>

        <b-form-group id="input-group-2" class="mt-2">
          <b-form-input
            id="input-2"
            v-model="user.password"
            type="password"
            placeholder="password"
            required
            style="border-radius:2px;font-size:13px;"
          ></b-form-input>
        </b-form-group>
        <div style="text-align:center">
          <b-button class="login-button" variant="btn btn-primary" @click="register()">Signup</b-button>
        </div>
      </b-form>
    </b-modal>
</template>

<script>
import { authService } from "../services/AuthService"
import { mapActions } from 'vuex'

  export default {
    data() {
      return {
        user: {},
        errors: {}
      }
    },
    methods: {
      ...mapActions({
          login: 'login'
      }),
      loginUser(){
        this.login(this.user).then(() => {
          this.user = {}
          this.$router.push({name: 'Dashboard'})
        })
      },
      register(){
        authService.register(this.user).then(() => {
            this.$nextTick(() => {
              this.loginUser(this.user)
              this.$bvModal.hide('modal-register')
            })
        }).catch(error => {
          this.errors = error.response.data.errors
        })
      },
      hideModal(){
        this.user = {}
        this.errors = {}
        this.$bvModal.hide('modal-prevent-closing')
      },
      removeErrorName(){
        if(this.errors.name){
          this.errors.name = null
        }
      },
      removeErrorEmail(){
        if(this.errors.email){
            this.errors.email = null
        }
      },
      removeErrorPassword(){
        if(this.errors.password){
            this.errors.password = null
        }
      },
      removeErrorConfirmPassword(){
        if(this.errors.repeat_password){
            this.errors.repeat_password = null
        }
      }
    }
  }
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,300;1,100;1,300&display=swap');
.form {
  font-family: 'Montserrat', sans-serif;
}
.login-button {
  border-radius: 2px;
  font-size: 13px;
  width: 100px;
  background-color:#66a3ff;
  border: none;
  margin-top: 35px;
  text-align: center;
}
</style>