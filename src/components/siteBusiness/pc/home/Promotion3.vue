<template>
  <div class="promotion-box">
    <p class="prm-title">{{content.Name}}</p>

    <div class="content">
      <div v-html="content.Content"></div>
    </div>
  </div>
</template>
<script lang="ts">
import { Vue, Prop, Component } from 'vue-property-decorator';
import { swiper, swiperSlide } from 'vue-awesome-swiper/src';
@Component
export default class Promotion1 extends Vue {
  @Prop() private cmsKey!: object;

  private content: any = '';

  getContent () {
    this.$Api.cms.getCategoryByDevice({ Key: this.cmsKey, IsMobile: false }).then(result => {
      this.content = result;
    });
  }

  mounted() {
    this.getContent();
  }
}
</script>

<style lang="less" scoped>
.prm-title {
  margin-bottom: 60px;
}

.promotion-box {
  padding: 80px 0;
  overflow: hidden;

  .content {
    font-size: 24px;
    color: #4d4d4d;
    line-height: 45px;
  }
}
</style>
