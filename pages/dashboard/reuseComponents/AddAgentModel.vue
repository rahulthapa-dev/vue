<template>
  <!-- ADD AGENT POPUP -->
  <md-dialog
    :md-active.sync="active"
    class="modal-medium"
    :md-click-outside-to-close="false"
    :md-close-on-esc="false"
  >
    <md-dialog-title class="modal-title d-block py-3 px-4 bg-light">
      <h4 class="my-0 md-headline text-left text-primary">Add agent</h4>
      <md-button @click="close" class="md-icon-button position-absolute">
        <md-icon class="text-primary">close</md-icon>
      </md-button>
    </md-dialog-title>
    <div class="md-dialog-content" v-click-outside="close">
      <md-radio v-model="agent_type" :value="1">New</md-radio>
      <md-radio v-model="agent_type" :value="2">Existing</md-radio>

      <form @submit.prevent="assignAgent" v-show="agent_type === 2">
        <md-content md-dynamic-height class="px-0">
          <tags-input
            @keyup="onKeyUp"
            element-id="tags"
            v-model="selectedAgents"
            :existing-tags="existingTags"
            :typeahead="true"
            :only-existing-tags="true"
            :placeholder="placeHolder"
            :typeahead-style="typeaheadStyle"
            :allow-duplicates="allowDuplicates"
            :typeaheadHideDiscard="typeaheadHideDiscard"
            class="mb-3"
          ></tags-input>

          <md-dialog-actions class="flex-wrap justify-content-center px-0">
            <md-button type="submit" class="md-primary button-primary-custom mb-2 mt-3 px-4 w-100">
              <span>Add</span>
            </md-button>

            <div class="d-block text-center w-100">
              <md-button
                :md-ripple="false"
                @click="close"
                class="cancel-link lighter-description mt-2 mb-2 text-capitalize"
              >Cancel</md-button>
            </div>
          </md-dialog-actions>
        </md-content>
      </form>

      <form @submit.prevent="saveAgent" v-show="agent_type === 1">
        <md-content md-dynamic-height class="px-0">
          <div class="md-layout">
            <div class="md-layout-item md-medium-size-20 md-size-20 md-small-size-100">
              <div class="modal-add-avatar d-flex flex-wrap justify-content-center">
                <!-- AGENT IMAGE UPLOAD -->
                <a class="btn" @click="toggleShow">
                  <img :src="imgDataUrl" />
                  <div
                    class="avatar-dialog d-flex align-center justify-content-center"
                    v-show="!imgDataUrl"
                  >
                    <md-icon>person_outline</md-icon>
                  </div>
                  <span
                    class="w-100 d-block brand-primary font-weight-regular description-normal text-center cursor-pointer"
                  >Profile Avatar</span>
                </a>

                <profile-image-upload
                  v-model="show"
                  field="upload"
                  @crop-success="cropSuccess"
                  @crop-upload-success="cropUploadSuccess"
                  @crop-upload-fail="cropUploadFail"
                  :width="300"
                  :height="300"
                  :headers="headers"
                  :params="params"
                  img-format="png"
                  lang-type="en"
                  method="post"
                ></profile-image-upload>
              </div>
            </div>
            <!-- url="/api/users/profileImage" -->
            <div class="md-layout-item md-medium-size-80 md-size-80 md-small-size-100">
              <div class="md-layout">
                <!-- FIRST NAME -->
                <div class="md-layout-item md-small-size-100 md-size-50">
                  <md-field
                    class="multi-error"
                    :class="propObj.getValidationClass($v.agentForm,'first_name')"
                  >
                    <label>First name</label>
                    <md-input v-model="agentForm.first_name"></md-input>
                    <!-- ERROR MESSAGE -->
                    <span
                      v-if="!$v.agentForm.first_name.required"
                      class="md-error"
                    >Firstname is required</span>
                  </md-field>
                </div>
                <!-- LAST NAME -->
                <div class="md-layout-item md-small-size-100 md-size-50">
                  <md-field
                    class="multi-error"
                    :class="propObj.getValidationClass($v.agentForm,'last_name')"
                  >
                    <label>Last name</label>
                    <md-input v-model="agentForm.last_name"></md-input>
                    <!-- ERROR MESSAGE -->
                    <span
                      v-if="!$v.agentForm.last_name.required"
                      class="md-error"
                    >Lastname is required</span>
                  </md-field>
                </div>
                <!-- EMAIL -->
                <div class="md-layout-item md-small-size-100 md-size-100">
                  <md-field
                    class="multi-error"
                    :class="propObj.getValidationClass($v.agentForm,'email')"
                  >
                    <label>Email</label>
                    <md-input v-model="agentForm.email"></md-input>
                    <!-- ERROR MESSAGE -->
                    <span v-if="!$v.agentForm.email.required" class="md-error">Email is required</span>
                    <span v-if="!$v.agentForm.email.email" class="md-error">Email is Invalid</span>
                  </md-field>
                </div>
                <!-- PHONE NO -->
                <div class="md-layout-item md-small-size-100 md-size-50">
                  <md-field
                    class="multi-error"
                    :class="propObj.getValidationClass($v.agentForm,'phone')"
                  >
                    <label>Phone no.</label>
                    <md-input v-model="agentForm.phone"></md-input>

                    <!-- ERROR MESSAGE -->

                    <span v-if="!$v.agentForm.phone.required" class="md-error">Phone no is required</span>
                    <span v-if="!$v.agentForm.phone.phoneValid" class="md-error">Phone no is Invalid</span>
                    <span v-if="!$v.agentForm.phone.minLength" class="md-error">minLength is 10</span>
                    <span v-if="!$v.agentForm.phone.maxLength" class="md-error">maxLength is 13</span>
                  </md-field>
                </div>

                <div class="md-layout-item md-small-size-100 md-size-50">
                  <md-field
                    class="multi-error"
                    :class="propObj.getValidationClass($v.agentForm,'commission')"
                  >
                    <label>Commission</label>
                    <md-input v-model="agentForm.commission"></md-input>
                    <!-- ERROR MESSAGE -->
                    <span
                      v-if="!$v.agentForm.commission.commissionValid"
                      class="md-error"
                    >no is Invalid</span>
                  </md-field>
                </div>

                <!-- TITLE -->
                <div class="md-layout-item md-small-size-100 md-size-100">
                  <md-field
                    class="multi-error"
                    :class="propObj.getValidationClass($v.agentForm,'title')"
                  >
                    <label>Title</label>
                    <md-input v-model="agentForm.title"></md-input>
                    <!-- ERROR MESSAGE -->
                    <span v-if="!$v.agentForm.title.required" class="md-error">Title is required</span>
                  </md-field>
                </div>
              </div>
            </div>
            <div class="md-layout-item md-size-100">
              <md-dialog-actions class="flex-wrap justify-content-center px-0">
                <md-button
                  type="submit"
                  class="md-primary button-primary-custom mb-2 mt-3 px-4 w-100"
                >
                  <LoadingButtonLoader
                    :typeEnable="app.checkType('simpleLoader')"
                    v-bind:enable="app.loading"
                    class="mr-1"
                  />
                  <span>Add</span>
                </md-button>

                <div class="d-block text-center w-100">
                  <md-button
                    :md-ripple="false"
                    @click="close"
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
  <!-- END AGENT POPUP -->
