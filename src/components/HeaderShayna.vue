<template>
    <!-- Header Section Begin -->
    <header class="header-section">
        <div class="header-top">
            <div class="container">
                <div class="ht-left">
                    <div class="mail-service">
                        <i class=" fa fa-envelope"></i> shayna.info@shayna.com
                    </div>
                    <div class="phone-service">
                        <i class=" fa fa-phone"></i> +62-821-2200-2212
                    </div>
                </div>
            </div>
        </div>
        <div class="container">
            <div class="inner-header">
                <div class="row">
                    <div class="col-lg-2 col-md-2">
                        <div class="logo">
                            <router-link to="/">
                                <img src="../../public/img/logo_website_shayna.png" alt="" />
                            </router-link>
                        </div>
                    </div>
                    <div class="col-lg-7 col-md-7"></div>
                    <div class="col-lg-3 text-right col-md-3">
                        <ul class="nav-right">
                            <li class="cart-icon">
                                Cart &nbsp;
                                <a href="#">
                                    <i class="icon_bag_alt"></i>
                                    <span>{{ keranjangUser.length }}</span>
                                </a>
                                <div class="cart-hover">
                                    <div class="select-items">
                                        <table>
                                            <tbody v-if="keranjangUser.length>0">
                                                <tr v-for="(cart , index) in keranjangUser" :key="index">
                                                    <td class="si-pic">
                                                        <img :src="cart.photo" alt="" width="100" />
                                                    </td>
                                                    <td class="si-text">
                                                        <div class="product-selected">
                                                            <p>${{ cart.price }} x 1</p>
                                                            <h6>{{ cart.name }}</h6>
                                                        </div>
                                                    </td>
                                                    <td class="si-close" @click="removeItem(index)">
                                                        <i class="ti-close"></i>
                                                    </td>
                                                </tr>
                                            </tbody>
                                            <tbody v-else>
                                                <tr>
                                                    <td>Keranjang Kosong</td>
                                                </tr>
                                            </tbody>
                                        </table>
                                    </div>
                                    <div class="select-total">
                                        <span>total:</span>
                                        <h5>${{ totalHarga }}.00</h5>
                                    </div>
                                    <div class="select-button">
                                        <router-link to="/shopping-cart" class="primary-btn view-card">VIEW CARD</router-link>
                                        <a href="#" class="primary-btn checkout-btn">CHECK OUT</a>
                                    </div>
                                </div>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </header>
    <!-- Header End -->
</template>

<script>
export default {
    name: 'Header',
    data: function(){
        return {
            keranjangUser: [],
        }
    },
    methods: {
        removeItem: function(index){
            this.keranjangUser.splice(index,1);
            const parsed = JSON.stringify(this.keranjangUser)
            localStorage.setItem("keranjangUser",parsed)
            window.location.reload()
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
    },
    computed: {
        totalHarga(){
            return this.keranjangUser.reduce((items,data)=>{
                return items + data.price
            },0)
        }
    }
}
</script>

<style scoped>

</style>