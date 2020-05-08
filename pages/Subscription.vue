<template>
<div class="section subscription py-0 auth-bg">
    <md-dialog-confirm :md-active.sync="dialogActive" md-title="Confirmation?" md-content="Are you sure want to change plan?." md-confirm-text="Agree" md-cancel-text="Disagree" @md-cancel="onCancel" @md-confirm="onConfirm" />
    <div class="full-page-loader">
        <LoadingBar :typeEnable="app.checkType('simpleLoader')" v-bind:enable="app.loading" />
    </div>
    <!---Choose  plan selection--->
    <div v-if="type === 'choose'" class="container">

        <div class="wrapper">
            <div class="md-layout md-gutter md-alignment-center h-100">
                <div class="md-layout-item md-medium-size-80 md-small-size-85 md-xsmall-size-90">
                    <div class="md-card-custom modal-style">
                        <div class="modal-title d-block py-3 px-4 bg-light position-relative">
                            <h4 class="my-0 card-title text-left text-primary">Choose plan</h4>
                            <md-button v-if="app.authStatus" @click="cancel" class="md-icon-button position-absolute">
                                <md-icon class="text-primary">close</md-icon>
                            </md-button>
                        </div>
                        <div class="modal-content p-4">
                            <h4 class="card-title text-center text-primary">Find the perfect plan for you</h4>
                            <p class="description text-center sub-title">Free 7-day trial with each plan</p>
                            <div class="md-success" md-alignment="centered">
                                <div v-if="isMonthly" id="monthly-plan">
                                    <div class="button-plan d-flex">
                                        <button @click="isMonthlyPlan" class="button-active">Monthly</button>
                                        <button @click="isAnnualPlan" class="">Annual</button>
                                    </div>
                                    <div v-if="monthlyPlans.length" class="md-layout md-gutter subscription-carousal-outer">
                                        <carousel :autoplay="false" :dots="true" :nav="false" :mouseDrag="true" :responsive="{0:{items:1,nav:false},600:{items:3,nav:false}}">
                                            <div v-for="(plan, index) in monthlyPlans" v-bind:key="plan.invoice_name" class="d-block w-100">
                                                <div class="md-card-custom border-box pricing-box text-center">
                                                    <div class="pricing-head-content py-4 px-3">
                                                        <span class="d-block text-uppercase pb-1">{{ plan.invoice_name }}</span>
                                                        <h3 class="md-headline d-block text-primary pb-1 my-0">$ {{ plan.price }}</h3>
                                                        <p class="md-body-1">{{ plan.description }}</p>
                                                    </div>
                                                    <div class="pricing-body-content py-3 px-3">
                                                        <md-list class="md-listing">
                                                            <md-list-item v-if="plan.period" class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                                <span class="md-list-item-text">{{ plan.period }} Month</span>
                                                            </md-list-item>
                                                            <md-list-item v-if="plan.trial_period" class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                                <span class="md-list-item-text">
                                                                    {{ plan.trial_period }}
                                                                    {{ plan.trial_period_unit }} Trial
                                                                </span>
                                                            </md-list-item>
                                                            <md-list-item v-else class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute text-disabled">close</md-icon>
                                                                <span class="md-list-item-text text-disabled">No trial</span>
                                                            </md-list-item>

                                                            <md-list-item v-if="plan.meta_data" class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                                <span v-if="plan.meta_data.users" class="md-list-item-text">{{ plan.meta_data.users }} Allowed Users</span>
                                                            </md-list-item>

                                                        </md-list>
                                                        <button class="md-button md-primary md-theme-default button-custom-regular button-outline" @click="payment('month',index)">Buy now</button>
                                                    </div>
                                                </div>
                                            </div>
                                        </carousel>
                                    </div>
                                    <div v-if="dailyPlans.length" class="md-layout md-gutter subscription-carousal-outer">
                                        <carousel :autoplay="false" :dots="true" :nav="false" :mouseDrag="true" :responsive="{0:{items:1,nav:false},600:{items:3,nav:false}}">
                                            <div v-for="(plan, index) in dailyPlans" v-bind:key="plan.invoice_name" class="d-block w-100">
                                                <div class="md-card-custom border-box pricing-box text-center">
                                                    <div class="pricing-head-content py-4 px-3">
                                                        <span class="d-block text-uppercase pb-1">{{ plan.invoice_name }}</span>
                                                        <h3 class="md-headline d-block text-primary pb-1 my-0">$ {{ plan.price }}</h3>
                                                        <p class="md-body-1">{{ plan.description }}</p>
                                                    </div>
                                                    <div class="pricing-body-content py-3 px-3">
                                                        <md-list class="md-listing">
                                                            <md-list-item v-if="plan.period" class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                                <span class="md-list-item-text">{{ plan.period }} day</span>
                                                            </md-list-item>
                                                            <md-list-item v-if="plan.trial_period" class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                                <span class="md-list-item-text">
                                                                    {{ plan.trial_period }}
                                                                    {{ plan.trial_period_unit }} Trial
                                                                </span>
                                                            </md-list-item>
                                                            <md-list-item v-else class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute text-disabled">close</md-icon>
                                                                <span class="md-list-item-text text-disabled">No trial</span>
                                                            </md-list-item>

                                                            <md-list-item v-if="plan.meta_data" class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                                <span v-if="plan.meta_data.users" class="md-list-item-text">{{ plan.meta_data.users }} Allowed Users</span>
                                                            </md-list-item>

                                                        </md-list>
                                                        <button class="md-button md-primary md-theme-default button-custom-regular button-outline" @click="payment('daily',index)">Buy now</button>
                                                    </div>
                                                </div>
                                            </div>
                                        </carousel>
                                    </div>
                                    <div v-else>
                                        <div class="text-center p-4">
                                            <img :src="'../img/no-data.svg'" style="max-width:400px;" class="mb-2" />
                                            <h5 class="text-primary font-weight-bold card-title">Don't have any monthly plans yet</h5>
                                        </div>
                                    </div>
                                </div>
                                <div v-if="isAnnual" id="annual-plan">
                                    <div class="button-plan d-flex">
                                        <button @click="isMonthlyPlan">Monthly</button>
                                        <button @click="isAnnualPlan" class="button-active">Annual</button>
                                    </div>
                                    <div v-if="yearlyPlans.length" class="md-layout md-gutter justify-content-center subscription-carousal-outer">
                                        <carousel :autoplay="false" :dots="true" :nav="false" :mouseDrag="true" :responsive="{0:{items:1,nav:false},600:{items:3,nav:false}}">
                                            <div v-for="(plan, index) in yearlyPlans" v-bind:key="plan.invoice_name" class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mb-3">
                                                <div class="md-card-custom border-box pricing-box">
                                                    <div class="pricing-head-content py-4 px-3 text-center">
                                                        <span v-if="plan.invoice_name" class="d-block text-uppercase pb-1">{{ plan.invoice_name }}</span>
                                                        <h3 v-if="plan.price" class="md-headline d-block text-primary pb-1 my-0">$ {{ plan.price }}</h3>
                                                        <p v-if="plan.description" class="md-body-1">{{ plan.description }}</p>
                                                    </div>
                                                    <div class="pricing-body-content py-3 px-3">
                                                        <md-list class="md-listing">
                                                            <md-list-item class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                                <span v-if="plan.period" class="md-list-item-text">{{ plan.period }} Month</span>
                                                            </md-list-item>
                                                            <md-list-item v-if="plan.trial_period" class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                                <span class="md-list-item-text">{{ plan.trial_period }} {{ plan.trial_period_unit }} Trial</span>
                                                            </md-list-item>
                                                            <md-list-item v-else>
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute text-disabled">close</md-icon>
                                                                <span class="md-list-item-text">No trial</span>
                                                            </md-list-item>

                                                            <md-list-item class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                                <span v-if="plan.meta_data" class="md-list-item-text">{{ plan.meta_data.users }} Allowed Users</span>
                                                            </md-list-item>
                                                        </md-list>
                                                        <button class="md-button md-primary md-theme-default button-custom-regular button-outline" @click="payment('month',index)">Buy now</button>
                                                    </div>
                                                </div>
                                            </div>
                                        </carousel>
                                    </div>
                                    <div v-else>
                                        <div class="text-center p-4">
                                            <img :src="'../img/no-data-text.svg'" style="max-width:400px;" class="mb-2" />
                                            <h5 class="text-primary font-weight-bold card-title">Don't have any Annual plans yet</h5>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!--close choose plan ending--->
    <!---Card Payment--->
    <div v-else-if="type === 'pay'" class="container">
        <div class="wrapper">
            <div class="md-layout md-gutter md-alignment-center h-100">
                <div class="md-layout-item md-medium-size-80 md-small-size-85 md-xsmall-size-90">
                    <div class="md-card-custom modal-style">
                        <div class="modal-title d-block py-3 px-4 bg-light">
                            <h4 class="my-0 card-title text-left text-primary">Make payment</h4>
                        </div>
                        <div class="modal-content p-4">
                            <div class="md-layout md-gutter">
                                <div class="md-layout-item md-medium-size-50 md-small-size-100 md-xsmall-size-100 px-1 mb-2">
                                    <div class="make-payment-box pt-3 pr-4 pl-2">
                                        <h4 class="md-title text-left text-primary text-uppercase">Card Info</h4>
                                        <!-- Pass options to card component -->
                                        <CardComponent ref="card" class="fieldset field card-custom" :styles="styles" :classes="classes" @ready="onReady" @change="onChange">
                                            <!-- ^ Add ready and change listener to card component -->

                                            <!-- Indivudual fields mode -->
                                            <!-- Attach listeners to detect focus, blur on each field -->
                                            <div class="md-layout">
                                                <div class="md-layout-item md-medium-size-100 md-small-size-100 md-xsmall-size-100">
                                                    <CardNumber class="ex3-input" :placeholder="'Card Number'" @focus="onFocus" @blur="onBlur" />
                                                </div>
                                            </div>
                                            <div class="md-layout">
                                                <div class="md-layout-item md-medium-size-50 md-small-size-100 md-xsmall-size-100">
                                                    <CardExpiry class="ex3-input" :placeholder="'Expiry Date'" @focus="onFocus" @blur="onBlur" />
                                                </div>
                                                <div class="md-layout-item md-medium-size-50 md-small-size-100 md-xsmall-size-100 position-relative cvv-div">
                                                    <CardCvv class="ex3-input" :placeholder="'CVV'" @focus="onFocus" @blur="onBlur" />
                                                    <md-icon class="text-primary position-absolute">info_outline</md-icon>
                                                </div>
                                            </div>
                                            <div v-if="errors" class="error card-error md-error">
                                                <!-- {{ errors }} -->
                                                {{ errorMessage }}
                                            </div>
                                        </CardComponent>
                                    </div>
                                </div>
                                <div class="md-layout-item md-medium-size-50 md-small-size-100 md-xsmall-size-100 px-1 mb-2">
                                    <div class="make-payment-box pt-3">
                                        <h4 class="md-title text-left text-primary text-uppercase pl-4 pr-2">Plan Info</h4>
                                        <div class="md-layout pl-4 pr-2">
                                            <div class="md-layout-item md-small-size-50 md-size-50">
                                                <h4 v-if="singlePlan.invoice_name" class="md-title text-left text-primary">{{ singlePlan.invoice_name }}</h4>
                                            </div>
                                            <div v-if="singlePlan.price" class="md-layout-item md-small-size-50 md-size-50">
                                                <h5 class="md-headline text-left brand-primary font-weight-semi-bold text-right">${{ singlePlan.price }}</h5>
                                            </div>
                                            <div v-if="singlePlan.description" class="md-layout-item md-small-size-100 md-size-100">
                                                <p slot="description" class="description-normal text-description text-left font-weight-regular">{{ singlePlan.description }}.</p>
                                            </div>
                                            <div class="md-layout-item md-small-size-100 md-size-100">
                                                <div class="md-layout md-gutter">
                                                    <div class="md-layout-item md-small-size-33 md-size-33">
                                                        <md-list class="md-listing">
                                                            <md-list-item v-if="singlePlan.period" class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                                <span class="md-list-item-text">{{ singlePlan.period }} {{ singlePlan.period_unit }}</span>
                                                            </md-list-item>
                                                            <md-list-item v-else>
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute text-disabled">close</md-icon>
                                                                <span class="md-list-item-text text-disabled">No period</span>
                                                            </md-list-item>
                                                        </md-list>
                                                    </div>
                                                    <div class="md-layout-item md-small-size-33 md-size-33">
                                                        <md-list class="md-listing">
                                                            <md-list-item v-if="singlePlan.trial_period" class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                                <span class="md-list-item-text">{{ singlePlan.trial_period }} {{ singlePlan.trial_period_unit }}</span>
                                                            </md-list-item>
                                                            <md-list-item v-else>
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute text-disabled">close</md-icon>
                                                                <span class="md-list-item-text text-disabled">No trial period</span>
                                                            </md-list-item>
                                                        </md-list>
                                                    </div>
                                                    <div class="md-layout-item md-small-size-33 md-size-33">
                                                        <md-list class="md-listing">
                                                            <md-list-item v-if="singlePlan.meta_data" class="position-relative">
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                                <span v-if="singlePlan.meta_data.users" class="md-list-item-text">{{ singlePlan.meta_data.users }} users</span>
                                                            </md-list-item>
                                                            <md-list-item v-else>
                                                                <md-icon class="md-icon md-icon-font md-theme-default position-absolute text-disabled">close</md-icon>
                                                                <span class="md-list-item-text text-disabled">No users allowed</span>
                                                            </md-list-item>
                                                        </md-list>
                                                    </div>

                                                </div>
                                            </div>
                                        </div>
                                        <!-- <div class="plan-info-footer py-2 px-4">
                                            <p class="md-subheading text-description font-weight-semi-bold text-uppercase mb-1">wednesday</p>
                                            <p class="md-subheading lighter-description font-weight-regular text-uppercase mb-0">december 20, 2019 | 11:03 pm</p>
                                        </div> -->
                                    </div>
                                </div>
                                <div class="md-layout-item md-size-100 md-medium-size-100 md-small-size-100 md-xsmall-size-100 px-0 d-flex">
                                    <button type="submit" class="md-button md-primary md-theme-default button-custom-regular d-flex align-center justify-content-center" @click="tokenize($route.params.id,singlePlan.id,singlePlan.price)">
                                        <LoadingButtonLoader :typeEnable="app.checkType('simpleLoader')" v-bind:enable="loading" class="mr-1 d-inline-block v-align-middle" /> <span>Submit</span></button>

                                    <md-button :md-ripple="false" class="cancel-link lighter-description mt-2" @click="cancel()">Cancel</md-button>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!--close Card payment-->

    <div v-else-if="type === 'thank'" class="container">
        <!-- Thanks confirmation -->
        <div class="md-layout md-gutter md-alignment-center h-100">
            <div class="md-layout-item md-size-60 md-medium-size-80 md-small-size-85 md-xsmall-size-90">
                <div class="md-card-custom modal-style">
                    <div class="modal-content p-4">
                        <div class="md-layout md-gutter">
                            <div class="md-layout-item md-medium-size-100 md-small-size-100 md-xsmall-size-100 px-1 mb-2">
                                <div class="pt-3 px-4 text-center">
                                    <img :src="'../img/smile.svg'" style="width: 55px;" />
                                    <h5 class="md-headline text-center text-primary font-weight-medium mb-4">Thank you!</h5>
                                    <p class="description description-confirm font-weight-medium">
                                        Your payment
                                        <span v-if="transaction.Plan_amount" class="text-primary">$ {{ transaction.Plan_amount }}</span> has been processed successfully.
                                    </p>
                                    <p class="description description-confirm font-weight-medium">
                                        For your reference, your payment transaction id is:
                                        <span v-if="transaction.Id" class="brand-primary">{{ transaction.Id }}</span>
                                    </p>
                                    <router-link to="/auth/login" exact>Go to login page</router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- close thanks confirmation -->
