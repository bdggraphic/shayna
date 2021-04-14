<template>
    <div class="ShoppingCart">
    <Header></Header>
    <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section text-left">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more">
                        <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                        <span>Shopping Cart</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Shopping Cart Section Begin -->
    <section class="shopping-cart spad">
        <div class="container">
            <div class="row">
                <div class="col-lg-8">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="cart-table">
                                <table>
                                    <thead>
                                        <tr>
                                            <th>Image</th>
                                            <th class="p-name text-center">Product Name</th>
                                            <th>Price</th>
                                            <th>Action</th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="cart in cartUser" :key="cart.id">
                                            <td class="cart-pic first-row">
                                                <img :src="cart.photos" />
                                            </td>
                                            <td class="cart-title first-row text-center">
                                                <h5>{{ cart.name }}</h5>
                                            </td>
                                            <td class="p-price first-row">Rp. {{ cart.price }} </td>
                                            <td class="delete-item"><a @click="removeItem(cart.id)" href="#"><i class="material-icons">
                                              close
                                              </i></a></td>
                                        </tr>
                                    </tbody>
                                </table>
                            </div>
                        </div>
                        <div class="col-lg-8">
                            <h4 class="mb-4 text-left">
                                Informasi Pembeli:
                            </h4>
                            <div class="user-checkout text-left">
                                <form>
                                    <div class="form-group">
                                        <label for="namaLengkap">Nama lengkap</label>
                                        <input 
                                            v-model="customerInfo.name"
                                            type="text" class="form-control" id="namaLengkap" aria-describedby="namaHelp" placeholder="Masukan Nama">
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">Email Address</label>
                                        <input 
                                            v-model="customerInfo.email"
                                            type="email" class="form-control" id="emailAddress" aria-describedby="emailHelp" placeholder="Masukan Email">
                                    </div>
                                    <div class="form-group">
                                        <label for="namaLengkap">No. HP</label>
                                        <input 
                                            v-model="customerInfo.phone"
                                            type="text" class="form-control" id="noHP" aria-describedby="noHPHelp" placeholder="Masukan No. HP">
                                    </div>
                                    <div class="form-group">
                                        <label for="alamatLengkap">Alamat Lengkap</label>
                                        <textarea class="form-control" id="alamatLengkap" rows="3" v-model="customerInfo.address"></textarea>
                                    </div>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="col-lg-4">
                    <div class="row">
                        <div class="col-lg-12">
                            <div class="proceed-checkout text-left">
                                <ul>
                                    <li class="subtotal">ID Transaction <span>#SH12000</span></li>
                                    <li class="subtotal mt-3">Subtotal <span>Rp. {{ totalPrice }} </span></li>
                                    <li class="subtotal mt-3">Pajak <span>10% ({{ pajak }})</span></li>
                                    <li class="subtotal mt-3">Total Biaya <span>Rp. {{ grandTotal }}</span></li>
                                    <li class="subtotal mt-3">Bank Transfer <span>Mandiri</span></li>
                                    <li class="subtotal mt-3">No. Rekening <span>2208 1996 1403</span></li>
                                    <li class="subtotal mt-3">Nama Penerima <span>Shayna</span></li>
                                </ul>
                                <router-link to="/Success" class="proceed-btn">
                                    <a href="#" @click="checkout()" >I ALREADY PAID</a>
                                </router-link>
                                
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <!-- Shopping Cart Section End -->
    <Footer></Footer>
    </div>
</template>

<script>
import Header from '@/components/Header.vue'
import Footer from '@/components/Footer.vue'
import axios from 'axios'

export default {
    name:'ShoppingCart',
    components: {
        Header,
        Footer
    },
    data() {
        return{
            cartUser: [],
            customerInfo: {}
        }
    },
    methods:{
        removeItem(idProduct){
            //breakdown item di local storage menggunakan fungsi map()
            let cartUserStorage = JSON.parse(localStorage.getItem('cartUser'));
            let itemStorage = cartUserStorage.map(itemStorage => itemStorage.id);
            
            //cari idproduct yg sama menggunakan fungsi findIndex()
            let index = itemStorage.findIndex(id => id == idProduct);
            //hapus data local storage menggunakan fungsi splice()
            this.cartUser.splice(index,1);

            const parsed = JSON.stringify(this.cartUser);
            localStorage.setItem('cartUser', parsed);            
            window.location.reload();
        },
        //fungsi mengirim data ke API
        checkout(){
            let productsID = this.cartUser.map(function(product){
                return product.id
            });

            let checkoutData = {
                'name': this.customerInfo.name,
                'email': this.customerInfo.email,
                'phone': this.customerInfo.phone,
                'address': this.customerInfo.address,
                'transaction_total': this.grandTotal,
                'transaction_status': 'PENDING',
                'transaction_details': productsID
            }

            axios
            .post('http://127.0.0.1:8000/api/checkout', checkoutData)
            .catch(err => {
                console.log(err);
            });

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
    },
    computed:{
        totalPrice(){
            return this.cartUser.reduce(function(items, data){
                return items + data.price;
            }, 0);
        },
        pajak(){
            return (this.totalPrice * 10) / 100;  
        },
        grandTotal(){
            return this.pajak + this.totalPrice;
        }
    }
}
</script>