<template>
  <div class="modal-wrapper" :class="resetWrapper || ''">
    <div class="modal-head">
      <h2>Masuk</h2>
      <router-link to="/register">Daftar</router-link>
    </div>
    <section class="modal-body">
      <form @submit.prevent="$store.commit('SUBMIT_EMAIL')" :class="{ hide: $store.getters.dataForm.currentForm, backIn: !$store.getters.dataForm.currentForm }">
        <div class="input-group">
          <label>Email</label>
          <input :class="{ inputError: $store.getters.dataForm.errorEmail }" type="text" v-model="$store.getters.dataForm.email" @input="$store.commit('CHECK_EMAIL')">
          <div class="input-info">
            <small :class="{ hide: $store.getters.dataForm.errorEmail }">Contoh: email@tokosidia.com</small>
            <small :class="{ hide: !$store.getters.dataForm.errorEmail, red: $store.getters.dataForm.errorEmail }">Format email salah</small>
          </div>
        </div>
        <button :class="{ disabled: $store.getters.dataForm.email.length === 0 || $store.getters.dataForm.errorEmail }">Selanjutnya</button>
      </form>
    </section>
    <section class="modal-body">
      <form @submit.prevent="submitLogin" :class="{ hide: !$store.getters.dataForm.currentForm, nextIn: $store.getters.dataForm.currentForm }">
        <label>Email</label>
        <div class="input-user">
          <span>{{ $store.getters.dataForm.email }}</span>
          <span @click="$store.getters.dataForm.currentForm = false" class="change-user">Ubah</span>
        </div>
        <div class="input-group password">
          <label>Kata Sandi</label>
          <div class="input password" :class="{ hidden: $store.getters.dataForm.hiddenPassword, visible: !$store.getters.dataForm.hiddenPassword }">
            <input :class="{ inputError: $store.getters.dataForm.errorPassword }" :type="$store.getters.dataForm.typePassword" v-model="$store.getters.dataForm.password">
            <img @click="$store.commit('TOGGLE_HIDDEN')" class="pass-hidden" src="@/assets/img/pass-hidden.png">
            <img @click="$store.commit('TOGGLE_HIDDEN')" class="pass-visible" src="@/assets/img/pass-visible.png">
          </div>
          <div class="input-info">
            <small :class="{ hide: !$store.getters.dataForm.errorPassword, red: $store.getters.dataForm.errorPassword }">Email / Kata Sandi salah</small>
          </div>
        </div>
        <div class="input-group checkbox">
          <label class="container">Ingat Saya
            <input type="checkbox">
            <span class="checkmark"></span>
          </label>
          <router-link to="/">Lupa kata sandi?</router-link>
        </div>
        <button>Masuk</button>
      </form>
    </section>
    <div class="modal-footer">
      <p>Belum punya akun Tokosidia? <router-link to="/register">Daftar</router-link></p>
    </div>
  </div>
</template>

<script>
export default {
  props: ['resetWrapper'],
  methods: {
    submitLogin () {
      this.$store.dispatch('submitLogin')
        .then(res => {
          if (res.data.status !== 200) {
            this.$store.state.dataForm.errorPassword = true
            return
          }
          this.$store.state.dataForm.errorPassword = false
          this.$router.go()
        })
    }
  }
}
</script>

