<template>
<div class="content">
    <div class="md-layout">
        <div class="md-layout-item md-medium-size-100 md-xsmall-size-100 md-size-100 justify-content-center align-center d-flex empty-div">
            <img :src="'../img/coming-soon.svg'" alt="Work in-progress" class="alt-img" />
        </div>

    </div>
</div>
</template>

<script>
import {
    HTTP
} from "../../httpCommon";
export default {
    name: "Dashboard",
    data() {
        return {
            test: true
        };
    },
    mounted() {
        this.init();
    },
    methods: {
        init() {

        }
    }
};
</script>
