<template>
  <div class="subscription-page">
    <md-dialog :md-active.sync="showStep">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light position-relative">
        <h4 class="my-0 md-headline text-left text-primary">Card Info</h4>
        <md-button @click="showStep = false" class="md-icon-button position-absolute">
          <md-icon class="text-primary">close</md-icon>
        </md-button>
      </md-dialog-title>
      <div class="md-dialog-content p-0">
        <md-content md-dynamic-height class="px-4">
          <!-- Pass options to card component -->
          <CardComponent
            ref="card"
            class="fieldset field card-custom"
            :styles="styles"
            :classes="classes"
            @ready="onReady"
            @change="onChange"
          >
            <!-- ^ Add ready and change listener to card component -->

            <!-- Indivudual fields mode -->
            <!-- Attach listeners to detect focus, blur on each field -->
            <div class="md-layout">
              <div class="md-layout-item md-medium-size-100 md-small-size-100 md-xsmall-size-100">
                <CardNumber
                  class="ex3-input"
                  :placeholder="'Card Number'"
                  @focus="onFocus"
                  @blur="onBlur"
                />
              </div>
            </div>
            <div class="md-layout">
              <div class="md-layout-item md-medium-size-50 md-small-size-100 md-xsmall-size-100">
                <CardExpiry
                  class="ex3-input"
                  :placeholder="'Expiry Date'"
                  @focus="onFocus"
                  @blur="onBlur"
                />
              </div>
              <div
                class="md-layout-item md-medium-size-50 md-small-size-100 md-xsmall-size-100 position-relative cvv-div"
              >
                <CardCvv class="ex3-input" :placeholder="'CVV'" @focus="onFocus" @blur="onBlur" />
                <md-icon class="text-primary position-absolute">info_outline</md-icon>
              </div>
            </div>
            <div v-if="errors" class="error card-error md-error">{{ errorMessage }}</div>
          </CardComponent>
        </md-content>
        <md-dialog-actions class="flex-wrap justify-content-center">
          <md-button
            type="submit"
            class="md-primary w-100 button-primary-custom mb-3 mt-3"
            @click="tokenize()"
          >
            <LoadingButtonLoader
              :typeEnable="app.checkType('simpleLoader')"
              v-bind:enable="loading"
              class="mr-1"
            />Submit
          </md-button>
          <div class="d-block w-100 text-center">
            <md-button
              :md-ripple="false"
              @click="showStep = false"
              class="cancel-link lighter-description mt-2 text-capitalize"
            >Cancel</md-button>
          </div>
        </md-dialog-actions>
      </div>
    </md-dialog>
    <md-dialog :md-active.sync="showDialog">
      <md-dialog-title class="modal-title d-block py-3 px-4 bg-light">
        <h4 class="my-0 md-headline text-left text-primary">Choose plan</h4>
      </md-dialog-title>
      <md-content md-dynamic-height class="px-4">
        <div class="md-layout md-gutter md-alignment-center h-100">
          <div class="md-layout-item md-medium-size-80 md-small-size-85 md-xsmall-size-90">
            <div class="md-card-custom modal-style">
              <div class="modal-content p-4">
                <h4 class="card-title text-center text-primary">Find the perfect plan for you</h4>
                <p class="description text-center sub-title">Free 7-day trial with each plan</p>
                <md-tabs class="md-success" md-alignment="centered">
                  <md-tab id="tab-home" md-label="Monthly">
                    <div class="md-layout md-gutter">
                      <div
                        class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mb-3"
                      >
                        <div class="md-card-custom border-box pricing-box">
                          <div class="pricing-head-content py-4 px-3">
                            <span class="d-block text-uppercase pb-1">Tier One</span>
                            <h3 class="md-headline d-block text-primary pb-1 my-0">$ 100</h3>
                            <p class="md-body-1">Best for individual agents</p>
                          </div>
                          <div class="pricing-body-content py-3 px-3">
                            <md-list class="md-listing">
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item One</span>
                              </md-list-item>
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item Two</span>
                              </md-list-item>
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute text-disabled"
                                >close</md-icon>
                                <span class="md-list-item-text text-disabled">Item Three</span>
                              </md-list-item>
                            </md-list>
                            <button
                              class="md-button md-primary md-theme-default button-custom-regular button-outline"
                            >Buy now</button>
                          </div>
                        </div>
                      </div>
                      <div
                        class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mb-3"
                      >
                        <div class="md-card-custom border-box pricing-box">
                          <div class="pricing-head-content py-4 px-3">
                            <span class="d-block text-uppercase pb-1">Tier One</span>
                            <h3 class="md-headline d-block text-primary pb-1 my-0">$ 100</h3>
                            <p class="md-body-1">Best for individual agents</p>
                          </div>
                          <div class="pricing-body-content py-3 px-3">
                            <md-list class="md-listing">
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item One</span>
                              </md-list-item>
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item Two</span>
                              </md-list-item>
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item Three</span>
                              </md-list-item>
                            </md-list>
                            <button
                              class="md-button md-primary md-theme-default button-custom-regular button-outline"
                            >Buy now</button>
                          </div>
                        </div>
                      </div>
                      <div
                        class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mb-3"
                      >
                        <div class="md-card-custom border-box pricing-box">
                          <div class="pricing-head-content py-4 px-3">
                            <span class="d-block text-uppercase pb-1">Tier One</span>
                            <h3 class="md-headline d-block text-primary pb-1 my-0">$ 100</h3>
                            <p class="md-body-1">Best for individual agents</p>
                          </div>
                          <div class="pricing-body-content py-3 px-3">
                            <md-list class="md-listing">
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item One</span>
                              </md-list-item>
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item Two</span>
                              </md-list-item>
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item Three</span>
                              </md-list-item>
                            </md-list>
                            <button
                              class="md-button md-primary md-theme-default button-custom-regular button-outline"
                            >Buy now</button>
                          </div>
                        </div>
                      </div>
                    </div>
                  </md-tab>
                  <md-tab id="tab-pages" md-label="Annual">
                    <div class="md-layout md-gutter">
                      <div
                        class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mb-3"
                      >
                        <div class="md-card-custom border-box pricing-box">
                          <div class="pricing-head-content py-4 px-3">
                            <span class="d-block text-uppercase pb-1">Tier One</span>
                            <h3 class="md-headline d-block text-primary pb-1 my-0">$ 100</h3>
                            <p class="md-body-1">Best for individual agents</p>
                          </div>
                          <div class="pricing-body-content py-3 px-3">
                            <md-list class="md-listing">
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item One</span>
                              </md-list-item>
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item Two</span>
                              </md-list-item>
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute text-disabled"
                                >close</md-icon>
                                <span class="md-list-item-text text-disabled">Item Three</span>
                              </md-list-item>
                            </md-list>
                            <button
                              class="md-button md-primary md-theme-default button-custom-regular button-outline"
                            >Buy now</button>
                          </div>
                        </div>
                      </div>
                      <div
                        class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mb-3"
                      >
                        <div class="md-card-custom border-box pricing-box">
                          <div class="pricing-head-content py-4 px-3">
                            <span class="d-block text-uppercase pb-1">Tier One</span>
                            <h3 class="md-headline d-block text-primary pb-1 my-0">$ 100</h3>
                            <p class="md-body-1">Best for individual agents</p>
                          </div>
                          <div class="pricing-body-content py-3 px-3">
                            <md-list class="md-listing">
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item One</span>
                              </md-list-item>
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item Two</span>
                              </md-list-item>
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item Three</span>
                              </md-list-item>
                            </md-list>
                            <button
                              class="md-button md-primary md-theme-default button-custom-regular button-outline"
                            >Buy now</button>
                          </div>
                        </div>
                      </div>
                      <div
                        class="md-layout-item md-medium-size-33 md-small-size-50 md-xsmall-size-100 mb-3"
                      >
                        <div class="md-card-custom border-box pricing-box">
                          <div class="pricing-head-content py-4 px-3">
                            <span class="d-block text-uppercase pb-1">Tier One</span>
                            <h3 class="md-headline d-block text-primary pb-1 my-0">$ 100</h3>
                            <p class="md-body-1">Best for individual agents</p>
                          </div>
                          <div class="pricing-body-content py-3 px-3">
                            <md-list class="md-listing">
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item One</span>
                              </md-list-item>
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item Two</span>
                              </md-list-item>
                              <md-list-item class="position-relative">
                                <md-icon
                                  class="md-icon md-icon-font md-theme-default position-absolute"
                                >done</md-icon>
                                <span class="md-list-item-text">Item Three</span>
                              </md-list-item>
                            </md-list>
                            <button
                              class="md-button md-primary md-theme-default button-custom-regular button-outline"
                            >Buy now</button>
                          </div>
                        </div>
                      </div>
                    </div>
                  </md-tab>
                </md-tabs>
              </div>
            </div>
          </div>
        </div>
      </md-content>
    </md-dialog>
    <div class="md-layout">
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-100">
        <md-card-header class="px-0 mb-0 mt-0 pb-4 pt-0">
          <span class="text-primary font-20">Subscription</span>
        </md-card-header>
      </div>
    </div>
    <div
      class="bg-white custom-shadow radius-10 mt-0 md-medium-size-100 md-xsmall-size-100 md-size-100 px-2 mb-5"
    >
      <div class="md-layout px-4 flex-wrap">
        <div class="md-layout-item md-medium-size-30 md-small-size-100 md-size-25 plan-section">
          <md-card class="md-card-plain text-center">
            <md-card-content class="py-0">
              <div class="d-flex justify-content-between mb-3 align-center">
                <h3 class="description text-primary my-2">Active plan</h3>
                <!-- <a href="#" class="brand-primary font-weight-medium description-normal my-2">Cancel</a> -->
              </div>
              <div class="border-box pricing-box active">
                <div class="pricing-head-content py-4 px-1">
                  <span
                    v-if="singlePlan.invoice_name"
                    class="d-block text-uppercase pb-1"
                  >{{ singlePlan.invoice_name }}</span>
                  <h3
                    v-if="singlePlan.price"
                    class="md-headline d-block text-primary pb-1 my-0"
                  >$ {{ singlePlan.price }}</h3>
                  <p class="md-body-1">Best for individual agents</p>
                </div>
                <div class="pricing-body-content pt-3 pb-4 px-1">
                  <md-list class="md-listing px-3">
                    <md-list-item class="position-relative">
                      <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                      <span class="md-list-item-text">Item One</span>
                    </md-list-item>
                    <md-list-item class="position-relative">
                      <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                      <span class="md-list-item-text">Item Two</span>
                    </md-list-item>
                    <md-list-item class="position-relative">
                      <md-icon
                        class="md-icon md-icon-font md-theme-default position-absolute text-disabled"
                      >close</md-icon>
                      <span class="md-list-item-text text-disabled">Item Three</span>
                    </md-list-item>
                  </md-list>
                  <button
                    class="md-button md-primary md-theme-default button-custom-regular mb-3"
                    @click="payment"
                  >Change Plan</button>
                  <!-- <a href="#" class="lighter-description d-block description" @click="showDialog = true">Downgrade</a> -->
                </div>
              </div>
            </md-card-content>
          </md-card>
        </div>
        <div class="md-layout-item md-medium-size-70 md-small-size-100 md-size-75 table-section">
          <md-card class="md-card-plain">
            <md-card-content class="py-0">
              <div class="md-layout-item d-flex justify-content-between mb-3">
                <h3 class="description text-primary my-2">Billing History</h3>
              </div>
              <!-- {{ invoiceLists }} -->
              <md-table class="mb-3 subscription-table">
                    <md-table-row class="table-head">
                        <md-table-head>Plan Name</md-table-head>
                        <md-table-head>Payment ID</md-table-head>
                        <md-table-head>Payment Type</md-table-head>
                        <md-table-head>Date</md-table-head>
                        <md-table-head>Invoice</md-table-head>
                    </md-table-row>
                <md-table-row
                  slot="md-table-row"
                  v-for="invoice in invoiceLists"
                  v-bind:key="invoice.id"
                >
                  <md-table-cell md-label="Plan Name">{{ invoice.plan_name }}</md-table-cell>
                  <md-table-cell md-label="Payment ID">{{ invoice.payment_id }}</md-table-cell>
                  <md-table-cell md-label="Payment Type">{{ invoice.payment_type }}</md-table-cell>
                  <md-table-cell md-label="Date">
                    <span>{{ invoice.updated_at|formatDate }}</span>
                    <br />
                    <!-- <small class="lighter-description">{{ item.time }}</small> -->
                  </md-table-cell>

                  <md-table-cell md-label="Invoice">
                    <a :href="appUrl+'api/download/'+invoice.payment_id" param target="_blank">
                      <md-icon class>get_app</md-icon>
                    </a>
                    <!-- <button class="md-button md-primary md-theme-default button-custom-regular mb-3" @click="download">download</button> -->
                  </md-table-cell>
                  <LoadingBar
                    :typeEnable="app.checkType('simpleLoader')"
                    v-bind:enable="app.loading"
                  />
                </md-table-row>
              </md-table>
            </md-card-content>
          </md-card>
        </div>
      </div>
    </div>
    <div class="md-layout">
      <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-100">
        <md-card-header class="px-0 mb-0 mt-0 pb-4">
          <span class="text-primary font-20">
            Update Card Info
            <img :src="'../img/credit-card.svg'" alt style="width: 26px;" />
          </span>
        </md-card-header>
      </div>
    </div>
    <div class="mt-0 px-3 mb-5">
      <div class="md-layout">
        <div
          class="md-layout-item md-medium-size-50 md-xsmall-size-100 md-medium-size-60 md-size-50 bg-white custom-shadow radius-10"
        >
          <!-- {{ card }} -->
          <md-card class="md-card-plain text-center">
            <md-card-content class="px-4 py-0">
              <div class="md-layout">
                <div class="md-layout-item md-small-size-100 md-medium-size-50 md-size-50">
                  <md-field slot="inputs">
                    <label>card no.</label>
                    <md-input v-model="cardNo"></md-input>
                  </md-field>
                </div>
                <div class="md-layout-item md-small-size-100 md-medium-size-50 md-size-50">
                  <md-field slot="inputs">
                    <label>card holder name</label>
                    <md-input v-model="cardHolderName"></md-input>
                  </md-field>
                </div>
                <div class="md-layout-item md-small-size-100 md-medium-size-50 md-size-50">
                  <md-field slot="inputs">
                    <label>Expiry date</label>
                    <md-input v-model="ExpiryDate"></md-input>
                  </md-field>
                </div>
                <div class="md-layout-item md-small-size-100 md-medium-size-50 md-size-50">
                  <md-field slot="inputs">
                    <label>CCV</label>
                    <md-input v-model="CCV"></md-input>
                  </md-field>
                </div>
                <button
                  class="md-button md-primary md-theme-default button-custom-regular"
                  @click="openUpdateCard"
                >Update</button>
              </div>
            </md-card-content>
          </md-card>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { HTTP } from "../../httpCommon";
