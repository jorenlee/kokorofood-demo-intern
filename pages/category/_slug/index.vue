<script>
export default {
  auth: "guest",
  async asyncData({ $axios, params, query, store }) {
    const store_id = store.getters.GetStore;
    const categorySlug = params.slug; // Category Slug
    const subCategoryFilter = query.filter; // Sub-Category Filter from query parameter
    const endpoint = subCategoryFilter
      ? `/api/main/products-by-category/${categorySlug}?store=${store_id}&sub_category_slug=${subCategoryFilter}`
      : `/api/main/products-by-category/${categorySlug}?store=${store_id}`; // Use default category endpoint if no subcategory filter

    const categoryData = await $axios.$get(endpoint);
    return { categoryData, filters: categoryData.subcategory };
  },
  // add watch if query.filter values changes then call asyncData again
  watch: {
    $route(to, from) {
      if (to.query.filter !== from.query.filter) {
        this.$nuxt.refresh();
      }
    },
  },
  methods: {
    getProductThumbnail(images) {
      const image = images.find((image) => image.type === "main");
      return image ? image.image_url : "";
    },
  },
  data() {
    return {
      filters: [],
      url_image: '~/assets/img/images/image16.png'
    };
  },
};
</script>
<style scoped>
 .price_line {
  text-decoration: line-through;
}
</style>

<template>
  <div>
    <BlogPageHeader2
      :categoryTitle="categoryData.category_name"
      :imageURL="categoryData.header_image"
    />
    <div class="flex gap-10">
      <div class="w-96">
        <div class="w-6/12 mx-auto mt-60">
          <h1
            class="text-black font-[Times New Roman] font-[700] text-[24px] leading-[29.26px] mb-5"
          >
            Filtrar por
          </h1>
          <ul class="p-0 font-[Helvetica] font-[700] text-[16px] text-black">
            <li
              class="border border-black w-full px-8 py-4 rounded-md leading-[18.4px] mb-4"
              v-for="(f, i) in filters"
              :key="i"
            >
              <NuxtLink
                :to="
                  localePath(`/category/${categoryData.slug}?filter=${f.slug}`)
                "
                class="text-black"
              >
                {{ f.title }}
              </NuxtLink>
            </li>
          </ul>
        </div>
      </div>
      <div class="w-11/12 mx-auto">
        <div class="w-11/12 mx-auto pr-12 py-12">
          <p
            class="text-black font-[Helvetica] font-[700] text-[16px] leading-[18.4px]"
          >
            {{ categoryData.description }}
          </p>
        </div>
        <div class="mx-auto w-11/12 pb-14">
          <div class="w-11/12 mx-auto">
            <h4
              v-if="categoryData.product.length === 0"
              class="text-black font-[Times New Roman] font-[700] text-[24px] leading-[29.26px] mb-5 text-center"
            >
              No hay productos en esta categoría
            </h4>
          </div>
          <ul
            class="grid 2xl:grid-cols-5 xl:grid-cols45 lg:grid-cols-4 md:grid-cols-2 grid-cols-1 gap-10"
          >
            <li
              v-for="(item, i) in categoryData.product"
              :key="i"
              class="text-black"
            >
              <span>
                <img
                  :src="getProductThumbnail(item.product_images)"
                  class="mx-auto h-[300px] object-cover"
                />
              </span>
              <span class="border-b block border-black py-1 min-h-[90px]">
                <h2 class="text-center text-[20px] font-[600] leading-[25px]">
                  {{ item.name }}
                </h2>
              </span>
              <span class="row justify-content-center mt-3">
                  <h3 :class="item.price > item.discount_price ? 'col-6' : 'col-12 text-center'">
                      ${{ item.discount_price }}
                  </h3>
                  <h3 v-if="item.price > item.discount_price" class="font-weight-lighter price_line col-6">
                      ${{item.price}}
                  </h3>
              </span>

              <NuxtLink
                class="text-md text-black hover:text-black font-bold leading-[18px] mx-auto border border-black w-fit block py-3 mt-4 px-6 rounded-md"
                :to="localePath('/productos/' + item.id)"
                >COMPRAR</NuxtLink
              >
            </li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>
