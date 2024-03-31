<script>
export default {
  props: {
    MasVendidoData: Array,
    DestacadoData: Array,
    SoldOutItems: Array,
  },
  data() {
    return {};
  },
  methods: {
    goToCategory(slug) {
      this.$router.push(this.localePath(`/category/${slug}`));
    },
    goToProducts() {
      this.$router.push(this.localePath(`/productos`));
    },
    goToProduct(id) {
      this.$router.push(this.localePath(`/productos/${id}`));
    },
    Img_product(images) {
      for (var i = 0; i < images.length; i++) {
        if (
          images[i].type == "main" 
        ) {
          var url = images[i].image_url;
        }
      }
      return url;
    },
  },
};
</script>

<template>
  <div>
    <div class="bg-white w-11/12 mx-auto lg:my-28 my-5">
      <div class="lg:flex lg:mt-24 mt-10">
        <div class="lg:w-4/12 pr-5">
          <h1 class="text-[51px] leading-[58.65px]">
            <span class="font-[400] block">Productos</span>
            <span class="font-[700]">más vendidos</span>
          </h1>
          <div class="w-[144.05px] h-[11px] bg-black mt-4"></div>
          <button
            class="w-[158.22px] h-[60.11px] text-white bg-black font-[700] text-[14px] leading-[16.1px] text-center uppercase rounded-[5px] my-7"
            @click="goToProducts()"
          >
            {{ $t("Buttons.GoProducts") }}
          </button>
        </div>
        <div class="lg:w-8/12">
          <ul class="lg:grid grid-cols-2 gap-5">
            <li
              class="lg:mb-0 mb-5"
              v-for="(product, k) in MasVendidoData"
              :key="k"
            >
              <span class="p-[10px] flex bg-[#D5441C] w-full h-[190px]">
                <span class="text-[#000000] w-7/12 block">
                  <h1
                    class="font-[700] text-[25px] leading-[28.75px] break-normal overflow-hidden"
                  >
                    {{ product.name }}
                  </h1>
                  <span v-if="product.precio_comparacion != 0">
                    <h3 class="font-[400] text-[14px] leading-[16.1px]">
                      Antes
                    </h3>
                    <h3
                      class="font-[400] text-[14px] leading-[16.1px] line-through"
                    >
                      ${{ product.price }}
                    </h3>
                  </span>
                  <h1 class="font-[700] text-[35px] leading-[40.25px]">
                    ${{ product.discount_price }}
                  </h1>
                </span>
                <span class="w-5/12" v-if="product.product_images.length > 0">
                  <img
                    class="mx-auto w-10/12 h-[270px] object-cover"
                    :src="Img_product(product.product_images)"
                    :alt="product.name"
                  />
                </span>
              </span>
              <span class="bg-white" v-if="product.quantity > 0">
                <button
                  class="w-[168.84px] h-58px font-[700] text-[16px] leading-[18.4px] text-[#000000] h-[58px] text-center uppercase border border-black rounded-[5px] mt-5"
                  @click="goToProduct(product.id)"
                >
                  {{ $t("Buttons.buy") }}
                </button>
              </span>
              <span class="bg-white" v-if="product.quantity < 1">
                <button
                  class="w-[168.84px] h-58px font-[700] text-[16px] leading-[18.4px] text-[#000000] h-[58px] text-center uppercase border border-black rounded-[5px] mt-5 disabled:opacity-50"
                  @click="goToProduct(product.id)"
                  disabled
                >
                  {{ $t("Buttons.OutStock") }}
                </button>
              </span>
            </li>
          </ul>
        </div>
      </div>
      <div class="lg:flex lg:mt-24 mt-10">
        <div class="lg:w-4/12">
          <h1 class="text-[51px] leading-[58.65px]">
            <span class="font-[400] block">Productos</span>
            <span class="font-[700]">Destacados</span>
          </h1>
          <div class="w-[144.05px] h-[11px] bg-black mt-4"></div>
          <button
            class="w-[158.22px] h-[60.11px] text-white bg-black font-[700] text-[14px] leading-[16.1px] text-center uppercase rounded-[5px] my-7"
            @click="goToProducts()"
          >
            {{ $t("Buttons.GoProducts") }}
          </button>
        </div>
        <div class="lg:w-8/12 mb-20">
          <ul class="lg:grid grid-cols-2 gap-5 mb-20">
            <li class="" v-for="(producto, k) in DestacadoData" :key="k">
              <span class="p-[10px] bg-[#CBD402] block w-full h-[136px]">
                <img
                  v-if="producto.product_images.length > 0"
                  :src="Img_product(producto.product_images)"
                  class="w-32 mx-auto h-[178px] object-cover"
                  :alt="producto.name"
                />
              </span>
              <span class="flex mt-16">
                <span class="w-6/12">
                  <span class="flex">
                    <h1 class="font-[700] text-[35px] leading-[40.25px]">
                      ${{ producto.discount_price }}
                    </h1>

                    <span v-if="producto.precio_comparacion != 0">
                      <h3
                        class="mt-1 pl-2 font-[400] text-[16px] leading-[18.4px]"
                      >
                        Antes
                      </h3>
                      <h3
                        class="pl-6 font-[400] text-[16px] leading-[18.4px] line-through"
                      >
                        ${{ producto.price }}
                      </h3>
                    </span>
                  </span>
                  <button
                    v-if="producto.quantity > 0"
                    class="w-[168.84px] h-58px font-[700] text-[16px] leading-[18.4px] text-[#000000] h-[58px] text-center uppercase border border-black rounded-[5px] mt-20"
                    @click="goToProduct(producto.id)"
                  >
                    {{ $t("Buttons.buy") }}
                  </button>
                  <button
                    v-if="producto.quantity < 1"
                    class="w-[168.84px] h-58px font-[700] text-[16px] leading-[18.4px] text-[#000000] h-[58px] text-center uppercase border border-black rounded-[5px] mt-20 disabled:opacity-50"
                    @click="goToProduct(producto.id)"
                    disabled
                  >
                    {{ $t("Buttons.OutStock") }}
                  </button>
                </span>
                <span class="w-6/12 border-l border-black px-4">
                  <h3 class="mb-3 font-[700] text-[16px] leading-[18.4px]">
                    {{ producto.name }}
                  </h3>
                  <p
                    class="text-[#000000] font-[400] text-[16px] leading-[18.4px]"
                  >
                    {{ producto.short_description }}
                  </p>
                </span>
              </span>
            </li>
          </ul>

          <ul class="lg:grid grid-cols-2 gap-5">
            <li class="" v-for="(i, k) in SoldOutItems" :key="k">
              <span class="flex gap-20">
                <span class="w-7/12">
                  <h3 class="font-[700] text-[16px] leading-[18.4px]">
                    {{ i.name }}
                  </h3>
                  <span v-if="i.precio_comparacion != 0">
                    <h3
                      class="mt-4 mb-2 font-[400] text-[16px] leading-[18.4px]"
                    >
                      Antes
                    </h3>
                    <h3
                      class="line-through font-[400] text-[16px] leading-[18.4px]"
                    >
                      ${{ i.price }}
                    </h3>
                  </span>
                  <h1 class="mt-5 font-[700] text-[35px] leading-[40.25px]">
                    ${{ i.discount_price }}
                  </h1>
                  <button
                    class="w-[168.84px] h-58px font-[700] text-[16px] leading-[18.4px] text-[#000000] h-[58px] text-center uppercase border border-black rounded-[5px] mt-5 disabled:opacity-50"
                    disabled
                  >
                    {{ $t("Buttons.OutStock") }}
                  </button>
                </span>
                <span class="w-5/12 bg-[#CBD402] relative h-[260px]">
                  <img
                    v-if="i.product_images.length > 0"
                    :src="Img_product(i.product_images)"
                    class="w-32 absolute -left-5 h-[204px] top-1/2 transform -translate-y-1/2 object-cover"
                  />
                </span>
              </span>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>
