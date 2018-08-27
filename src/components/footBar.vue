<template>
  <div class="footBar">
    <footer class="fixed bottom0 width100 padding5X bordertop1 bgcolor-white flex-align-spacebetween">
      <div :class="['width33', 'textAlignCenter', footerTab===1 ? 'the-color-red':'color-999']" @tap="switchTab('footer', 1)">
        <i class="icon-film fontsize5 bold"></i>
        <p class="fontsize-1 margintop5">电影</p>
      </div>
      <div :class="['width33', 'textAlignCenter', footerTab===2 ? 'the-color-red':'color-999']" @tap="switchTab('footer', 2);changePath('/pages/cinema/main')">
        <i class="icon-cinema fontsize5 bold"></i>
        <p class="fontsize-1 margintop5">影院</p>
      </div>
      <div :class="['width33', 'textAlignCenter', footerTab===3 ? 'the-color-red':'color-999']" @tap="switchTab('footer', 3)">
        <i class="icon-person fontsize5 bold"></i>
        <p class="fontsize-1 margintop5">我的</p>
      </div>
    </footer>
  </div>
</template>
<script>
  import { resFilmData } from '@/utils/resData'
  export default {
    components: {
    },
    data: () => ({
      headerTab: 1,
      footerTab: 1,
      curCity: '',
      ifList: true,
      listData: [],
      isLoading: false,
      noMore: false
    }),
    async onLoad() {
      this.listData.push(...resFilmData)
      this.curCity = this.$root.$mp.query.city || '杭州'
    },
    methods: {
      // tab切换
      switchTab(type, val) {
        if(type === 'header') {
          this.headerTab = val
        } else {
          this.footerTab = val
        }
        this.ifList = false
        this.listData = [...resFilmData]
        this.noMore = false
        setTimeout(() => {
          this.ifList = true
        }, 100)
      },
      changePath (path) {
        wx.navigateTo({
          url: path
        })
      },
      // 加载更多
      loadMore(e) {
        if(this.listData.length < 40) {
          this.isLoading = true
          setTimeout(() => {
            this.isLoading = false
            this.listData.push(...resFilmData)
          }, 1000)
        } else {
          this.noMore = true
        }
      }
    }
  }
</script>
<style lang="scss" scoped>
  $the-color-red: #f0423b;
  .home {
    padding-bottom: 45px;
    .headerTab {
      margin: 0 10px;
      height: 15px;
      padding: 10px 5px;
      color: 999;
      border-bottom: 1px solid transparent;
      &.cur {
        color: $the-color-red;
        border-color: $the-color-red;
      }
    }
  }
</style>
