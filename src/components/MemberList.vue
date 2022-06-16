<template>
  <div class="member-list">
    <div class="data">
      <table class="ui celled table">
        <thead><tr>
          <th style="width: 5%;text-align:center;">No.</th>
          <th>Nama Member</th>
          <th>NIM</th>
          <th>Email UB</th>
          <th style="width: 20%"></th>
        </tr></thead>
        <tbody>
          <MemberData 
          v-for="member in members" 
          :key="member.no" 
          :member="member" 
          @onDelete="onDelete"
          @onEdit="onEdit"
          />
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import MemberData from "./MemberData";

export default {
  name: "MemberList",
  components: {
    MemberData
  },
  props: {
    members: {
      type: Array
    }
  },
  methods: {
    onDelete(no) {
      // window.console.log("deleting " + no);
      this.$emit("onDelete", no);
    },
    onEdit(data) {
      this.$emit("onEdit", data);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
