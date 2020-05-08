<template>
  <!-- <AddNewTransactionUserModal/> -->
  <md-dialog :md-active.sync="active" class="modal-large" :md-click-outside-to-close="false">
    <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
      <h4 class="my-0 md-headline text-left text-primary">Add Transaction User</h4>
      <md-button @click="close" class="md-icon-button position-absolute">
        <md-icon class="text-primary">close</md-icon>
      </md-button>
    </md-dialog-title>
    <div class="md-dialog-content p-0">
      <md-content md-dynamic-height class="px-4 mb-4">
        <div class="md-layout">
          <div class="md-layout-item md-size-100 mb-3">
            <div class="d-flex align-center justify-content-between">
              <div class="md-layout">
                <div class="md-layout-item md-size-25 md-medium-size-20 md-xsmall-size-100">
                  <h3 class="md-title text-left text-primary my-3">Users</h3>
                </div>
                <div
                  class="md-layout-item md-size-50 md-medium-size-60 md-xsmall-size-75 template-tag"
                >
                  <!-- <img
                    :src="'../img/search-icon.svg'"
                    alt
                    style="width: 20px;"
                    class="position-absolute"
                  />-->
                  <div
                    class="d-flex align-center search-tag-class search-tag-inner"
                    v-if="!$route.params.id"
                  >
                    <tags-input
                      class="tag-modal-search"
                      element-id="tags"
                      :placeholder="'Search existing user'"
                      v-model="selectedTags"
                      :existing-tags="existingTagsUser"
                      :typeahead="true"
                      :typeahead-style="typeaheadStyle"
                      :allow-duplicates="allowDuplicates"
                      :typeaheadHideDiscard="typeaheadHideDiscard"
                      @tag-added="onTagAdded"
                      @tag-removed="onTagRemoved"
                      @change="onTagChange"
                    ></tags-input>
                    <div v-if="tagSearchResult" class="tag-not-found tag-not-found-300">
                      <ul class="typeahead-dropdown">
                        <li class="tags-input-typeahead-item-highlighted-default">Tag not found</li>
                      </ul>
                    </div>
                    <!-- <span v-if="tagSearchResult" class="md-error">Tag not found</span> -->
                    <md-button
                      class="md-primary button-custom-regular heading-add-button px-2"
                      @click="save"
                      style="min-width: auto;"
                    >
                      <LoadingButtonLoader
                        :typeEnable="'simpleLoader'"
                        v-bind:enable="app.loading"
                        class="mr-1"
                      />save
                    </md-button>
                  </div>
                </div>
                <div
                  class="md-layout-item md-size-25 md-medium-size-20 md-xsmall-size-25 d-flex justify-content-end align-center"
                >
                  <md-button class="md-primary md-icon-button" @click="openAddUserModel">
                    <md-icon>add</md-icon>
                  </md-button>
                </div>
              </div>
            </div>
          </div>
          <div class="md-layout-item md-small-size-100 md-size-100">
            <md-table class="mb-3">
              <md-table-row class="table-head">
                <md-table-head></md-table-head>
                <md-table-head>NAME</md-table-head>
                <md-table-head>EMAIL ADDRESS</md-table-head>
                <md-table-head>PHONE NUMBER</md-table-head>
                <md-table-head>ROLE</md-table-head>
                <md-table-head>TITLE</md-table-head>
                <md-table-head width="158">ACTION</md-table-head>
              </md-table-row>
              <md-table-row slot="md-table-row" v-for="user in users" v-bind:key="user.id">
                <md-table-cell>
                  <div class="md-avatar">
                    <div
                      v-if="!user.image && user.first_name"
                      class="md-avatar md-intial"
                    >{{user.first_name.substring(0,1).toUpperCase()}} {{user.last_name.substring(0,1).toUpperCase()}}</div>
                    <div v-if="user.image" class="md-avatar">
                      <img v-bind:src="'/images/users/' + user.image" alt="People" />
                    </div>
                    <!-- <img src="https://placeimg.com/40/40/people/1" alt="People" /> -->
                  </div>
                </md-table-cell>
                <md-table-cell>
                  <div>
                    <span class="d-block w-100 mb-1">{{ user.username }}</span>
                  </div>
                </md-table-cell>
                <md-table-cell>
                  <span v-if="user.email">{{ user.email }}</span>
                  <span v-else>No email</span>
                </md-table-cell>
                <md-table-cell>
                  <span v-if="user.phone">+ {{ user.phone }}</span>
                  <span v-else>No phone</span>
                </md-table-cell>
                <md-table-cell>
                  <span class="text-primary" v-if="user.role_name">{{ user.role_name }}</span>
                </md-table-cell>
                <md-table-cell>
                  <span class="text-primary" v-if="user.title">{{ user.title }}</span>
                  <span class="text-primary" v-else>No title</span>
                </md-table-cell>
                <md-table-cell width="158">
                  <md-button
                    class="md-primary button-custom-regular heading-add-button font-12 font-weight-normal x-y-auto px-2"
                    @click="assignSteps(user)"
                  >Assign Steps</md-button>
                </md-table-cell>
              </md-table-row>
            </md-table>
          </div>
        </div>
      </md-content>

      <!-- <md-checkbox v-model="emailSend" >Check this box to send an email to user</md-checkbox>
            <md-dialog-actions class="flex-wrap justify-content-center">
                <md-button type="submit" class="md-primary w-100 button-primary-custom mb-2 mt-3">
                    <LoadingButtonLoader :typeEnable="app.checkType('socialLoader')" v-bind:enable="app.loading" class="mr-1" />Add</md-button>
      </md-dialog-actions>-->
    </div>
  </md-dialog>
