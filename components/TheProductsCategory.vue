<script>
export default {
  props: {
    Items: Array,
    Lists: Array,
  },
  data() {
    return {};
  },
  methods: {
    goToCategory(slug) {
      this.$router.push(this.localePath(`/category/${slug}`));
    },
  },
};
</script>

<template>
  <div>
    <div class="pt-24 pb-10 font-[Helvetica] bg-[#F4F5E6]">
      <div>
        <ul class="xl:gap-10 lg:gap-5 xl:w-10/12 w-11/12 mx-auto lg:flex">
          <li
            v-for="(item, i) in Items"
            :key="i"
            class="lg:w-1/3 text-center h-fit"
          >
            <div class="" :style="`background-color: ${item.bg_color}`">
              <h1
                class="font-[700] uppercase pt-4 pb-4 text-[29px] leading-[33.35px] text-center text-[#000000]"
              >
                {{ item.category }}
              </h1>
              <img :src="item.image_url" key="" class="h-[259px] w-11/12 mx-auto object-cover" />
            </div>
            <div class="h-[140px] bg-white -mt-[60px] pt-[70px]">
              <button
                class="w-[168.84px] h-[58px] uppercase font-[700] text-[16px] leading-[18.4px] border border-black text-black rounded-md"
                @click="goToCategory(item.slug)"
              >
                COMPRAR
              </button>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <div class="pt-10 pb-24 font-[Helvetica] bg-[#F4F5E6]">
      <div>
        <ul class="xl:gap-10 lg:gap-5 xl:w-10/12 w-11/12 mx-auto lg:flex">
          <li
            v-for="(item, i) in Lists"
            :key="i"
            class="lg:w-1/3 h-[362px] relative bg-white"
          >
            <img
              :src="require(`~/assets/img/cards2/${item.productItems}.png`)"
              class="relative mx-auto w-full h-[362px] object-cover"
            />
            <div class="absolute top-0 z-10 w-full border h-full">
              <div class="justify-center w-full h-full flex items-center">
                <h1
                  class="text-white font-[700] uppercase pt-5 pb-4 text-[29px] leading-[33.35px]"
                >
                  {{ item.productName }}
                </h1>
              </div>
              <button
                class="rounded-md bg-white font-[700] text-sm leading-[16.1px] bottom-[50px] absolute left-1/2 transform -translate-x-1/2 w-[155px] h-[55px]"
              >
                LEER MAS
              </button>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>