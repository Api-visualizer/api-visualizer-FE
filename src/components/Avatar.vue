<template>
    <div>
        <img alt="Avatar" class="img-fluid" :src="require(`@/assets/${filename}`)">
      <h4> {{ name }}</h4>
    </div>
</template>

<script>
export default {
  name: "LandingPage",

  props: ['name', 'filename'],

  components: {},
  data() {
    return {};
  },
  methods: {},
  mounted() {},
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
img {
  border-radius: 50%;
  max-width: 8em;
}
h4 {
  font-size: medium;
  margin-top: 1vh;
}

</style>
