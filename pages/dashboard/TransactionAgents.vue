
<template>
  <div>
    <!-- DELETE AGENT MODEL : POPUP -->
    <md-dialog :md-active.sync="dialogActive" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Delete</h4>
        <md-button @click="dialogActive = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content pb-0">
        <form>
          <md-content md-dynamic-height class="px-0">
            <div class="md-layout">
              <div class="md-layout-item md-medium-size-100 md-size-100 md-small-size-100">
                <p class="text-primary description text-center mt-4 mb-3">
                  You have selected to delete
                  <span class="brand-primary">{{this.userName}}</span>.
                </p>
                <md-dialog-actions class="flex-wrap justify-content-center">
                  <md-button
                    class="md-danger w-100 button-primary-custom mb-2 mt-3 no-shadow"
                    @click="confirmDelete"
                  >
                    <LoadingButtonLoader
                      :typeEnable="'simpleLoader'"
                      v-bind:enable="app.loading"
                      class="mr-1"
                    />Delete
                  </md-button>
                  <div class="d-block w-100 text-center">
                    <md-button
                      :md-ripple="false"
                      @click="dialogActive = false"
                      class="cancel-link lighter-description mt-2 mb-2 text-capitalize"
                    >Cancel</md-button>
                  </div>
                </md-dialog-actions>
              </div>
            </div>
          </md-content>
        </form>
      </div>
    </md-dialog>
    <!-- EDIT AGENT MODEL : POP UP -->

    <md-dialog :md-active.sync="active" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Edit agents</h4>
        <md-button @click="active = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content pb-0">
        <form @submit.prevent="updateAgent">
          <md-content md-dynamic-height class="px-4">
            <div class="md-layout">
              <div v-for="(agent,index) in agents" :key="index" class="md-layout-item md-size-100">
                <div class="md-layout p-30 position-relative">
                  <div class="remove-icon position-absolute" v-on:click="removeElement(index)">
                    <md-icon>remove</md-icon>
                  </div>

                  <div
                    class="md-layout-item md-small-size-100 md-size-50 video-integration-field position-relative"
                  >
                    <!-- IMAGE SECTION -->
                    <div v-if="agent.image" class="md-avatar position-absolute">
                      <img v-bind:src="'/images/users/' + agent.image" alt="Agent" />
                    </div>
                    <div
                      v-else
                      class="md-avatar md-intial position-absolute"
                    >{{agent.first_name.substring(0,1).toUpperCase()}}{{agent.last_name.substring(0,1).toUpperCase()}}</div>
                    <!-- AGENT DROPDOWN -->
                    <md-field slot="inputs">
                      <label>Agent name</label>
                      <md-select
                        name="role"
                        id="role"
                        v-model="agent.user_id"
                        @md-selected="checkifAgentexist()"
                      >
                        <md-option
                          :value="sysAgent.id"
                          v-for="(sysAgent) in systemAgents"
                          :key="sysAgent.id"
                        >{{sysAgent.first_name}} {{sysAgent.last_name}}</md-option>
                      </md-select>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field>
                      <label>Comission Earned</label>
                      <span class="md-prefix">$</span>
                      <!-- v-bind:value="brewMethod.id" v-model="locations[key].methodsAvailable" -->
                      <md-input v-model="agents[index].commission"></md-input>
                    </md-field>
                  </div>
                </div>
              </div>

              <!-- <div class="md-layout-item md-size-100">
                <div class="add-agent-link d-flex align-center cursor-pointer">
                  <div class="add-icon mr-2">
                    <md-icon>add</md-icon>
                  </div>
                  <span class="text-success">Add agent</span>
                </div>
              </div>-->
            </div>
          </md-content>
          <div class="d-block w-100 text-center">
            <md-button type="submit" class="md-primary button-primary-custom mb-2 mt-3 px-4">
              <LoadingButtonLoader
                :typeEnable="'simpleLoader'"
                v-bind:enable="app.loading"
                class="mr-1"
              />
              <span>Save changes</span>
            </md-button>
            <br />
            <div class="d-block text-center">
              <md-button
                :md-ripple="false"
                @click="active = false"
                class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
              >Cancel</md-button>
            </div>
          </div>
        </form>
      </div>
    </md-dialog>
    <!-- TRANSACTION AGENTS LISTING : POPUP -->
    <md-table class="mb-3 table-agent">
      <md-table-row class="table-head">
        <md-table-head></md-table-head>
        <md-table-head>First Name</md-table-head>
        <md-table-head>Last Name</md-table-head>
        <md-table-head>Email Address</md-table-head>
        <md-table-head>Phone Number</md-table-head>
        <md-table-head>Title</md-table-head>
        <md-table-head>Commission Earned</md-table-head>
        <md-table-head></md-table-head>
      </md-table-row>
      <!-- WHEN NO AGENTS ADDED YET-->

      <md-table-row v-if="!agents.length > 0">
        <md-table-cell colspan="8">
          <!-- no data start -->
          No record exist
          <!-- no data end -->
        </md-table-cell>
      </md-table-row>
      <!-- LOOP THROUGH AGENTS-->

      <md-table-row v-for="(agent,index) in agents" :key="index">
        <md-table-cell>
          <div
            v-if="!agent.image"
            class="md-avatar md-intial"
          >{{agent.first_name.substring(0,1).toUpperCase()}}{{agent.last_name.substring(0,1).toUpperCase()}}</div>
          <div v-if="agent.image" class="md-avatar">
            <img v-bind:src="'/images/users/' + agent.image" alt="Agent" />
          </div>
        </md-table-cell>
        <md-table-cell>{{agent.first_name}}</md-table-cell>
        <md-table-cell>
          <span class="text-primary">{{agent.last_name}}</span>
        </md-table-cell>
        <md-table-cell>{{agent.email}}</md-table-cell>
        <md-table-cell>{{agent.phone}}</md-table-cell>
        <md-table-cell>{{agent.title}}</md-table-cell>
        <md-table-cell>
          <span
            v-if="agent.commission != 0"
            class="text-primary md-title font-weight-semi-bold"
          >${{agent.commission}}</span>
          <span
            v-if="agent.commission == 0"
            class="text-primary md-title font-weight-semi-bold"
          >No comission assigned</span>
        </md-table-cell>
        <md-table-cell md-label>
          <md-menu md-direction="bottom-end">
            <md-button md-menu-trigger class="md-icon-button icon-button-flat">
              <md-icon>more_vert</md-icon>
            </md-button>
            <md-menu-content class="menu-content-custom">
              <md-menu-item @click="editUser(agent.id,index)">Edit</md-menu-item>
              <md-menu-item @click="deleteUser(agent)">Delete</md-menu-item>
            </md-menu-content>
          </md-menu>
        </md-table-cell>
      </md-table-row>
    </md-table>
  </div>
