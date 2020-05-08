<template>
<div class="wrapper">
    <div class="section text-center">
        <h4 slot="title" class="card-title text-center text-primary">Login</h4>
        <p slot="description" class="description text-center sub-title">Lorem ipsum dolor sit amet, consectetur adipising elit.</p>
        <div class="md-card container-auth md-card-custom">
            <div class="md-layout">
                <div class="md-layout-item md-small-size-100 md-xsmall-size-100 mx-auto">
                    <div class="text-left">
                        <form @submit.prevent="submit">
                            <md-field slot="inputs" :class="app.getValidationClass($v,'email')">
                                <label>E-mail address</label>
                                <md-input v-model="email" type="email" ref='email' />
                                <span v-if="!$v.email.required" class="md-error">Email is required</span>
                                <span v-if="!$v.email.email" class="md-error">Email is Invalid</span>
                            </md-field>
                            <md-field slot="inputs" class="password-eye pr-4" :class="app.getValidationClass($v,'password')">
                                <label>Password</label>
                                <md-input v-model="password" type="password" ref='password' />
                                <div v-if="!$v.password.required" class="md-error">Password is required</div>
                                <div v-if="!$v.password.minLength" class="md-error">Password must be at least 6 characters</div>
                            </md-field>
                            <md-checkbox v-model="checkbox1" class>Remember me</md-checkbox>

                            <md-button type="submit" class="md-primary w-100 button-primary-custom mb-3 mt-3">
                                <LoadingButtonLoader :typeEnable="app.checkType('simpleLoader')" v-bind:enable="app.loading" class="mr-1" />Login
                            </md-button>
                            <div class="text-center">
                                <router-link to="/auth/forget" exact>Forgot password?</router-link>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>

        <div class="container-bottom mx-auto">
            <div class="text-primary text-center">
                Don't have account?
                <router-link to="/auth/register" exact>Register</router-link>
            </div>
        </div>
    </div>
</div>
</template>

<script>
import {HTTP} from '../../httpCommon';
import {
    validationMixin
} from "vuelidate";
import LoadingBar from "../../components/LoadingBar";
import LoadingButtonLoader from "../../components/LoadingButtonLoader";
import {
    required,
    email,
    minLength,
    sameAs
} from "vuelidate/lib/validators";
export default {
    name: "Login",
    mixins: [validationMixin],
    bodyClass: "login-page",
    props: ["app"],
    components: {
        LoadingBar,
        LoadingButtonLoader
    },
    data() {
        return {
            email: "",
            password: "",
            errors: [],
            checkbox1: false
        };
    },
    validations: {
        email: {
            required,
            email
        },
        password: {
            required,
            minLength: minLength(6)
        }
    },
    mounted() {
        // console.log("mounted");
        this.init(); // get plans at subscription

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
        init() {
            console.log('url defined ');
            console.log(process.env.MIX_API_URL);
             //Autofocus
            if (localStorage.getItem("auth")) {
                HTTP
                    .get("api/auth/init")
                    .then(response => {
                        this.user = response.data.User;
                        this.$router.push("/dashboard");
                    })
                    .catch(error => {
                        console.log(error);
                    });
            }
        },
        submit() {
            this.errors = [];
            this.$v.$touch();
            if (this.$v.$error) return;

            const data = {
                email: this.email,
                password: this.password
            };

            this.app.loading = true;

            HTTP
                .post("api/auth/login", data)
                .then(response => {
                    this.app.loading = false;
                    if (response.data.token) {
                        localStorage.setItem("auth", JSON.stringify(response.data));
                        this.$router.push("/dashboard");
                        return false;
                    }
                })
                .catch(error => {
                    //   this.errors.push(error.response.data.error);
                    //   debugger;
                    let message = "";
                    this.app.loading = false;
                    if (error.response.data) {
                        if (error.response.data.errors) {
                            if (error.response.data.errors.Message)
                                message = error.response.data.errors.Message;
                            else message = error.response.data.errors;
                        }
                    }

                    this.$toasted
                        .show(message, {
                            type: 'error',
                        })
                        .goAway(1500);
                });
        }
    }

};
</script>

<style lang="scss" scoped>
.md-checkbox,
.md-radio {
    display: flex;
    margin: 0;
    margin-bottom: 0.5rem;
}
</style>
