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
                  <span class="brand-primary">{{userName}}</span>.
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
    <!-- EDIT VENDOR POPUP -->
    <md-dialog :md-active.sync="active" class="modal-medium">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light">
        <h4 class="my-0 md-headline text-left text-primary">Edit Vendor</h4>
        <md-button class="md-icon-button position-absolute" @click="active=false">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content">
        <form @submit.prevent="updateVendor">
          <md-content md-dynamic-height class="px-0">
            <div class="md-layout">
              <div class="md-layout-item md-medium-size-20 md-size-20 md-small-size-100">
                <div class="modal-add-avatar d-flex flex-wrap justify-content-center mt-4">
                  <!-- VENDOR IMAGE UPLOAD -->

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
                      <md-input v-model="vendorForm.email" :disabled="true"></md-input>
                      <!-- ERROR MESSAGE -->
                      <span v-if="!$v.vendorForm.email.required" class="md-error">Email is required</span>
                      <span v-if="!$v.vendorForm.email.email" class="md-error">Email is Invalid</span>
                    </md-field>
                  </div>
                  <!-- PHONE NO -->
                  <div class="md-layout-item md-small-size-100 md-size-100">
                    <md-field
                      class="multi-error"
                      :class="propObj.getValidationClass($v.vendorForm,'phone')"
                    >
                      <label>Phone no.</label>
                      <md-input v-model="vendorForm.phone"></md-input>
                      <!-- ERROR MESSAGE -->
                      <span
                        v-if="!$v.vendorForm.phone.required"
                        class="md-error"
                      >Phone no is required</span>
                      <span
                        v-if="!$v.vendorForm.phone.phoneValid"
                        class="md-error"
                      >Phone no is Invalid</span>
                      <span v-if="!$v.vendorForm.phone.minLength" class="md-error">minLength is 10</span>
                      <span v-if="!$v.vendorForm.phone.maxLength" class="md-error">maxLength is 13</span>
                    </md-field>
                  </div>
                  <!-- TITLE -->
                  <div class="md-layout-item md-small-size-100 md-size-100">
                    <md-field class="multi-error">
                      <label>Title</label>
                      <md-input v-model="vendorForm.title"></md-input>
                      <!-- ERROR MESSAGE -->
                    </md-field>
                  </div>
                  <!-- company NO -->
                  <div class="md-layout-item md-small-size-100 md-size-100">
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
                      :typeEnable="'simpleLoader'"
                      v-bind:enable="app.loading"
                      class="mr-1"
                    />
                    <span>Update</span>
                  </md-button>

                  <div class="d-block text-center">
                    <md-button
                      :md-ripple="false"
                      @click="active=false"
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
    <!-- @END EDIT VENDOR -->
    <section class="mb-5">
      <md-table class="mb-3 table-agent">
        <md-table-row class="table-head">
          <md-table-head></md-table-head>
          <md-table-head>First Name</md-table-head>
          <md-table-head>Last Name</md-table-head>
          <md-table-head>Email Address</md-table-head>
          <md-table-head>Phone Number</md-table-head>
          <md-table-head>Title</md-table-head>
          <md-table-head>Company</md-table-head>
          <md-table-head></md-table-head>
        </md-table-row>
        <md-table-row v-if="!users.length > 0">
          <md-table-cell colspan="7">
            <!-- no data start -->
            No record exist
            <!-- no data end -->
          </md-table-cell>
        </md-table-row>
        <md-table-row v-for="(vendor,index) in users" :key="index">
          <md-table-cell>
            <div
              v-if="!vendor.image"
              class="md-avatar md-intial"
            >{{vendor.first_name.substring(0,1).toUpperCase()}}{{vendor.last_name.substring(0,1).toUpperCase()}}</div>
            <div v-if="vendor.image" class="md-avatar">
              <img v-bind:src="'/images/users/' + vendor.image" alt="Agent" />
            </div>
          </md-table-cell>
          <md-table-cell>{{ vendor.first_name }}</md-table-cell>
          <md-table-cell>
            <span class="text-primary">{{ vendor.last_name }}</span>
          </md-table-cell>
          <md-table-cell>{{ vendor.email }}</md-table-cell>
          <md-table-cell>{{ vendor.phone }}</md-table-cell>
          <md-table-cell>{{ vendor.title }}</md-table-cell>
          <md-table-cell>{{ vendor.user_company }}</md-table-cell>
          <md-table-cell md-label>
            <md-menu md-direction="bottom-end">
              <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                <md-icon>more_vert</md-icon>
              </md-button>
              <md-menu-content class="menu-content-custom">
                <md-menu-item @click="editUser(vendor.id,index)">Edit</md-menu-item>
                <md-menu-item @click="deleteUser(vendor)">Delete</md-menu-item>
              </md-menu-content>
            </md-menu>
          </md-table-cell>
        </md-table-row>
      </md-table>
    </section>
  </div>
