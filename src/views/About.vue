<template>
  <div class="about">
    <Header></Header>
    <Slider></Slider>
    <div class="col-lg-1-12">
      <div class="card">
        <div class="card-body">
          <h4 class="card-title">Title</h4>
          <img src="img/banner-3.jpg" alt="">
          <div class="form-group" >
            <label for=""></label>
            <select class="form-control" v-model="insert.name">
              <option v-for="data in datas" :key="data.id" > {{ data.nama }} </option>             
            </select>
          </div>
        </div>
      </div>
    </div>
    <Footer></Footer>
  </div>
</template>


<script>
import Header from '@/components/Header.vue'
import Footer from '@/components/Footer.vue'
import Slider from '@/components/Slider.vue'
export default {
  components:{
    Header,Footer,Slider
  },
  data(){
      return {
        insert:[],
        datas:[
          {nama:'sandi'},
          {nama:'agus'},
          {nama:'rendi'}
        ]
         
      }
  },
}
</script>