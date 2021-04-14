<template>
  <div class="Productdetail">
    <Header></Header>
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more text-left">
                        <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details text-left">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-6">
                            <div class="product-pic-zoom">
                                <img class="product-big-img" :src="img_default" alt="" />
                            </div>
                            <div class="product-thumbs" v-if="productDetails.galleries.length > 0">
                                <carousel class="product-thumbs-track ps-slider" :dots="false" :nav="false">
                                    <div 
                                        v-for="photo in productDetails.galleries"
                                        :key="photo.id"
                                        class="pt" 
                                        @click="changeImage(photo.photos)" :class="photo.photos == img_default ? 'active' : ''">
                                        <img :src="photo.photos" alt="" />
                                    </div>
                                </carousel>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details">
                                <div class="pd-title">
                                    <span> {{ productDetails.type }} </span>
                                    <h3>{{ productDetails.name }}</h3>
                                </div>
                                <div class="pd-desc">
                                    <p>
                                        {{ productDetails.description }}
                                    </p>
                                    <h4>Rp. {{ productDetails.price }}</h4>
                                </div>
                                <div class="quantity">
                                  <router-link to="/ShoppingCart">
                                    <a 
                                      href="#"
                                        class="primary-btn pd-cart"
                                        @click="saveCart(productDetails.id,productDetails.name,productDetails.price,productDetails.galleries[0].photos)"
                                        >Add To Cart
                                    </a>
                                  </router-link>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Product Shop Section End -->

    <Related></Related>

    <Footer></Footer>
  </div>
</template>

<script>
// @ is an alias to /src
// import HelloWorld from '@/components/HelloWorld.vue'
import Header from '@/components/Header.vue'
import Footer from '@/components/Footer.vue'
import Related from '@/components/Related.vue'
import carousel from 'vue-owl-carousel'

import axios from 'axios'

export default {
  name: 'Productdetail',
  components: {
    Header,
    Footer,
    carousel,
    Related
  },
  data() {
    return{
      img_default: '',
      productDetails: [],
      cartUser: []
    }
  },
  methods:{
    changeImage(urlImage){
      this.img_default = urlImage;
    },
    setDataPicture(data){
        // replace object productDetails dengan data dari API
        this.productDetails = data;
        // replace value gambar default dengan data dari API(galleries)
        this.img_default = data.galleries[0].photos;
    },
    saveCart(idProduct, nameProduct, priceProduct, photoProduct){
        
        var productStored = {
            'id': idProduct,
            'name': nameProduct,
            'price': priceProduct,
            'photos': photoProduct
        }
        this.cartUser.push(productStored);
        const parsed = JSON.stringify(this.cartUser);
        localStorage.setItem('cartUser', parsed);
    }
  },
  mounted(){
    if (localStorage.getItem('cartUser')) {
      try {
        this.cartUser = JSON.parse(localStorage.getItem('cartUser'));
      } catch(e) {
        localStorage.removeItem('cartUser');
      }
    }
      axios
        .get("http://127.0.0.1:8000/api/products",{
            params:{
                id:this.$route.params.id
            }
        })
        .then(res => (this.setDataPicture(res.data.data)))
        .catch(err => console.log(err)); 
  }
}
</script>

<style scoped>
.product-thumbs .pt{
  margin-right: 10px;
}
</style>
