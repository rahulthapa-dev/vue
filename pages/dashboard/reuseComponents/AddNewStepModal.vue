<template>
  <md-dialog
    class="modal-medium"
    :md-active.sync="active"
    :md-click-outside-to-close="false"
    :md-close-on-esc="false"
  >
    <md-dialog-title class="modal-title d-block py-3 px-4 bg-light">
      <h4 class="my-0 md-headline text-left text-primary" v-if="editId">Edit Step</h4>
      <h4 class="my-0 md-headline text-left text-primary" v-else>Add Step</h4>
      <md-button @click="close" class="md-icon-button position-absolute">
        <md-icon class="text-primary">close</md-icon>
      </md-button>
    </md-dialog-title>
    <div class="md-dialog-content">
      <!-- <form> -->
      <md-content md-dynamic-height class="px-0">
        <div class="w-100">
          <md-radio v-model="step_type" :value="1">
            <span class="text-primary description">New</span>
          </md-radio>
          <md-radio v-model="step_type" :value="2">
            <span class="text-primary description">Existing</span>
          </md-radio>
        </div>
        <div class="md-layout">
          <div class="md-layout-item md-medium-size-100 md-size-100 md-small-size-100">
            <form @submit.prevent="save" v-show="step_type === 1" class="mt-1">
              <md-content md-dynamic-height class="px-4">
                <div class="md-layout">
                  <!-- STEP  -->
                  <div class="md-layout-item md-small-size-100 md-size-100">
                    <md-field slot="inputs" :class="app.getValidationClass($v,'step_title')">
                      <label>Step Title</label>

                      <md-input v-model="step_title"></md-input>
                      <span v-if="!$v.step_title.required" class="md-error">Title is required</span>
                    </md-field>
                  </div>
                  <!-- URL -->
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs" :class="app.getValidationClass($v,'url')">
                      <label>URL (Vimeo, Youtube, ...)</label>
                      <md-input v-model="url"></md-input>
                      <span v-if="!$v.url.urlValid" class="md-error">Url invalid</span>
                    </md-field>
                  </div>

                  <!-- EST DUE DATE -->
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      :class="app.getValidationClass($v,'estimated_due_date')"
                      class="multi-error datepicker-md-field"
                    >
                      <md-datepicker v-model="estimated_due_date" class="date-picker-custom">
                        <label>Est Due Date</label>
                      </md-datepicker>
                      <span
                        v-if="!$v.estimated_due_date.required"
                        class="md-error"
                      >Est Due Date is required</span>
                    </md-field>
                  </div>
                  <!-- DESCRIPTION -->
                  <div class="md-layout-item md-small-size-100 md-size-100">
                    <md-field slot="inputs" :class="app.getValidationClass($v,'description')">
                      <label>Description</label>
                      <md-textarea
                        class="px-0"
                        v-model="description"
                        value="At vero eos et accusamus et iusto odio dignissimos ducimus qui blanditiis praesentium voluptatum deleniti atque corrupti quos dolores et quas molestias."
                      ></md-textarea>
                      <span v-if="!$v.description.required" class="md-error">Description is required</span>
                      <span v-if="!$v.description.maxLength" class="md-error">maxLength is 300</span>
                    </md-field>
                  </div>

                  <div class="md-layout-item md-small-size-100 md-size-100">
                    <md-checkbox v-model="access">
                      <span class="text-primary">Make it available for other user</span>
                    </md-checkbox>
                  </div>
                </div>
              </md-content>
              <md-dialog-actions class="flex-wrap justify-content-center">
                <div class="d-block w-100 text-center">
                  <md-button
                    type="submit"
                    class="md-primary w-100 button-primary-custom mb-2 mt-3"
                    v-if="editId"
                  >
                    <LoadingButtonLoader
                      :typeEnable="'simpleLoader'"
                      v-bind:enable="app.loading"
                      class="mr-1"
                    />Update
                  </md-button>

                  <md-button
                    type="submit"
                    class="md-primary w-100 button-primary-custom mb-2 mt-3"
                    v-else
                  >
                    <LoadingButtonLoader
                      :typeEnable="'simpleLoader2'"
                      v-bind:enable="app.loading"
                      class="mr-1"
                    />Add
                  </md-button>

                  <br />
                  <md-button
                    :md-ripple="false"
                    @click="close"
                    class="cancel-link lighter-description mt-2 text-capitalize mb-2"
                  >Cancel</md-button>
                </div>
              </md-dialog-actions>
            </form>
            <form v-show="step_type === 2" class="mt-1">
              <h3 class="md-title text-left text-primary my-3">Add existing step</h3>
              <div class="search-tag-class position-relative">
                <tags-input
                  element-id="search_step"
                  :placeholder="'Select step'"
                  v-model="selectedStepTags"
                  :existing-tags="existingStepTags"
                  :typeahead="true"
                  :typeahead-style="typeaheadStyle"
                  :allow-duplicates="allowDuplicates"
                  :typeaheadHideDiscard="typeaheadHideDiscard"
                  @change="onTagChange"
                ></tags-input>
                <div v-if="stepTagSearchResult" class="tag-not-found">
                  <ul class="typeahead-dropdown">
                    <li class="tags-input-typeahead-item-highlighted-default">Tag not found</li>
                  </ul>
                </div>
              </div>
              <!-- <span v-if="stepTagSearchResult" class="md-error">Tag not found</span> -->

              <div class="d-flex justify-content-start">
                <md-button
                  type="button"
                  @click.prevent="addExistingStep($event)"
                  class="md-primary button-primary-custom mb-2 mt-3 mx-0"
                >
                  <LoadingButtonLoader
                    :typeEnable="'simpleLoader'"
                    v-bind:enable="app.loading"
                    class="mr-1"
                  />Add existing step
                </md-button>
              </div>
            </form>
          </div>
        </div>
      </md-content>
      <!-- </form> -->
    </div>
  </md-dialog>
