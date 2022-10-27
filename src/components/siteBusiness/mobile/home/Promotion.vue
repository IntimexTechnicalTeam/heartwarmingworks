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
    this.$Api.cms.getCategoryByDevice({ Key: this.cmsKey, IsMobile: true }).then(result => {
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
  padding: 3rem 0 0;
  overflow: hidden;

  .msg {
    .cover {

      img {
        max-width: 100%;
      }
    }

    .content {
      font-size: 1.1rem;
      color: #4d4d4d;
      line-height: 2.2rem;
      padding: 2rem 0;

      hr {
        width: 50%;
        margin: 1rem 0;
        color: #4d4d4d;
      }
    }
  }
}
</style>
