<script>
export default {
  props: {
    BannerTitle: String,
    BannerSubTitle: String,
    BannerBtnText: String,
    goToDestination: Function,
  },
  data() {
    return {};
  },
};
</script>

<template>
  <div class="lg:flex bg-white lg:w-11/12 mx-auto lg:py-14 sm:py-2">
    <div class="lg:w-4/12 h-full block box-border lg:pr-20">
      <h1
        class="font-[700] lg:text-[55px] text-[40px] lg:leading-[55.94px] leading-[40px] mb-5"
      >
        {{ BannerTitle }}
      </h1>
      <h1
        class="font-[400] lg:text-[46px] text-[35px] lg:leading-[49.08px] leading-[35px]"
      >
        {{ BannerSubTitle }}
      </h1>
      <div class="w-[122px] h-[10px] bg-black mt-3 mb-2"></div>
      <button @click="goToDestination"
        class="lg:mt-8 mt-3 w-[248px] lg:h-[55px] h-[50px] text-white bg-black font-[700] text-[14px] leading-[16.1px] text-center rounded-[5px]"
      >
        {{ BannerBtnText }}
      </button>
    </div>
    <div class="lg:w-8/12 box-border">
      <div class="lg:bg-[#D5441C]">
        <img
          src="~/assets/img/images/image16.png"
          class="lg:h-[400px] h-auto lg:w-10/12 w-full"
        />
      </div>
    </div>
  </div>
</template>