</template>

<script>
import { HTTP } from "../../../httpCommon";
import {
  required,
  email,
  minLength,
  maxLength,
  sameAs
} from "vuelidate/lib/validators";
// import VoerroTagsInput from "@voerro/vue-tagsinput";
import LoadingButtonLoader from "../../../components/LoadingButtonLoader";
import VoerroTagsInput from "@voerro/vue-tagsinput";
const isUrl = value =>
  /^(|https?:\/\/[\w\-_]+(\.[\w\-_]+)+([\w\-\.,@?^=%&amp;:/~\+#]*[\w\-\@?^=%&amp;/~\+#])?)$/.test(
    value
  );
export default {
  name: "AddNewStepModal",
  props: ["active", "app", "transaction_id"],
  components: {
    LoadingButtonLoader,
    "tags-input": VoerroTagsInput
  },
  // props: ['steps'],
  data: function() {
    return {
      step_title: "",
      url: "",
      description: "",
      access: false,
      editId: "",
      estimated_due_date: "",
      step_type: 1,
      /**step tags***/
      allowDuplicates: false,
      typeaheadStyle: "dropdown",
      typeaheadHideDiscard: true,
      selectedStepTags: [],
      existingStepTags: [
        {
          key: 1,
          value: "step1"
        },
        {
          key: 2,
          value: "step2"
        },
        {
          key: 3,
          value: "step3"
        }
      ],
      stepTagSearchResult: false
    };
  },
  validations: {
    step_title: {
      required
    },
    url: {
      urlValid: isUrl
    },
    description: {
      required,
      maxLength: maxLength(300)
    },
    estimated_due_date: {
      required
    }
  },
  created() {
    this.init();
  },
  methods: {
    init(param = null) {
      this.existingStepTags = []; //existingStepTags to be empty
      this.selectedStepTags = [];
      if (param) {
        this.step_title = "";
        this.url = "";
        this.description = "";
        this.access = false;
        this.estimated_due_date = "";
        this.$v.$reset(); //do not remove this
      }
      this.getAllSteps();
    },
    getAllSteps() {
      HTTP.get("api/steps/getAllSteps")
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            this.existingStepTags = response.data.Steps;

            // this.existingUserTags = users.map((item) => {
            //     return {
            //         key: item.id,
            //         value: item.username + " " + "(" + item.role_name + ")"
            //     }
            // });
          }
        })
        .catch(error => {
          // this.app.loading = false;
        });
    },
    close() {
      console.log("close step model");
      this.$emit("dialogClose", "add-steps");
    },
    getStep() {
      HTTP.get("api/transactions/getStep", {
        params: {
          step_id: this.editId,
          transaction_id: this.transaction_id
        }
      })
        .then(response => {
          console.log(response);
          if (response.data.Status) {
            console.log(response.data);
            this.step_title = response.data.Step.steps_title;
            this.url = response.data.Step.video;
            this.description = response.data.Step.description;
            this.estimated_due_date = response.data.Step.due_date;
            // this.estimated_due_date = response.data.Step.due_date;
            //2 : Make it available for other user
            //1 : Not available
            this.access = response.data.Step.steps_type == 2 ? true : false;
            console.log("This access");
            console.log(response.data.Step.due_date);
            console.log(this.estimated_due_date);
          }
        })
        .catch(error => {});
    },
    editStep(event) {
      this.step_title = "";
      this.url = "";
      this.description = "";
      this.access = false;
      if (event.key == "add-step") {
        this.step_title = "";
        this.url = "";
        this.description = "";
        this.access = false;
      } else if (event.key == "edit-step") {
        this.editId = event.param;
        this.getStep(this.editId);
      }
      // Make edit step field empty if event is "add step"
    },
    addExistingStep(evt) {
      evt.preventDefault();
      const data = {
        steps: this.selectedStepTags,
        paramType: "existing",
        transaction_id: this.transaction_id
      };
      this.app.loading = true;
      HTTP.post("api/transactions/addTransactionSteps", data)
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            this.$emit("dialogClose", "add-new-step");
            this.$root.$emit("steps-created", "success");
            this.$toasted
              .show(response.data.Message, {
                type: "success"
                // icon: "error_outline"
              })
              .goAway(1500);
            this.getTransactionSteps();
          }
        })
        .catch(error => {
          this.app.loading = false;
        });
    },
    onTagChange(search) {
      if (this.existingStepTags.length) {
        let existingTags = this.existingStepTags
          .map(item => {
            var re = new RegExp(search, "gi");
            return item.value.match(re);
          })
          .filter(function(el) {
            return el != null;
          });
        //  console.log(existingTags);
        //  console.log(existingTags.includes(null));
        if (!existingTags.length) {
          this.stepTagSearchResult = true;
        } else {
          this.stepTagSearchResult = false;
        }
      } else {
        this.stepTagSearchResult = false;
      }
    },
    save() {
      //  this.$v.$reset();
      //     this.$v.step_title.$touch();
      //     this.$v.url.$touch();
      //     this.$v.description.$touch();
      this.$v.$touch(); //do not remove this
      if (this.$v.$error) return;
      let step_type = 1;
      if (this.access) {
        step_type = 2; //Make it available for other user  for 2
      }
      const data = {
        paramType: "new",
        transaction_id: this.transaction_id,
        step_title: this.step_title,
        steps_type: step_type,
        url: this.url,
        description: this.description,
        estimated_due_date: this.estimated_due_date
      };
      this.app.loading = true;
      //EDIT STEP
      if (this.editId) {
        data.step_id = this.editId;
        HTTP.put("api/steps/updateSteps", data)
          .then(response => {
            if (response.data.Status) {
              this.app.loading = false;
              this.$emit("dialogClose", "add-new-step");
              this.$root.$emit("steps-created", "success");
              this.$toasted
                .show(response.data.Message, {
                  type: "success"
                  // icon: "error_outline"
                })
                .goAway(1500);
              // this.getransactionSteps();
            }
          })
          .catch(error => {
            this.app.loading = false;
          });
      } else {
        //ADD STEP
        HTTP.post("api/steps/addSteps", data)
          .then(response => {
            if (response.data.Status) {
              this.app.loading = false;
              this.$emit("dialogClose", "add-new-step");
              this.$root.$emit("steps-created", "success");
              this.$toasted
                .show(response.data.Message, {
                  type: "success"
                  // icon: "error_outline"
                })
                .goAway(1500);
              // this.getransactionSteps();
            }
          })
          .catch(error => {
            this.app.loading = false;
          });
      }
    }
  },
  computed: {},
  watch: {}
  // beforeUpdate: function() {
  //    console.log('All those data changes happened '
  //                  + 'before the output gets updated.');
  // }
};
</script>

<style scoped>
</style>
