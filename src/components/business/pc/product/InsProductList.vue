<template>
    <div class="products_container">
        <inProductWindow v-for="item in allItems" :key="item.id" :item="item"></inProductWindow>
    </div>
</template>
<script lang="ts">
import { Vue, Prop, Component, Watch } from 'vue-property-decorator';
import Product from '@/model/youWouldLike';
@Component({ components: { inProductWindow: () => import(/* webpackChunkName: "product" */ '@/components/siteBusiness/pc/product/ProItem.vue') } })
export default class InsProductList extends Vue {
    @Prop() private readonly column!: number;
    @Prop() private readonly allItems!: Product[];
}
</script>
<style lang="less" scoped>
.products_container{
    display: flex;
    flex-wrap: wrap;
    width: 100%;
    margin: 0 auto;
    margin-top: 30px;

    .product_item{
        box-sizing: border-box;
        width: 24%;
        float: left;
        margin-right: 1.33%;
        margin-bottom: 4%;
    }
    .product_item:nth-child(4n){
        margin-right: 0%!important;
    }
}
</style>
