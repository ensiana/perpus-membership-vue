<template>
  <div class="input-form">
    <form class="ui form">
      <div class="fields">
        <div class="sixteen wide field">
          <label>Nama Lengkap</label>
          <input
            type="text"
            name="nama"
            placeholder="Nama Lengkap"
            @change="handlerChange"
            :value="form.nama"
          />
        </div>
      </div>
      <div class="fields">
        <div class="sixteen wide field">
          <label>NIM</label>
          <input
            type="text"
            name="nim"
            placeholder="NIM"
            @change="handlerChange"
            :value="form.nim"
          />
        </div>
      </div>
      <div class="fields">
        <div class="sixteen wide field">
          <label>Email</label>
          <input
            type="text"
            name="email"
            placeholder="Email"
            @change="handlerChange"
            :value="form.email"
          />
        </div>
      </div>
      <div class="fields">
        <div class="sixteen wide field">
          <button :class="btnClass" @click="onSubmit">
            {{ btnName }}
          </button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  name: "InputForm",
  props: {
    form: {
      type: Object,
    },
  },
  data() {
    return {
      formLocal: this.form,
      btnName: "Submit",
      btnClass: "ui primary button submit-button"
    };
  },
  methods: {
    handlerChange(event) {
      const { name, value } = event.target;
      let form = this.form;
      form[name] = value;
      this.formLocal = form;
    },
    onSubmit(e) {
      e.preventDefault();

      if (this.formValid()) {
        // window.console.log("ready");
        this.$emit("onSubmit", this.form);
        this.btnName = "Submit";
        this.btnClass = "ui primary button submit-button";
        this.clearForm();
      }
    },
    formValid() {
      if (document.getElementsByName("nama")[0].value === "") {
        alert("Nama is empty!");
        return false;
      }
      if (document.getElementsByName("nim")[0].value === "") {
        alert("NIM is empty!");
        return false;
      }
      if (document.getElementsByName("email")[0].value === "") {
        alert("Email is empty!");
        return false;
      }

      return true;
    },
    clearForm() {
      this.formLocal.nama = "";
      this.formLocal.nim = "";
      this.formLocal.email = "";
      this.formLocal.isEdited = false;
      document.querySelector(".formLocal").reset();
    }
  },
  updated() {
    if (this.form.isEdited) {
      this.btnName = "Edit";
      this.btnClass = "ui yellow button submit-button"
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
