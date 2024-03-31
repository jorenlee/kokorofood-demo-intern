<template>
    <main role="main">
        <section class="p-0 m-0 col-lg-12 row mt-5">
            <iframe src="https://somoslokal.cl/embed/maker_referred?slug=kokorofoods&with_context=true&without_title=false"
  title="Compra los productos de Kokorofoods en Lokal" width="100%"
  height="700" frameBorder="0"></iframe>
        </section>
    </main>
</template>

<script>
export default {
    auth: 'guest',
    head(){
        return {
    title: 'Compra al por mayor ajo negro',
    meta: [
      {name: "google-site-verification", content: "YawYAJZ2jL7NvIUf13QCPadM5MyYk5Izcc99vJmdGec"},
      {name: "description", content: "Compra ajo negro al por mayor y paga en 60 dias para tiendas fisicas"},
      {
          hid: "description",
          name: "description",
          content:
            "Compra ajo negro al por mayor y paga en 60 dias para tiendas fisicas"
        },
        {
          hid: "og:title",
          name: "og:title",
          content:
            "Compra al por mayor ajo negro",
        },
        {
          hid: "og:description",
          name: "og:description",
          content:
            "Compra ajo negro al por mayor y paga en 60 dias para tiendas fisicas" 
        },
        {
          hid: "og:image",
          name: "og:image",
          content: "https://kokorofoods.cl/img/img-cont-1.jpg",
        },
        {
          hid: "og:url",
          name: "og:url",
          content: "https://kokorofoods.cl/es/por-mayor",
        },
    ]
  }
    },
}
</script>

<style>
h1.titulo-exito {
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    align-content: center;
    justify-content: center;
    align-items: center;
}
img.img-fluid.img-exito {
    width: 75%;
    display: flex;
    flex-direction: row;
    flex-wrap: nowrap;
    align-content: center;
    justify-content: center;
    align-items: center;
    margin: 15px auto;
}
img.icon-ex {
    width: 5%;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}
.center-itms {
    width: 250px;
    display: block !important;
    margin: 20px auto;
    text-align: center;
}
</style>