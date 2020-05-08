<template>
  <div class="container">
    <div class="row">
      <div class="col-md-8 col-md-offset-2">
        <div class="panel panel-default">
          <div class="panel-heading">List All Campaign</div>

          <!-- <ul v-for="campaign in campaigns.data">
            <li>{{ campaign.name }}</li>
          </ul>-->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      campaigns: []
    };
  },

  created() {
    axios.get("/api/v1/campaigns").then(({ data }) => (this.campaigns = data));
  }
};
</script>