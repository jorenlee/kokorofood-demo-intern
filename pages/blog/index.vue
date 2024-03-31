<script>
  export default {
    auth: "guest",
    head() {
    return {
      title: "Beneficios e Historia del Ajo Negro | Todo lo que Necesitas Saber",
      link: [
        {
          rel: 'canonical',
          href: 'https://kokorofoods.cl/es/blog'
        }
      ],
      meta: [
        {
          name: "description",
          content:
            "Sumérgete en la fascinante historia del ajo negro y descubre los increíbles beneficios para la salud de este superalimento. Aprende cómo puede mejorar tu bienestar y energía.",
        },
        {
          hid: "description",
          name: "description",
          content: "Sumérgete en la fascinante historia del ajo negro y descubre los increíbles beneficios para la salud de este superalimento. Aprende cómo puede mejorar tu bienestar y energía.",
        },
        {
          hid: "og:title",
          name: "og:title",
          content: "Beneficios e Historia del Ajo Negro | Todo lo que Necesitas Saber",
        },
        {
          hid: "og:description",
          name: "og:description",
          content: "Sumérgete en nuestra colección de recetas de ajo negro. Desde entrantes hasta platos principales, descubre cómo el ajo negro puede transformar tus comidas y mejorar tu energía y bienestar",
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
    async asyncData({ $axios, query, store }) {
      const store_id = store.getters.GetStore;
      const filter = query.filter; // Category id Filter from query parameter
      const endpoint = filter
        ? `/api/main/blogs/?store=${store_id}&category=${filter}`
        : `/api/main/blogs/?store=${store_id}`;
  
      const response = await $axios.$get(endpoint);
      return {
        Blogs: response.results,
        allCategories: response.all_categories,
        nextEndpoint: response.next, // Set the nextEndpoint from the API response
      };
    },
    watch: {
      $route(to, from) {
        if (to.query.filter !== from.query.filter) {
          this.$nuxt.refresh();
        }
      },
    },
  
    methods: {
      async loadMore() {
        if (this.nextEndpoint) {
          const response = await this.$axios.$get(this.nextEndpoint);
          this.Blogs = [...this.Blogs, ...response.results]; // Append new blog posts to the existing ones
          this.nextEndpoint = response.next; // Update the nextEndpoint for further pagination
        }
      },
  
      getBlogThumbnail(images) {
        const image = images.find((image) => image.image_type === "thumbnail");
        return image ? image.image_url : "";
      },
  
      formatDate(date) {
        const newDate = new Date(date);
        const options = { year: "numeric", month: "long", day: "numeric" };
        return newDate.toLocaleDateString("es-ES", options);
      },
      getTagClass(tag) {
        return this.tagColorMap[tag] || "bg-black text-white pl-6 pr-8 pt-3 pb-3"; // if css class not defined in api, this is default
      },
    },
  
    data() {
      return {
        Blogs: [], // An empty array to store the blog posts
        nextEndpoint: null, // Initialize nextEndpoint to null
        allCategories: [],
        BlogPageBannerTitle: "Tips y recetas para disfrutar el ajo negro",
        BlogPageBannerDate: "",
        BlogPageBannerDateDisplay: false,
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
    <div>
      <Header />
      <BlogPageHeader
        :BlogPageBannerTitle="BlogPageBannerTitle"
        :BlogPageBannerDate="BlogPageBannerDate"
        :BlogPageBannerDateDisplay="BlogPageBannerDateDisplay"
        class="mt-[38px]"
      />
      <div class="flex">
        <div class="w-3/12">
          <div class="w-10/12 mx-auto mt-60">
            <h1
              class="text-black font-[Times New Roman] font-[700] text-[24px] leading-[29.26px] mb-4"
            >
              Filtrar por
            </h1>
            <ul class="p-0">
              <li
                class="border-2 border-black font-[Helvetica] font-[700] text-[16px] w-32 px-8 py-3 rounded-md leading-[18.4px] mb-4"
                v-for="(c, i) in allCategories"
                :key="i"
              >
                <NuxtLink
                  :to="localePath(`/blog?filter=${c.id}`)"
                  class="text-black"
                >
                  {{ c.category }}
                </NuxtLink>
              </li>
            </ul>
          </div>
        </div>
        <div class="w-9/12">
          <div class="w-11/12 p-12">
            <p
              class="text-black font-[Helvetica] font-[700] text-[16px] leading-[18.4px]"
            >
              Bienvenido al blog de Kokorofoods, tu recurso confiable para todo lo relacionado con el ajo negro y un estilo de vida saludable. Aquí, exploramos los numerosos beneficios del ajo negro para la salud, desde su capacidad para impulsar tu sistema inmunológico hasta cómo puede mejorar tu energía y vitalidad. Además, ofrecemos una visión profunda de diversos estudios que destacan las mejores prácticas para llevar una vida llena de energía y salud. Nuestro objetivo es proporcionarte la información más actualizada y basada en la evidencia para ayudarte a tomar decisiones informadas sobre tu salud y bienestar. Únete a nosotros en este viaje hacia un estilo de vida más saludable con el ajo negro.
            </p>
          </div>
          <div class="pb-14">
            <div class="w-11/12 mx-auto">
              <h4
                v-if="Blogs.length === 0"
                class="text-black font-[Times New Roman] font-[700] text-[24px] leading-[29.26px] mb-5 text-center"
              >
                No se encontraron blogs
              </h4>
            </div>
            <ul class="gap-14 w-11/12 mx-auto flex flex-wrap">
              <li
                v-for="(item, i) in Blogs"
                :key="i"
                class="w-5/12 h-auto bg-white"
              >
                <ul class="flex font-[700] text-xs leading-[14.52px]">
                  <li
                    v-for="(tag, t) in item.tags"
                    :key="t"
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
                <span class="flex p-3 pb-1 justify-between">
                  <NuxtLink
                    class="flex items-center text-black w-[120px] h-[50px] font-[700] text-[16px] leading-[18.4px] border-2 border-black rounded-md"
                    :to="localePath(`/blog/${item.slug}/`)"
                  >
                    <span class="mx-auto">LEER +</span>
                  </NuxtLink>
  
                  <span class="flex items-center mt-5">
                    <button class="pt-3">
                      <i
                        class="fa fa-share text-[30px] mx-2"
                        aria-hidden="true"
                      ></i>
                    </button>
                    <button class="">
                      <i
                        class="fa fa-bookmark-o text-[30px] mx-2"
                        aria-hidden="true"
                      ></i>
                    </button>
                  </span>
                </span>
              </li>
            </ul>
            <button
              v-if="nextEndpoint"
              class="text-[#FFFFFF] font-[Helvetica] font-[700] text-[16px] leading-[18.4px] bg-black px-5 py-4 w-96 rounded-lg mx-auto block mt-20 mb-3 h-fit"
              @click="loadMore"
            >
              Cargar más
            </button>
          </div>
        </div>
      </div>
    </div>
  </template>
  