</template>
<script>
import { HTTP } from "../../../httpCommon";
import VoerroTagsInput from "@voerro/vue-tagsinput";
import myUpload from "vue-image-crop-upload";
import LoadingButtonLoader from "../../../components/LoadingButtonLoader";
import {
  required,
  email,
  minLength,
  maxLength,
  sameAs
} from "vuelidate/lib/validators";
const isPhone = value => /^(?=.*[0-9])[- +()0-9]+$/.test(value); //phone valid
const isNumber = value => /^(\d|,)*\d*$/.test(value); //phone valid
export default {
  name: "AddAgentModel",
  props: ["active", "app", "propObj"],
  components: {
    "tags-input": VoerroTagsInput,
    "profile-image-upload": myUpload,
    LoadingButtonLoader
  },
  data: function() {
    let authtoken = JSON.parse(localStorage.getItem("auth"));
    this.token = authtoken.token;
    return {
      agents: [],
      agent_type: 1,
      agentForm: {
        first_name: "",
        last_name: "",
        email: "",
        phone: "",
        commission: "",
        title: ""
      },
      placeHolder: "Add a agent",
      existingTags: [],
      selectedAgents: [],
      //IMAGE UPLOAD
      allowDuplicates: false,
      typeaheadStyle: "dropdown",
      typeaheadHideDiscard: true,
      headers: {
        smail: "*_~",
        Authorization: `Bearer ${this.token}`
      },
      params: {
        type: "add-agent"
      },
      imgDataUrl: "", // the datebase64 url of created image
      show: false
    };
  },

  created() {
    this.init();
    this.getAgents();
  },
  methods: {
    init() {},
    onInitialized() {
      console.log("Initialized");
    },

    onTagAdded(slug) {
      console.log(`Tag added: ${slug}`);
    },

    onTagRemoved(slug) {
      console.log(`Tag removed: ${slug}`);
    },

    onTagsUpdated() {
      console.log("Tags updated");
    },

    onLimitReached() {
      console.log("Max Reached");
    },

    onKeyDown() {
      console.log("Key down");
    },

    onKeyUp(value) {
      console.log("Key up");
      console.log(`Input changed: ${value}`);
    },

    onFocus() {
      console.log("Input focused");
    },

    onBlur() {
      console.log("Input blurred");
    },

    onChange(value) {
      console.log(`Input changed: ${value}`);
    },
    close() {
      this.show = false; // profile upload set flase
      this.$emit("dialogClose", "add-agent");
    },
    optionChange(event) {
      // debugger;
    },
    setSelectedTags() {
      // this.selectedTags = [{ key: "php", value: "PHP" }];
    },
    toggleShow() {
      this.show = !this.show;
    },
    /**
     * Get all agent related to this company / admin
     * except those already added for this transaction
     *
     */
    getAgents() {
      HTTP.get("api/users/getAgents", {
        params: {
          transaction_id: this.$route.params.id
        }
      })
        .then(response => {
          if (response.data.Status) {
            this.agents = response.data.User;
            for (let i = 0; i < this.agents.length; i++) {
              const fullName =
                this.agents[i].first_name + " " + this.agents[i].last_name;
              this.existingTags[i] = {
                key: this.agents[i].id,
                value: fullName
              };
            }
          }
        })
        .catch(error => {});
    },
    /**
     * Assign agent to transaction
     * assign agent to existing transaction
     */
    assignAgent: function(event) {
    //   console.log(this.selectedAgents);
      this.app.loading = true;
      const formdata = {
        transaction_id: this.$route.params.id,
        selected_users: this.selectedAgents
      };
      HTTP.post("api/transactions/assignUser", formdata)
        .then(response => {
          if (response.data.Status) {
            this.$toasted
              .show(response.data.Message, {
                type: "success"
                // icon: "error_outline"
              })
              .goAway(1500);
            this.app.loading = false;
            this.selectedAgents = [];
            // this.$v.agentForm.$reset();
            this.close();
          }
        })
        .catch(error => {
          console.log("catch error");
          this.app.loading = false;
          // debugger;
          // let message = "";
          // this.app.loading = false;
        });
      console.log("assign agent");
    },
    /**
     * Save agent
     *
     * [param] event
     */
    saveAgent: function(event) {
      console.log("save agent");
      event.preventDefault();
      this.$v.agentForm.$touch(); //do not remove this
      if (this.$v.agentForm.$error) return;

      const formdata = {
        first_name: this.agentForm.first_name,
        last_name: this.agentForm.last_name,
        email: this.agentForm.email,
        phone: this.agentForm.phone,
        commission: this.agentForm.commission,
        title: this.agentForm.title,
        transaction_id: this.$route.params.id,
        image: this.imgDataUrl
      };
      this.app.loading = true;
      HTTP.post("api/users/addAgent", formdata)
        .then(response => {
          if (response.data.Status) {
            this.getAgents();
            this.$toasted
              .show(response.data.Message, {
                type: "success"
                // icon: "error_outline"
              })
              .goAway(1500);
            this.app.loading = false;
            this.$v.agentForm.$reset();
            this.close();
          }
        })
        .catch(error => {
          console.log("catch error");
          this.app.loading = false;
          // debugger;
          // let message = "";
          // this.app.loading = false;
        });
    },
    /**
     * crop success
     *
     * [param] imgDataUrl
     * [param] field
     */
    cropSuccess(imgDataUrl, field) {
      console.log("-------- crop success --------");
      console.log(field);
      this.imgDataUrl = imgDataUrl;
    },
    /**
     * upload success
     *
     * [param] jsonData  server api return data, already json encode
     * [param] field
     */
    cropUploadSuccess(jsonData, field) {
      console.log("-------- upload success --------");
      console.log(jsonData);
      console.log("field: " + field);
    },
    srcFileSet(fileName, fileType, fileSize) {
      console.log("file NAME" + fileName);
      console.log("file fileType" + fileType);
      console.log("file fileSize" + fileSize);
    },
    /**
     * upload fail
     *
     * [param] status    server api return error status, like 500
     * [param] field
     */
    cropUploadFail(status, field) {
      console.log("-------- upload fail --------");
      console.log(status);
      console.log("field: " + field);
    }
  },
  validations: {
    agentForm: {
      first_name: {
        required
      },
      last_name: {
        required
      },
      email: {
        required,
        email
      },
      phone: {
        required,
        phoneValid: isPhone,
        minLength: minLength(10),
        maxLength: maxLength(13)
      },
      commission: {
        commissionValid: isNumber
      },
      title: {
        required
      }
    }
  },
  computed: {},
  watch: {}
};
</script>

<style scoped>
</style>
