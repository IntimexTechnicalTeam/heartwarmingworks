<template>
  <div class="promotion-box">
    <p class="prm-title">{{prmName}}</p>

    <div class="prm-pros">
      <ProItem1 v-for="(pro,index) in proList.slice(0,12)" :key="index" :item="pro" />
    </div>
  </div>
</template>
<script lang="ts">
import { Vue, Prop, Component } from 'vue-property-decorator';
import { swiper, swiperSlide } from 'vue-awesome-swiper/src';
@Component({
  components: {
    ProItem1: () => import('@/components/siteBusiness/pc/product/ProItem1.vue')
  }
})
export default class Promotion1 extends Vue {
    @Prop({ default: '' }) private page!: string; // 推廣頁面
    @Prop({ default: 0 }) private position!: number; // 推廣位置

    private proList: any[] = [];
    private prmName: string = '';

    getPromotion () {
        this.$Api.promotion.getPromotion(this.page, this.position).then(result => {
            this.prmName = result.Promotion.Name;
            this.proList = result.Promotion.PrmtProductList;
        });
    }

    created () {
        this.getPromotion();
    }
}
</script>

<style lang="less" scoped>
.promotion-box {
  padding: 3rem 0 0;
  overflow: hidden;

  .prm-pros {
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;

    .product_item {
      width: 48%;
      margin-bottom: 1rem;
    }
  }
}
</style>
