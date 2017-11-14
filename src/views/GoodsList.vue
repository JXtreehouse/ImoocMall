<template lang="pug">
  div
    nav-header
    nav-bread
      span goods
    .accessory-result-page.accessory-page
      .container
        .filter-nav
         span.sortby Sort by:
          a.default.cur(href="javascript:") Default
          a.price(href="javascript:")
            | Price 
            svg.icon.icon-arrow-short
              use(xlink:href="#icon-arrow-short")
          a.filterby.stopPop(href="javascript:", @click="showFilterPop") Filter by
        .accessory-result
          // filter
          #filter.filter.stopPop(:class="{'filterby-show' : filterBy}")
            dl.filter-price
              dt Price:
              dd
                a(href="javascript:", :class="{'cur' : priceChecked === 'all'}", @click="priceChecked = 'all'") All
              dd(v-for="(price, index) in priceFilter", :key="price")
                a(href="javascript:", @click="setPriceFilter(index)", :class="{'cur': priceChecked === index}") {{ price.startPrice }} - {{ price.endPrice }}
          // search result accessories list
          .accessory-list-wrap
            .accessory-list.col-4
              ul
                li(v-for="(good, index) in goodsList", :key="good")
                  .pic
                    a(href="#")
                      img(v-lazy="'/static/' + good.productImage")
                  .main
                    .name {{ good.productName }}
                    .price {{ good.productPrice }}
                    .btn-area
                      a.btn.btn--m(href="javascript:;") 加入购物车
    .md-overlay(v-show="overLayFlag", @click="closePop")
    nav-footer
</template>
<script>
  import axios from 'axios'

  import NavHeader from '@/components/NavHeader.vue'
  import NavFooter from '@/components/NavFooter.vue'
  import NavBread from '@/components/NavBread.vue'

  export default {
    data() {
      return {
        goodsList: [],
        priceFilter: [
          {
            startPrice: '0.00',
            endPrice: '500.00'
          },
          {
            startPrice: '500.00',
            endPrice: '1000.00'
          },
          {
            startPrice: '1000.00',
            endPrice: '2000.00'
          }
        ],
        priceChecked: 'all',
        filterBy: false,
        overLayFlag: false
      }
    },
    components: {
      NavHeader, NavFooter, NavBread
    },
    mounted() {
      this.getGoodsList()
    },
    methods: {
      getGoodsList() {
        axios.get('/goods').then((result) => {
          let res = result.data
          this.goodsList = res.result.list
        })
      },
      showFilterPop() {
        this.filterBy = true
        this.overLayFlag = true
      },
      setPriceFilter(index) {
        this.priceChecked = index
        this.closePop()
      },
      closePop() {
        this.filterBy = false
        this.overLayFlag = false
      }
    }
  }
</script>
