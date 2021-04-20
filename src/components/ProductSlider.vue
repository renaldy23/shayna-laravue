<template>
    <section class="women-banner spad">
        <div class="container-fluid">
            <div class="row">
                <div class="col-lg-12 mt-5" v-if="products.length > 0">
                    <h3 class="text-center mb-4">Recently Added</h3>
                    <carousel class="product-slider" :nav="false" :items="3" :autoplay="true" :dots="false">
                        <div class="product-item" v-for="(items,index) in products" :key="index">
                            <div class="pi-pic">
                                <img :src="items.galleries[0].photo" alt="" />
                                <ul>
                                    <li class="w-icon active">
                                        <a href="#" @click="addCart(items.id,items.name,items.price,items.galleries[0].photo)"><i class="icon_bag_alt"></i></a>
                                    </li>
                                    <li class="quick-view"><router-link :to="'/product/'+items.id">+ Quick View</router-link></li>
                                </ul>
                            </div>
                            <div class="pi-text">
                                <div class="catagory-name">{{ items.type }}</div>
                                <a href="#">
                                    <h5>{{ items.name }}</h5>
                                </a>
                                <div class="product-price">
                                    ${{ items.price }}
                                    <span>$35.00</span>
                                </div>
                            </div>
                        </div>
                    </carousel>
                    <div class="d-flex justify-content-center">
                        <router-link href="#" class="primary-btn">More</router-link>
                    </div>
                </div>
                <div class="col-lg-12 mt-5" v-else>
                    <p class="text-center">Produk belum tersedia</p>
                </div>
            </div>
        </div>
    </section>
</template>

<script>
import carousel from 'vue-owl-carousel'
import axios from 'axios'

export default {
    name: 'ProductSlider',
    components: {
        carousel
    },
    data: function(){
        return {
            products: [],
            keranjangUser: []
        }
    },
    methods: {
        addCart(id_product,name,price,photo){
            var dataProduct = {
                id_product,
                name,
                price,
                photo
            }

            this.keranjangUser.push(dataProduct);
            const parsed = JSON.stringify(this.keranjangUser)
            localStorage.setItem("keranjangUser",parsed)
            window.location.reload();
        }
    },
    mounted(){
        if (localStorage.getItem('keranjangUser')) {
            try {
                this.keranjangUser = JSON.parse(localStorage.getItem('keranjangUser'));
            } catch(e) {
                localStorage.removeItem('keranjangUser');
            }
        }
        axios.get("http://127.0.0.1:8000/api/products")
        .then(res => (this.products = res.data.data.data))
        .catch(err => console.log(err))
    }
}
</script>

<style scoped>
.product-item{
    margin-right: 25px;
}
</style>