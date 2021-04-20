<template>
    <div class="cart">
        <HeaderShayna></HeaderShayna>
        <div class="breacrumb-section">
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
        <!-- Shopping Cart Section Begin -->
        <section class="shopping-cart">
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
                                        <tbody v-if="keranjangUser.length>0">
                                            <tr v-for="(cart,index) in keranjangUser" :key="index">
                                                <td class="cart-pic first-row">
                                                    <img :src="cart.photo" />
                                                </td>
                                                <td class="cart-title first-row text-center">
                                                    <h5>{{ cart.name }}</h5>
                                                </td>
                                                <td class="p-price first-row">${{ cart.price }}</td>
                                                <td class="delete-item" @click="removeItem(index)"><a href="#"><i class="material-icons">
                                                close
                                                </i></a></td>
                                            </tr>
                                        </tbody>
                                        <tbody v-else>
                                            <tr>
                                                <td colspan="4">Belum ada data pada keranjang</td>
                                            </tr>
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                            <div class="col-lg-8">
                                <h4 class="mb-4">
                                    Informasi Pembeli:
                                </h4>
                                <div class="user-checkout">
                                    <form>
                                        <div class="form-group">
                                            <label for="namaLengkap">Nama lengkap</label>
                                            <input type="text" class="form-control" id="namaLengkap" aria-describedby="namaHelp" placeholder="Masukan Nama" v-model="custInfo.name">
                                        </div>
                                        <div class="form-group">
                                            <label for="namaLengkap">Email Address</label>
                                            <input type="email" class="form-control" id="emailAddress" aria-describedby="emailHelp" placeholder="Masukan Email" v-model="custInfo.email">
                                        </div>
                                        <div class="form-group">
                                            <label for="namaLengkap">No. HP</label>
                                            <input type="text" class="form-control" id="noHP" aria-describedby="noHPHelp" placeholder="Masukan No. HP" v-model="custInfo.number">
                                        </div>
                                        <div class="form-group">
                                            <label for="alamatLengkap">Alamat Lengkap</label>
                                            <textarea class="form-control" id="alamatLengkap" rows="3" v-model="custInfo.address"></textarea>
                                        </div>
                                    </form>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="col-lg-4">
                        <div class="row">
                            <div class="col-lg-12">
                                <div class="proceed-checkout">
                                    <ul>
                                        <li class="subtotal">ID Transaction <span>#SH12000</span></li>
                                        <li class="subtotal mt-3">Subtotal <span>${{ totalHarga }}.00</span></li>
                                        <li class="subtotal mt-3">Pajak <span>10%</span></li>
                                        <li class="subtotal mt-3">Total Biaya <span>${{ totalBiaya }}.00</span></li>
                                        <li class="subtotal mt-3">Bank Transfer <span>Mandiri</span></li>
                                        <li class="subtotal mt-3">No. Rekening <span>2208 1996 1403</span></li>
                                        <li class="subtotal mt-3">Nama Penerima <span>Shayna</span></li>
                                    </ul>
                                    <a to="#" @click="checkout()" class="proceed-btn">I ALREADY PAID</a>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>
        <!-- Shopping Cart Section End -->
    </div>
</template>

<script>
import HeaderShayna from '../components/HeaderShayna'
import axios from 'axios';

export default {
    name: "ShoppingCart",
    data: function(){
        return {
            keranjangUser: [],
            custInfo : {
                name: '',
                email: '',
                number: '',
                address: ''
            }
        }
    },
    methods: {
        removeItem: function(index){
            this.keranjangUser.splice(index,1);
            const parsed = JSON.stringify(this.keranjangUser)
            localStorage.setItem("keranjangUser",parsed)
        },
        checkout: function(){
            let id_products = this.keranjangUser.map(function(product){
                return product.id_product;
            });
            console.log(id_products);

            let checkoutData = {
                'name' : this.custInfo.name,
                'email' : this.custInfo.email,
                'number' : this.custInfo.number,
                'address' : this.custInfo.name,
                'transaction_total' : this.totalBiaya,
                'transaction_status' : 'PENDING',
                'transaction_details' : id_products
            };

            axios.post("http://127.0.0.1:8000/api/checkout", checkoutData)
            .then(() => 
                localStorage.removeItem('keranjangUser'),
                this.$router.push('success-pay')
            )
            .catch(err => console.log(err))
        }
    },
    components: {
        HeaderShayna
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
        },
        pajak(){
            return (this.totalHarga*10)/100
        },
        totalBiaya(){
            return this.totalHarga + this.pajak
        }
    }
}
</script>

<style scoped>
p,.breacrumb-section,span{
    text-align: left;
}
textarea{
    resize: none;
}
</style>