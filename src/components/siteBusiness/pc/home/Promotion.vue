<template>
  <div class="promotion-box">
    <p class="prm-title">{{content.Name}}</p>

    <div class="msg">
      <div class="cover">
        <img :src="content.ImagePath" />
      </div>

      <div class="content">
        <p class="desc">{{content.Description}}</p>

        <hr>

        <div v-html="content.Content"></div>
      </div>
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
.promotion-box {
  padding: 80px 0;
  overflow: hidden;

  .msg {
    .cover {
      max-width: 780px;
      margin-left: 45px;
      float: right;

      img {
        max-width: 100%;
      }
    }

    .content {
      font-size: 24px;
      color: #4d4d4d;
      line-height: 45px;
      padding: 35px 0;

      hr {
        margin: 30px 0;
      }
    }
  }
}
</style>
