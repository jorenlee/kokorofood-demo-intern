<script>
export default {
  props: {
    BlogContent: String,
  },
};
</script>

<template>
  <div>
    <p
      v-html="BlogContent"
    ></p>
  </div>
</template>

<style scoped>
@import "~/assets/css/v-deep.css";
</style>