</template>
<script>
import { HTTP } from "../../httpCommon";
import myUpload from "vue-image-crop-upload";
import LoadingButtonLoader from "../../components/LoadingButtonLoader";
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
  name: "TransactionVendors",
  props: ["app", "propObj"],
  components: { "profile-image-upload": myUpload, LoadingButtonLoader },
  data: function() {
    return {
      users: [],
      vendorForm: {
        first_name: "",
        last_name: "",
        email: "",
        phone: "",
        company: "",
        title: ""
      },
      active: false,
      headers: {
        smail: "*_~",
        Authorization: `Bearer ${this.token}`
      },
      params: {
        type: "edit-vendor"
      },
      imgDataUrl: "", // the datebase64 url of created image,
      show: false,
      dialogActive: false,
      userName: ""
    };
  },
  watch: {},
  mounted() {
    this.init();
    this.getUsers();
  },
  methods: {
    init() {},
    getUsers() {
      HTTP.get("api/transactions/getUsers", {
        params: {
          transaction_id: this.$route.params.id,
          user_role: 4 //4 MEANS GET USER WITH ROLE VENDOR
        }
      })
        .then(response => {
          if (response.data.Status) {
            console.log(response.data);
            this.users = response.data.Users;
            // this.transactionDesc = response.data.Agents[0].description;
          }
        })
        .catch(error => {});
    },
    editUser(id, index) {
      this.user_id = id;
      this.user_role_id = 4;
      this.active = true; // SHOW EDIT DIALOG
      let checkIndex = index;
      if (checkIndex !== -1) {
        let user = this.users[checkIndex];
        this.imgDataUrl = user.image ? "/images/users/" + user.image : "";
        this.vendorForm = {
          first_name: user.first_name,
          last_name: user.last_name,
          email: user.email,
          phone: user.phone,
          user_role_id: user.role_id,
          title: user.title,
          company: user.user_company
        };
      }
    },
    deleteUser(data) {
      this.dialogActive = true;
      this.deleteId = data.user_id;
      this.userName = data.first_name + " " + data.last_name;
    },
    confirmDelete() {
      HTTP.get("api/transactions/unassignUser", {
        params: {
          transaction_id: this.$route.params.id,
          delete_id: this.deleteId //basically its a user id that we are going to unassociate
        }
      })
        .then(response => {
          if (response.data.Status) {
            this.getUsers();
            console.log("success");
            this.$toasted
              .show(response.data.Message, {
                type: "success"
                // icon: "error_outline"
              })
              .goAway(1500);
            this.dialogActive = false;
          }
        })
        .catch(error => {});
    },
    /**
     * upload image show /hide
     *
     * [param] field
     */
    toggleShow() {
      this.show = !this.show;
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
    updateVendor() {
      event.preventDefault();
      this.$v.vendorForm.$touch(); //do not remove this
      if (this.$v.vendorForm.$error) return;
      const formdata = {
        user_id: this.user_id,
        user_role_id: this.user_role_id,
        first_name: this.vendorForm.first_name,
        last_name: this.vendorForm.last_name,
        email: this.vendorForm.email,
        phone: this.vendorForm.phone,
        image: this.imgDataUrl, //shalu
        title: this.vendorForm.title,
        user_company: this.vendorForm.company
      };

      this.app.loading = true;
      HTTP.post("api/users/updateSubUsers", formdata)
        .then(response => {
          if (response.data.Status) {
            this.getUsers();
            this.$toasted
              .show(response.data.Message, {
                type: "success"
              })
              .goAway(1500);
            this.app.loading = false;
            this.$v.vendorForm.$reset();
            this.active = false;
          }
        })
        .catch(error => {
          console.log("catch error");
          this.app.loading = false;
        });
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
  },

  computed: {}
};
</script>

 <style>
</style>
