<template>
  <!-- DELETE MODEL -->

  <div class="users">
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
                  <span class="brand-primary">{{userSelectedName}}</span> from the team.
                </p>
                <md-dialog-actions class="flex-wrap justify-content-center">
                  <md-button
                    class="md-danger w-100 button-primary-custom mb-2 mt-3 no-shadow"
                    @click="onConfirm"
                  >Delete</md-button>
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
    <!-- <md-dialog-confirm
      :md-active.sync="dialogActive"
      md-title="Confirmation?"
      md-content="Are you sure want to delete?."
      md-confirm-text="Agree"
      md-cancel-text="Disagree"
      @md-cancel="onCancel"
      @md-confirm="onConfirm"
    />-->

    <md-dialog :md-active.sync="showDialog">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light">
        <h4 class="my-0 md-headline text-left text-primary">{{headingTitle}}</h4>
      </md-dialog-title>
      <div class="md-dialog-content">
        <form @submit.prevent="submit">
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
              <div class="md-layout-item md-medium-size-80 md-size-80 md-small-size-100">
                <div class="md-layout">
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      :class="app.getValidationClass($v.userForm,'first_name')"
                    >
                      <label>First name</label>
                      <md-input v-model="userForm.first_name"></md-input>
                      <span
                        v-if="!$v.userForm.first_name.required"
                        class="md-error"
                      >Firstname is required</span>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      :class="app.getValidationClass($v.userForm,'last_name')"
                    >
                      <label>Last name</label>
                      <md-input v-model="userForm.last_name"></md-input>
                      <span
                        v-if="!$v.userForm.last_name.required"
                        class="md-error"
                      >Lastname is required</span>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      :class="app.getValidationClass($v.userForm,'email')"
                      class="multi-error"
                    >
                      <label>Email</label>

                      <md-input v-model="userForm.email" :disabled="editStatus"></md-input>
                      <span v-if="!$v.userForm.email.required" class="md-error">Email is required</span>
                      <span v-if="!$v.userForm.email.email" class="md-error">Email is Invalid</span>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      :class="app.getValidationClass($v.userForm,'phone')"
                      class="multi-error"
                    >
                      <label>Phone no.</label>
                      <md-input v-model="userForm.phone"></md-input>
                      <span v-if="!$v.userForm.phone.required" class="md-error">Phone no is required</span>
                      <span
                        v-if="!$v.userForm.phone.phoneValid"
                        class="md-error"
                      >Phone no is Invalid</span>
                      <span v-if="!$v.userForm.phone.minLength" class="md-error">minLength is 10</span>
                      <span v-if="!$v.userForm.phone.maxLength" class="md-error">maxLength is 13</span>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs" :class="app.getValidationClass($v.userForm,'role_id')">
                      <label>Select Role</label>
                      <md-select id="role" v-model="userForm.role_id">
                        <md-option value="2">Agent</md-option>
                        <md-option value="3">Co-ordinator</md-option>
                      </md-select>
                      <span v-if="!$v.userForm.role_id.required" class="md-error">Role is required</span>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs" :class="app.getValidationClass($v.userForm,'title')">
                      <label>Title</label>
                      <md-input v-model="userForm.title"></md-input>
                      <span v-if="!$v.userForm.title.required" class="md-error">Title is required</span>
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
                    <span v-if="!editStatus">Add</span>
                    <span v-else>Update</span>
                  </md-button>

                  <div class="d-block text-center">
                    <md-button
                      :md-ripple="false"
                      @click="showDialog = false"
                      class="cancel-link lighter-description mt-2 text-capitalize"
                    >Cancel</md-button>
                  </div>
                </md-dialog-actions>
              </div>
            </div>
          </md-content>
          <!-- <md-dialog-actions class="flex-wrap justify-content-center">
                        <md-button type="submit" class="md-primary w-100 button-primary-custom mb-3 mt-3">
                        <span v-if="!editStatus">Add</span>
                        <span v-else>Update</span>
                        </md-button>
                        <div v-if="app.loading == true" class="loader">
                        <LoadingBar />
                        </div>
                        <div class="d-block w-100 text-center">
                        <md-button
                            :md-ripple="false"
                            @click="showDialog = false"
                            class="cancel-link lighter-description mt-2 text-capitalize"
                        >Cancel</md-button>
                        </div>
          </md-dialog-actions>-->
        </form>
      </div>
    </md-dialog>
    <div class="md-layout flex-md-column-reverse">
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-75">
        <md-card class="md-card-plain md-card-custom custom-shadow radius-10 mt-0">
          <md-card-header class="px-4 mb-0 mt-0">
            <div class="md-headline">
              <span class="text-primary">Bolleen Limited</span>
              <span class="brand-primary">
                <md-icon>keyboard_arrow_right</md-icon>Team
              </span>
            </div>
          </md-card-header>
          <md-card-content class="px-4 py-3">
            <div class="md-layout">
              <div class="md-layout-item d-flex justify-content-between mb-3">
                <h3 class="md-title text-primary">Team members</h3>
                <md-button class="md-primary md-icon-button" @click="addUsers">
                  <md-icon>add</md-icon>
                </md-button>
              </div>
            </div>

            <!-- <VuePerfectScrollbar class="scroll-area" v-if="users" v-once :settings="settings" @ps-scroll-y="scrollHandle"> -->
            <md-table class="mb-3" md-sort="name" md-sort-order="asc">
              <md-table-row>
                <md-table-head>Name</md-table-head>
                <md-table-head>EMAIL ADDRESS</md-table-head>
                <md-table-head>PHONE NUMBER</md-table-head>
                <md-table-head>ROLE</md-table-head>
                <md-table-head>TITLE</md-table-head>
                <md-table-head>ACCESS</md-table-head>
                <md-table-head></md-table-head>
              </md-table-row>

              <md-table-row v-if="!users.length > 0">
                <md-table-cell colspan="6">
                  <!-- no data start -->
                  <div
                    class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-100 justify-content-center align-center d-flex empty-div"
                  >
                    <img :src="'../img/no-data.svg'" alt="No data" class="alt-img" />
                  </div>
                  <!-- no data end -->
                </md-table-cell>
              </md-table-row>

              <md-table-row v-for="user in users" v-bind:key="user.id">
                <md-table-cell md-label="Name" class="avatar-cell">
                  <div
                    v-if="!user.image"
                    class="md-avatar position-absolute md-intial"
                  >{{user.first_name.substring(0,1).toUpperCase()}}{{user.last_name.substring(0,1).toUpperCase()}}</div>

                  <div v-if="user.image" class="md-avatar position-absolute">
                    <!-- VENDOR  SRC-->

                    <img
                      v-show="user.role_id == 4"
                      v-bind:src="'/images/users/' + user.image"
                      alt="Vendor"
                    />
                    <!-- AGENT SRC-->
                    <img
                      v-show="user.role_id == 2"
                      v-bind:src="'/images/users/' + user.image"
                      alt="Agent"
                    />
                    <!-- CO-ORDINATE SRC-->
                    <img
                      v-show="user.role_id == 3"
                      v-bind:src="'/images/co-ordinate/' + user.image"
                      alt="Co-ordinate"
                    />
                  </div>

                  <!-- <div class="md-avatar position-absolute">
                    <img src="https://placeimg.com/40/40/people/1" alt="People" />
                  </div>-->
                  <span>
                    {{ user.first_name }}
                    <label class="review-label">
                      4
                      <md-icon>star</md-icon>
                    </label>
                  </span>
                </md-table-cell>
                <md-table-cell md-label="Email Address">{{ user.email }}</md-table-cell>
                <md-table-cell md-label="Phone Number">
                  <span v-if="user.phone">{{ user.phone }}</span>
                </md-table-cell>
                <md-table-cell md-label="Role">
                  <span v-if="user.roles">{{ user.roles.role_name }}</span>
                </md-table-cell>
                <md-table-cell md-label="Title">{{ user.title }}</md-table-cell>
                <md-table-cell md-label="Access">
                  <span v-if="user.status">Active</span>
                  <span v-else>Not Active</span>
                </md-table-cell>
                <md-table-cell md-label>
                  <md-menu md-direction="bottom-end">
                    <md-button md-menu-trigger class="md-icon-button icon-button-flat">
                      <md-icon>more_vert</md-icon>
                    </md-button>
                    <md-menu-content class="menu-content-custom">
                      <md-menu-item @click="editUser(user.id)">Edit</md-menu-item>
                      <md-menu-item @click="deleteUser(user)">Delete</md-menu-item>
                    </md-menu-content>
                  </md-menu>
                </md-table-cell>
              </md-table-row>
            </md-table>
            <!-- </VuePerfectScrollbar> -->
            <div class="md-layout" v-if="totalUsers > 10">
              <div
                class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50 d-flex align-center"
              >
                <p
                  class="description font-weight-medium"
                  v-if="totalUsers"
                >Displaying {{ currentCount }} of {{ totalUsers }} records</p>
              </div>
              <div class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-size-50">
                <!-- for pagination you can check vue material kit -->
                <ul class="pagination">
                  <li
                    class="page-item prev-page"
                    v-if="page != 1"
                    v-bind:class="[changePage == pageNumber ? 'active' : '']"
                  >
                    <a @click="page--" class="page-link" aria-label="Previous">
                      <i class="fas fa-angle-double-left"></i>
                      <span v-if="withText" class="pr-1">Prev</span>
                    </a>
                  </li>
                  <li
                    class="page-item"
                    v-for="pageNumber in pages.slice(page-1, page+5)"
                    v-bind:key="pageNumber"
                    v-bind:class="[changePage == pageNumber ? 'active' : '']"
                  >
                    <a @click="page = pageNumber" class="page-link">{{ pageNumber }}</a>
                  </li>

                  <li
                    class="page-item page-pre next-page"
                    v-if="page < pages.length"
                    v-bind:class="[changePage == pageNumber ? 'active' : '']"
                  >
                    <a @click="page++" class="page-link" aria-label="Next">
                      <span class="pl-1">Next page</span>
                      <i class="fas fa-angle-double-right"></i>
                    </a>
                  </li>

                  <span class="md-hide">{{ changePage }}</span>
                </ul>
              </div>
            </div>
          </md-card-content>
        </md-card>
      </div>
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-25">
        <div class="search-right mb-4">
          <md-field
            class="custom-shadow bg-white search-outer p-3 radius-10 position-relative"
            md-inline
          >
            <img :src="'../img/search-icon.svg'" alt style="width: 20px;" class="position-absolute" />
            <label>Search</label>
            <md-input v-on:keyup.native="doFilter" class="search-input"></md-input>
          </md-field>
          <div class="mx-auto filter-section-outer md-hide">
            <div class="d-flex justify-content-between align-center mb-3 filter-heading-section">
              <h4 class="md-headline text-primary font-weight-medium my-0 hide-md">Filter</h4>
              <md-icon class="display-md">filter_list</md-icon>
              <small class="lighter-description font-weight-medium">Reset filter</small>
            </div>
            <div class="filter-section">
              <div>
                <div class="d-flex justify-content-start">
                  <md-icon class="text-primary">inbox</md-icon>
                  <h4 class="description text-primary w-100 pl-2">Access</h4>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <md-checkbox v-model="checkbox3" class>Normal</md-checkbox>
                  <label for class="label-account">295</label>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <md-checkbox v-model="checkbox4" class>Disable</md-checkbox>
                  <label for class="label-account">236</label>
                </div>
                <hr />
              </div>
              <div>
                <div class="d-flex justify-content-start">
                  <md-icon class="text-primary">people_alt</md-icon>
                  <h4 class="description text-primary w-100 pl-2">By Role</h4>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <md-checkbox v-model="checkbox5" class>Agent</md-checkbox>
                  <label for class="label-account">20</label>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <md-checkbox v-model="checkbox6" class>Co-ordinator</md-checkbox>
                  <label for class="label-account">35</label>
                </div>
                <hr />
              </div>
              <div>
                <div class="d-flex justify-content-start">
                  <md-icon class="text-primary">star</md-icon>
                  <h4 class="description text-primary w-100 pl-2">By Rating</h4>
                </div>
                <div class="d-flex justify-content-between align-center">
                  <span>
                    <md-checkbox v-model="checkbox7" class>
                      1
                      <md-icon class="star"></md-icon>
                    </md-checkbox>
                    <md-checkbox v-model="checkbox8" class>
                      2
                      <md-icon class="star"></md-icon>
                    </md-checkbox>
                    <md-checkbox v-model="checkbox9" class>
                      3
                      <md-icon class="star"></md-icon>
                    </md-checkbox>
                    <md-checkbox v-model="checkbox10" class>
                      4
                      <md-icon class="star"></md-icon>
                    </md-checkbox>
                    <md-checkbox v-model="checkbox11" class>
                      5
                      <md-icon class="star"></md-icon>
                    </md-checkbox>
                  </span>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { HTTP } from "../../httpCommon";