</div>
</template>

<script>
import {
    HTTP
} from '../httpCommon';
import RingLoader from "vue-spinner/src/RingLoader.vue";
import carousel from "v-owl-carousel";
import LoadingBar from "../components/LoadingBar";
import LoadingButtonLoader from "../components/LoadingButtonLoader";
import {
    CardComponent,
    CardNumber,
    CardExpiry,
    CardCvv
} from "@chargebee/chargebee-js-vue-wrapper";
// ​​Initializing a Chargebee instance
export default {
    name: "Subscription",
    props: ["app"],
    components: {
        CardComponent,
        CardNumber,
        CardExpiry,
        CardCvv,
        LoadingBar,
        carousel,
        RingLoader,
        LoadingButtonLoader
    },
    data() {
        return {
            isMonthly: true,
            isAnnual: false,
            isDaily: true,
            type: "choose",
            loading: false,
            token: "",
            subscriptionId: "",
            dailyPlans: [],
            monthlyPlans: [],
            yearlyPlans: [],
            singlePlan: "",
            transaction: "",

            // To store validation error messages
            errors: {
                number: null,
                expiry: null,
                cvv: null
            },
            errorMessage: "",

            // Custom classes
            classes: {
                focus: "focus",
                invalid: "invalid",
                empty: "empty",
                complete: "complete"
            },

            // Custom styles for default and invalid field state
            styles: {
                base: {
                    color: "#152E99",
                    fontWeight: "500",
                    fontFamily: "'sofia-pro', sans-serif;",
                    fontSize: "16px",
                    fontSmoothing: "antialiased",

                    ":focus": {
                        color: "#152E99"
                    },

                    "::placeholder": {
                        color: "#B2B2DB"
                    },

                    ":focus::placeholder": {
                        color: "#B2B2DB"
                    }
                },

                invalid: {
                    color: "#e41029",

                    ":focus": {
                        color: "#e44d5f"
                    },
                    "::placeholder": {
                        color: "#FFCCA5"
                    }
                }
            },
            dialogActive: false
        };
    },
    beforeCreate() {
        // result = JSON.parse(localStorage.getItem("Data"));
        // if (result) {
        //   if (result.Payment) this.$router.push("/auth/login");
        // }
        // console.log("Nothing gets called before me!");
    },
    /* In the created hook, you will be able to access reactive data and events are active. Templates and Virtual DOM have not yet been mounted or rendered. */
    created() {
        this.app.loading = true;
        console.log(process.env.MIX_SITE);
        console.log(process.env.MIX_PUBLISHABLE_KEY);
        window.Chargebee.init({
            site: process.env.MIX_SITE,
            publishableKey: process.env.MIX_PUBLISHABLE_KEY
        });

        if (localStorage.getItem("auth")) {
            this.enableInterceptor();
        }
    },
    beforeMount() {
        // console.log("this.$el doesn't exist yet, but it will soon!");
    },
    mounted() {
        // console.log("mounted"); // I'm text inside the component.
        this.init(); // get plans at subscription
    },
    beforeUpdate() {
        // console.log("Logs the counter value every second, before the DOM updates"); // Logs the counter value every second, before the DOM updates.
    },
    updated() {
        // Fired every second, should always be true
        // console.log(
        //   "The updated hook runs after data changes on your component and the DOM re-renders. If you need to access the DOM after a property change, here is probably the safest place to do it."
        // );
    },
    destroyed: function () {
        console.log("component destroyed");
    },
    methods: {
        init() {
            if (localStorage.getItem("auth")) {
                this.getPlans();
            } else {
                /**check the client paid status */
                HTTP
                    .get("api/subscription/checkPaidStatus", {
                        params: {
                            customerToken: this.$route.params.id
                        }
                    })
                    .then(response => {
                        if (response.data.Status) {
                            this.getPlans();
                        }
                    })
                    .catch(error => {
                        let message = "";
                        if (error.response.data) {
                            if (error.response.data.errors) {
                                if (error.response.data.errors.PaidStatus)
                                    message = error.response.data.errors.Message;
                                this.$toasted
                                    .show(message, {
                                        type: "error"
                                        // icon: "error_outline"
                                    })
                                    .goAway(2500);
                                this.$router.push("/auth/login");
                                return false;
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
        },
        enableInterceptor() {
            HTTP.interceptors.request.use(
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
                function (response) {
                    // Any status code that lie within the range of 2xx cause this function to trigger
                    // Do something with response data
                    return response;
                },
                function (error) {
                    let message = "";
                    //Setup Error Message
                    if (typeof error !== "undefined") {
                        if (error.hasOwnProperty("message")) {
                            //toast.error(error.message);
                        }
                        if (
                            typeof error.response !== "undefined" &&
                            error.hasOwnProperty("response") &&
                            error.response.hasOwnProperty("data")
                        ) {
                            if (error.response.data.hasOwnProperty("errors")) {
                                if (error.response.data.errors.hasOwnProperty("phone")) {
                                    message = error.response.data.errors.phone[0];
                                }

                                if (error.response.data.errors.hasOwnProperty("email")) {
                                    message = error.response.data.errors.email[0];
                                }
                            } else {
                                if (error.response.hasOwnProperty("data")) {
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
        isMonthlyPlan() {
            this.isMonthly = true;
            this.isAnnual = false;
        },
        isAnnualPlan() {
            this.isMonthly = false;
            this.isAnnual = true;
        },
        getPlans() {
            /**get all plans */
            HTTP
                .get("api/subscription/getplans")
                .then(response => {
                    //   this.user = response.data.User;
                    //   this.loading = false;
                    //   this.initiated = true;
                    if (response.data.Status) {
                        // this.$root.$emit('mainLoaderEvent', {
                        //     loader: false
                        // });
                        this.app.loading = false;
                        this.monthlyPlans = response.data.MonthlyPlans;
                        this.yearlyPlans = response.data.YearlyPlans;
                        this.dailyPlans = response.data.DailyPlans;
                    }
                })
                .catch(error => {
                    let message = "";
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
        },
        payment(paymenttype, index) {
            this.singlePlan = "";
            if (paymenttype == "month") {
                let monthlyPlans = this.monthlyPlans;
                this.singlePlan = monthlyPlans[index];
            }

            if (paymenttype == "year") {
                let yearlyPlans = this.yearlyPlans;
                this.singlePlan = yearlyPlans[index];
            }

            if (paymenttype == "daily") {
                let dailyPlans = this.dailyPlans;
                this.singlePlan = dailyPlans[index];
            }
            if (localStorage.getItem("auth")) {
                this.dialogActive = true;
            } else {
                //   console.log(this.singlePlan);
                this.type = "pay";
            }

        },
        cancel() {
            if (this.app.authStatus) {
                this.$router.push("/subscription-profile");
            } else {
                this.type = "choose";
            }

        },
        // Method for tokenizing card details
        tokenize(routeId, planId, price) {
            this.loading = true;
            // Call tokenize method through the card component ref
            // Additional data can be passed to the tokenize method
            this.$refs.card
                .tokenize({})
                .then(data => {
                    this.token = data.token;
                    let token = this.token;
                    const req = {
                        token: data.token,
                        planId: planId,
                        customerToken: routeId
                    };

                    HTTP
                        .post("api/chargebeeSubscription", req)
                        .then(response => {
                            if (response.data.Status) {
                                this.loading = false;
                                this.transaction = response.data.Transaction;
                                this.$toasted
                                    .show(response.data.Message, {
                                        type: "success"
                                        // icon: "error_outline"
                                    })
                                    .goAway(1500);
                                this.type = "thank";
                                // localStorage.setItem(
                                //   "Data",
                                //   JSON.stringify([{ Payment: true }])
                                // );
                                // this.$destroy();
                            }
                        })
                        .catch(error => {
                            console.log(error);
                            let message = "";
                            this.loading = false;

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
                    //   debugger;
                })
                .catch(error => {
                    console.error(error);
                    //   this.loading = false;
                });
        },
        // Called when component is mounted in the DOM & ready
        onReady() {
            // this.app.mainLoader = false;
            // console.log("card component ready");
        },

        // Called when a field is focused
        onFocus(event) {
            console.log(event.field, "focused");
        },

        // Called when a field is blurred
        onBlur(event) {
            console.log(event.field, "blurred");
        },

        // Called on every state change in the card component
        onChange(event) {
            // Check for validation errors
            if (event.error) {
                this.errors[event.field] = event.error;

                // Display error message
                this.errorMessage = event.error.message;
            } else {

                this.errors[event.field] = null;
                // If there's no error, check for existing error
                const _errors = Object.values(this.errors).filter(val => val);

                // The errorObject holds a message and code
                // Custom error messages can be displayed based on the error code
                const errorObj = _errors.pop();

                // Display existing message
                if (errorObj) this.errorMessage = errorObj.message;
                else this.errorMessage = "";
            }
        },
        deleteStep() {
            this.dialogActive = true;
        },
        onConfirm() {

            this.app.loading = true;
            let singlePlan = this.singlePlan;

            let data = {
                newPlanId: singlePlan.id
            }

            HTTP
                .put("api/subscription/updateChargebeeSubscription", data)
                .then(response => {
                    if (response.data.Status) {
                        this.app.loading = false;
                        this.$router.push('/subscription-profile');
                        return false;
                    }
                })
                .catch(error => {
                    console.log(error);

                });

        },
        onCancel() {},

    }
};
</script>

<style lang="scss" scoped>
.pricing-head-content {
    min-height: 193px;
}

.pricing-box {
    max-width: 310px;
}
</style>
