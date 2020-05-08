/* eslint-disable vue/multiline-html-element-content-newline */
<template>
  <div class="wrapper">
    <div class="section text-center">
      <h4 slot="title" class="card-title text-center text-primary">Company Register</h4>
      <p
        slot="description"
        class="description text-center sub-title"
      >Lorem ipsum dolor sit amet, consectetur adipising elit.</p>
      <div class="md-card container-auth">
        <div class="md-layout">
          <div class="md-layout-item md-small-size-100 md-xsmall-size-100 mx-auto">
            <form @submit.prevent="submit" class="register-form">
              <div class="text-left">
                <div class="md-layout">
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs" :class="app.getValidationClass($v,'firstname')">
                      <label>First name</label>
                      <!--eslint-disable-next-line vue/html-self-closing-->
                      <md-input v-model="firstname"></md-input>
                      <div v-if="!$v.firstname.required" class="md-error">First name is required</div>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field slot="inputs" :class="app.getValidationClass($v,'lastname')">
                      <label>Last name</label>
                      <!--eslint-disable-next-line vue/html-self-closing-->
                      <md-input v-model="lastname"></md-input>
                      <div v-if="!$v.lastname.required" class="md-error">Last name is required</div>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-100">
                    <md-field
                      slot="inputs"
                      :class="app.getValidationClass($v,'email')"
                      class="multi-error"
                    >
                      <label>E-mail address</label>
                      <!--eslint-disable-next-line vue/html-self-closing-->
                      <md-input v-model="email"></md-input>
                      <div v-if="!$v.email.required" class="md-error">Email is required</div>
                      <div v-if="!$v.email.email" class="md-error">Email is Invalid</div>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      class="password-eye pr-4 multi-error"
                      :class="app.getValidationClass($v,'password')"
                    >
                      <label>Password</label>
                      <!--eslint-disable-next-line vue/html-self-closing-->
                      <md-input v-model="password" type="password"></md-input>
                      <div v-if="!$v.password.required" class="md-error">Password is required</div>
                      <div
                        v-if="!$v.password.minLength"
                        class="md-error"
                      >Password must be at least 6 characters</div>
                    </md-field>
                  </div>
                  <div class="md-layout-item md-small-size-100 md-size-50">
                    <md-field
                      slot="inputs"
                      class="password-eye pr-4 multi-error"
                      :class="app.getValidationClass($v,'confirmpassword')"
                    >
                      <label>Confirm Password</label>
                      <md-input v-model="confirmpassword" type="password" />
                      <div
                        v-if="!$v.confirmpassword.required"
                        class="md-error"
                      >Confirm password is required</div>
                      <div
                        v-else-if="!$v.confirmpassword.sameAsPassword"
                        class="md-error"
                      >Passwords must match</div>
                    </md-field>
                  </div>
                </div>
                <md-checkbox v-model="checkbox1" class="remember">
                  By signing up you agree with your our
                  <a
                    href
                    class="text-underline terms-link"
                  >Terms and Conditions</a>
                </md-checkbox>
                <md-button type="submit" class="md-primary w-100 button-primary-custom mb-3 mt-3">
                  <LoadingButtonLoader
                    :typeEnable="app.checkType('simpleLoader')"
                    v-bind:enable="app.loading"
                    class="mr-1"
                  />Register
                </md-button>
              </div>
            </form>
          </div>
          <!--md-layout-item md-small-size-100 md-xsmall-size-100 mx-auto-->
        </div>
      </div>
      <div class="container-bottom mx-auto">
        <div class="text-primary text-center">
          Already have an account?
          <router-link to="/auth/login" exact>Login</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { HTTP } from "../../httpCommon";
import LoadingBar from "../../components/LoadingBar";
import LoadingButtonLoader from "../../components/LoadingButtonLoader";
import { required, email, minLength, sameAs } from "vuelidate/lib/validators";
export default {
  name: "Register",
  props: ["app"],
  components: {
    LoadingBar,
    LoadingButtonLoader
  },
  bodyClass: "register-page",
  data() {
    return {
      firstname: "",
      lastname: "",
      email: "",
      password: "",
      confirmpassword: "",
      checkbox1: false,
      status: false
    };
  },
  validations: {
    firstname: {
      required
    },
    lastname: {
      required
    },
    email: {
      required,
      email
    },
    password: {
      required,
      minLength: minLength(6)
    },
    confirmpassword: {
      required,
      sameAsPassword: sameAs("password")
    }
  },
  methods: {
    getValidationClass(fieldName) {
      const field = this.$v[fieldName];

      if (field) {
        return {
          "md-invalid": field.$invalid && field.$dirty
        };
      }
    },
    submit() {
      this.$v.$touch();
      if (this.$v.$error) return;

      // to form submit after this
      //   alert("Form submitted");
      const data = {
        firstname: this.firstname,
        lastname: this.lastname,
        email: this.email,
        password: this.password
      };

      this.app.loading = true;

      HTTP.post("api/register", data)
        .then(response => {
          if (response.data.Status) {
            this.firstname = "";
            this.lastname = "";
            this.email = "";
            this.password = "";
            this.confirmpassword = "";
            this.checkbox1 = false;
            this.status = false;

            this.$v.$reset(); //Reset form
            this.app.loading = false;
            this.$toasted
              .show(response.data.Message, {
                type: "success"
                // icon: "error_outline"
              })
              .goAway(2500);
          }
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
            }
          }
        });
    }
  }
};
</script>

<style lang="scss" scoped>
</style>