<style lang="scss" scoped>
@keyframes backIn {
  from {
    opacity: 0;
    visibility: hidden;
    transform: translateX(-20px);
  }
  to {
    opacity: 1;
    visibility: visible;
    transform: translateX(0px);
  }
}
@keyframes nextIn {
  from {
    opacity: 0;
    visibility: hidden;
    transform: translateX(20px);
  }
  to {
    opacity: 1;
    visibility: visible;
    transform: translateX(0px);
  }
}
.modal-wrapper {
  width: 368px;
  background-color: white;
  box-shadow: 0 0 6px rgba(0, 0, 0, 0.136);
  padding: 40px 32px 20px;
  border-radius: 12px;
  margin: 0 auto;
  overflow: hidden;
  &.yes {
    box-shadow: none;
    padding: 0;
    margin: 0;
    width: auto;
    overflow: visible;
  }
}
.modal-head {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 24px;
  h2 {
    font-size: 20px;
    font-weight: 600;
    color: rgb(50, 50, 50);
  }
  a {
    font-size: 13px;
    font-weight: 600;
    color: #42b549;
  }
}
.modal-body {
  line-height: normal;
  form {
    background-color: white;
    position: relative;
    animation-duration: .4s;
    animation-timing-function: ease;
    &.hide {
      visibility: hidden;
      position: absolute;
      animation-duration: .4s;
      animation-timing-function: ease;
    }
    &.backIn {
      animation-name: backIn;
      animation-fill-mode: forwards;
      animation-duration: .4s;
      animation-timing-function: ease;
      z-index: 1;
    }
    &.nextIn {
      animation-name: nextIn;
      animation-fill-mode: forwards;
      animation-duration: .4s;
      animation-timing-function: ease;
      z-index: 1;
    }
  }
  label {
    display: block;
    font-size: 12px;
    color: rgba(0, 0, 0, 0.54);
    margin-bottom: 7px;
  }
  input {
    width: 100%;
    padding: 10px 14px;
    border-radius: 3px;
    border: 1px solid rgba(0, 0, 0, 0.122);
    background-color: white;
    outline: none;
    margin-bottom: 4px;
    font-size: 14px;
    color: rgba(0, 0, 0, .7);
    line-height: normal;
    transition: border .2s linear;
    &.inputError {
      border: 1px solid red;
      &:focus {
        border: 1px solid red;
      }
    }
    &#password {
      padding-right: 35px;
    }
    &:active {
      border: none;
      outline: none;
    }
    &:focus {
      border: 1px solid #42b549;
    }
  }
  .input-user {
    margin-bottom: 18px;
    span {
      font-size: 14px;
      color: rgba(0, 0, 0, .7);
      &.change-user {
        margin-left: 8px;
        font-size: 12px;
        color: #42b549;
        cursor: pointer;
      }
    }
  }
  .input.password {
    position: relative;
    display: flex;
    align-items: center;
    img {
      height: 20px;
      position: absolute;
      right: 10px;
      cursor: pointer;
    }
  }
  .input.password.visible {
    img.pass-hidden {
      visibility: hidden;
    }
  }
  .input.password.hidden {
    img.pass-visible {
      visibility: hidden;
    }
  }
  .input-info {
    display: flex;
    flex-direction: column;
    margin-bottom: 5px;
    position: relative;
    height: 22px;
    small {
      font-size: 12px;
      color: rgba(0, 0, 0, .38);
      line-height: normal;
      position: absolute;
      &.hide {
        visibility: hidden;
      }
      &.red {
        color: red;
      }
    }
  }
  button {
    width: 100%;
    padding: 12px 0;
    border: none;
    background-color: #03ac0e;
    border-radius: 8px;
    font-family: 'Arial', Arial, sans-serif;
    font-weight: bold;
    font-size: 13px;
    color: white;
    transition: background-color .3s;
    &:hover {
      background-color: darken($color: #03ac0e, $amount: 5);
    }
    &.disabled {
      background-color: #e4e4e4;
      color: #bfbfbf;
      cursor: not-allowed;
    }
  }
  .input-group.checkbox {
    display: flex;
    justify-content: space-between;
    margin: 18px 0 7px;
    a {
      color: #03ac0e;
    }
  }
  .container {
    display: block;
    position: relative;
    padding-left: 28px;
    margin-bottom: 12px;
    cursor: pointer;
    font-size: 12px;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    user-select: none;
  }
  /* Hide the browser's default checkbox */
  .container input {
    position: absolute;
    opacity: 0;
    cursor: pointer;
    height: 0;
    width: 0;
  }
  /* Create a custom checkbox */
  .checkmark {
    position: absolute;
    top: 0;
    left: 0;
    height: 17px;
    width: 17px;
    border: 1px solid rgba(0, 0, 0, 0.149);
    border-radius: 4px;
  }
  /* When the checkbox is checked, add a blue background */
  .container input:checked ~ .checkmark {
    background-color: #03ac0e;
    border: 1px solid #03ac0e;
  }
  /* Create the checkmark/indicator (hidden when not checked) */
  .checkmark:after {
    content: "";
    position: absolute;
    display: none;
  }
  /* Show the checkmark when checked */
  .container input:checked ~ .checkmark:after {
    display: block;
  }
  /* Style the checkmark/indicator */
  .container .checkmark:after {
    left: 4px;
    top: 0px;
    width: 5px;
    height: 10px;
    border: solid white;
    border-width: 0 1.5px 1.5px 0;
    -webkit-transform: rotate(45deg);
    -ms-transform: rotate(45deg);
    transform: rotate(45deg);
  }
}
.modal-footer {
  margin: 20px 0 5px;
  p {
    text-align: center;
    font-size: 13px;
    color: rgba(0, 0, 0, .5);
    a {
      font-size: 13px;
      font-weight: 600;
      color: #42b549;
    }
  }
}
</style>
