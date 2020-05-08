<template>
<div class="section make-payment py-0 auth-bg">
    <div class="container">
    <div class="wrapper">
        <div class="md-layout md-gutter md-alignment-center h-100 mb-3">
            <div class="md-layout-item md-medium-size-80 md-small-size-85 md-xsmall-size-90">
                <div class="md-card-custom modal-style">
                    <div class="modal-title d-block py-3 px-4 bg-light"><h4 class="my-0 card-title text-left text-primary">Make payment</h4></div>
                    <div class="modal-content p-4">
                        <div class="md-layout md-gutter">
                            <div class="md-layout-item md-medium-size-50 md-small-size-100 md-xsmall-size-100 px-1 mb-2">
                                <div class="make-payment-box pt-3 px-4">
                                    <h4 class="md-title text-left text-primary text-uppercase">Select card type</h4>
                                    <div>
                                        <md-radio v-model="radio" :value="true"><img :src="'./img/american.svg'" style="width: 40px;" /></md-radio>
                                        <md-radio v-model="radio" :value="false"><img :src="'./img/visa.svg'" style="width: 40px;" /></md-radio>
                                        <md-radio v-model="radio" :value="false"><img :src="'./img/credit-card.svg'" style="width: 40px;" /></md-radio>
                                    </div>
                                    <div class="md-layout">
                                        <div class="md-layout-item md-small-size-100 md-size-100">
                                            <md-field slot="inputs">
                                                <label>Name on card</label>
                                                <md-input v-model="nameoncard"></md-input>
                                            </md-field>
                                        </div>
                                        <div class="md-layout-item md-small-size-100 md-size-100">
                                            <md-field slot="inputs">
                                                <label>Card number</label>
                                                <md-input v-model="cardnumber"></md-input>
                                            </md-field>
                                        </div>
                                        <div class="md-layout-item md-small-size-100 md-size-50">
                                            <md-field slot="inputs">
                                                <label>Expiry date</label>
                                                <md-input v-model="expirycard"></md-input>
                                            </md-field>
                                        </div>
                                        <div class="md-layout-item md-small-size-100 md-size-50">
                                            <md-field slot="inputs">
                                                <label>CCV</label>
                                                <md-input v-model="ccv"></md-input>
                                                <md-icon class="text-primary">info_outline</md-icon>
                                            </md-field>
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <div class="md-layout-item md-medium-size-50 md-small-size-100 md-xsmall-size-100 px-1 mb-2">
                                <div class="make-payment-box pt-3">
                                    <h4 class="md-title text-left text-primary text-uppercase px-4">Plan Info</h4>
                                    <div class="md-layout px-4">
                                        <div class="md-layout-item md-small-size-50 md-size-50">
                                            <h4 class="md-title text-left text-primary">Tier 1</h4>
                                        </div>
                                        <div class="md-layout-item md-small-size-50 md-size-50">
                                            <h5 class="md-headline text-left brand-primary font-weight-semi-bold text-right">$100</h5>
                                        </div>
                                        <div class="md-layout-item md-small-size-100 md-size-100">
                                            <p slot="description" class="description-normal text-description text-left font-weight-regular">Lorem ipsum dolor sit amet, consectetuer adipiscing elit. Aenean commodo ligula eget dolor. </p>
                                        </div>
                                        <div class="md-layout-item md-small-size-100 md-size-100">
                                            <div class="md-layout md-gutter">
                                                <div class="md-layout-item md-small-size-33 md-size-33">
                                                    <md-list class="md-listing">
                                                        <md-list-item class="position-relative">
                                                            <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                            <span class="md-list-item-text">Feature 1</span>
                                                        </md-list-item>
                                                    </md-list>
                                                </div>
                                                <div class="md-layout-item md-small-size-33 md-size-33">
                                                    <md-list class="md-listing">
                                                        <md-list-item class="position-relative">
                                                            <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                            <span class="md-list-item-text">Feature 2</span>
                                                        </md-list-item>
                                                    </md-list>
                                                </div>
                                                <div class="md-layout-item md-small-size-33 md-size-33">
                                                    <md-list class="md-listing">
                                                        <md-list-item class="position-relative">
                                                            <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                            <span class="md-list-item-text">Feature 3</span>
                                                        </md-list-item>
                                                    </md-list>
                                                </div>
                                                <div class="md-layout-item md-small-size-33 md-size-33">
                                                    <md-list class="md-listing">
                                                        <md-list-item class="position-relative">
                                                            <md-icon class="md-icon md-icon-font md-theme-default position-absolute">done</md-icon>
                                                            <span class="md-list-item-text">Feature 4</span>
                                                        </md-list-item>
                                                    </md-list>
                                                </div>
                                                    <div class="md-layout-item md-small-size-33 md-size-33">
                                                    <md-list class="md-listing">
                                                        <md-list-item class="position-relative ">
                                                            <md-icon class="md-icon md-icon-font md-theme-default position-absolute text-disabled">close</md-icon>
                                                            <span class="md-list-item-text text-disabled">Feature 5</span>
                                                        </md-list-item>
                                                    </md-list>
                                                    </div>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="plan-info-footer py-2 px-4">
                                        <p class="md-subheading text-description font-weight-semi-bold text-uppercase mb-1">wednesday</p>
                                        <p class="md-subheading lighter-description font-weight-regular text-uppercase mb-0">december 20, 2019 | 11:03 pm</p>
                                    </div>
                                </div>
                            </div>
                            <div class="md-layout-item md-size-100  md-medium-size-100 md-small-size-100 md-xsmall-size-100 px-0">
                                <button class="md-button md-primary md-theme-default button-custom-regular">Submit</button>
                                <md-button :md-ripple="false" class="cancel-link lighter-description mt-2">Cancel</md-button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="md-layout md-gutter md-alignment-center h-100 mb-3">
            <div class="md-layout-item md-size-60 md-medium-size-80 md-small-size-85 md-xsmall-size-90">
                <div class="md-card-custom modal-style">
                    <div class="modal-content p-4">
                        <div class="md-layout md-gutter">
                            <div class="md-layout-item md-medium-size-100 md-small-size-100 md-xsmall-size-100 px-1 mb-2">
                                <div class="pt-3 px-4 text-center">
                                    <img :src="'./img/smile.svg'" style="width: 55px;" />
                                    <h5 class="md-headline text-center text-primary font-weight-medium mb-4">Thank you!</h5>
                                    <p class="description description-confirm font-weight-medium">Your payment <span class="text-primary">$100</span> has been processed successfully.</p>
                                    <p class="description description-confirm font-weight-medium">For your reference, your payment transaction id is: <span class="brand-primary">5550992360</span></p>
                                    <a href="#/auth/login" class="cancel-link text-underline mt-4 d-block">Go to login page</a>
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
</template>
<script>
export default {
  name: "MakePayment",
    data() {
        return {
        nameoncard: null,
        cardnumber: null,
        expirycard: null,
        ccv: null,
        radio: true,
        };
    }
};
</script>
<style scoped>
</style>
