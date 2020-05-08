<template>
    <div class="wrapper-home">
        <div class="container">
            <!-- <moon-loader :loading="this.app.mainLoader" :color="this.app.color" :size="this.app.size"></moon-loader> -->
            <div class="wrapper w-100 h-100">
                <div>
                    <div class="md-layout md-alignment-center-center h-100">
                        <div class="md-layout-item md-small-size-66 mx-auto">
                            <div class="text-center p4">
                                <img :src="'../img/logo-white.svg'" style="width: 196px;" class="mb-5">
                                <span class="md-title w-100 d-block">Page under reconstruction</span>
                                <span class="md-display-3 w-100 d-block pt-3 pb-5">coming soon</span>
                                 <router-link to="/auth/login" class="mx-auto md-primary button-primary-custom md-button mb-3 mt-3 text-center w-100"  exact>Login</router-link>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import MoonLoader from "vue-spinner/src/MoonLoader.vue";
export default {
    name: "Home",
    bodyClass: "home",
    props: ["app"],
    components: {
        MoonLoader
    },
    data() {
        return {
            test:true
        }
    },
    mounted() {}
};
</script>
