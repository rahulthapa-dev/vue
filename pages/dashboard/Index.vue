<template>
  <div class="wrapper w-100">
    <div class="sidebar">
      <div class="logo pt-5 ml-3">
        <a href="#" class>
          <div class="logo-img">
            <img :src="'../img/logo-white.svg'" alt />
          </div>
        </a>
      </div>
      <div class="sidebar-wrapper">
        <md-list class="nav">
          <router-link to="/dashboard" v-slot="{ href, route, navigate, isActive, isExactActive }">
            <md-list-item
              :class="[isActive && 'router-link-active', isExactActive && 'router-link-exact-active']"
            >
              <md-icon>dashboard</md-icon>
              <a :href="href" @click="navigate">Dashboard</a>
            </md-list-item>
          </router-link>

          <router-link
            to="/transactions"
            v-slot="{ href, route, navigate, isActive, isExactActive }"
          >
            <md-list-item
              :class="[isActive && 'router-link-active', isExactActive && 'router-link-exact-active']"
            >
              <md-icon>person</md-icon>
              <a :href="href" @click="navigate">Transactions</a>
            </md-list-item>
            <!-- <p class="font-weight-medium">Transactions</p> -->
          </router-link>

          <router-link
            to="/steps-templates/"
            v-slot="{ href, route, navigate, isActive, isExactActive }"
          >
            <md-list-item
              :class="[isActive && 'router-link-active', isExactActive && 'router-link-exact-active']"
            >
              <md-icon>content_paste</md-icon>
              <p class="font-weight-medium">Steps & Templates</p>
              <md-list>
                <router-link
                  to="/steps-templates/steps"
                  v-slot="{ href, route, navigate, isActive, isExactActive }"
                >
                  <md-list-item
                    :class="[isActive && 'router-link-active', isExactActive && 'router-link-exact-active']"
                  >
                    <a :href="href" @click="navigate">Steps</a>
                    <!-- <router-link to="/steps-templates/steps" exact>Steps</router-link> -->
                  </md-list-item>
                </router-link>
                <router-link
                  to="/steps-templates/templates"
                  v-slot="{ href, route, navigate, isActive, isExactActive }"
                >
                  <md-list-item
                    :class="[isActive && 'router-link-active', isExactActive && 'router-link-exact-active']"
                  >
                    <!-- <router-link to="/steps-templates/templates" exact>Templates</router-link> -->
                    <a :href="href" @click="navigate">Templates</a>
                  </md-list-item>
                </router-link>
              </md-list>
            </md-list-item>
          </router-link>

          <router-link
            to="/manage-users/"
            v-slot="{ href, route, navigate, isActive, isExactActive }"
          >
            <md-list-item
              :class="[isActive && 'router-link-active', isExactActive && 'router-link-exact-active']"
            >
              <md-icon>people</md-icon>
              <p class="font-weight-medium">Manage Users</p>
              <md-list>
                <router-link
                  to="/manage-users/team"
                  v-slot="{ href, route, navigate, isActive, isExactActive }"
                >
                  <md-list-item
                    :class="[isActive && 'router-link-active', isExactActive && 'router-link-exact-active']"
                  >
                    <!-- <router-link to="/manage-users/team" exact>Team</router-link> -->
                    <a :href="href" @click="navigate">Team</a>
                  </md-list-item>
                </router-link>

                <router-link
                  to="/manage-users/vendors"
                  v-slot="{ href, route, navigate, isActive, isExactActive }"
                >
                  <md-list-item
                    :class="[isActive && 'router-link-active', isExactActive && 'router-link-exact-active']"
                  >
                    <!-- <router-link to="/manage-users/team" exact>Team</router-link> -->
                    <a :href="href" @click="navigate">Vendors</a>
                  </md-list-item>
                </router-link>
              </md-list>
            </md-list-item>
          </router-link>
          <!-- 
          <router-link
            to="/transaction"
            v-slot="{ href, route, navigate, isActive, isExactActive }"
          >
            <md-list-item
              :class="[isActive && 'router-link-active', isExactActive && 'router-link-exact-active']"
            >
              <md-icon>person</md-icon>
              <a :href="href" @click="navigate">Single Transaction</a>
          </md-list-item>-->
          <!-- <p class="font-weight-medium">Transactions</p>-->
          <!-- </router-link> -->

          <md-list-item>
            <router-link to="/company-setting" exact>
              <md-icon>bubble_chart</md-icon>
              <p class="font-weight-medium">Company settings</p>
            </router-link>
          </md-list-item>
        </md-list>

        <div class="active-pro">
          <div class="profile-sidebar">
            <div class="d-flex justify-content-between">
              <div class="d-flex justify-content-start align-center">
                <md-avatar>
                  <img :src="profileImage" alt="Avatar" class="avatar-sidebar" />
                </md-avatar>
                <p
                  class="font-weight-medium md-title text-primary ml-2"
                >Hey, {{userInfo.first_name}}!</p>
              </div>
              <span v-on:click="away" class="d-flex align-center cursor-pointer">
                <md-icon v-show="isProfile">more_vert</md-icon>
                <md-icon v-show="!isProfile">close</md-icon>
              </span>
            </div>
            <div v-show="!isProfile" class="medium-block">
              <md-list>
                <md-list-item>
                  <router-link to="/profile" exact>My Profile</router-link>
                </md-list-item>
                <md-list-item>
                  <router-link to="/subscription-profile" exact>Subscription</router-link>
                </md-list-item>
                <md-list-item @click="logout()">
                  <a class="cursor-pointer">Logout</a>
                </md-list-item>
              </md-list>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="main-panel">
      <div class="display-sm">
        <md-toolbar md-elevation="0" class="md-transparent mb-0">
          <div class="md-toolbar-row mx-0">
            <div class="md-toolbar-section-start">
              <img :src="'../img/logo.svg'" style="width: 150px;" />
            </div>
            <div class="md-toolbar-section-end">
              <md-button
                class="md-just-icon md-simple md-toolbar-toggle"
                v-bind:class="{ toggled: isActive }"
                v-on-clickaway="responsiveAway"
                @click="responsiveMenu"
              >
                <span class="icon-bar text-primary"></span>
                <span class="icon-bar text-primary"></span>
                <span class="icon-bar text-primary"></span>
              </md-button>
            </div>
          </div>
        </md-toolbar>
      </div>
      <transition name="fade" mode="out-in">
        <div class="px-5 pt-5 content-wrapper">
          <router-view :app="this.app" />
        </div>
      </transition>
    </div>
  </div>
