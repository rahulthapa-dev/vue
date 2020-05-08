<template>
  <div>
    <!-- DELETE MODEL -->
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
                  <span class="brand-primary">step</span> from the project.
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
    <!--- EDIT VENDOR MODEL -->
    <md-dialog :md-active.sync="active" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Edit transaction step</h4>
        <md-button @click="active = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <form @submit.prevent="updateVendor">
          <md-content md-dynamic-height class="px-4">
            <div class="md-layout">
              <div class="md-layout-item md-small-size-100 md-size-100">
                <md-field slot="inputs">
                  <label>Step</label>
                  <md-input></md-input>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>Title</label>
                  <md-input v-model="stepForm.title"></md-input>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>Est. due date</label>
                  <md-input></md-input>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>Select a video integration</label>
                  <md-select name="role" id="role">
                    <md-option value="admin">Admin</md-option>
                    <md-option value="agent">Agent</md-option>
                    <md-option value="junior-agent">Junior Agent</md-option>
                    <md-option value="senior-agent">Senior Agent</md-option>
                  </md-select>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-50">
                <md-field slot="inputs">
                  <label>URL (Vimeo, Youtube, ...)</label>
                  <md-input></md-input>
                </md-field>
              </div>
              <div class="md-layout-item md-small-size-100 md-size-100">
                <md-field slot="inputs">
                  <label>Description</label>
                  <md-textarea
                    class="pl-0"
                    value="At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias."
                  ></md-textarea>
                </md-field>
              </div>
            </div>
          </md-content>
        </form>
        <md-dialog-actions class="flex-wrap justify-content-center">
          <md-button
            class="md-primary w-100 button-primary-custom mb-1 mt-3"
            @click="active = false"
          >Add</md-button>
          <div class="d-block w-100 text-center">
            <md-button
              :md-ripple="false"
              @click="active = false"
              class="cancel-link lighter-description mt-2 mb-3 text-capitalize"
            >Cancel</md-button>
          </div>
        </md-dialog-actions>
      </div>
    </md-dialog>

    <ul class="transaction-step-timeline">
      <!-- WHEN NO AGENTS ADDED YET-->

      <li v-if="!steps.length > 0" class="py-2 px-4 bg-light">
        <!-- no data start -->
        <span class="text-primary description py-2">No record exist</span>
        <!-- no data end -->
      </li>
      <li
        v-bind:class="{'active':(active_el === index)}"
        v-for="(step, index) in steps"
        :key="step.id"
      >
        <div class="step-icon">
          <span>{{index +1}}</span>
        </div>
        <article class="message transaction-step w-100" :class="accordionClasses">
          <div class="message-header px-4 py-3" @click="toggleAccordion(index)">
            <div class="d-flex justify-content-between align-center">
              <h4 class="my-1">{{step.steps_title}}</h4>
              <div>
                <p class="my-1">
                  <span class="text-primary description">Est. Due Date:</span>
                  <span v-if="step.due_date" class="description">{{step.due_date | formatDate}}</span>
                  <span v-else class="description">N/A</span>
                </p>
              </div>
            </div>
          </div>
          <div class="message-body custom-shadow">
            <div class="message-content">
              <div class="md-layout">
                <div class="md-layout-item md-size-60">
                  <div class="md-layout">
                    <div class="md-layout-item md-size-33">
                      <label for>Title</label>
                      <p class="text-primary">{{step.steps_title}}</p>
                    </div>
                    <div class="md-layout-item md-size-33">
                      <label for>Agent</label>
                      <p class="text-primary">
                        {{step.recent_assign_agent}}
                        <span
                          class="brand-primary"
                          v-if="step.total_agents >1"
                        >+{{step.total_agents -1 }}</span>
                      </p>
                    </div>
                    <div class="md-layout-item md-size-33">
                      <label for>Transaction user</label>
                      <p class="text-primary">
                        {{step.recent_assign_tuser}}
                        <span
                          class="brand-primary"
                          v-if="step.total_transaction_users >1"
                        >+{{step.total_transaction_users -1 }}</span>
                      </p>
                    </div>
                    <div class="md-layout-item md-size-33">
                      <label for>video</label>
                      <p class="brand-primary">{{step.video}}</p>
                    </div>
                  </div>
                </div>
                <div class="md-layout-item md-size-40">
                  <div class="md-layout-item md-size-100">
                    <label for>Description</label>
                    <p class="text-primary">{{step.description}}</p>
                  </div>
                  <div
                    class="md-layout-item md-size-100 d-flex align-center justify-content-between"
                  >
                    <div
                      class="d-flex justify-content-between align-center"
                      @click="enableConfirmation = true"
                    >
                      <md-checkbox class>Enable the option for confirmation</md-checkbox>
                    </div>
                    <!-- EDIT /DELETE -->
                    <div class="transaction-steps-option">
                      <span class="cursor-pointer" @click="edit(step.id)">
                        <md-icon>edit</md-icon>
                      </span>
                      <span class="cursor-pointer ml-2" @click="del(step)">
                        <img :src="'../img/trashcan.svg'" alt />
                      </span>
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </article>
      </li>
    </ul>
  </div>
</template>

<script>
import { HTTP } from "../../httpCommon";
import LoadingButtonLoader from "../../components/LoadingButtonLoader";
export default {
  name: "TransactionSteps",
  props: ["app", "transaction_id"],
  components: { LoadingButtonLoader },
  data: function() {
    return {
      stepForm: {
        title: "",
        est_due_date: ""
      },
      isOpen: true,
      steps: "",
      active: false,
      dialogActive: false,
      active_el: 0,
      stepDeleteId: ""
    };
  },
  created: function() {},
  watch: {},
  mounted() {
    this.getSteps(); //get steps realated to transaction
  },
  methods: {
    toggleAccordion: function(el) {
      if (this.active_el == el) {
        this.active_el = "-1";
      } else {
        this.active_el = el;
      }
    },
    deleteStep(step_id) {
      this.app.loading = true;
      HTTP.delete("api/transactions/deleteStep", {
        params: {
          transaction_id: this.transaction_id,
          step_id: step_id
        }
      })
        .then(response => {
          console.log(response);
          if (response.data.Status) {
            this.app.loading = false;
            this.dialogActive = false;
            this.$toasted
              .show(response.data.Message, {
                type: "success"
              })
              .goAway(1500);
            this.getSteps();
          }
        })
        .catch(error => {});
    },
    getSteps() {
      HTTP.get("api/transactions/getSteps", {
        params: {
          transaction_id: this.$route.params.id
        }
      })
        .then(response => {
          if (response.data.Status) {
            console.log("get setps here");
            this.steps = response.data.Steps;
          }
        })
        .catch(error => {});
    },
    edit(id) {
      console.log("id" + id);
      this.$emit("dialogOpen", { key: "edit-step", param: id });
    },
    del(data) {
      this.dialogActive = true;
      this.stepDeleteId = data.id;
    },
    confirmDelete() {
      console.log("Confirm delete" + this.stepDeleteId);
      this.deleteStep(this.stepDeleteId);
    }
  },
  computed: {
    accordionClasses: function() {
      return {
        "is-closed": !this.isOpen,
        "is-primary": this.isOpen,
        "is-dark": !this.isOpen
      };
    }
  }
};

//transactions/getSteps
</script>

