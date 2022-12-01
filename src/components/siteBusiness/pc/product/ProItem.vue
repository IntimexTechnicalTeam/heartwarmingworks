<template>
  <div class="product_item">
    <div class="productMain" v-if="item">
      <div class="in_pdWindow_page_item" :style="styla" @mouseenter="Enter=true" @mouseleave="Enter=false" @click="click">
        <div class="topWindowsImg imgbox">
          <img :src="(item.Image?item.Image:item.Img_L?item.Img_L:item.Img)"  :class="{'height_line':Enter}" :style="imgStyla" :data-key="item.Sku" @error="loadError" />
        </div>
          <div class="in_pdWindow_item_description">
              <a  href="javascript:;" class="in_pdWindow_item_title" v-on:click="addCart(item)">{{item.Name}}</a>
              <div class="in_pdWindow_item_price">
                <inPrices :primePrices="item.ListPrice" :currentPrices="item.SalePrice" :currency="item.Currency" :DefaultListPrice="item.DefaultListPrice" :DefaultSalePrice="item.DefaultSalePrice" :DefaultCurrency="item.DefaultCurrency" size="small"></inPrices>
              </div>
          </div>
      </div>
    </div>
  </div>
</template>
<script lang="ts">
import YouWouldLike from '@/model/youWouldLike';
import { Vue, Prop, Component } from 'vue-property-decorator';
@Component({ components: { inButton: () => import(/* webpackChunkName: "product" */ '@/components/base/pc/InsButton.vue'),
  inPrices: () => import(/* webpackChunkName: "product" */ '@/components/base/pc/InsPrices.vue') } })
export default class InsProductWindow extends Vue {
    private Enter:boolean = false;
    private newArray= [];
    @Prop() private item!:YouWouldLike;
    @Prop() private imgStyla!:string;
    @Prop() private styla!:string;
    buttonSubmit (item) {
      this.$router.push({
        path: '/product/detail',
        name: 'ProductsDetail',
        params: {
          id: item.Sku
        }
      });
    }
    addToFavorite (p) {
      if (p.IsFavorite) {
        this.$Api.member.removeFavorite(p.Sku).then((result) => {
          if (result.Succeeded) {
            p.IsFavorite = false;
            this.$message({
                message: this.$t('MyFavorite.RemoveSuccess') as string,
                type: 'error',
                customClass: 'messageboxNoraml'
              });
          }
        });
      } else {
        this.$Api.member.addFavorite(p.Sku).then((result) => {
          if (result.Succeeded) {
            p.IsFavorite = true;
            this.$message({
              message: this.$t('MyFavorite.AddSuccess') as string,
              type: 'success',
              customClass: 'messageboxNoraml'
            });
          } else {
            this.$router.push('/Account/login');
          }
        });
      }
    }
    addCart (val) {
      this.$router.push('/product/detail/' + val.Sku);
    }
    click (e) {
      let target = e.target as HTMLElement;
      if (target.nodeName === 'IMG') { this.$router.push('/product/detail/' + target.dataset.key); };
    }
    loadError (e) {
      e.target.src = '/static/Image/proddef.jpg';
    }
}
</script>
<style lang="less">
.messageboxNoraml{
  z-index: 1000000000!important;
}
</style>
<style lang="less">
.product_item .in_pdWindow_item_price .currentPricesMain ,.in_pdWindow_item_price .primePricesMain{
  width: 100%;
  display: inline-block;
  text-align: center;
}
.product_item .in_pdWindow_item_price .currentPricesMain  .small:nth-child(1) {
  font-size: 1.2rem;
  word-break: break-all;
  text-align: center;
  color: #0b0b0b;
  display: inline-block;
}
.product_item .in_pdWindow_item_price .currentPricesMain .small:nth-child(2) {
    font-size: 1.4rem;
    color: #cd0909;
    display: inline-block;
}
.product_item .in_pdWindow_item_price .primePricesMain  .small:nth-child(1) {
  font-size: 1rem;
  word-break: break-all;
  text-align: center;
  color: #999;
  display: inline-block;
  text-decoration:line-through;
}
.product_item .in_pdWindow_item_price .primePricesMain .small:nth-child(2) {
  font-size: 1rem;
  color: #999;
  display: inline-block;
}

</style>
<style lang="less" scoped>
.imgbox{
    position: relative;
    display: inline-block;
    width: 100%;
    overflow: hidden;
}

.imgbox:hover .shopMark{
    bottom: 0px;
}
.imgbox .shopMark{
    position: absolute;
    left: 0px;
    bottom: -100%;
    right: 0px;
    height:100%;
    background:rgba(0,0,0,.5);
    transition: all .3s;
    display: flex;
    align-items: center;
    justify-content: center;
    .innerBox{
      width: 100%;
      display: inline-block;
    }
}

.imgbox img{
    width: 100%;
    border-radius:0px;
    transition: border all 1s;
    box-sizing: border-box;
}
.in_pdWindow_page_item img {
  box-sizing: border-box;
  cursor: pointer;
  border-radius:0px;
}
.in_pdWindow_item_title {
    font-size: 24px;
    word-break: break-all;
    text-align: center;
    color: #4d4d4d;
    display: inline-block;
    text-align: center;
    line-height: 25px;
    overflow: hidden;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    word-break: break-word;
    margin-top: 10px;
    margin-bottom: 10px;
}
.in_pdWindow_item_code {
  margin-top: 1rem;
  color: #999999;
  text-align: center;
}
</style>

<style lang="less" scoped>
.product_item {
    .in_pdWindow_page_item {
      border: 3px solid #fff;
      border-radius: 20px;
      overflow: hidden;
    }

    /deep/ .in_pdWindow_item_description {
      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: center;
      opacity: 0;

      .primePricesMain {
        margin-bottom: 10px;
        .small {
          font-size: 14px;
          color: #808080;
        }
      }

      .currentPricesMain {
        .small {
          font-size: 18px;
          color: #000;
        }
      }
    }

    .in_pdWindow_item_price {
      margin: 15px 0;
    }

    &:hover {
      .in_pdWindow_page_item {
        border-color: @base_color2;
      }

      .in_pdWindow_item_description {
        opacity: 1;
      }
    }
}
</style>
