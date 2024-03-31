<template>
  <div>
    <LazyTheSlider :sliders="sliderData.results" v-if="!mobile" />
    <LazyMobileTheSliderMob :sliders="sliderData.results" v-if="mobile" />
    <LazyTheProductsCategory :Items="productCategory.results" :Lists="Lists" />
    <LazySoldAndHighlightedProducts
      :MasVendidoData="MasVendidoData.results"
      :DestacadoData="DestacadoData.results"
      :SoldOutItems="SoldOutItems"
    />
    <LazyTips :Tips="Tips.results" :TipsTitle="TipsTitle"  />
    <LazyBottomBanner
      :BannerTitle="BannerInfo.BannerTitle"
      :BannerSubTitle="BannerInfo.BannerSubTitle"
      :BannerBtnText="BannerInfo.BannerBtnText"
      :goToDestination="goToDestination"
    />
    <!-- <LazyTheProductM v-if="!mobile" />
    <LazyMobileTheProductMob v-if="mobile" />
    <LazyTheArticleM v-if="!mobile" />
    <LazyMobileTheArticleMMob v-if="mobile" />
    <LazyTheTextArticle v-if="!mobile" />
    <LazyMobileTheTextArticleMob v-if="mobile" />
    <LazyTheMostSale :MasVendidos="MasVendido.results" v-if="!mobile" />
    <LazyMobileTheMostSaleMob :MasVendidosMob="MasVendido.results" v-if="mobile" /> -->
    <!-- <LazyTheDestacado
      :MasDestacado="MasDestacadoG.results"
      :ButtonON="true"
      :ModuleName="name"
      v-if="!mobile"
    /> -->
    <!-- <LazyMobileTheDestacadoMob
      :MasDestacado="MasDestacadoG.results"
      :ButtonON="true"
      :ModuleName="name"
      v-if="mobile"
    />
    <LazyTheStoreBuy
      :title="title"
      :Button_text="bu_text"
      :Image_URL="image_url"
      :Button_link="bu_link"
      :class_section="class_section"
      v-if="!mobile"
    />
    <LazyMobileTheStoreBuyMob
      :title="title"
      :Button_text="bu_text"
      :Image_URL_mob="image_url_mob"
      :Button_link="bu_link"
      :class_section="class_section"
      v-if="mobile"
    /> -->
    <!-- <TheTips /> -->
  </div>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  name: "index",
  auth: "guest",
  async asyncData({ $axios, store }) {
    const store_id = store.getters.GetStore
    const sliderData = await $axios.$get("/api/main/slider/");
    const productCategory = await $axios.$get(
      "/api/main/product_category/?q=homepage"
    );
    const Tips = await $axios.$get("/api/main/blogs/?q=homepage");
    const DestacadoData = await $axios.$get(
      `/api/main/product/?is_destacado=true&store=${store_id}`
    );
    const MasVendidoData = await $axios.$get(
      `/api/main/product/?is_masvendido=true&store=${store_id}`
    );
    const Products = await $axios.$get(`/api/main/product/?store=${store_id}`);
    const SoldOutItems = Products.results
      .filter((item) => item.quantity < 1)
      .slice(0, 2);
    return {
      sliderData,
      productCategory,
      Tips,
      DestacadoData,
      MasVendidoData,
      SoldOutItems,
    };
  },
  head() {
    return {
      title: "Tienda de ajo negro",
      meta: [
        {
          name: "google-site-verification",
          content: "YawYAJZ2jL7NvIUf13QCPadM5MyYk5Izcc99vJmdGec",
        },
        {
          name: "description",
          content:
            "Disfruta de todo los beneficios del ajo negro, compra online con despacho a todo Chile",
        },
        {
          hid: "description",
          name: "description",
          content: "Tienda de ajo negro en capsulas, dientes y pastas",
        },
        {
          hid: "og:title",
          name: "og:title",
          content: "Tienda de ajo negro",
        },
        {
          hid: "og:description",
          name: "og:description",
          content: "Tienda de ajo negro con despacho a todo Chile ",
        },
        {
          hid: "og:image",
          name: "og:image",
          content: "https://kokorofoods.cl/img/img-cont-1.jpg",
        },
        {
          hid: "og:url",
          name: "og:url",
          content: "https://kokorofoods.cl/es",
        },
      ],
    };
  },
  computed: {
    ...mapGetters(["MasVendido", "MasDestacadoG"]),
  },
  data() {
    return {
      name: this.$t("TheDestacadoModule.name"),
      title: this.$t("TheStoreBuyModule.Title"),
      bu_text: this.$t("Buttons.ViewStores"),
      bu_link: this.localePath("/tiendas"),
      image_url: "/img/img-donde.jpg",
      image_url_mob: "/img/mobile/donde-comprar-1.png",
      class_section: "donde-comprar",
      mobile: false,
      BannerInfo: {
        BannerTitle: "Vende en tu tienda",
        BannerSubTitle: "Revisa el catalogo para precios al por mayor",
        BannerBtnText: "VER CATALOGO",
      },
      Items: [
        {
          productName: "Capsulas de ajo negro",
          bgColor: "#D5441C",
          productItems: "merge1",
        },
        {
          productName: "PASTAS de ajo negro",
          bgColor: "#CBD402",
          productItems: "merge1",
        },
        {
          productName: "PRODUCTOS GOURMET",
          bgColor: "#CBD402",
          productItems: "merge1",
        },
      ],
      TipsTitle: ["Tips & Recetas", "Para incorporar el Ajo Negro"],
      Lists: [
        {
          productName: "AYUDA AL COLESTEROL",
          productItems: "Rectangle127",
        },
        {
          productName: "ENERGIZANTE NATURAL",
          productItems: "Rectangle128",
        },
        {
          productName: "AYUDA A LA DIABETES",
          productItems: "Rectangle129",
        },
      ],
    };
  },
  mounted: function () {
    if (screen.width < 769) {
      this.mobile = true;
    }
  },
  methods: {
    goToDestination() {
      this.$router.push(this.localePath(`/es/por-mayor`));
    }
  }
};
</script>

<style scoped>
#main {
  transition: margin-left 0.5s;
  transition: ease-in 0.5s;
  padding: 16px;
}
section {
  padding-top: 100px;
}
.section {
  padding-top: 100px;
  padding-bottom: 50px;
}
@media (min-width: 768px) {
  .bd-placeholder-img-lg {
    font-size: 3.5rem;
  }
}
</style>