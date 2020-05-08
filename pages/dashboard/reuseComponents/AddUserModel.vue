<template>
  <!-- ADD AGENT POPUP -->
  <md-dialog
    :md-active.sync="active"
    class="modal-medium"
    :md-click-outside-to-close="false"
    :md-close-on-esc="false"
  >
    <md-dialog-title class="modal-title d-block py-3 px-4 bg-light mb-1">
      <h4 class="my-0 md-headline text-left text-primary">{{userModelTitle}}</h4>
      <md-button @click="close" class="md-icon-button position-absolute">
        <md-icon class="text-primary">close</md-icon>
      </md-button>
    </md-dialog-title>
    <div class="md-dialog-content" v-click-outside="close">
      <md-radio v-model="agent_type" :value="1">New</md-radio>
      <md-radio
        v-model="agent_type"
        :value="2"
        v-if="$route.params.id && show_existing_field"
      >Existing</md-radio>

      <form @submit.prevent="assignAgent" v-show="agent_type === 2" class="mt-1">
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
              <span>
                <LoadingButtonLoader
                  :typeEnable="'simpleLoader'"
                  v-bind:enable="app.loading"
                  class="mr-1"
                />Add
              </span>
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

      <form @submit.prevent="saveUser" v-show="agent_type === 1" class="userForm">
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
                    :class="app.getValidationClass($v.agentForm,'first_name')"
                  >
                    <label>First name</label>
                    <md-input v-model="agentForm.first_name"></md-input>
                    <!-- ERROR MESSAGE -->
                    <span
                      v-if="!$v.agentForm.first_name.required"
                      class="md-error"
                    >First name is required</span>
                  </md-field>
                </div>
                <!-- LAST NAME -->
                <div class="md-layout-item md-small-size-100 md-size-50">
                  <md-field
                    class="multi-error"
                    :class="app.getValidationClass($v.agentForm,'last_name')"
                  >
                    <label>Last name</label>
                    <md-input v-model="agentForm.last_name"></md-input>
                    <!-- ERROR MESSAGE -->
                    <span
                      v-if="!$v.agentForm.last_name.required"
                      class="md-error"
                    >Last name is required</span>
                  </md-field>
                </div>
                <!-- EMAIL -->
                <div class="md-layout-item md-small-size-100 md-size-100">
                  <md-field
                    class="multi-error"
                    :class="app.getValidationClass($v.agentForm,'email')"
                  >
                    <label>Email</label>
                    <md-input v-model="agentForm.email"></md-input>
                    <!-- ERROR MESSAGE -->
                    <span v-if="!$v.agentForm.email.required" class="md-error">Email is required</span>
                    <span v-if="!$v.agentForm.email.email" class="md-error">Email is Invalid</span>
                  </md-field>
                </div>
                <!-- PHONE NO -->
                <div class="md-layout-item md-small-size-100 md-size-100">
                  <md-field
                    class="multi-error"
                    :class="app.getValidationClass($v.agentForm,'phone')"
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
                <!-- COMMISSION FIELD -->
                <div v-if="propObj1" class="md-layout-item md-small-size-100 md-size-100">
                  <md-field
                    class="multi-error"
                    :class="app.getValidationClass($v.agentForm,'commission')"
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
                <!--  USER ROLE -->
                <div class="md-layout-item md-small-size-100 md-size-100" v-if="show_roles_field">
                  <md-field
                    slot="inputs"
                    :class="app.getValidationClass($v.agentForm,'role_id')"
                    v-if="!$route.params.id"
                  >
                    <label>Select Role</label>
                    <md-select id="role" v-model="agentForm.user_role_id">
                      <md-option value="2">Agent</md-option>
                      <md-option value="4">Vendor</md-option>
                      <md-option value="3">Co-ordinator</md-option>
                      <md-option value="5">Transactional</md-option>
                    </md-select>
                  </md-field>
                  <md-field
                    slot="inputs"
                    :class="app.getValidationClass($v.agentForm,'role_id')"
                    v-else
                  >
                    <label>Select Role</label>
                    <md-select id="role" v-model="agentForm.user_role_id">
                      <md-option value="5">Transactional</md-option>
                    </md-select>
                  </md-field>
                </div>

                <!--------- TITLE --------->
                <div class="md-layout-item md-small-size-100 md-size-100">
                  <md-field
                    class="multi-error"
                    :class="app.getValidationClass($v.agentForm,'title')"
                  >
                    <label>Title</label>
                    <md-input v-model="agentForm.title"></md-input>
                    <!-- ERROR MESSAGE -->
                    <span v-if="!$v.agentForm.title.required" class="md-error">Title is required</span>
                  </md-field>
                </div>

                <!-- COMPANY FIELD -->
                <div v-if="propObj3" class="md-layout-item md-small-size-100 md-size-100">
                  <md-field class="multi-error">
                    <label>Company</label>
                    <md-input v-model="agentForm.user_company"></md-input>
                  </md-field>
                </div>
              </div>
            </div>
            <!-- SUBMIT BUTTON AND LOADER-->
            <div class="md-layout-item md-size-100">
              <md-dialog-actions class="flex-wrap justify-content-center px-0">
                <md-button
                  type="submit"
                  class="md-primary button-primary-custom mb-2 mt-3 px-4 w-100"
                >
                  <LoadingButtonLoader
                    :typeEnable="'simpleLoader'"
                    v-bind:enable="app.loading"
                    class="mr-1"
                  />
                  <span>Save</span>
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
  name: "AddUserModel",
  props: [
    "active",
    "app",
    "propObj",
    "userModelTitle",

    "propObj1",
    "propObj2",
    "propObj3",
    "transaction_id"
  ],
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
      show_roles_field: true,
      agentForm: {
        first_name: "",
        last_name: "",
        email: "",
        phone: "",
        commission: "",
        title: "",
        user_company: ""
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
      show: false,
      show_existing_field: true
    };
  },

  created() {
    this.init();
  },
  mounted() {},
  methods: {
    init(param) {
      this.imgDataUrl = ""; // 2 APRIL
      console.log("paramss" + param);
      console.log(this.existingTags);
      this.agentForm = {
        first_name: "",
        last_name: "",
        email: "",
        phone: "",
        commission: "",
        title: ""
      };

      this.$v.agentForm.$reset(); /*reset validation*/
      //ITS REFER FROM PARENT COMPONENT
      if (param == "add-agent") {
        this.placeHolder = "Add a agent";
        this.existingTags = [];
        this.agentForm.user_role_id = "2"; //for agent
        this.show_roles_field = false; // as I dont need that dropdown so I just hide that from my field

        this.getAgents(); //get the list of agents those not assigned to this transaction yet
      } else if (param == "add-vendor") {
        this.placeHolder = "Add a vendor";
        this.existingTags = [];
        this.agentForm.user_role_id = "4"; //for vendor
        this.show_roles_field = false; // as I dont need that dropdown so I just hide that from my field
        console.log("add vendor");
        this.getVendors();
      } else if (param == "add-user") {
        this.existingTags = [];
        this.show_existing_field = false;
        if (this.$route.params.id) {
          this.agentForm.user_role_id = "5";
        }
        this.agent_type = 1; //do not remove i
        //in case we need to create a new function to call all user
        this.placeHolder = "Add a user";
        console.log("Her ww we call all agents");
      }
    },
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
      // this.$emit("dialogClose", "add-agent");
      this.$emit("dialogClose", "add-user");
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
     * GET LIST OF EXISTING AGENTS
     * Get all agent related to this company / admin
     * except those already added for this transaction
     * @param transaction_id
     */
    getAgents() {
      console.log("Here is get agents");
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
              console.log(this.existingTags);
            }
          }
        })
        .catch(error => {});
    },
    /**
     * GET LIST OF EXISTING VENDORS
     * Get all vendor related to this company / admin
     * except those already added for this transaction
     * @param transaction_id
     */

    getVendors() {
      HTTP.get("api/users/getVendors", {
        params: {
          transaction_id: this.$route.params.id
        }
      })
        .then(response => {
          if (response.data.Status) {
            this.vendors = response.data.User;
            for (let i = 0; i < this.vendors.length; i++) {
              const fullName =
                this.vendors[i].first_name + " " + this.vendors[i].last_name;
              this.existingTags[i] = {
                key: this.vendors[i].id,
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
      this.app.loading = true;

      // this.app.loading = true;
      const formdata = {
        transaction_id: this.$route.params.id,
        user_type: "agent",
        selected_users: this.selectedAgents
      };
      HTTP.post("api/transactions/assignUser", formdata)
        .then(response => {
          if (response.data.Status) {
            this.$emit("dialogClose", "load-user");
            this.$toasted
              .show(response.data.Message, {
                type: "success"
                // icon: "error_outline"
              })
              .goAway(1500);
            // this.app.loading = false;
            this.selectedAgents = [];

            this.app.loading = false;
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
    editVendor: function(event) {
      console.log("edit going here");
    },
    getRandomString(count) {
      let current_time = new Date();
      let transactional_string = current_time.valueOf().toString();
      return transactional_string.substring(count);
    },
    /**
     * Save agent
     *
     * [param] event
     */
    saveUser: function(event) {
      console.log("save user");
      event.preventDefault();
      this.$v.agentForm.$touch(); //do not remove this
      if (this.$v.agentForm.$error) return;
      this.app.loading = true;
      if (this.$route.params.id) {
        this.agentForm.transaction_id = this.$route.params.id;
      } else {
        this.agentForm.transaction_id = this.transaction_id;
      }

      this.agentForm.image = this.imgDataUrl;

      // APPEND ACCESS CODE ONLY FOR USER ROLE 5 THAT IS TRANSACTIONAL USER
      if (this.agentForm.user_role_id == 5) {
        this.agentForm.access_code = this.getRandomString(7);
      }

      HTTP.post("api/transactions/addUser", this.agentForm)
        .then(response => {
          if (response.data.Status) {
            // this.getAgents();
            this.$toasted
              .show(response.data.Message, {
                type: "success"
                // icon: "error_outline"
              })
              .goAway(1500);

            if (this.transaction_id) {
              console.log("add user after");
              this.$emit("dialogClose", "add-user");
              this.$root.$emit("add-user-created", "success");
            }
            //if its agent we are adding we will emit the event to ->Single Transaction ->Transaction agents
            this.$emit("dialogClose", "load-user");
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
