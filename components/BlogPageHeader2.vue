<template>
  <div class="flex bg-black">
    <div class="w-4/12 relative">
      <div class="bg-[#F4F5E6] w-11/12 h-4/6 absolute -right-20 pt-3 top-12">
        <h1
          class="text-black font-[Times New Roman] text-[55px] w-10/12 mx-auto tracking-[0em] text-[left]"
        >
          <span class="leading-[67px] font-[400]">Disfrutas Nuestras </span>
          <span class="leading-[63px] font-[700]">{{ categoryTitle }}</span>
        </h1>
      </div>
    </div>
    <div class="w-8/12 d-md-block">
      <img :src="imageURL" class="w-full h-[436px]" />
    </div>
  </div>
</template>

<script>
export default {
  props: {
    categoryTitle: String,
    imageURL: String,
  },
  data() {
    return {};
  },
};
</script>
