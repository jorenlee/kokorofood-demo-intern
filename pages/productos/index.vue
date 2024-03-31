<template>
  <main role="main">
    <section class="py-3 bg-[#2ddc55] px-3 mt-20 pb-4" v-if="CartMessage">
      <p class="mb-3 justify-center text-center text-white">
        {{ $t("Alertas.Agrego_carro") }}
      </p>
      <button type="button" class="bg-black text-white uppercase tracking-widest px-3 py-2 block w-fit mx-auto" @click="viewCart">
        {{ $t("Alertas.button_pagar") }}
      </button>
    </section>
    <section class="info-ajo container col-md-12 section">
      <LazyTheHeaderOP
        :title="Title"
        :image="Image_url"
        :background="Background_color"
        v-if="!mobile"
      />
      <LazyMobileTheHeaderOPMob :title="Title" v-if="mobile" />
    </section>
    <LazyTheMostSale :MasVendidos="MasVendido.results" v-if="!mobile" />
    <LazyMobileTheMostSaleMob
      :MasVendidosMob="MasVendido.results"
      v-if="mobile"
    />
    <LazyTheDestacado
      :MasDestacado="MasDestacadoG.results"
      :ButtonON="true"
      :ModuleName="name"
      v-if="!mobile"
    />
    <LazyMobileTheDestacadoMob
      :MasDestacado="MasDestacadoG.results"
      :ButtonON="true"
      :ModuleName="name"
      v-if="mobile"
    />
    <section
      class="info-productos container"
      :class="{ 'col-md-12': !mobile, 'col-12 p-0': mobile }"
    >
      <div class="row info-text-cat featurette" :class="{ 'p-0': mobile }">
        <div
          :class="{
            'col-md-4 order-md-1': !mobile,
            'col-12 fondo-prd p-5': mobile,
          }"
        >
          <div class="categorizacion">
            <div class="categorias">
              <h1
                :class="{ 'n-articulo': !mobile, 'n-articulo-lg mt-5': mobile }"
              >
                Filtrar por
              </h1>
              <input
                v-for="category in CategoryData.results"
                :key="category.id"
                :class="{
                  'btn btn-secondary text-nowrap': !mobile,
                  'btn btn-primary btn-light text-nowrap': mobile,
                }"
                type="button"
                :value="category.category"
                @click="filterCategory(category.id)"
              />
            </div>
            <div class="pagination"></div>
          </div>
          <input
            class="btn btn-primary text-nowrap"
            :class="{ 'btn-light m-1': mobile }"
            type="reset"
            value="Limpiar"
          />
        </div>
        <div class="col-md-8 order-md-2">
          <!-- paginacion con productoas -->
          <div class="col-md-12 row">
            <!-- paginacion-->
            <div class="pagination">
              <nav aria-label="Page navigation">
                <ul class="pagination">
                  <li class="page-item">
                    <nuxt-link
                      class="page-link"
                      :to="localePath('/productos')"
                      @click.native="previousPage"
                      aria-label="Previous"
                      v-if="ProductoData.previous"
                    >
                      <span aria-hidden="true">&laquo;</span>
                    </nuxt-link>
                  </li>
                  <li class="page-item">
                    <p class="page-link">
                      <strong>Página {{ page }}</strong
                      >/{{ countPages }}
                    </p>
                  </li>
                  <li class="page-item">
                    <nuxt-link
                      class="page-link"
                      :to="localePath('/productos')"
                      @click.native="nextPage"
                      aria-label="Next"
                      v-if="ProductoData.next"
                    >
                      <span aria-hidden="true">&raquo;</span>
                    </nuxt-link>
                  </li>
                </ul>
              </nav>
            </div>
          </div>
          <div
            class="single-articulo row"
            :class="{ 'col-md-12': !mobile, 'col-12': mobile }"
            v-for="(product, index) in chunkedProducts"
            :key="index"
          >
            <!-- Articulos -->
            <div
              class="card card-art"
              :class="{ 'col-md-4': !mobile, 'col-12 p-2': mobile }"
              v-for="item in product"
              :key="item.id"
            >
              <!-- Articulos 1 -->
              <img
                :src="Img_product(item.product_images)"
                class="card-img-top img-fluid"
                :class="{ 'p-5': mobile }"
                alt="articulo 1"
              />
              <div class="card-body">
                <h5 class="card-title">{{ item.name }}</h5>
                <div class="row">
                  <div class="precio-normal" v-if="item.precio_comparacion > item.discount_price">
                    <p>
                      <span>${{ item.precio_comparacion }}</span>
                    </p>
                  </div>
                  <div class="precio-oferta">
                    ${{ $n(item.discount_price, "currency") }}
                  </div>
                </div>
                <NuxtLink
                  :class="{
                    'btn btn-secondary': !mobile, 
                    'btn btn-primary center-button-2': mobile,
                  }"
                  :to="localePath('/productos/' + item.id)"
                  v-if="item.quantity > 0"
                  >{{ $t("Buttons.buy") }}</NuxtLink
                >
               
                <NuxtLink
                  :class="{
                    'btn btn-secondary': !mobile, 
                    'btn btn-primary center-button-2': mobile,
                  }"
                  :to="localePath('/productos/' + item.id)"
                  v-if="item.quantity < 1"
                  >{{ $t("Buttons.OutStock") }}</NuxtLink
                >
                
              </div>
            </div>
          </div>
          <div class="col-md-12 row">
            <!-- paginacion-->
            <div class="pagination">
              <nav aria-label="Page navigation">
                <ul class="pagination">
                  <li class="page-item">
                    <nuxt-link
                      class="page-link"
                      :to="localePath('/productos')"
                      @click.native="previousPage"
                      aria-label="Previous"
                      v-if="ProductoData.previous"
                    >
                      <span aria-hidden="true">&laquo;</span>
                    </nuxt-link>
                  </li>
                  <li class="page-item">
                    <p class="page-link">
                      <strong>Página {{ page }}</strong
                      >/{{ countPages }}
                    </p>
                  </li>
                  <li class="page-item">
                    <nuxt-link
                         class="page-link"
                         :to="localePath('/productos')"
                         @click.native="nextPage"
                         aria-label="Next"
                         v-if="ProductoData.next">
                         <span aria-hidden="true">
                           &raquo;
                         </span>
                    </nuxt-link>
                  </li>
                </ul>
              </nav>
            </div>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

