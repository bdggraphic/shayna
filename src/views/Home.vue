<template>
  <div class="home">
    <Header></Header>
    <Slider></Slider>
    <Product></Product>
    <Insta></Insta>
    <Logo></Logo>
    <Footer></Footer>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import Header from '@/components/Header.vue'
import Slider from '@/components/Slider.vue'
import Product from '@/components/Product.vue'
import Insta from '@/components/Insta.vue'
import Logo from '@/components/Logo.vue'
import Footer from '@/components/Footer.vue'



export default {
  name: 'Home',
  components: {
    Header,
    Slider,
    Product,
    Insta,
    Logo,
    Footer
  }
}
</script>
