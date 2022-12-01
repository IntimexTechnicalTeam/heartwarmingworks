<template>
  <div class="home HW-top" v-cloak>
    <div class="mid-box">
      <HomeBanner :initOptions="swiperOption" :page="'Home'" :initSwiper="true" class="banner" />

      <Promotion cmsKey="k_abs" />
      <Promotion1 page="home" :position="1" />
      <Promotion2 cmsKey="k_news" />

      <router-link to="/vr/vr360" class="vr-box"></router-link>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from 'vue-property-decorator';
import api from '@/sdk/api/Api.ts';
@Component({
  components: {
    HomeBanner: () => import('@/components/base/pc/InsBanner.vue'),
    Promotion: () => import('@/components/siteBusiness/pc/home/Promotion.vue'),
    Promotion1: () => import('@/components/siteBusiness/pc/home/Promotion1.vue'),
    Promotion2: () => import('@/components/siteBusiness/pc/home/Promotion2.vue')

    // HkPromotion: () => import(/* webpackChunkName: "home" */ '@/components/hkTasteBusiness/pc/home/HkPromotion.vue'),
    // HkBranch: () => import(/* webpackChunkName: "home" */ '@/components/hkTasteBusiness/pc/home/HkBranch.vue'),
    // HkLiveBox: () => import(/* webpackChunkName: "home" */ '@/components/hkTasteBusiness/pc/home/HkLiveBox.vue')
  }
})
export default class InsHome extends Vue {
  swiperOption: object = {
    autoplay: {
      disableOnInteraction: false
    }
  };
  mounted () {
    api
      .getData(1, 1)
      .then((res: any) => {
        var _this = this;
        this.$HiddenLayer();
      })
      .catch((err: any) => {
        // 请求失败后的处理函数
        console.log(err);
      });
  }
  created () {
    // let keywords = document.createElement('meta');
    // keywords.setAttribute('name', 'keywords');
    // keywords.setAttribute('content', require('../../sdk/common/SeoData').seoItem['home'].keyword);
    // document.head.appendChild(keywords);
    // let description = document.createElement('meta');
    // description.setAttribute('name', 'description');
    // description.setAttribute('content', require('../../sdk/common/SeoData').seoItem['home'].description);
    // document.head.appendChild(description);
    // document.dispatchEvent(new Event('render-event'));
  }
}
</script>

<style lang="less">
.prm-title {
  font-size: 48px;
  text-align: center;
  margin-bottom: 80px;
}
</style>

<style lang="less" scoped>
v-cloak{
  display: none;
}
.mainbanner {
  height: 200px;
}

.vr-box {
  display: block;
  height: 375px;
  background: url(/HW/vr-bg.jpg) no-repeat center;
  background-size: 100% 100%;
  cursor: pointer;
  margin-bottom: 50px;
}
</style>
