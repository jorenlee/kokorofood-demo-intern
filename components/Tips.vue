<script>
export default {
  props: {
    Tips: Array,
    TipsTitle: Array,
    BtnMore: Boolean,
  },
  methods: {
    formatDate(date) {
      const newDate = new Date(date);
      const options = { year: "numeric", month: "long", day: "numeric" };
      return newDate.toLocaleDateString("es-ES", options);
    },
    getBlogThumbnail(blog_images) {
      if (blog_images) {
        const image = blog_images.find(
          (image) => image.image_type === "thumbnail"
        );
        return image ? image.image_url : "";
      }
    },
    getTagClass(tag) {
      return this.tagColorMap[tag] || "bg-black text-white pl-6 pr-8 pt-3 pb-3"; // if css class not defined in api, this is default
    },
  },
  data() {
    return {
      //Tage color map for blog posts, keys should be provided by api
      tagColorMap: {
        receta: "bg-black text-white pl-6 pr-8 pt-3 pb-3",
        ensalada: "bg-[#CBD402] pl-6 pr-8 pt-3 pb-3 text-black font-semibold",
        vegan: "bg-[#D5441C] pl-6 pr-8 pt-3 pb-3 text-white",
      },
    };
  },
};
</script>

<template>
  <div class="font-[Helvetica] bg-[#F6F0EE] py-14">
    <div>
      <h1
        class="font-[700] lg:text-[55px] text-lg leading-[63.24px] justify-center text-center lg:pb-14 pb-3 text-[#000000]"
      >
        {{ TipsTitle[0] }}<br />
        {{ TipsTitle[1] }}
      </h1>
      <ul class="gap-5 w-11/12 mx-auto lg:flex">
        <li v-for="(item, i) in Tips" :key="i" class="lg:w-1/3 h-auto bg-white">
          <ul class="flex font-[700] text-xs leading-[14.52px]">
            <li
              v-for="(tag, j) in item.tags"
              :key="j"
              :class="getTagClass(tag.class_css)"
            >
              {{ tag.tag }}
            </li>
          </ul>

          <img
            :src="getBlogThumbnail(item.blog_images)"
            class="w-full pr-2 -mt-0.5"
          />
          <span class="block p-3">
            <h1
              class="font-[Times New Roman] font-[700] text-[18px] leading-[21.94px]"
            >
              {{ item.blog_title }}
            </h1>
            <h2
              class="font-[Times New Roman] font-[100] italic text-[13px] leading-[15.85px] my-3 text-[#a5a5a5]"
            >
              {{ formatDate(item.created_at) }}
            </h2>

            <p
              class="text-black font-[400] text-[13px] leading-[15.85px] w-full h-[50px]"
            >
              {{ item.short_description }}
            </p>
          </span>
          <span class="flex px-3 pt-3 pb-2 justify-between">
            <NuxtLink
              class="flex items-center text-black w-[120px] h-[50px] font-[700] text-[16px] leading-[18.4px] border-2 border-black rounded-md"
              :to="localePath(`/blog/${item.slug}`)"
            >
              <span class="mx-auto">LEER +</span>
            </NuxtLink>
            <span class="flex items-center mt-3 pb-2">
              <button class="">
                <img
                  class="share"
                  src="~/assets/img/icons/Icons-icon-share-filled.jpg"
                  alt="Share"
                />
              </button>
              <button class="">
                <img
                  class="bookmark"
                  src="~/assets/img/icons/Icons-icon-bookmark-filled.jpg"
                  alt="Bookmark"
                />
              </button>
            </span>
          </span>
        </li>
      </ul>
      <NuxtLink
        v-show="BtnMore"
        class="text-center flex items-center w-[200px] h-[55px] text-white bg-black mx-auto block mt-20 rounded-md font-[700] text-sm leading-[16.1px]"
        :to="localePath('/blog')"
      >
        <span class="mx-auto">IR AL BLOG</span>
      </NuxtLink>
    </div>
  </div>
</template>
