<template>
<div class="cateMain">
  <!-- 获取所在的产品目录 -->
  <div class="pro-attr category">
    <div class="attribute-item" v-for="(cat,index) in cateTree" :key="index">
      <span class="attr-label" v-if="!cat.ParentId">{{cat.Name}} ：</span>
      <span v-else>{{cat.Name}}</span>
    </div>
  </div>

  <!-- 获取所在的产品非库存属性 -->
  <div class="pro-attr attr">
    <div class="attribute-item" v-for="(attr,index) in source" :key="index">
      <template v-if="attr.length">
        <span class="attr-label">{{attr[0].Type.Name}} ：</span>
        <span v-for="(one,index2) in attr" :key="index2">{{one.Name}}</span>
      </template>
    </div>
  </div>
</div>
</template>
<script lang="ts">
import { Vue, Prop, Component, Watch } from 'vue-property-decorator';
@Component
export default class PkProductDetailCate extends Vue {
  @Prop() private readonly source!: string[];
  @Prop() private readonly cateTree!: string[];

  mounted() {
    console.log(this.cateTree, 'iii');
    console.log(this.source, 'iisourcei');
  }
}
</script>
<style scoped lang="less">
.cateMain {
  margin: 20px 0 !important;
}
.pro-attr {
  .attribute-item {
    > span {
      font-size: 21px;
      color: #4d4d4d;

      &:not(.attr-label) {
        padding: 0 10px;
      }
    }
  }

  &.category {
    .attribute-item{
      display: inline-block;
    }
  }

  &.attr {
    .attribute-item {
      margin: 10px 0;
    }
  }
}
</style>
