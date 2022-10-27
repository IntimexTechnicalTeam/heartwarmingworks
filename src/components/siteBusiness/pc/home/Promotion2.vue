<template>
  <div class="promotion-box">
    <p class="prm-title">{{title}}</p>

    <cmsList :cmsData="content.slice(0,6)" />
  </div>
</template>
<script lang="ts">
import { Vue, Prop, Component } from 'vue-property-decorator';
import { swiper, swiperSlide } from 'vue-awesome-swiper/src';
@Component({
  components: {
    cmsList: () => import('@/components/siteBusiness/pc/cms/cmsList.vue')
  }
})
export default class Promotion1 extends Vue {
  @Prop() private cmsKey!: object;

  private title: string = '';
  private content: any = '';

  getContent () {
    this.$Api.cms.getCategoryByDevice({ Key: this.cmsKey, IsMobile: false }).then(result => {
      this.title = result.Name;
      this.content = result.Contents;
    });
  }

  mounted() {
    this.getContent();
  }
}
</script>

<style lang="less" scoped>
.promotion-box {
  padding: 80px 0;
  overflow: hidden;

  .content-list {

  }
}
</style>