</template>


<script>
import { HTTP } from "../../httpCommon";
import LoadingButtonLoader from "../../components/LoadingButtonLoader";
export default {
  name: "TransactionAgents",
  props: ["app", "transaction_id"],
  components: { LoadingButtonLoader },
  data: function() {
    return {
      active: false,
      dataAgent: [],
      agents: [],
      dialogActive: false,
      userName: "", //for delete dialog box
      systemAgents: [],
      commissions: [],
      methodsAvailable: [],
      list: [{}]
    };
  },
  created: function() {},
  watch: {},

  mounted() {
    this.init();
    this.getAllTransactionAgents();
  },
  methods: {
    init() {
      this.getSystemAgents(); // calling here so that it will not delay when pop up open
    },
    checkifAgentexist() {
      console.log("here is");
      console.log(this.list);
    },

    updateAgent() {
      this.app.loading = true;
      let dataAgent = {};
      let list = [];
      dataAgent["transaction_id"] = this.transaction_id;
      this.agents.map(function(value, key) {
        list[key] = {
          user_id: value.user_id,
          commission: value.commission
        };
      });
      dataAgent["list"] = list;
      console.log("Here is list");
      console.log(dataAgent);
      //UPDATE AGENTS CORRESPONDING TO TRANSACTION
      HTTP.post("api/transactions/updateAgents", dataAgent)
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            this.getAllTransactionAgents();
            this.active = false;
          }
        })
        .catch(error => {});
    },
    removeElement: function(index) {
      this.agents.splice(index, 1);
    },
    /**EDIT AGENT POP UP MODEL */
    editUser(id, index) {
      this.active = true;
    },
    getSystemAgents() {
      HTTP.get("api/users/getAgents", {
        params: {}
      })
        .then(response => {
          if (response.data.Status) {
            this.systemAgents = response.data.User;
          }
        })
        .catch(error => {});
    },
    getAllTransactionAgents() {
      HTTP.get("api/transactions/getUsers", {
        params: {
          transaction_id: this.$route.params.id,
          user_role: 2 //4 MEANS GET USER WITH ROLE VENDOR
        }
      })
        .then(response => {
          if (response.data.Status) {
            this.agents = response.data.Users;
            this.transactionDesc = response.data.Users[0].description;
          }
        })
        .catch(error => {});
    },
    deleteUser(data) {
      this.dialogActive = true;
      this.deleteId = data.user_id; //do not remove user_id
      this.userName = data.first_name + " " + data.last_name;
    },
    confirmDelete() {
      this.app.loading = true;
      //Unassociate that user from this transaction
      HTTP.get("api/transactions/unassignUser", {
        params: {
          transaction_id: this.$route.params.id,
          delete_id: this.deleteId //basically its a user id that we are going to unassociate
        }
      })
        .then(response => {
          if (response.data.Status) {
            this.getAllTransactionAgents();
            console.log("success");
            this.$toasted
              .show(response.data.Message, {
                type: "success"
                // icon: "error_outline"
              })
              .goAway(1500);
            this.dialogActive = false;
            this.app.loading = false;
          }
        })
        .catch(error => {});
    }
  },

  computed: {}
};
</script>