<script>
var chunk = require('chunk')
import { mapGetters } from 'vuex'
export default {
    auth: 'guest',
    async asyncData( { $axios, store }) {
        const store_id = store.getters.GetStore
        const CategoryData = await $axios.$get('/api/main/product_category/')
        const ProductoData = await $axios.$get('/api/main/product/?store='+ store_id)
    return { CategoryData, ProductoData }
    },
     head() {
		return {
		title: "Productos de Ajo Negro | Mejora tu Salud y Cocina con Kokorooods",
		meta: [
			{
			name: "description",
			content:
				"Explora nuestra amplia gama de productos de ajo negro, perfectos tanto para los amantes de la cocina como para los que buscan mejorar su salud. ¡Descubre el poder del ajo negro hoy!",
			},
			{
			hid: "description",
			name: "description",
			content: "Explora nuestra amplia gama de productos de ajo negro, perfectos tanto para los amantes de la cocina como para los que buscan mejorar su salud. ¡Descubre el poder del ajo negro hoy!",
			},
			{
			hid: "og:title",
			name: "og:title",
			content: "Productos de Ajo Negro | Mejora tu Salud y Cocina con Kokorooods",
			},
			{
			hid: "og:description",
			name: "og:description",
			content: "Explora nuestra amplia gama de productos de ajo negro, perfectos tanto para los amantes de la cocina como para los que buscan mejorar su salud. ¡Descubre el poder del ajo negro hoy!",
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
    data () {
        return {
        Title: this.$t('TheHeaderOP.title'),
        Image_url: '/img/tips.jpg',
        Background_color: "#000000",
        text_destacados:"La mayoría de los estudios del ajo negro utilizan una dosis entre 3 gr a 10 gr de ajo negro por día. Estas dosis varían de acuerdo al formato del ajo negro que puede ser extracto, pasta, diente o en polvo.\
        <p> Si es en pasta, la dosis recomendad es una cucharadita de té al día, lo que equivale a 6gr a 8gr de pasta.</p>",
        name: this.$t('TheDestacadoModule.name'),
        page: 1,
        mobile: false,
        lang: this.$i18n.locale
        }
    },
    watch: {
        lang(value){
            console.log(value)
            const ProductoData = this.$store.dispatch('StoreCatalogChange', value)
            return { ProductoData}
        }
    },
    computed: {
        ...mapGetters(['MasVendido', 'MasDestacadoG', 'GetStore']),
        ...mapGetters('cart', ['CartMessage']),
        chunkedProducts() {
            return chunk(this.ProductoData.results, 3)
        },
        countPages() {
            var pages = Math.ceil(this.ProductoData.count/6)
            return ('0' + pages).slice(-2)
        },
    },
    methods: {
        async filterCategory(id) {
            const store = this.$store.getters.GetStore
            let response = await this.$axios.$get('/api/main/product/?category=' + id + '&store=' + store)
            this.ProductoData = response
        },
        async nextPage(){
            let response = await this.$axios.$get(this.ProductoData.next)
            this.ProductoData = response
            this.page += 1
            this.page = ('0' + this.page).slice(-2)

        },
        async previousPage(){
            let response = await this.$axios.$get(this.ProductoData.previous)
            this.ProductoData = response
            this.page -= 1
            this.page = ('0' + this.page).slice(-2)
        },
        viewCart: function () {
        this.$router.push(this.localePath('/carro'))
      },
       Img_product(images) {
            for (var i=0; i<images.length; i++){
                if ( images[i].type == 'main'){
                    var url = images[i].image_url
                }
            }
        return url
        }
    },
    mounted: function() {
      if (screen.width < 769 ){
        this.mobile = true
      }
    }

}
</script>

<style>
@import "~/assets/css/mobile.css";
@media screen and (min-width: 768px) {
  .categorias input {
    margin: 10px 10px 10px 0;
  }
  .categorias {
    flex-wrap: wrap;
  }
  .categorizacion {
    padding: 0 !important;
  }
  .info-productos {
    background-color: #f4f5e6;
    padding-left: 80px !important;
    padding-right: 80px !important;
  }
  .categorizacion {
    padding: 0 !important;
  }
  img.img-fluid.separador {
    padding-bottom: 35px;
  }
  .featurette-heading {
    margin-top: 0 !important;
    color: #000 !important;
    font-weight: bold !important;
    font-size: 2.5em !important;
  }
  .featurette-heading span {
    font-weight: normal !important;
    color: #000 !important;
  }
  h4.featurette-title {
    color: #000;
    font-weight: 400;
  }
  .info-text p {
    color: #000;
    font-weight: 400;
  }
  .card-art {
    background-color: #fff0 !important;
  }
  .card {
    background-color: none !important;
    border: none !important;
  }
  .card-body {
    padding: 10px 0 !important;
  }
  .card-title {
    color: #000;
    font-weight: 400;
  }
  .card-text {
    font-weight: 300;
  }
  .precio {
    margin-left: 0 !important;
    margin-right: 0 !important;
  }
  .precio-oferta {
    padding-left: 0 !important;
    padding-right: 0 !important;
  }
  .precio-normal {
    padding-left: 20px !important;
  }
  .precio-normal h4,
  .precio-normal p {
    color: #000;
    font-size: 1em;
    font-weight: 400;
    text-transform: capitalize;
  }
  .precio-normal h4 > span,
  .precio-normal p > span {
    color: #000;
    font-size: 1em;
    font-weight: 400;
    text-decoration: line-through;
  }
  .precio-dest .precio-normal {
    padding: 0 !important;
  }
}
</style>