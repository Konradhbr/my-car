<template>
  <modal @close="close">
    <div class="modal__info">
      <span class="decor">Reset hasła</span>
      <div class="modal__text">
        <form @submit.prevent="onSubmit()">
          <label for="name">E-mail</label><br />
          <input
            name="name"
            type="email"
            value=""
            v-model="form.email"
            required
          />
          <br />
          <button type="submit" class="button button--full" @click="sendEmail">
            <span>zresetuj</span>
          </button>
        </form>
      </div>
      <div
        class="modal__message"
        :class="{ 'modal__message--active': recoverAlert }"
      >
        <p>
          Link do zresetowania hasła został wysłany na powyższy adres e-mail
        </p>
      </div>
    </div>
  </modal>
</template>

<script>
import Modal from "@/components/Modals/Modal.vue";
import firebase from "firebase";

export default {
  name: "ModalRecoverPassword",
  components: { Modal },
  data() {
    return {
      recoverAlert: false,
      form: {
        email: ""
      },
      error: null,
      emailSending: false
    };
  },
  methods: {
    close() {
      this.$emit("close", true);
    },
    onSubmit() {
      this.recoverAlert = true;
    },
    sendEmail() {
      this.error = null;
      this.emailSending = true;
      firebase
        .auth()
        .sendPasswordResetEmail(this.form.email)
        .then(() => {
          this.emailSending = false;
        })
        .catch(error => {
          this.emailSending = false;
          this.error = error.message;
        });
    }
  }
};
</script>

<style lang="scss" scoped>
button {
  margin-top: 20px;
}
.modal__message {
  text-align: center;
  display: none;
}
.modal__message--active {
  display: block;
}
</style>
