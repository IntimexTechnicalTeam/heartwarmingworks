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
  </div>
</template>
<script lang="ts">
import { Vue, Prop, Component } from 'vue-property-decorator';
@Component
export default class cmsList extends Vue {
  @Prop({ default: () => [] }) private cmsData!: object[]; // cms内容数据
}
</script>

<style lang="less" scoped>
.content-list {
  li {
    width: 28.5%;
    float: left;

    &:not(:nth-child(3n)) {
      margin-right: 7.25%;
    }

    .cover {
      height: 342px;
      img {
        width: 100%;
        height: 342px;
        object-fit: cover;
        background-clip: padding-box;
      }
    }

    .introduce {
      padding: 10px;

      .title {
        font-size: 24px;
        color: #000;
        padding: 10px 0 15px;
        border-bottom: 1px solid;
        margin-bottom: 15px;
      }

      .desc {
        font-size: 18px;
        color: #4d4d4d;
        overflow: hidden;
        text-overflow: ellipsis;
        display: -webkit-box;
        -webkit-line-clamp: 3;
        line-clamp: 3;
        -webkit-box-orient: vertical;
        line-height: 30px;
        max-height: 90px;
      }
    }
  }
}
</style>
