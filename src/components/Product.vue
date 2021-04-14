<template>
<!-- Women Banner Section Begin -->
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <carousel class="product-slider" :dots="false" :items="3" :nav="false" :autoplay="true" >
                        <div class="product-item mr-3" v-for="product in products" :key="product.id">
                            <div class="pi-pic">
                                <img v-bind:src="product.galleries[0].photos" alt="" />
                                <ul>
                                    <li class="w-icon active">
                                        <a href="#"
                                        @click="saveCart(product.id,product.name,product.price,product.galleries[0].photos)"
                                        ><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view">
                                        <router-link v-bind:to="'/Productdetail/'+product.id">+ Quick View</router-link>
                                    </li>
                                        
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{ product.type }}</div>
                                <router-link v-bind:to="'/Productdetail/'+product.id">
                                        <h5>{{ product.name }}</h5>
                                </router-link>
                                <div class="product-price">
                                    Rp. {{product.price}}
                                    <span>Rp.{{ product.price + 50000 }}</span>
                                </div>
                            </div>
                        </div>
                    </carousel>
                </div>
                <div class=" col-lg-12" v-else>
                    <p>data tidak ditemukan</p>
                </div>
            </div>
        </div>
    </section>
    <!-- Women Banner Section End -->
    
</template>

<script>
import carousel from 'vue-owl-carousel';
import axios from 'axios';

export default {
    name:'Product',
    components: {
        carousel
  },
  data(){
      return {
          products:[],
          cartUser:[]
      }
  },
  methods:{
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
        
        window.location.reload();
    }
  },
  mounted(){
        axios
        .get("http://127.0.0.1:8001/api/products")
        .then(res => (this.products = res.data.data.data))
        .catch(err => console.log(err));

        if (localStorage.getItem('cartUser')) {
            try {
                this.cartUser = JSON.parse(localStorage.getItem('cartUser'));
            } catch(e) {
                localStorage.removeItem('cartUser');
            }
        }
  }
}
</script>

<style scoped>
.product-item .pi-pic img{
    min-width: 100%;
    max-height: 600px;
}
</style>