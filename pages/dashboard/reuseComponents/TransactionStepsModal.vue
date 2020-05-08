<template>
  <md-dialog
    :md-active.sync="active"
    class="modal-medium"
    :md-click-outside-to-close="false"
    :md-close-on-esc="false"
  >
    <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
      <h4 class="my-0 md-headline text-left text-primary">Add steps to transaction</h4>
      <md-button @click="close" class="md-icon-button position-absolute">
        <md-icon class="text-primary">close</md-icon>
      </md-button>
    </md-dialog-title>
    <div class="md-dialog-content p-0">
      <form>
        <md-content md-dynamic-height class="px-4 mb-4" id="transaction-steps-modal">
          <div class="md-layout">
            <div class="md-layout-item md-small-size-100 md-size-100 pb-3">
              <div
                slot="inputs"
                :class="app.getValidationClass($v,'selectedUserTags')"
                class="mb-3 search-tag-class position-relative"
              >
                <h3 class="md-title text-left text-primary my-3">Choose transaction users</h3>
                <tags-input
                  element-id="search_user"
                  :placeholder="'Search existing user'"
                  v-model="selectedUserTags"
                  :existing-tags="existingUserTags"
                  :typeahead="true"
                  :typeahead-style="typeaheadStyle"
                  :allow-duplicates="allowDuplicates"
                  :typeaheadHideDiscard="typeaheadHideDiscard"
                  @change="onUserTagChange"
                ></tags-input>
                <div v-if="!$v.selectedUserTags.required" class="tag-not-found">
                  <ul class="typeahead-dropdown">
                    <li class="tags-input-typeahead-item-highlighted-default">User required</li>
                  </ul>
                </div>
                <div v-if="userTagSearchResult" class="tag-not-found">
                  <ul class="typeahead-dropdown">
                    <li class="tags-input-typeahead-item-highlighted-default">Tag not found</li>
                  </ul>
                </div>
                <!-- <span v-if="!$v.selectedUserTags.required" class="md-error">User required</span> -->
                <!-- <span v-if="userTagSearchResult" class="md-error">Tag not found</span> -->
              </div>
              <!-- <div class="search-tag-class">
                <h3 class="md-title text-left text-primary my-3">Add existing step</h3>
                <tags-input
                  element-id="search_step"
                  :placeholder="'Select step'"
                  v-model="selectedStepTags"
                  :existing-tags="existingStepTags"
                  :typeahead="true"
                  :typeahead-style="typeaheadStyle"
                  :allow-duplicates="allowDuplicates"
                  :typeaheadHideDiscard="typeaheadHideDiscard"
                  @change="onStepTagChange"
                ></tags-input>
                <span v-if="stepTagSearchResult" class="md-error">Tag not found</span>
              </div>

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
              </div>-->

              <!-- <div class="mb-3">
                <tags-input
                  element-id="search_template"
                  :placeholder="'Template name'"
                  v-model="selectedTemplateTags"
                  :existing-tags="existingTemplateTags"
                  :typeahead="true"
                  :typeahead-style="typeaheadStyle"
                  :allow-duplicates="allowDuplicates"
                  :typeaheadHideDiscard="typeaheadHideDiscard"
                ></tags-input>
              </div>-->

              <!-- <md-button type="button" @click.prevent="addTemplate($event)" class="md-primary w-100 button-primary-custom mb-2 mt-3">
              <LoadingButtonLoader :typeEnable="app.checkType('simpleLoader')" v-bind:enable="app.loading" class="mr-1" />Add template</md-button>-->
            </div>
          </div>
          <div class="md-layout">
            <div class="md-layout-item md-size-100 d-flex justify-content-between">
              <hr class="hr-custom" />
            </div>
          </div>
          <div class="md-layout">
            <div class="md-layout-item md-size-100 d-flex justify-content-between align-center">
              <h3 class="md-title text-left text-primary my-3">Add new step</h3>
              <md-button class="md-primary md-icon-button" @click="openStepModal">
                <md-icon>add</md-icon>
              </md-button>
            </div>
          </div>
          <div class="md-layout">
            <!-- <span>Checked items: {{ checkedSteps }}</span> -->
            <div class="md-layout-item md-size-45 md-medium-size-45 md-small-size-100">
              <h4 class="md-title text-left description">Available steps</h4>
              <md-list class="list-add-template">
                <draggable
                  v-model="transactionSteps"
                  group="people"
                  @start="onStart"
                  @end="onEnd"
                  @change="checkMove"
                >
                  <md-list-item
                    class="mb-2 add-step-list-item"
                    v-for="item in transactionSteps"
                    :key="item.id"
                  >
                    <input
                      type="checkbox"
                      :id="item.id"
                      :value="item.id"
                      v-model="item.checked"
                      @change="checkedItem(item)"
                    />
                    <label :for="item.id" class="flex">
                      <div
                        class="position-relative flex-wrap justify-content-start d-flex align-center"
                      >
                        <!-- <div class="md-avatar position-absolute">AB</div> -->
                        <!--Match word length-->
                        <p
                          class="md-title text-primary p-l-remove"
                          v-if="item.steps_title.length > 12"
                        >{{ item.steps_title.substring(0, 12) }}{{'...'}}</p>
                        <p class="md-title text-primary p-l-remove" v-else>{{ item.steps_title }}</p>
                      </div>
                      <i class="fas fa-angle-double-right description pr-3"></i>
                    </label>
                    <i
                      class="fa fa-check"
                      aria-hidden="true"
                      v-if="item.choose"
                      style="color:#70E7F1"
                    ></i>
                  </md-list-item>
                </draggable>
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
              <h4 class="md-title text-left description">Assigned steps</h4>
              <md-list class="list-add-template assigned-list assigned-list-steps">
                <md-list-item
                  class="mb-2"
                  :data-id="index"
                  v-for="(item,index) in userSteps"
                  :key="item.id"
                  @click="assignedSteps(item)"
                >
                  <!-- <input type="checkbox" :id="item.id" :value="item.id" v-model="item.checked"  /> -->
                  <div class="pr-4 justify-content-between align-center label-div">
                    <div
                      class="position-relative flex-wrap justify-content-start d-flex align-center"
                    >
                      <!-- <div class="md-avatar position-absolute">AB</div> -->
                      <p
                        class="md-title text-primary p-l-remove"
                        v-if="item.steps_title.length > 12"
                      >{{ item.steps_title.substring(0, 12) }}{{'...'}}</p>
                      <p class="md-title text-primary p-l-remove" v-else>{{ item.steps_title }}</p>
                    </div>
                    <!-- <i class="fas fa-angle-double-left description pr-3"></i> -->
                  </div>
                  <a @click="removeStep(item.id)">
                    <md-icon
                      class="cursor-pointer closed-label mr-3 closed-label assign-close-label close-label-circle description d-flex align-center justify-content-center position-absolute"
                    >close</md-icon>
                  </a>
                </md-list-item>
              </md-list>
            </div>
            <!-- <span>Selectd steps: {{ selectedSteps }}</span> -->
          </div>
        </md-content>
        <!-- <md-checkbox v-model="transactionUser.emailSend" class>Check this box to send an email to user</md-checkbox> -->
        <md-dialog-actions class="flex-wrap justify-content-center">
          <div class="w-100 d-flex align-center justify-content-between">
            <md-button
              :md-ripple="false"
              @click="close"
              class="cancel-link lighter-description my-2 text-capitalize"
            >
              <md-icon style="color:#C2C2E4 !important;">close</md-icon>Cancel
            </md-button>
            <div>
              <md-button
                type="button"
                @click.prevent="save($event,1)"
                class="md-primary button-primary-custom mb-2 mt-3"
              >
                <LoadingButtonLoader
                  :typeEnable="'simpleLoader2'"
                  v-bind:enable="app.loading2"
                  class="mr-1"
                />Assign & Complete
              </md-button>

              <md-button
                type="button"
                @click.prevent="save($event,2)"
                class="md-primary button-primary-custom mb-2 mt-3"
              >
                <LoadingButtonLoader
                  :typeEnable="'simpleLoader3'"
                  v-bind:enable="app.loading3"
                  class="mr-1"
                />Assign & Continue
              </md-button>
            </div>
          </div>

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
import draggable from "vuedraggable";
import {
  required,
  email,
  minLength,
  maxLength,
  sameAs
} from "vuelidate/lib/validators";
import LoadingButtonLoader from "../../../components/LoadingButtonLoader";
import VoerroTagsInput from "@voerro/vue-tagsinput";

