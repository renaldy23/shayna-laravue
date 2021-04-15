<template>
  <div class="home">
    <HeaderShayna></HeaderShayna>
    <OwlCaraousel></OwlCaraousel>
    <ProductSlider></ProductSlider>
    <GalleryShayna></GalleryShayna>
    <LogoPartner></LogoPartner>
    <FooterShayna></FooterShayna>
  </div>
</template>

<script>
import HeaderShayna from '../components/HeaderShayna.vue'
import OwlCaraousel from '../components/OwlCaraousel.vue'
import ProductSlider from '../components/ProductSlider.vue'
import GalleryShayna from '../components/GalleryShayna.vue'
import LogoPartner from '../components/LogoPartner.vue'
import FooterShayna from '../components/FooterShayna'

export default {
  name: 'Home',
  components:{
    HeaderShayna,
    OwlCaraousel,
    ProductSlider,
    GalleryShayna,
    LogoPartner,
    FooterShayna
  }
}
</script>
