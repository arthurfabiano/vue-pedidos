<template>
  <NavBar :logo="logo_src" :alt="app_name"/>
  <router-view/>
  <Footer/>
</template>

<script>
  import NavBar from "@/components/NavBar.vue";
  import Footer from "@/components/Footer.vue";

  export default {
      components: {
          NavBar,
          Footer
      },
      data() {
          return {
              logo_src: '/img/logo.png',
              app_name: 'Meu Pedido'
          }
      }
  }
</script>

<style>
  * {
      font-family: Helvetica, sans-serif;
      padding: 0;
      margin: 0;
      box-sizing: border-box;
  }
  .main-container {
      margin: 50px;
      min-height: 250px;
  }
  h1 {
      text-align: center;
      font-size: 42px;
      margin-bottom: 40px;
      color: #222;
  }
</style>