<template>
  <div>
    <span v-for="item in steps2" :key="item.id">
      <span>{{item.steps_title}}</span>
      <span>{{item.description}}</span>
      <span>{{item.video}}</span>
    </span>
  </div>
</template>

<script>
export default {
  name: "TransactionUsers",
  components: {},
  // props: ['steps'],
  data: function() {
    return {
      steps2: [
        {
          id: "1",
          steps_title: "Hunger",
          description: "This is a sample description",
          video: "https://vimeo.com/123",
          created_at: "Aug 30, 2019"
        },
        {
          id: "2",
          steps_title: "Moee",
          description: "This is a sample description",
          video: "https://vimeo.com/123",
          created_at: "Aug 30, 2019"
        }
      ]
    };
  },
  created() {
    this.int();
  },
  methods: {
    int() {}
  },
  computed: {},
  watch: {}
};
</script>

<style scoped>
</style>
