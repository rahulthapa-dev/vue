<template>
  <section class="mb-5">
    <md-table class="mb-3 table-transaction-user">
      <md-table-row class="table-head">
        <md-table-head>First Name</md-table-head>
        <md-table-head>Last Name</md-table-head>
        <md-table-head>Email Address</md-table-head>
        <md-table-head>Phone Number</md-table-head>
        <md-table-head>Role</md-table-head>
        <md-table-head>Allocated Steps</md-table-head>
        <md-table-head>Access Code</md-table-head>
      </md-table-row>
      <md-table-row v-if="!users.length > 0">
        <md-table-cell colspan="7">
          <!-- no data start -->
          No record exist
          <!-- no data end -->
        </md-table-cell>
      </md-table-row>
      <md-table-row v-for="(transaction_user,index) in users" :key="index">
        <md-table-cell>{{transaction_user.first_name}}</md-table-cell>
        <md-table-cell>{{transaction_user.last_name}}</md-table-cell>
        <md-table-cell>{{transaction_user.email}}</md-table-cell>
        <md-table-cell>{{transaction_user.phone}}</md-table-cell>
        <md-table-cell>{{transaction_user.role_name | uppercase}}</md-table-cell>
        <md-table-cell class="steps">
          <md-list>
            <md-list-item
              v-for="(step_user,index) in transaction_user.steps_users"
              :key="index"
            >{{step_user.steps_title}}</md-list-item>
          </md-list>
        </md-table-cell>
        <md-table-cell>{{transaction_user.access_code}}</md-table-cell>
      </md-table-row>
    </md-table>
  </section>
</template>
<script>
import { HTTP } from "../../httpCommon";
export default {
  name: "TransactionUsers",
  props: ["app"],
  data: function() {
    return {
      users: []
    };
  },
  filters: {
    uppercase: function(s) {
      return s.charAt(0).toUpperCase() + s.slice(1);
    }
  },
  mounted() {
    this.getUsers();
  },
  methods: {
    getUsers() {
      HTTP.get("api/transactions/getUsers", {
        params: {
          transaction_id: this.$route.params.id,
          user_role: 5 //5 MEANS GET USER WITH ROLE tRANSACTUI USER
        }
      })
        .then(response => {
          if (response.data.Status) {
            this.users = response.data.Users;
            // this.transactionDesc = response.data.Agents[0].description;
          }
        })
        .catch(error => {});
    }
  }
};
</script>
