<template>
  <div class="wrapper w-100">
    <div class="md-layout w-100 md-alignment-center-center h-100 mx-0">
      <div class="md-layout-item md-xsmall-size-100">
        <div class="content section">
          <div class="text-center">
            <img :src="'../img/logo.svg'" style="width: 196px;" />
          </div>
          <router-view :app="this.app" />
        </div>
      </div>
      <div
        class="md-layout-item md-xsmall-size-100 h-100 carousal-auth"
        style="background-image:url('../img/bg-auth.jpg');"
      >
        <carousel :per-page="1" :mouse-drag="false" class="h-100">
          <slide>
            <div class="testimonial-outer w-100">
              <div class="testimonial mx-auto text-center mb-3">
                <span>“</span>
                Repylot helped me and my clients to communicate more effectively and improved our cash flow significantly. I recommend it to every real state agent. You won’t regret it!
                <span>”</span>
              </div>
              <div class="testimonial-user mx-auto">
                <div
                  class="testimonial-user-inner position-relative mx-auto flex-wrap justify-content-start d-flex align-center"
                >
                  <div class="md-avatar position-absolute">
                    <img src="https://placeimg.com/40/40/people/1" alt="People" />
                  </div>
                  <div class="user-name">Peter Newman</div>
                  <div class="company-name">Real Estate Ltd.</div>
                </div>
              </div>
            </div>
          </slide>
          <slide>
            <div class="testimonial-outer w-100">
              <div class="testimonial mx-auto text-center mb-3">
                <span>“</span>
                Repylot helped me and my clients to communicate more effectively and improved our cash flow significantly. I recommend it to every real state agent. You won’t regret it!
                <span>”</span>
              </div>
              <div class="testimonial-user mx-auto">
                <div
                  class="testimonial-user-inner position-relative mx-auto flex-wrap justify-content-start d-flex align-center"
                >
                  <div class="md-avatar position-absolute">RT</div>
                  <div class="user-name">Peter Newman</div>
                  <div class="company-name">Real Estate Ltd.</div>
                </div>
              </div>
            </div>
          </slide>
        </carousel>
      </div>
    </div>
  </div>
</template>

<script >
import {HTTP} from '../../httpCommon';
import { Carousel, Slide } from "vue-carousel";
export default {
  name: "Auth",
  props: ["app"],
  bodyClass: "login-wrapper",
  components: {
    Carousel,
    Slide
  },
  data() {
    return {
      classicModal: false
      // logo: require("@/img/logo.svg")
    };
  },
  mounted() {
    // console.log("mounted"); // I'm text inside the component.
    this.init(); // get plans at subscription
  },
  methods: {
    init() {
      //   console.log("Index component");
      //   console.log(this.app.loading);
    }
  },
  computed: {
    carousalStyle() {
      return {
        backgroundImage: `url(${this.carousal})`
      };
    }
  }
};
</script>
<style scoped>
</style>
