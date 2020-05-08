<template>
  <!-- ADD AGENT POPUP -->
  <md-dialog
    :md-active.sync="active"
    class="modal-medium"
    :md-click-outside-to-close="false"
    :md-close-on-esc="false"
  >
    <md-dialog-title class="modal-title d-block py-3 px-4 bg-light">
      <h4 class="my-0 md-headline text-left text-primary">Add Vendor</h4>
      <md-button @click="close" class="md-icon-button position-absolute">
        <md-icon class="text-primary">close</md-icon>
      </md-button>
    </md-dialog-title>
    <div class="md-dialog-content" v-click-outside="close">
      <md-radio v-model="vendor_type" :value="1">New</md-radio>
      <md-radio v-model="vendor_type" :value="2">Existing</md-radio>

      <form @submit.prevent="assignVendor" v-show="vendor_type === 2">
        <md-content md-dynamic-height class="px-0">
          <tags-input
            element-id="tags"
            v-model="selectedVendors"
            :existing-tags="existingTags"
            :typeahead="true"
            :placeholder="placeHolder"
            :typeahead-style="typeaheadStyle"
            :allow-duplicates="allowDuplicates"
            :typeaheadHideDiscard="typeaheadHideDiscard"
          ></tags-input>

          <md-dialog-actions class="flex-wrap justify-content-start px-0">
            <md-button type="submit" class="md-primary button-primary-custom mb-3 mt-3 px-4">
              <span>Add</span>
            </md-button>

            <div class="d-block text-center">
              <md-button
                :md-ripple="false"
                @click="close"
                class="cancel-link lighter-description mt-2 text-capitalize"
              >Cancel</md-button>
            </div>
          </md-dialog-actions>
        </md-content>
      </form>

      <form @submit.prevent="saveVendor" v-show="vendor_type === 1">
        <md-content md-dynamic-height class="px-0">
          <div class="md-layout">
            <div class="md-layout-item md-medium-size-20 md-size-20 md-small-size-100">
              <div class="modal-add-avatar d-flex flex-wrap justify-content-center mt-4">
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
                    class="w-100 d-block brand-primary font-weight-regular description-normal text-center"
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
                    :class="propObj.getValidationClass($v.vendorForm,'first_name')"
                  >
                    <label>First name</label>
                    <md-input v-model="vendorForm.first_name"></md-input>
                    <!-- ERROR MESSAGE -->
                    <span
                      v-if="!$v.vendorForm.first_name.required"
                      class="md-error"
                    >Firstname is required</span>
                  </md-field>
                </div>
                <!-- LAST NAME -->
                <div class="md-layout-item md-small-size-100 md-size-50">
                  <md-field
                    class="multi-error"
                    :class="propObj.getValidationClass($v.vendorForm,'last_name')"
                  >
                    <label>Last name</label>
                    <md-input v-model="vendorForm.last_name"></md-input>
                    <!-- ERROR MESSAGE -->
                    <span
                      v-if="!$v.vendorForm.last_name.required"
                      class="md-error"
                    >Lastname is required</span>
                  </md-field>
                </div>
                <!-- EMAIL -->
                <div class="md-layout-item md-small-size-100 md-size-100">
                  <md-field
                    class="multi-error"
                    :class="propObj.getValidationClass($v.vendorForm,'email')"
                  >
                    <label>Email</label>
                    <md-input v-model="vendorForm.email"></md-input>
                    <!-- ERROR MESSAGE -->
                    <span v-if="!$v.vendorForm.email.required" class="md-error">Email is required</span>
                    <span v-if="!$v.vendorForm.email.email" class="md-error">Email is Invalid</span>
                  </md-field>
                </div>
                <!-- PHONE NO -->
                <div class="md-layout-item md-small-size-100 md-size-50">
                  <md-field
                    class="multi-error"
                    :class="propObj.getValidationClass($v.vendorForm,'phone')"
                  >
                    <label>Phone no.</label>
                    <md-input v-model="vendorForm.phone"></md-input>
                    <!-- ERROR MESSAGE -->
                    <span v-if="!$v.vendorForm.phone.required" class="md-error">Phone no is required</span>
                    <span
                      v-if="!$v.vendorForm.phone.phoneValid"
                      class="md-error"
                    >Phone no is Invalid</span>
                    <span v-if="!$v.vendorForm.phone.minLength" class="md-error">minLength is 10</span>
                    <span v-if="!$v.vendorForm.phone.maxLength" class="md-error">maxLength is 13</span>
                  </md-field>
                </div>
                <div class="md-layout-item md-small-size-100 md-size-50">
                  <md-field class="multi-error">
                    <label>Company</label>
                    <md-input v-model="vendorForm.company"></md-input>
                    <!-- ERROR MESSAGE -->
                  </md-field>
                </div>
              </div>
              <md-dialog-actions class="flex-wrap justify-content-start px-0">
                <md-button type="submit" class="md-primary button-primary-custom mb-3 mt-3 px-4">
                  <LoadingButtonLoader
                    :typeEnable="app.checkType('simpleLoader')"
                    v-bind:enable="app.loading"
                    class="mr-1"
                  />
                  <span>Add</span>
                </md-button>

                <div class="d-block text-center">
                  <md-button
                    :md-ripple="false"
                    @click="close"
                    class="cancel-link lighter-description mt-2 text-capitalize"
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
  name: "AddVendorModel",
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
      vendor_type: 1,
      vendorForm: {
        first_name: "",
        last_name: "",
        email: "",
        phone: "",
        company: ""
      },
      placeHolder: "Add a vendor",
      existingTags: [],
      selectedVendors: [],
      //IMAGE UPLOAD
      allowDuplicates: false,
      typeaheadStyle: "dropdown",
      typeaheadHideDiscard: true,
      headers: {
        smail: "*_~",
        Authorization: `Bearer ${this.token}`
      },
      params: {
        type: "add-vendor" // it used when close the related popup
      },
      imgDataUrl: "", // the datebase64 url of created image
      show: false
    };
  },
  created() {
    this.init();
    this.getVendors();
  },
  methods: {
    init() {},
    close() {
      this.show = false; // profile upload set flase
      this.$emit("dialogClose", "add-vendor");
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
     * crop success
     *
     * [param] imgDataUrl
     * [param] field
     */ cropSuccess(imgDataUrl, field) {
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
    },
    /**
     * Assign agent to transaction
     * assign agent to existing transaction
     */
    assignVendor: function(event) {
      console.log("selectedVendors");
      console.log(this.selectedVendors);
      if (this.selectedVendors.length == 0) return;

      this.app.loading = true;
      const formdata = {
        transaction_id: this.$route.params.id,
        user_type: "vendor",
        selected_vendors: this.selectedVendors
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
            this.selectedVendors = [];
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
    saveVendor: function(event) {
      event.preventDefault();
      event.target.reset();
      this.$v.vendorForm.$touch(); //do not remove this

      if (this.$v.vendorForm.$error) return;
      const formdata = {
        transaction_id: this.$route.params.id,
        user_role_id: 4, // for vendor
        first_name: this.vendorForm.first_name,
        last_name: this.vendorForm.last_name,
        email: this.vendorForm.email,
        phone: this.vendorForm.phone,
        image: this.imgDataUrl
      };
      this.app.loading = true;
      HTTP.post("api/users/addUser", formdata)
        .then(response => {
          if (response.data.Status) {
            this.$toasted
              .show(response.data.Message, {
                type: "success"
                // icon: "error_outline"
              })
              .goAway(1500);
            this.app.loading = false;

            this.close();
            this.$v.vendortForm.$reset();
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
    }
  },
  validations: {
    vendorForm: {
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
      }
    }
  }
};
</script>