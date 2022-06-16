<template>
  <tr>
    <td>{{ member.no }}</td>
    <td>{{member.nama}}</td>
    <td>{{member.nim}}</td>
    <td>{{member.email}}</td>
    <td>
      <button class="mini ui blue button" @click="onEdit">Edit</button>
      <button class="mini ui red button" @click="onDelete">Delete</button>
    </td>
  </tr>
</template>

<script>
export default {
  name: "MemberData",
  props: {
    member: {
      type: Object
    }
  },
  methods: {
    onDelete() {
      // window.console.log(this.member.nama + " has been deleted!")
      this.$emit("onDelete", this.member.no);
    },
    onEdit() {
      this.$emit("onEdit", this.member);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
