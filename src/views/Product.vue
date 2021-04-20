<template>
  <div class="product">
    <HeaderShayna></HeaderShayna>
     <!-- Breadcrumb Section Begin -->
    <div class="breacrumb-section">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="breadcrumb-text product-more">
                        <router-link to="/"><i class="fa fa-home"></i> Home</router-link>
                        <span>Detail</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <!-- Breadcrumb Section Begin -->

    <!-- Product Shop Section Begin -->
    <section class="product-shop spad page-details">
        <div class="container">
            <div class="row">
                <div class="col-lg-12">
                    <div class="row">
                        <div class="col-lg-6">
                            <div class="product-pic-zoom">
                                <img class="product-big-img" :src="defPic" alt="" />
                            </div>
                            <div class="product-thumbs" v-if="product.galleries.length > 0">
                                <carousel class="product-thumbs-track ps-slider" :dots="false" :nav="false">
                                    <div class="pt" @click="changeImg(photos.photo)" :class="photos.photo == defPic ? 'active' : '' " v-for="(photos , index) in product.galleries" :key="index">
                                        <img :src="photos.photo" alt="" />
                                    </div>
                                </carousel>
                            </div>
                        </div>
                        <div class="col-lg-6">
                            <div class="product-details">
                                <div class="pd-title">
                                    <span>{{ product.type }}</span>
                                    <h3>{{ product.name }}</h3>
                                </div>
                                <div class="pd-desc">
                                    <p v-html="product.description"></p>
                                    <h4>${{ product.price }}</h4>
                                </div>
                                <div class="quantity">
                                    <router-link to="/shopping-cart">
                                        <a href="#" class="primary-btn pd-cart" @click="addCart(product.id,product.name,product.price,product.galleries[0].photo)">Add To Cart</a>
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
    <RelatedProduct></RelatedProduct>
    <FooterShayna></FooterShayna>
  </div>
</template>

<script>
import HeaderShayna from '../components/HeaderShayna.vue'
import FooterShayna from '../components/FooterShayna'
import RelatedProduct from '../components/RelatedProduct'
import carousel from 'vue-owl-carousel'
import axios from 'axios'

export default {
  name: 'Product',
  components:{
    HeaderShayna,
    FooterShayna,
    RelatedProduct,
    carousel
  },
  data: function(){
    return{
        defPic: "",
        idProduct : this.$route.params.id,
        product: [],
        keranjangUser: []
    }
  },
  methods : {
      changeImg: function(path){
          this.defPic = path;
      },
      setDataImg: function(data){
          this.product = data;
          this.defPic = data.galleries[0].photo;
      },
      addCart: function(id_product,name,price,photo){
        var productData = {
            id_product,
            name,
            price,
            photo
        }
        this.keranjangUser.push(productData);
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
      axios.get("http://127.0.0.1:8000/api/products",{
          params: {
              id : this.$route.params.id,
          }
      })
      .then(res => (this.setDataImg(res.data.data)))
      .catch(err => console.log(err))
  }
}
</script>

<style scoped>
.product-thumbs .pt{
    margin-right: 14px;
}
p,.breacrumb-section,span,h3,h4{
    text-align: left;
}
</style>
