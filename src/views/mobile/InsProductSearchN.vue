<template>
  <div id="container" class="ProductSearch">
        <pageBanner page="PromProductList" :position="0" />
        <div class="SearchSlide">
          <div class="leftSide">
            <advancedSearch @advancedChange="advancedChange" v-if="isAdvanced"  @closeSub="closeSub" @resetAll="resetAll" />
          </div>
        </div>
      <div class="selectBar">
          <ul>
            <li @click="showSearchSlide"><b>{{$t('product.Screening')}}</b></li>
            <li>
              <select v-model="PriceItem" @change="getselect(PriceItem)">
                <option value="desc">{{$t('product.PriceHL')}}</option>
                <option value="asc">{{$t('product.PriceLH')}}</option>
              </select>
            </li>
          </ul>
        </div>
    <!-- <advancedSearch :attrType="2"  @advancedChange="advancedChange" /> -->

    <div class="prolist-box">
      <div class="products_container" v-if="proList.length>0">
          <InsProductList v-for="item in proList" :key="item.productCode" :item="item" :needCode="false" class="product_item" ></InsProductList>
        </div>
        <div class="products_container" v-else>
             <h3 class="nocontentTips">{{$t('messageTips.NoContent')}}</h3>
        </div>
          <div ref="load" class="loading" @touchstart="loading" v-if="totalRecord>pageSize"><p>{{tips?$t('Action.LoadMore'):$t('home.Thatsall')}}</p></div>
          <div class="loading" v-else>{{$t('home.Thatsall')}}</div>
    </div>
  </div>
</template>
<script lang="ts">
import { Vue, Prop, Component, Watch } from 'vue-property-decorator';
import YouWouldLike from '@/model/youWouldLike';
import { Loading } from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import $ from 'jquery';
@Component({
  components: {
    InsProductList: () => import('@/components/siteBusiness/mobile/product/ProItem.vue'),
    advancedSearch: () => import('@/components/hkTasteBusiness/mobile/product/InsAdvancedSearch.vue'),
    pageBanner: () =>
      import(
        /* webpackChunkName: 'product' */ '@/components/siteBusiness/mobile/pageBanner.vue'
      )
  }
})
export default class InsProductSearch extends Vue {
  proList: YouWouldLike[] = []; // ????????????
  currentPage: number = 1; // ?????????
  pageSize: number = 6; // ????????????????????????
  totalRecord: number = 0;// ????????????
  private tips:boolean = true;
  private LoadingInstance!: any;

  attrs: object[] = []; // ???????????????????????????
  searchCatalogs: number[] = []; // ???????????????????????????
  searchType: number = 1; // ???????????????0 => ?????????1 => ?????????
  PriceItem:string='desc';
  isAdvanced: boolean = true;

  // ???????????????
  get searchKey () {
    let a = this.$store.state.searchKey;
    if (a === '-' || a === '') {
      return '';
    } else {
      return a;
    }
  }
  // ????????????
  getselect (val) {
    this.PriceItem = val;
    this.productSearch();
  }
  // ??????????????????
  productSearch (flag: string = '') {
    this.tips = true;
    this.$Api.product.search({
      Key: this.searchKey,
      PageInfo: {
        Page: this.currentPage,
        PageSize: this.pageSize,
        SortName: 'SalePrice',
        SortOrder: this.PriceItem
      },
      CatIdS: this.searchCatalogs,
      Attrs: this.attrs,
      Type: this.searchType,
      Reflesh: 1
    }).then((result) => {
      if (flag === 'loadpage') {
        this.proList = this.proList.concat(result.YouWouldLike);
        this.totalRecord = result.TotalRecord;
      } else {
        this.proList = result.YouWouldLike;
        this.totalRecord = result.TotalRecord;
      }

      this.$HiddenLayer();
    });
  }