export default {
  name: "TransactionStepsModal",
  props: ["active", "app", "transaction_id", "templates"],
  components: {
    draggable,
    LoadingButtonLoader,
    "tags-input": VoerroTagsInput
  },
  // props: ['steps'],
  data: function() {
    return {
      steps2: [
        {
          id: "1",
          steps_title: "Hunger",
          description: "This is a sample description",
          video: "https://vimeo.com/123",
          created_at: "Aug 30, 2019"
        },
        {
          id: "2",
          steps_title: "Moee",
          description: "This is a sample description",
          video: "https://vimeo.com/123",
          created_at: "Aug 30, 2019"
        }
      ],
      option_steps: "",
      template_model: "",
      singleTemplate: "",
      requestTemplate: "",
      user: "",
      selectedUserTags: [],
      existingUserTags: [],
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
      selectedTemplateTags: [],
      // existingTemplateTags: [{
      //         key: 1,
      //         value: "temp1"
      //     },
      //     {
      //         key: 2,
      //         value: "temp2"
      //     },
      //     {
      //         key: 3,
      //         value: "temp3"
      //     }
      // ],
      transactionSteps: [],
      userSteps: [],
      userTagSearchResult: false,
      stepTagSearchResult: false
    };
  },
  created() {
    this.$root.$on("selected-user", arg => {
      console.log("selected user");
      // console.log(arg);
      this.user = arg;
      // this.setSelectedTags();
    });

    this.$root.$on("add-step", arg => {
      console.log("add -step");
      // console.log(arg);
      //this.user = arg;
      //this.getTransactionSteps(this.user.user_id);
    });

    this.$root.$on("steps-created", arg => {
      this.getTransactionSteps();
    });
    this.init();
  },
  mounted() {},
  validations: {
    selectedUserTags: {
      required
    }
  },
  methods: {
    init(param) {
      this.existingStepTags = []; //empty
      this.transactionSteps = [];
      this.userSteps = [];
      this.selectedStepTags = [];
      this.app.loading = false;
      if (param) {
        HTTP.get("api/transactions/getAllTransactionUsers", {
          params: {
            transaction_id: this.transaction_id
          }
        })
          .then(response => {
            if (response.data.Status) {
              let users = response.data.Users;

              this.existingUserTags = users.map(item => {
                return {
                  key: item.id,
                  value: item.username + " " + "(" + item.role_name + ")"
                };
              });
            }
          })
          .catch(error => {
            // this.app.loading = false;
          });

        this.setSelectedTags();
        this.getTransactionSteps();
        this.getAllSteps();
      }
    },
    removeStep(id) {
      // console.log(id);
      let userSteps = this.userSteps;
      let findIndex = this.userSteps
        .map(item => {
          return item.id;
        })
        .indexOf(id);
      //  console.log(findIndex);
      this.userSteps.splice(findIndex, 1);

      this.transactionSteps.map(item => {
        if (item.id == id) {
          return (item.choose = false);
        }
      });
    },
    openStepModal() {
      this.$emit("dialogOpen", "add-new-step");
    },
    onStart(evt) {
      console.log(evt.oldIndex);
    },
    checkMove(event) {
      let data = {
        transactionSteps: this.transactionSteps
      };

      this.app.loading = true;
      HTTP.put("api/transactions/updateOrder", data)
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            this.$toasted
              .show(response.data.Message, {
                type: "success"
                // icon: "error_outline"
              })
              .goAway(1500);
          }
        })
        .catch(error => {
          this.app.loading = false;
        });
    },
    onEnd: function(/**Event*/ evt) {
      var itemEl = evt.item; // dragged HTMLElement
      evt.to; // target list
      evt.from; // previous list
      evt.oldIndex; // element's old index within old parent
      evt.newIndex; // element's new index within new parent
      evt.oldDraggableIndex; // element's old index within old parent, only counting draggable elements
      evt.newDraggableIndex; // element's new index within new parent, only counting draggable elements
      evt.clone; // the clone element
      evt.pullMode; // when item is in another sortable: `"clone"` if cloning, `true` if moving

      //debugger;
      console.log(evt.oldIndex);

      console.log(evt.newIndex);

      // console.log(this.myArray);
      // console.log(this.move(this.myArray, 0, 2));
    },
    getAllSteps() {
      //   HTTP.get("api/steps/getAllSteps")
      //     .then(response => {
      //       if (response.data.Status) {
      //         this.app.loading = false;
      //         this.existingStepTags = response.data.Steps;
      //         // this.existingUserTags = users.map((item) => {
      //         //     return {
      //         //         key: item.id,
      //         //         value: item.username + " " + "(" + item.role_name + ")"
      //         //     }
      //         // });
      //       }
      //     })
      //     .catch(error => {
      //       // this.app.loading = false;
      //     });
    },
    getTransactionSteps(user_id = null) {
      this.app.loading = true;

      HTTP.get("api/transactions/getTransactionSteps", {
        params: {
          transaction_id: this.transaction_id,
          user_id: user_id
        }
      })
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
            if (user_id) {
              this.userSteps = response.data.Steps;
            } else {
              this.transactionSteps = response.data.Steps;

              this.transactionSteps.map(item => {
                let checkIndex = this.userSteps
                  .map(item1 => {
                    return item1.id;
                  })
                  .indexOf(item.id);

                if (checkIndex !== -1) {
                  return (item.choose = true);
                } else {
                  return (item.choose = false);
                }
              });
            }
          }
        })
        .catch(error => {
          // this.app.loading = false;
        });
    },
    checkedItem(item) {
      let index = this.userSteps
        .map(i => {
          return i.id;
        })
        .indexOf(item.id);
      if (index != -1) {
        item.checked = false;
      }
    },
    dragSelection() {
      //For Checked selection
      this.transactionSteps.map((item, i) => {
        if (item.checked) {
          console.log(i);
          this.userSteps.push({
            id: item.id,
            steps_title: item.steps_title,
            checked: false
          });
          item.checked = false;
          item.choose = true;
          return true;
        }
      });

      console.log(this.transactionSteps);
    },
    assignedSteps(item) {
      // let userSteps = this.userSteps;
      item.checked = false;
      // if(!item.checked){
      //     let index = this.userSteps.map((i)=> { return i.id }).indexOf(item.id);
      //     this.userSteps.splice(index, 1);
      // }
      // debugger;
    },
    close() {
      this.$emit("dialogClose", "transaction-steps");
    },
    selectTemplate(event) {
      //console.log(event);
      let findIndex = this.templates
        .map(item => {
          return item.id;
        })
        .indexOf(event);
      //console.log(this.templates[findIndex]);
      let templates = this.templates[findIndex];

      var r = [];
      let array1 = this.templates[findIndex].step_query.split(",");
      let array2 = this.templates[findIndex].steps_titles.split(",");

      for (let i = 1; i < array1.length; i++) {
        r[i] = {
          step_id: array1[i],
          title: array2[i],
          listOrder: i
        };
      }

      this.singleTemplate = r;

      let singleTemplate = this.singleTemplate;

      // singleTemplate.map((item,i)=>{return item ; })

      // debugger;
    },
    setSelectedTags() {
      this.selectedUserTags = [
        {
          key: this.user.user_id,
          value: this.user.username + " " + "(" + this.user.role_name + ")"
        }
      ];
      if (this.user.user_id) {
        this.getTransactionSteps(this.user.user_id);
      }
      // this.existingTemplateTags = this.templates;
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
    addTemplate(evt) {
      evt.preventDefault();
      const data = {
        paramType: "template", //Add template first time
        steps: this.selectedTemplateTags,
        transaction_id: this.transaction_id
      };

      this.app.loading = true;
      HTTP.post("api/transactions/addTransactionSteps", data)
        .then(response => {
          if (response.data.Status) {
            this.app.loading = false;
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
    save(evt, param) {
      evt.preventDefault();
      // this.$refs.form.submit();
      this.$v.$touch();
      if (this.$v.$error) {
        let div = document.getElementById("transaction-steps-modal");
        div.scrollTop = 0;
        return;
      }

      let data = {
        selected_users: this.selectedUserTags,
        userSteps: this.userSteps,
        transaction_id: this.transaction_id
      };

      if (param == 1) {
        this.app.loading2 = true;
      }

      if (param == 2) {
        this.app.loading3 = true;
      }
      HTTP.post("api/transactions/addStepsToUsers", data)
        .then(response => {
          if (response.data.Status) {
            if (param == 1) {
              this.app.loading2 = false;
            }

            if (param == 2) {
              this.app.loading3 = false;
            }

            this.$toasted
              .show(response.data.Message, {
                type: "success"
                // icon: "error_outline"
              })
              .goAway(1500);
          }
        })
        .catch(error => {
          if (param == 1) {
            this.app.loading2 = false;
          }

          if (param == 2) {
            this.app.loading3 = false;
          }
        });

      if (param == 1) {
        // this.$root.$emit('steps-assigned-user','success');
        this.$emit("dialogOpen", "steps-assigned-success");
      }

      if (param == 2) {
        this.userSteps = [];
        this.$emit("dialogClose", "transaction-steps-open-transaction-users");
      }
    },
    onUserTagChange(search) {
      if (this.existingUserTags.length) {
        //It will use Username as well as rolename for search
        let existingTags = this.existingUserTags
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
          this.userTagSearchResult = true;
        } else {
          this.userTagSearchResult = false;
        }
      } else {
        this.userTagSearchResult = false;
      }
    },
    onStepTagChange(search) {
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
