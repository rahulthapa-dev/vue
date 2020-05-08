<template>
<div class="wrapper">
    <div class="section text-center">
        <h4 slot="title" class="card-title text-center text-primary">
            Reset Password
        </h4>
        <p slot="description" class="description text-center sub-title">
            Lorem ipsum dolor sit amet, consectetur adipising elit.
        </p>
        <div class="md-card container-auth">
            <div class="md-layout">
                <div class="md-layout-item md-small-size-100 md-xsmall-size-100 mx-auto">
                    <div class="text-left">
                        <form @submit.prevent="submit">
                            <md-field slot="inputs" :class="app.getValidationClass($v,'password')">
                                <label>New Password</label>
                                <md-input v-model="password" type="password" />
                                <div v-if="!$v.password.required" class="md-error">Password is required</div>
                                <div v-if="!$v.password.minLength" class="md-error">Password must be at least 6 characters
                                </div>
                            </md-field>
                            <md-field slot="inputs" :class="app.getValidationClass($v,'confirmPassword')">
                                <label>Confirm Password</label>
                                <md-input v-model="confirmPassword" type="password" />
                                <div v-if="!$v.confirmPassword.required" class="md-error">Confirm password is required</div>
                                <div v-else-if="!$v.confirmPassword.sameAsPassword" class="md-error">Passwords must match</div>
                            </md-field>
                            <md-button type="submit" class="md-primary w-100 button-primary-custom mb-3">
                               <LoadingButtonLoader :typeEnable="app.checkType('simpleLoader')" v-bind:enable="app.loading" class="mr-1" /> Reset
                            </md-button>
                        </form>
                    </div>
                </div>
            </div>
        </div>
        <div class="container-bottom mx-auto">
            <div class="text-primary text-center">
                <span class="icon-arrow mr-2 position-relative">
                    <md-icon>navigate_next</md-icon>
                    <md-icon>navigate_next</md-icon>
                </span> <span>Go back to <router-link to="/auth/login" exact>Login</router-link></span>
            </div>
        </div>
    </div>
</div>
</template>

<script>
import {HTTP} from '../../httpCommon';
import {
    validationMixin
} from 'vuelidate';
import {
    required,
    minLength,
    sameAs
} from "vuelidate/lib/validators";

import LoadingBar from "../../components/LoadingBar";
import LoadingButtonLoader from "../../components/LoadingButtonLoader";
export default {
    name: "Reset",
    props: ["app"],
    mixins: [validationMixin],
    components: {
        LoadingBar,
        LoadingButtonLoader
    },
    data() {
        return {
            password: "",
            confirmPassword: ""
        };
    },
    validations: {
        password: {
            required,
            minLength: minLength(6)
        },
        confirmPassword: {
            required,
            sameAsPassword: sameAs("password")
        }
    },
    mounted() {
        this.init(); // get plans at subscription
    },
    methods: {
        init() {

        },
        submit() {
            this.$v.$touch();
            if (this.$v.$error) return;
            this.app.loading = true;
            const data = {
                password: this.password,
                customerToken: this.$route.params.id
            }

            HTTP
                .post("api/auth/reset", data)
                .then(response => {
                    if (response.data.Status) {
                        this.app.loading = false;
                        this.$toasted
                            .show(response.data.Message, {
                                type: "success"
                            })
                            .goAway(1500);

                        this.$router.push("/auth/login");
                        return false;
                    }
                })
                .catch(error => {
                    console.log(error);
                    let message = "";
                    this.app.loading = false;

                    if (error.response.data) {
                        if (error.response.data.errors) {
                            if (error.response.data.errors.email)
                                message = error.response.data.errors.email[0];
                            if (error.response.data.errors.Message)
                                message = error.response.data.errors.Message;
                            else message = error.response.data.errors;
                        }
                    }

                    this.$toasted
                        .show(message, {
                            type: "error"
                            // icon: "error_outline"
                        })
                        .goAway(2500);
                });
        }
    }
};
</script>

<style lang="scss" scoped>

</style>
