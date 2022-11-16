<template>
  <div class="home" v-cloak>
    <HomeBanner :initOptions="swiperOption" :page="'Home'" :initSwiper="true" class="banner" />

    <div class="mid-box">
      <Promotion cmsKey="k_abs" />
      <Promotion1 page="home" :position="1" />
      <Promotion2 cmsKey="k_news" />

      <router-link to="/vr/vr360" class="vr-box"></router-link>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
@Component({
  components: {
    HomeBanner: () => import('@/components/base/mobile/InsBanner.vue'),
    Promotion: () => import('@/components/siteBusiness/mobile/home/Promotion.vue'),
    Promotion1: () => import('@/components/siteBusiness/mobile/home/Promotion1.vue'),
    Promotion2: () => import('@/components/siteBusiness/mobile/home/Promotion2.vue')

    // HkPromotion: () => import('@/components/hkTasteBusiness/mobile/home/HkPromotion.vue'),
    // HkBranch: () => import('@/components/hkTasteBusiness/mobile/home/HkBranch.vue'),
    // HkLiveBox: () => import('@/components/hkTasteBusiness/mobile/home/HkLiveBox.vue')
  }
})
export default class InsHome extends Vue {
  swiperOption: object = {
    autoplay: {
      disableOnInteraction: false
    }
  };
}
</script>

<style lang="less">
.prm-title {
    font-size: 2rem;
    text-align: left;
    margin-bottom: 4rem;
    text-indent: 15%;
}
</style>

<style lang="less" scoped>
.banner {
  // min-height: 100vh;
  background-color: white;
}

.vr-box {
  display: block;
  height: 16rem;
  background: url(/HW/m-vr-bg.jpg) no-repeat center;
  background-size: 100% 100%;
  cursor: pointer;
  width: 88%;
  margin: 0 auto 3rem;
}
</style>
