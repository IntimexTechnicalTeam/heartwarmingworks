<template>
  <div class="content-list">
    <ul>
        <li v-for="(cms,index) in cmsData" :key="index">
            <router-link :to="'/cms/content/'+cms.Id">
                <div class="cover">
                    <img :src="cms.Cover" alt="cms.Title"/>
                </div>
                <div class="introduce">
                    <p class="title">{{cms.Title}}</p>
                    <p class="desc">{{cms.Desc}}</p>
                </div>
            </router-link>
        </li>
    </ul>

    <div class="pager" v-if="pager && pager.totalRecord > pager.pageSize && Object.keys(pager).length">
      <ins-page :total="pager.totalRecord" v-model="pager.currentPage" :pageNum="pager.pageSize"></ins-page>
    </div>
  </div>
</template>
<script lang="ts">
import { Vue, Prop, Component } from 'vue-property-decorator';
@Component({
  components: {
    InsPage: () => import('@/components/base/mobile/InsPage.vue')
  }
})
export default class cmsList extends Vue {
  @Prop({ default: () => [] }) private cmsData!: object[]; // cms内容数据
  @Prop({ default: () => {} }) private pager!: object; // 分頁器數據
}
</script>

<style lang="less" scoped>
.content-list {
  > ul {
    padding-left: 15%;
  }

  li {
    width: 16rem;

    &:not(:nth-child(3n)) {
      margin-right: 7.25%;
    }

    .cover {
      height: 16rem;
      img {
        width: 100%;
        height: 16rem;
        object-fit: cover;
        background-clip: padding-box;
      }
    }

    .introduce {
      padding: 0.8rem;

      .title {
        font-size: 1.1rem;
        color: #000;
        padding: 0.3rem 0 0.8rem;
        border-bottom: 1px solid;
        margin-bottom: 0.8rem;
      }

      .desc {
        font-size: 1rem;
        color: #4d4d4d;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 3;
        line-clamp: 3;
        -webkit-box-orient: vertical;
        line-height: 1.8rem;
      }
    }
  }
}
</style>
