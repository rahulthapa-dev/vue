<template>
<div class="wrapper">
    <div class="section text-center">
        <h4 slot="title" class="card-title text-center text-primary">
            Forgot Password
        </h4>
        <p slot="description" class="description text-center sub-title">
            Lorem ipsum dolor sit amet, consectetur adipising elit.
        </p>
        <div class="md-card container-auth">
            <div class="md-layout">
                <div class="md-layout-item md-small-size-100 md-xsmall-size-100 mx-auto">
                    <div class="text-left">
                        <form @submit.prevent="submit">
                            <md-field slot="inputs" :class="app.getValidationClass($v,'email')">
                                <label>E-mail address</label>
                                <md-input v-model="email" type="email" />
                                <span v-if="!$v.email.required" class="md-error">Email is required</span>
                                <span v-if="!$v.email.email" class="md-error">Email is Invalid</span>
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
                </span> <span>Go back to <router-link to="/auth/login" exact>login</router-link></span>
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
    email,
} from "vuelidate/lib/validators";

import LoadingBar from "../../components/LoadingBar";
import LoadingButtonLoader from "../../components/LoadingButtonLoader";

export default {
    name: "Forget",
    props: ["app"],
    mixins: [validationMixin],
    components: {
        LoadingBar,
        LoadingButtonLoader
    },
    data() {
        return {
            email: ""
        };
    },
    validations: {
        email: {
            required,
            email
        },
    },
    mounted() {
        this.init();
    },
    methods: {
        init() {},
        submit() {

            this.$v.$touch();
             if (this.$v.$error) return;
            this.app.loading = true;
            const data = {
                email: this.email
            };

            HTTP
                .post("api/auth/forget", data)
                .then(response => {
                    this.app.loading = false;
                    this.$toasted
                        .show(response.data.Message, {
                            type: "success"
                            // icon: "error_outline"
                        })
                        .goAway(2500);
                })
                .catch(error => {
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