import LoadingBar from "../../components/LoadingBar";
import {
  CardComponent,
  CardNumber,
  CardExpiry,
  CardCvv
} from "@chargebee/chargebee-js-vue-wrapper";
import LoadingButtonLoader from "../../components/LoadingButtonLoader";
export default {
  name: "SubscriptionProfile",
  props: ["app"],
  components: {
    LoadingBar,
    CardComponent,
    CardNumber,
    CardExpiry,
    CardCvv,
    LoadingButtonLoader
  },
  data() {
    return {
      showDialog: false,
      showStep: false,
      loading: false,
      users: [
        {
          planname: "Tier 1",
          paymentid: "65465456",
          paymenttype: "First time",
          date: "24 Dec, 2019",
          time: "11:13 pm"
        }
      ],
      singlePlan: "",
      appUrl: process.env.MIX_API_URL,
      card: "",
      invoiceLists: "",
      customerDetail: "",
      cardNo: null,
      cardHolderName: null,
      ExpiryDate: null,
      CCV: null,
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
      }
    };
  },
  created() {
    // console.log(process.env.MIX_SITE);
    // console.log(process.env.MIX_PUBLISHABLE_KEY);
    window.Chargebee.init({
      site: process.env.MIX_SITE,
      publishableKey: process.env.MIX_PUBLISHABLE_KEY
    });
  },
  mounted() {
    this.init();
  },
  methods: {
    init() {
      HTTP.get("api/users/getInvoicesList")
        .then(response => {
          this.singlePlan = response.data.CurrentPlan;
          this.card = response.data.Card_details;
          this.invoiceLists = response.data.InvoiceData;
          this.customerDetail = response.data.Customer_details;

          if (this.card) {
            this.cardNo = this.card.masked_number;
            this.cardHolderName =
              this.customerDetail.first_name +
              " " +
              this.customerDetail.last_name;
            this.ExpiryDate =
              this.card.expiry_month + "/" + this.card.expiry_year;
            this.CCV = null;
          }
        })
        .catch(error => {});
    },
    payment() {
      console.log(this.$route);
      this.$router.push("/subscription/user");
    },
    download() {
      console.log("download-----");
      this.app.loading = true;
      HTTP.get("api/download")
        .then(response => {
          this.app.loading = false;
          // this.singlePlan = response.data.CurrentPlan
        })
        .catch(error => {});
    },
    openUpdateCard() {
      this.showStep = true;
    }, // Method for tokenizing card details
    tokenize(routeId, planId, price) {
      // Call tokenize method through the card component ref
      // Additional data can be passed to the tokenize method
      this.loading = true;
      this.$refs.card
        .tokenize({})
        .then(data => {
            this.token = data.token;
            let token = this.token;

            const req = {
                token: data.token,

            };


          HTTP.put("api/subscription/updateCard", req)
            .then(response => {
              if (response.data.Status) {
                this.loading = false;
                // this.transaction = response.data.Transaction;
                this.showStep = false;
                this.$toasted
                  .show(response.data.Message, {
                    type: "success"
                    // icon: "error_outline"
                  })
                  .goAway(1500);
              }
            })
            .catch(error => {
              console.log(error);
              // let message = "";
              this.loading = false;

              // if (error.response.data) {
              //     if (error.response.data.errors) {
              //         if (error.response.data.errors.email)
              //             message = error.response.data.errors.email[0];
              //         if (error.response.data.errors.Message)
              //             message = error.response.data.errors.Message;
              //         else message = error.response.data.errors;
              //     }
              // }

              // this.$toasted
              //     .show(message, {
              //         type: "error"
              //         // icon: "error_outline"
              //     })
              //     .goAway(2500);
            });
          //   debugger;
        })
        .catch(error => {
          console.log(error);
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
    onSubmit (e) {
    },
  }
};
</script>

<style lang="scss" scoped>
.md-dialog {
  max-width: 700px;
  width: 100%;
}
</style>
