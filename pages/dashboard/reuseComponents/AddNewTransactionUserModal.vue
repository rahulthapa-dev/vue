<template>
  <!-- THIS MODEL NOT IN USE -->
  <md-dialog :md-active.sync="active" class="modal-medium" :md-click-outside-to-close="false">
    <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
      <h4 class="my-0 md-headline text-left text-primary">Add Transaction User</h4>
      <md-button @click="close" class="md-icon-button position-absolute">
        <md-icon class="text-primary">close</md-icon>
      </md-button>
    </md-dialog-title>
    <div class="md-dialog-content p-0">
      <button @click="toggleOpen">Communicate</button>
      <form @submit.prevent="save">
        <md-content md-dynamic-height class="px-4 mb-4">
          <div class="md-layout">
            <div class="md-layout-item md-small-size-100 md-size-100">
              <md-field>
                <label>First name</label>
                <md-input v-model="firstName"></md-input>
                <!-- <span v-if="!$v.template_name.required" class="md-error">Template name is required</span> -->
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-100">
              <md-field>
                <label>Last name</label>

                <md-input v-model="lastName"></md-input>
                <!-- <span v-if="!$v.template_name.required" class="md-error">Template name is required</span> -->
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100 md-size-100">
              <md-field>
                <label>Email Address</label>

                <md-input v-model="emailAddress"></md-input>
                <!-- <span v-if="!$v.template_name.required" class="md-error">Template name is required</span> -->
              </md-field>
            </div>
          </div>
          <div class="md-layout">
            <!-- <span>Checked items: {{ checkedSteps }}</span> -->
            <!-- <div class="md-layout-item md-size-45 md-medium-size-45 md-small-size-100">
                        <h4 class="md-title text-left text-primary">Available steps</h4>
                        <md-list class="list-add-template">
                            <md-list-item class="mb-2" v-for="item in checkedSteps" :key="item.id">
                                <input type="checkbox" :id="item.id" :value="item.id" v-model="item.checked" />
                                <label :for="item.id">
                                    <div class="position-relative flex-wrap justify-content-start d-flex align-center">
                                        <div class="md-avatar position-absolute">AB</div>
                                        <p class="md-title text-primary">{{ item.steps_title}}</p>
                                    </div>
                                    <i class="fas fa-angle-double-right description pr-3"></i>
                                </label>
                            </md-list-item>
                        </md-list>
                    </div>
                    <div class="d-flex align-center justify-content-center mx-auto outer-swap-wrapper">
                        <div class="outer-swap">
                            <div class="inner-swap" @click="dragSelection">
                                <i class="fas fa-angle-double-left text-primary"></i>
                                <i class="fas fa-angle-double-right text-primary"></i>
                            </div>
                        </div>
                    </div>
                    <div class="md-layout-item md-size-45 md-medium-size-45 md-small-size-100">
                        <h4 class="md-title text-left text-primary">Assigned steps</h4>

                        <md-list class="list-add-template assigned-list">
                            <draggable v-model="selectedSteps" group="people" @start="onStart" @end="onEnd">
                                <md-list-item class="mb-2" :data-id="index" v-for="(item,index) in selectedSteps" :key="item.id">
                                    <input type="checkbox" :id="item.id" :value="item.id" v-model="item.checked" />
                                    <label :for="item.id">
                                        <div class="position-relative flex-wrap justify-content-start d-flex align-center">
                                            <div class="md-avatar position-absolute">AB</div>
                                            <p class="md-title text-primary">{{ item.steps_title}}</p>
                                        </div>
                                        <i class="fas fa-angle-double-left description pr-3"></i>
                                    </label>
                                </md-list-item>
                            </draggable>
                        </md-list>
            </div>-->
            <!-- <span>Selectd steps: {{ selectedSteps }}</span> -->
          </div>
        </md-content>

        <md-checkbox v-model="emailSend">Check this box to send an email to user</md-checkbox>
        <md-dialog-actions class="flex-wrap justify-content-center">
          <md-button type="submit" class="md-primary w-100 button-primary-custom mb-2 mt-3">
            <LoadingButtonLoader
              :typeEnable="app.checkType('socialLoader')"
              v-bind:enable="app.loading"
              class="mr-1"
            />Add
          </md-button>
          <!-- <div class="d-block w-100 text-center">
                        <md-button :md-ripple="false" @click="addTemplate = false" class="cancel-link lighter-description mt-2 mb-2 text-capitalize">Cancel</md-button>
          </div>-->
        </md-dialog-actions>
      </form>
    </div>
  </md-dialog>
</template>


<script>
import { HTTP } from "../../../httpCommon";
import LoadingButtonLoader from "../../../components/LoadingButtonLoader";
export default {
  name: "AddNewTransactionUserModal",
  props: ["active", "app", "transaction_id"],
  components: {
    LoadingButtonLoader
  },
  data: function() {
    return {
      firstName: "",
      lastName: "",
      emailAddress: "",
      phone: "",
      accessCode: "",
      emailSend: ""
    };
  },
  created() {
    this.init();
  },
  methods: {
    init() {
      // HTTP.get("api/templates/getTemplates")
      //     .then(response => {
      //         if (response.data.Status) {
      //             // this.app.loading = false;
      //             // this.transactions = response.data.Transactions;
      //             this.templates = response.data.ActiveTemplates;
      //             let templates = this.templates;
      //             // debugger;
      //         }
      //     })
      //     .catch(error => {
      //         // this.app.loading = false;
      //     });
    },
    toggleOpen: function() {
      this.$root.$emit("message-transaction", "test refersh component-------");
      // this.$root.$on('accopen',(arg)=>{
      //     console.log(arg);
      // });
      // this.isOpen = !this.isOpen;
    },
    close() {
      this.$emit("dialogClose", "transaction-steps");
    },
    dragSelection() {},
    optionChange(event) {
      if (event) {
        this.$emit("dialogOpen", "add-steps");
      }
    },
    save() {
      // let singleTemplate = [];
      // for (let i = 1; i < this.singleTemplate.length; i++) {
      //     singleTemplate[i] = {
      //         "step_id": this.singleTemplate[i]['step_id'],
      //         "transaction_id": "2",
      //         "listOrder": i
      //     };
      // }
      // let data = {
      //     paramType:'template',
      //     steps: singleTemplate
      // }
      // this.app.loading = true;
      // HTTP.post("api/transactions/addTransactionSteps", data)
      //     .then(response => {
      //         if (response.data.Status) {
      //             this.app.loading = false;
      //         }
      //     })
      //     .catch(error => {
      //         this.app.loading = false;
      //     });
    }
  },
  computed: {},
  watch: {}
};
</script>

<style scoped>
</style>
