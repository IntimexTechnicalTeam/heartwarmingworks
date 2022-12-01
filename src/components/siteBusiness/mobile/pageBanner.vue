<template>
    <div class="prm_banner">
      <swiper class="swiper-no-swiping" :options="swiperOption" ref="mySwiper" v-if="(page || imgPath) && bannerList.length">
        <swiperSlide v-for="(slide, index) in bannerList" :key="index">
          <a :href="slide.Url || 'javascript:;'" :target="slide.Url ? slide.IsRedirect ? '_blank' : '_self' : ''">
            <img :src="slide.Image" />
          </a>
        </swiperSlide>
      </swiper>
    </div>
</template>
<script lang="ts">
import { Vue, Prop, Component, Watch } from 'vue-property-decorator';
import { swiper, swiperSlide } from 'vue-awesome-swiper/src';
@Component({
  components: {
    swiper,
    swiperSlide
  }
})
export default class PrmBanner extends Vue {
    @Prop({ default: '' }) private page!: string; // 推廣頁面
    @Prop({ default: 0 }) private position!: number; // 推廣位置
    @Prop({ default: '' }) private imgPath!: string; // banner圖片地址

    private bannerList: any[] = [];
    private prmName: string = '';
    swiperOption: any = {
        autoplay: {
            disableOnInteraction: false
        }
    };

    getBanner () {
        this.$Api.promotion.getPromotion(this.page, this.position).then(result => {
            this.prmName = result.Promotion.Name;
            this.bannerList = result.Promotion.BannerList;
        });
    }

    created () {
        // banner有分頁器或前進後退按鈕時，僅有單張圖片的情況下允許滑动切換效果
        if (this.swiperOption.pagination || this.swiperOption.navigation) {
        this.swiperOption.noSwiping = false;
        }

        if (this.page) {
          this.getBanner();
        }
    }

    @Watch('imgPath')
    onChange () {
      this.bannerList = [];
      this.bannerList.push({
        Image: this.imgPath
      });
    }
}
</script>

<style lang="less" scoped>
.prm_banner {
    position: relative;

    .swiper-slide {
      img {
        width: 100%;
      }
    }
}
</style>
