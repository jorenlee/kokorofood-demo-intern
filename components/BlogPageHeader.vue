<script>
export default {
  props: {
    BlogPageBannerTitle: String,
    BlogPageBannerDate: String,
    BlogPageBannerDateDisplay: Boolean,
    BlogBannerImage: String,
  },
  data() {
    return {};
  },
  methods: {
    formatDate(date) {
      const newDate = new Date(date);
      const options = { year: "numeric", month: "long", day: "numeric" };
      return newDate.toLocaleDateString("es-ES", options);
    },
  },
};
</script>

<template>
  <div class="lg:flex bg-black h-full">
    <div class="lg:w-4/12">
      <div class="bg-[#F4F5E6] lg:w-9/12 mr-0 ml-auto">
        <p
          class="text-black font-[Helvetica] font-[400] lg:text-[50px] text-lg leading-2 lg:mt-0 mt-[70px] lg:leading-[55px] lg:w-10/12 w-11/12 mx-auto pt-3 lg:pb-10 pb-8"
        >
          {{ BlogPageBannerTitle }}
        </p>
        <h6 class="text-left px-2 py-2" v-show="BlogPageBannerDateDisplay">
          {{ formatDate(BlogPageBannerDate) }}
        </h6>
      </div>
      <div class="lg:h-[100px]"></div>
    </div>
    <div class="lg:w-8/12">
      <img
        :src="BlogBannerImage"
        class="w-full h-full"
        v-if="BlogBannerImage"
      />
      <img src="~/assets/img/images/image15.png" class="w-full h-full" v-else />
    </div>
  </div>
</template>
