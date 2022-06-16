<template>
  <div id="app">
    <div class="ui top attached menu">
      <div class="container">
        <a href="javascript:void(0)" class="item">Perpus Membership</a>
      </div>
      <div class="right menu">
        <a v-if="!$auth.isAuthenticated" @click="login" class="ui item">
          Login
        </a>
        <a v-if="$auth.isAuthenticated" @click="logout" class="ui item">
          Logout
        </a>
      </div>
    </div>
    <div v-if="!$auth.isAuthenticated" class="ui container placeholder segment">
      <h1 class="ui center aligned huge header">
        Perpus Membership
        <div class="sub header">Login di pojok kanan atas untuk memulai!</div>
      </h1>
    </div>
    <div v-if="$auth.isAuthenticated" class="ui main container">
      <div class="ui two column stackable grid">
        <div class="five wide column">
          <InputForm :form="form" @onSubmit="onSubmit" />
        </div>
        <MyLoader v-if="loader" />
        <div class="eleven wide column">
          <MemberList
            :members="members"
            @onDelete="onDelete"
            @onEdit="onEdit"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import InputForm from "./components/InputForm";
import MemberList from "./components/MemberList";
import MyLoader from "./components/MyLoader";

export default {
  name: "App",
  components: {
    InputForm,
    MemberList,
    MyLoader,
  },
  data() {
    return {
      url: "http://localhost/projekakhir-rest-api/public/api/member",
      members: [],
      form: { nama: "", nim: "", email: "", isEdited: false },
      loader: false,
    };
  },
  methods: {
    login() {
      this.$auth.loginWithRedirect();
    },
    logout() {
      this.$auth.logout({
        returnTo: window.location.origin
      });
    },
    getMembers() {
      this.loader = true;
      axios.get(this.url).then((data) => {
        this.members = data.data;
        this.loader = false;
      });
    },
    createMember(data) {
      this.loader = true;
      axios
        .post(this.url, {
          nama: data.nama,
          nim: data.nim,
          email: data.email,
        })
        .then(() => {
          this.getMembers();
        })
        .catch((e) => {
          alert(e);
        });
    },
    editMember(data) {
      this.loader = true;
      axios
        .put(`${this.url}/${data.no}`, {
          nama: data.nama,
          nim: data.nim,
          email: data.email,
        })
        .then(() => {
          this.getMembers();
        })
        .catch((e) => {
          alert(e);
        });
    },
    deleteMember(no) {
      this.loader = true;

      axios
        .delete(`${this.url}/${no}`)
        .then(() => {
          this.getMembers();
        })
        .catch((e) => {
          alert(e);
        });
    },
    onEdit(data) {
      this.form = data;
      this.form.isEdited = true;
    },
    onDelete(no) {
      // window.console.log("delet from app "+no);
      this.deleteMember(no);
    },
    onSubmit(data) {
      // window.console.log("app is ready", data);
      if (data.isEdited) {
        this.editMember(data);
      } else {
        this.createMember(data);
      }
    },
  },
  created() {
    this.getMembers();
  },
};
</script>

<style>
.main.container {
  margin-top: 60px;
}
.placeholder.segment {
  margin-top: 10% !important;
}
.sub.header {
  margin-top: 1.5% !important;
}
.data {
  margin-top: 20px;
}
.submit-button {
  margin-top: 24px !important;
  float: right;
}
</style>