</template>

<script>
import { HTTP } from "../../httpCommon";
import toast from "../../services/toast";
import { mixin as clickaway } from "vue-clickaway";
// import ValidationResponseHandler from '../../mixins/ValidationResponseHandler';
export default {
  name: "DashboardLayout",
  props: ["app"],
  mixins: [clickaway],
  components: {},
  data() {
    return {
      isProfile: true,
      isCompany: true,
      profileImage: "",
      userInfo: "",
      isActive: false,
      hasError: false,
      profileButton: false
    };
  },
  created() {
    //initiate interceptor
    this.enableInterceptor();
    this.$root.$on("profileImagEvent", obj => {
      // console.log(obj);
      this.profileImage = obj.profileImage;
    });
  },
  mounted() {
    // console.log("Dashboard ----Layouit");
    // console.log(this.$toasted);
    this.init();
  },
  methods: {
    away: function() {
      console.log("clicked away");
      this.isProfile = !this.isProfile;
      this.profileButton = true;
      // this.isActive = false;
    },
    responsiveAway: function() {
      console.log("responsive clicked away");
      // this.isActive = false;
      if (!this.profileButton) {
        if (this.isActive) {
          this.isActive = !this.isActive;
          let testarray = document.getElementsByClassName("wrapper");
          for (var i = 0; i < testarray.length; i++) {
            testarray[i].classList.remove("nav-open");
          }
        }
      } else {
        this.profileButton = false;
      }
    },
    init() {
      HTTP.get("api/users/usersInfo")
        .then(response => {
          this.profileImage = response.data.Image;
          this.userInfo = response.data.User;
        })
        .catch(error => {
          // console.log(error);
          // if (error.response.data.errors.Message) {
          //     if (error.response.data.errors.Message == "Token is Expired" || error.response.data.errors.Message == "Token is Invalid") {
          //         this.$toasted
          //             .show(error.response.data.errors.Message, {
          //                 type: "success"
          //                 // icon: "error_outline"
          //             })
          //             .goAway(1500);
          //         localStorage.removeItem("auth");
          //         this.$router.push("/auth/login");
          //         return false;
          //     }
          // }
        });
      // this.mainLoader = true;
    },
    responsiveMenu(e) {
      this.isActive = !this.isActive;
      var testarray = document.getElementsByClassName("wrapper");

      if (this.isActive) {
        for (var i = 0; i < testarray.length; i++) {
          testarray[i].className += " nav-open";
        }
      } else {
        for (var i = 0; i < testarray.length; i++) {
          testarray[i].classList.remove("nav-open");
        }
      }
    },
    enableInterceptor() {
      this.axiosInterceptor = HTTP.interceptors.request.use(
        config => {
          // console.log(config);
          // this.mainLoader = true;
          if (localStorage.getItem("auth")) {
            let authtoken = JSON.parse(localStorage.getItem("auth"));
            config.headers["Authorization"] = `Bearer ${authtoken.token}`;
          }

          return config;
        },
        error => {
          // this.mainLoader = false;
          return Promise.reject(error);
        }
      );

      // Add a response interceptor
      HTTP.interceptors.response.use(
        function(response) {
          // Any status code that lie within the range of 2xx cause this function to trigger
          // Do something with response data
          return response;

          //Store Current User
          // if (response.data.hasOwnProperty('currentUser')) {
          //     vm.setCurrentUser(response.data.currentUser)
          // }

          //Toast Message
          // if (response.data.hasOwnProperty('message')) {
          //     Toast.create.positive({
          //         html: response.data.message,
          //         icon: 'fa-check-circle',
          //         timeout: 2500,
          //     })
          // }

          // return Promise.resolve(response)
        },
        function(error) {
          // this.app.loading = false;
          let message = "";
          //Setup Error Message
          if (typeof error !== "undefined") {
            // debugger;
            if (error.hasOwnProperty("message")) {
              //toast.error(error.message);
            }
            if (
              typeof error.response !== "undefined" &&
              error.hasOwnProperty("response") &&
              error.response.hasOwnProperty("data")
            ) {
              if (error.response.data.hasOwnProperty("errors")) {
                if (error.response.data.errors.Message == "Token is Expired") {
                  // debugger;
                  localStorage.removeItem("auth");
                  this.$router.push("/auth/login");
                  return false;
                }

                if (error.response.data.errors.hasOwnProperty("phone")) {
                  message = error.response.data.errors.phone[0];
                }

                if (error.response.data.errors.hasOwnProperty("email")) {
                  message = error.response.data.errors.email[0];
                }
              } else {
                if (error.response.hasOwnProperty("data")) {
                  if (error.response.data.hasOwnProperty("error")) {
                    message = error.response.data.error.errorInfo;
                    if (!message) {
                      message = error.response.data.error;
                    }
                  }

                  if (
                    error.response.data.hasOwnProperty("message") &&
                    error.response.data.message.length > 0
                  ) {
                    message = error.response.data.message;
                  }
                }
              }
            }

            toast.error(message);
          }
        }
      );
    },
    logout() {
      localStorage.removeItem("auth");
      this.$router.push("/auth/login");
    }
  }
};
</script>

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.1s;
}

.fade-enter,
.fade-leave-to
/* .fade-leave-active in <2.1.8 */

 {
  opacity: 0;
}
</style>