  advancedChange (Attrs, Catalogs) {
    this.currentPage = 1;
    this.attrs = Attrs;
    this.searchCatalogs = Catalogs;
    this.productSearch();
  }
  // ????????????
  resetAll () {
    window.location.href = '/product/search/-';
    this.closeSub();
  }
  // ??????????????????
  closeSub () {
    $('.SearchSlide').fadeOut();
    $('.leftSide').removeClass('closeBar');
    document.body.style.overflow = 'auto';
  }
  // ??????????????????
  showSearchSlide () {
    $('.SearchSlide').fadeIn();
    $('.leftSide').addClass('closeBar');
    document.body.style.overflow = 'hidden';
  }
  loading (e) {
    if (this.tips) {
      this.LoadingInstance = Loading.service({
        target: this.$refs.load as any,
        fullscreen: false,
        // spinner: 'el-icon-loading',
        text: 'Loading...'
      });
      setTimeout(() => {
        this.load();
        this.LoadingInstance.close();
      }, 2000);
    }
  }
  load () {
    console.log(this.totalRecord, this.proList.length);
    if (this.totalRecord !== this.proList.length) { this.currentPage++; } else { this.tips = false; }
  }

  mounted () {

  }

  @Watch('searchKey', { deep: true })
  onSearchKeyChange () {
    this.productSearch();
  }

  @Watch('currentPage', { deep: true })
  onCurrentPage () {
    if (this.currentPage !== 1) {
      this.productSearch('loadpage');
    }
  }
}
</script>

<style lang="less">
.ProductSearch {
  .el-loading-spinner .circular {
    display: none;
  }

  .el-loading-text {
    font-size: 1.3rem;
    color: #cccccc;
    font-family: 'Arial';
  }

  .el-loading-parent--relative {
    > p {
      display: none;
    }
  }
}
</style>

<style scoped lang="less">
.prolist-box {
  padding-bottom: 2rem;
}

.nocontentTips{
  width: 95%;
  margin: 0 auto;
  font-size: 1.4rem;
  font-weight: 500;
  text-align: left;
  display: block;
  padding: 1rem;
  color: #666;
}
.product_warpper{
  width: 100%;
  margin:0 auto;
}
.products_container{
  display: flex;
  flex-wrap: wrap;
}

.product_item{
    width: 50% !important;
    padding:2rem 1rem 0;
    box-sizing:border-box;
}

.loading{
    text-align: center;
    height: 3rem;
    line-height: 3rem;
    margin: 1rem 0 2rem;
}

.ProductSearch {
  .InsAdvancedSearch {
    background: #fff;
    min-height: 100vh;
  }
}
.SearchSlide{
  width: 100%;
  position: fixed;
  left: 0;
  top: 0px;
  bottom: 0px;
  background: rgba(0,0,0,.6);
  overflow-x: scroll;
  z-index: 999999;
  display: none;
  .leftSide{
    width: 80%;
    left:-80%;
    min-height: 100%;
    position: absolute;
    transition: all .5s;
  }

}
.closeBar{
    left: 0%!important;
  }
.selectBar{
    width: 100%;
    margin: 2.5rem auto 1rem;
    display: inline-block;
  ul{
    width: 95%;
    margin: 0 auto;
  }
  li{
    font-size: 1.6rem;
    color: #1a1a1a;
    padding: 0.5rem 0;
    list-style: none;
    border-bottom: 1px solid;
    position: relative;
    text-align: left;
    margin-bottom: 1rem;

    &::after {
      position: absolute;
      content: '\25BC';
      right: 1.5rem;
      font-size: 1rem;
    }

    span{
    width: 20%;
    display: inline-block;
    font-size: 1.4rem;
    text-align: center;
    }
    b{
      display: inline-block;
      text-align: center;
      font-size: 1rem;
      font-weight: 500;
      color: #1a1a1a;
    }
    select{
    width: 100%;
    border: none;
    height: 90%;
    -webkit-appearance: none;
    -moz-appearance: none;
    appearance: none;
    // background: url(/images/mobile/arrow-down-back.png) 98% 15px no-repeat;
    background-size: 15px;
    outline: none;
    font-size: 1rem;
    font-weight: 500;
    color: #1a1a1a;
    }
  }
}
</style>