import myUpload from "vue-image-crop-upload";
import LoadingBar from "../../components/LoadingBar";
import LoadingButtonLoader from "../../components/LoadingButtonLoader";
import {
  required,
  email,
  minLength,
  maxLength,
  sameAs
} from "vuelidate/lib/validators";
//import vueCustomScrollbar from "vue-custom-scrollbar";
//import VuePerfectScrollbar from "vue-perfect-scrollbar";
const isPhone = value => /^(?=.*[0-9])[- +()0-9]+$/.test(value); //phone valid
const isCharacterValid = value =>
  /^[^~!@#$%\^&*()_+={}|[\]\\:';]*$/.test(value); //special character valid
export default {
  props: ["app"],
  components: {
    //vueCustomScrollbar,
    LoadingBar,
    LoadingButtonLoader,
    //VuePerfectScrollbar,
    "profile-image-upload": myUpload
  },
  data() {
    return {
      checkbox1: true,
      checkbox2: false,
      checkbox3: false,
      checkbox4: false,
      checkbox5: false,
      checkbox6: false,
      checkbox7: false,
      checkbox8: false,
      checkbox9: false,
      checkbox10: false,
      checkbox11: false,
      user_role_id: [],
      showDialog: false,
      headingTitle: "",
      name: "",
      email: "",
      phoneno: "",
      role: "",
      title: "",
      selected: [],
      users: [],
      totalUsers: "",
      posts: [],
      baseUrl: "https://jsonplaceholder.typicode.com/",
      page: 1, //current page
      perPage: 10,
      pages: [],
      userForm: {
        first_name: "",
        last_name: "",
        email: "",
        phone: "",
        role_id: "",
        title: ""
      },
      withText: Boolean,
      editStatus: false,
      settings: {
        maxScrollbarLength: 60
      },
      currentCount: "",
      pageNumber: "",
      dialogActive: false,
      userSelectedName: "",
      deleteId: "",
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
  validations: {
    userForm: {
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
      role_id: {
        required
      },
      title: {
        required
      }
    }
  },
  mounted() {
    // console.log("mounted"); // I'm text inside the component.
    // this.init(); // get plans at subscription
    // console.log(this.$toasted);
  },
  methods: {
    doFilter($event) {
      // if ($event.target.value) {
      HTTP.get("api/users/getSubUsers", {
        params: {
          from: 0,
          to: 10,
          user_role_id: [3, 2],
          search: $event.target.value
        }
      })
        .then(response => {
          this.users = response.data.Users;
          this.currentCount = this.users.length;
          this.totalUsers = response.data.Count;
        })
        .catch(error => {
          if (error.response.data.errors.Message) {
            if (
              error.response.data.errors.Message == "Token is Expired" ||
              error.response.data.errors.Message == "Token is Invalid"
            ) {
              this.$toasted
                .show(error.response.data.errors.Message, {
                  type: "success"
                  // icon: "error_outline"
                })
                .goAway(1500);
              localStorage.removeItem("auth");
              this.$router.push("/auth/login");
              return false;
            }
          }
        });
    },
    scrollHandle(evt) {
      console.log(evt);
    },
    init() {
      //call users for new entry
      HTTP.get("api/users/getSubUsers", {
        params: {
          from: 0,
          to: 10,
          user_role_id: [2, 3]
        }
      })
        .then(response => {
          this.users = response.data.Users;
          this.totalUsers = response.data.Count;
        })
        .catch(error => {
          console.log(error);
          if (error.response.data.errors.Message) {
            if (
              error.response.data.errors.Message == "Token is Expired" ||
              error.response.data.errors.Message == "Token is Invalid"
            ) {
              this.$toasted
                .show(error.response.data.errors.Message, {
                  type: "success"
                  // icon: "error_outline"
                })
                .goAway(1500);
              localStorage.removeItem("auth");
              this.$router.push("/auth/login");
              return false;
            }
          }
        });
    },
    getUsers() {
      console.log("Get user");
      HTTP.get("api/users/getSubUsers", {
        params: {
          from: 0,
          to: 10,
          user_role_id: [3, 2]
        }
      })
        .then(response => {
          this.users = response.data.Users;
          this.currentCount = this.users.length;
          this.totalUsers = response.data.Count;

          this.setPages();
          //   this.$router.push("/dashboard");
        })
        .catch(error => {
          if (error.response.data.errors.Message) {
            console.log(error.response.data.errors.Message);
            if (
              error.response.data.errors.Message == "Token not found" ||
              error.response.data.errors.Message == "Token is Expired" ||
              error.response.data.errors.Message == "Token is Invalid"
            ) {
              debugger;
              this.$toasted
                .show(error.response.data.errors.Message, {
                  type: "success"
                  // icon: "error_outline"
                })
                .goAway(1500);
              localStorage.removeItem("auth");
              this.$router.push("/auth/login");

              return false;
            }
          }
        });
    },
    toggleShow() {
      this.show = !this.show;
    },
    editUser(id) {
      this.headingTitle = "Edit user";
      this.editStatus = true;
      let checkIndex = this.users
        .map((item, index) => {
          return item.id;
        })
        .indexOf(id);

      if (checkIndex !== -1) {
        let user = this.users[checkIndex];
        this.imgDataUrl = "/images/users/" + user.image;
        // if (user.role_id == 2) {
        //   this.imgDataUrl = "/images/agents/" + user.image;
        // } else if (user.role_id == 3) {
        //   this.imgDataUrl = "/images/co-ordinate/" + user.image;
        // } else if (user.role_id == 4) {
        //   this.imgDataUrl = "/images/vendors/" + user.image;
        // }

        this.userForm = {
          first_name: user.first_name,
          last_name: user.last_name,
          email: user.email,
          phone: user.phone,
          role_id: user.role_id,
          title: user.title
        };
      }

      this.showDialog = true;
      //    debugger;
    },
    onConfirm() {
      if (this.deleteId) {
        this.app.loading = true;
        HTTP.delete("api/users/deleteSubUsers", {
          params: {
            id: this.deleteId
          }
        })
          .then(response => {
            if (response.data.Status) {
              this.dialogActive = false;
              this.app.loading = false;
              this.init(); //refresh new results
              this.$toasted
                .show(response.data.Message, {
                  type: "success"
                  // icon: "error_outline"
                })
                .goAway(1500);
            }
          })
          .catch(error => {
            let message = "";
            this.app.loading = false;
            if (error.response.data) {
              if (error.response.data.errors) {
                if (error.response.data.errors.Message) {
                  if (
                    error.response.data.errors.Message == "Token is Expired" ||
                    error.response.data.errors.Message == "Token is Invalid"
                  ) {
                    this.$toasted
                      .show(error.response.data.errors.Message, {
                        type: "error"
                        // icon: "error_outline"
                      })
                      .goAway(1500);
                    localStorage.removeItem("auth");
                    this.$router.push("/auth/login");
                    return false;
                  }
                }
                if (error.response.data.errors.email) {
                  message = error.response.data.errors.email[0];
                } else {
                  message = error.response.data.errors;
                }

                this.$toasted
                  .show(message, {
                    type: "error"
                    // icon: "error_outline"
                  })
                  .goAway(1500);
              }
            }
          });
      }
    },
    onCancel() {},
    deleteUser(data) {
      this.dialogActive = true;
      this.userSelectedName = data.first_name;
      this.deleteId = data.id;
    },
    setPages() {
      let numberOfPages = Math.ceil(this.totalUsers / this.perPage);
      for (let index = 1; index <= numberOfPages; index++) {
        this.pages.push(index);
      }
      // debugger;
    },
    paginate() {
      console.log("paginate");
      let page = this.page;
      let perPage = this.perPage;
      let from = page * perPage - perPage;
      let to = page * perPage;
      // return posts.slice(from, to);
      HTTP.get("api/users/getSubUsers", {
        params: {
          from: from,
          to: to,
          user_role_id: [3, 2]
        }
      })
        .then(response => {
          this.users = response.data.Users;
          this.currentCount = this.users.length;
        })
        .catch(error => {
          console.log(error);
        });
    },
    sort: function(s) {
      //if s == current sort, reverse
      // if (s === this.currentSort) {
      //     this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc';
      // }
      // this.currentSort = s;
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
    addUsers() {
      this.imgDataUrl = "";
      this.editStatus = false;
      this.headingTitle = "Add user";
      this.userForm = {
        first_name: "",
        last_name: "",
        email: "",
        phone: "",
        role_id: "",
        title: ""
      };
      this.$v.userForm.$reset();
      this.showDialog = true;
    },
    submit(event) {
      this.$v.userForm.$touch();

      if (this.$v.userForm.$error) return;

      const data = {
        first_name: this.userForm.first_name,
        last_name: this.userForm.last_name,
        email: this.userForm.email,
        phone: this.userForm.phone,
        user_role_id: this.userForm.role_id,
        title: this.userForm.title,
        image: this.imgDataUrl
      };

      this.app.loading = true;

      if (!this.editStatus) {
        //add
        HTTP.post("api/users/addUser", data)
          .then(response => {
            if (response.data.Status) {
              this.$v.userForm.$reset();
              this.userForm = {
                first_name: "",
                last_name: "",
                email: "",
                phone: "",
                role_id: "",
                title: ""
              };

              this.init(); //refresh new results
              console.log(parseInt(this.totalUsers));
              if (parseInt(this.totalUsers) == 10) {
                //we have setpages when first time pagination
                this.totalUsers = 11;
                //this.setPages();
                this.$router.push("/manage-users/team");
              }

              this.app.loading = false;
              this.$toasted
                .show(response.data.Message, {
                  type: "success"
                  // icon: "error_outline"
                })
                .goAway(1500);
              this.showDialog = false;
            }
          })
          .catch(error => {
            // debugger;
            let message = "";
            this.app.loading = false;

            // return Promise.reject(error);
            // if (error.response.data) {
            //     if (error.response.data.errors) {
            //         if (error.response.data.errors.Message) {
            //             if (
            //                 error.response.data.errors.Message == "Token is Expired" ||
            //                 error.response.data.errors.Message == "Token is Invalid"
            //             ) {
            //                 this.$toasted
            //                     .show(error.response.data.errors.Message, {
            //                         type: "error"
            //                         // icon: "error_outline"
            //                     })
            //                     .goAway(1500);
            //                 localStorage.removeItem("auth");
            //                 this.$router.push("/auth/login");
            //                 return false;
            //             }
            //         }
            //         if (error.response.data.errors.email) {
            //             message = error.response.data.errors.email[0];
            //         } else {
            //             message = error.response.data.errors;
            //         }

            //         this.$toasted
            //             .show(message, {
            //                 type: "error"
            //                 // icon: "error_outline"
            //             })
            //             .goAway(1500);
            //     }
            // }
          });
      } else {
        HTTP.post("api/users/updateSubUsers", data)
          .then(response => {
            if (response.data.Status) {
              this.app.loading = false;
              this.init(); //refresh new results
              this.$toasted
                .show(response.data.Message, {
                  type: "success"
                  // icon: "error_outline"
                })
                .goAway(1500);
            }
            this.showDialog = false;
          })
          .catch(error => {
            let message = "";
            this.app.loading = false;
            if (error.response.data) {
              if (error.response.data.errors) {
                if (error.response.data.errors.email) {
                  message = error.response.data.errors.email[0];
                } else {
                  message = error.response.data.errors;
                }

                this.$toasted
                  .show(message, {
                    type: "error"
                    // icon: "error_outline"
                  })
                  .goAway(1500);

                if (error.response.data.errors.Message) {
                  if (
                    error.response.data.errors.Message == "Token is Expired" ||
                    error.response.data.errors.Message == "Token is Invalid"
                  ) {
                    localStorage.removeItem("auth");
                    this.$router.push("/auth/login");
                  }
                }
              }
            }
          });
      }
    }
  },
  created() {
    this.getUsers();
    this.setPages();
  },
  watch: {
    page(newVal, oldVal) {
      console.log(
        "The old value of length was: " +
          oldVal +
          "\nThe new value of length is: " +
          newVal
      );
    }
  },
  computed: {
    changePage() {
      console.log("computed execution---");
      this.paginate();
      return this.page;
    },
    sortedCats: function() {
      return this.cats
        .sort((a, b) => {
          // console.log("sort--------"+JSON.stringify(a) + "next compare" + JSON.stringify(b));
          let modifier = 1;
          if (this.currentSortDir === "desc") modifier = -1;
          if (a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
          if (a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
          return 0;
        })
        .filter((row, index) => {
          // console.log("filter--------"+JSON.stringify(row));
          let start = (this.currentPage - 1) * this.pageSize;
          let end = this.currentPage * this.pageSize;
          if (index >= start && index < end) return true;
        });
    }
  }
};
</script>

<style lang="scss" scoped>
.md-dialog {
  max-width: 700px;
}

.empty-div {
  max-height: 340px;

  .alt-img {
    max-width: 430px;
  }
}

.md-icon-button:not(.md-just-icon) {
  .md-button-content {
    i:not(.md-tab-icon) {
      font-size: 22px !important;
    }
  }
}
</style>