</template>

<script>
import { HTTP } from "../../../httpCommon";
import draggable from "vuedraggable";
import LoadingButtonLoader from "../../../components/LoadingButtonLoader";
import VoerroTagsInput from "@voerro/vue-tagsinput";
export default {
  name: "AddTransactionUserModal",
  props: ["active", "app", "transaction_id"],
  components: {
    draggable,
    LoadingButtonLoader,
    "tags-input": VoerroTagsInput
  },
  data: function() {
    return {
      //   firstName: "",
      //   lastName: "",
      //   emailAddress: "",
      //   phone: "",
      selectedTags: [],
      existingTagsUser: [],
      allowDuplicates: false,
      typeaheadStyle: "dropdown",
      typeaheadHideDiscard: true,
      users: "",
      tagSearchResult: false
    };
  },
  created() {
    //   console.log(this.$route.params.id);
    this.init();
    // this.$on('accopen', () => {
    //     console.log("Add new User Modal---- fri 19:46");
    // });

    this.$root.$on("add-user-created", arg => {
      this.init(arg);
    });
  },
  mounted() {
    // this.$root.$on('message-transaction', (arg) => {
    //     console.log(arg);
    // });
  },
  methods: {
    init(param = null) {
      console.log("param" + param);
      this.selectedTags = [];//initiate selected tags
      this.users = ""; //empty users lists
      if (param) {
        HTTP.get("api/users/getUsersForSearchTags")
          .then(response => {
            if (response.data.Status) {
              let users = response.data.Users;
              this.existingTagsUser = users.map(item => {
                return {
                  key: item.id,
                  value:
                    item.first_name +
                    " " +
                    item.last_name +
                    " " +
                    "(" +
                    item.roles.role_name +
                    ")"
                };
              });
            }
          })
          .catch(error => {
            // this.app.loading = false;
          });
        this.getAllTransactionUsers();
      }
    },
    openAddUserModel() {
      console.log("open add user dialog box");
      this.$emit("dialogOpen", "add-user");
    },
    getAllTransactionUsers() {
      HTTP.get("api/transactions/getAllTransactionUsers", {
        params: {
          transaction_id: this.transaction_id
          // user_role_id: this.$route.params.id ? 5 : null // removed ny shalu(1apr2020) as it was creating issue not able to assign step to agent
        }
      })
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            this.users = response.data.Users;
          }
        })
        .catch(error => {
          // this.app.loading = false;
        });
    },
    close() {
      console.log("add transaction user");
      this.$emit("dialogClose", "add-transaction-user");
    },
    assignSteps(user) {
      console.log("Clicked ");
      console.log(user);
      if (user) {
        this.$root.$emit("selected-user", user);
        this.$emit("dialogOpen", "add-steps");
      }
    },
    onTagAdded() {},
    save() {
      // let selectedTags = this.selectedTags;
      const data = {
        transaction_id: this.transaction_id,
        selected_users: this.selectedTags
      };

      this.app.loading = true;
      HTTP.post("api/transactions/assignUser", data)
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            this.getAllTransactionUsers();
            // this.transactionAutoIncrementId = response.data.ResultId;
            // this.openCompleteStep = true; //Address Dialog for subscription user
          }
        })
        .catch(error => {
          this.app.loading = false;
        });
    },
    onTagChange(search) {
      if (this.existingTagsUser.length) {
        let existingTags = this.existingTagsUser
          .map(item => {
            var re = new RegExp(search, "gi");
            return item.value.match(re);
          })
          .filter(function(el) {
            return el != null;
          });
        console.log(existingTags);
        //  console.log(existingTags.includes(null));
        if (!existingTags.length) {
          this.tagSearchResult = true;
        } else {
          this.tagSearchResult = false;
        }
      } else {
        this.tagSearchResult = false;
      }
    },
    onTagRemoved(removed) {
      console.log(`removed event triggereered` + removed);
      this.tagSearchResult = false;
    }
  },
  computed: {},
  watch: {}
};
</script>

<style scoped>
</style>
