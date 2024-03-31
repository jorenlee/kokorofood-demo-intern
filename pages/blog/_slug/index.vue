<script>
export default {
  auth: "guest",
  async asyncData({ $axios, params }) {
    const slugBlog = await $axios.$get(`/api/main/blog/${params.slug}/`);
    // Get banner image
    const mainImage = slugBlog.blog.blog_images.find(
      (image) => image.image_type === "main"
    );
    return {
      blog: slugBlog.blog,
      relatedBlogs: slugBlog.related_blogs,
      BlogBannerImage: mainImage ? mainImage.image_url : "",
    };
  },
   head() {
    return {
      title: this.blog.blog_title,
      meta: [
        {
          hid: "description",
          name: "description",
          content: this.blog.short_description,
        },
        {
          hid: "og:title",
          name: "og:title",
          content: this.blog.blog_title,
        },
        {
          hid: "og:description",
          name: "og:description",
          content: this.blog.short_description,
        },
        {
          hid: "og:image",
          name: "og:image",
          content: this.blog.blog_images[0].image_url,
        },
        {
          hid: "og:url",
          name: "og:url",
          content: "https://kokorofoods.cl/es",
        },
      ],
    };
   },
  data() {
    return {
      TipsTitle: ["Otros Artículos Relacionados"],
      BtnMore: false,
      BannerInfo: {
        BannerTitle: "Conoce nuestros",
        BannerSubTitle: "Productos de ajo negro",
        BannerBtnText: "Ver Catalogo",
      },
      BlogPageBannerDateDisplay: true,
    };
  },
   methods: {
    goToDestination() {
      this.$router.push(this.localePath(`/es/ajonegro/`));
    }
  },
  computed: {},
};
</script>

<template>
  <div>
    <Header />
    <BlogPageHeader
      :BlogPageBannerTitle="blog.blog_title"
      :BlogPageBannerDate="blog.created_at"
      :BlogPageBannerDateDisplay="BlogPageBannerDateDisplay"
      :BlogBannerImage="BlogBannerImage"
      class="mt-[38px]"
    />
    <div class="">
      <div class="w-11/12 mx-auto pt-10 pb-14">
        <div class="">
          <p class="text-black font-[Helvetica] leading-[19.5px]">
            {{ blog.short_description }}
          </p>
          <DynamicBlogContent
            :BlogContent="blog.blog_content"
            class="text-black font-[Helvetica] leading-[19.5px] mb-2"
          />
        </div>
      </div>
    </div>
    <LazyTips
      :Tips="relatedBlogs || []"
      :TipsTitle="TipsTitle"
      :BtnMore="BtnMore"
      class="lg:pb-36 pb-10"
    />
    <LazyBottomBanner
      :BannerTitle="BannerInfo.BannerTitle"
      :BannerSubTitle="BannerInfo.BannerSubTitle"
      :BannerBtnText="BannerInfo.BannerBtnText"
      :goToDestination="goToDestination"
    />
  </div>
</template>
