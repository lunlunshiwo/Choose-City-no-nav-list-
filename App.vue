<template>
  <div id="app">
    <search @txtdata="searchText"></search>
    <scroll :data="citylist" ref="suggest">
      <div>
        <position-box :chooseCity="chooseCity" :orientate="nowCity" @changeCity="changeCity"></position-box>
        <city-list :citylist="citylist" :cityIndexList='cityIndexList' @positionCity="changeCity" :toCityGroup="searchContent" @singleLetter='singleLetter'></city-list>
      </div>
    </scroll>
    <nav-list></nav-list>
    <mask-box v-if="maskShow" :message="maskMessage" @chooseing="chooseResult"></mask-box>
  </div>
</template>

<script>
import axios from 'axios'
import Search from './components/Search'
import Scroll from './components/Scroll.vue'
import PositionBox from './components/PositionBox'
import CityList from './components/CityList'
import NavList from './components/NavList'
import MaskBox from './components/MaskBox'

export default {
  name: 'App',
  data () {
    return {
      nowCity: '', // 当前所在的城市
      choiceCity: '', // 选择查看的城市
      choiceCityName: '', // 选择查看的城市
      citylist: [], // 城市列表
      cityIndexList: [], // 右边导航栏列表
      maskShow: false, // 弹窗
      maskMessage: '', // 弹窗展示的信息
      searchContent: '' // 搜索内容
    }
  },
  created () {
    this.getNowCity()
    this.getCityListApi()
  },
  computed: {
    chooseCity () {
      return this.choiceCityName ? this.choiceCityName : this.nowCity
    }
  },
  methods: {
    // 定位当前所在城市
    getNowCity () {
      axios.get('http://localhost:1234/nowcity').then((res) => {
        this.nowCity = res.data.city
        if (!this.choiceCity && !this.choiceCityName) {
          this.choiceCity = this.nowCity
          this.choiceCityName = this.nowCity
        }
      })
    },
    // 获取城市列表
    getCityListApi () {
      axios.get('http://localhost:1234/').then((res) => {
        this.citylist = res.data.openCityList
        this.citylist.map((item) => {
          this.cityIndexList.push(item[0])
        })
      })
    },
    searchText (text) {
      // console.log(text)
      this.searchContent = text
    },
    changeCity (name) {
      if (this.choiceCityName === name) {
        return false
      }
      this.choiceCity = name
      this.maskMessage = `你确定要选择${name}么？`
      this.maskShow = true
    },
    maskClose () {
      this.maskShow = false
    },
    chooseResult (res) {
      if (!res) {
        this.maskClose()
      } else {
        this.choiceCityName = this.choiceCity
        this.$refs.suggest.scrollTo(0, 0, 200)
        this.maskClose()
      }
    },
    singleLetter (dom) {
      this.$refs.suggest.scrollToElement(dom, 200, false, false)
    }
  },
  watch: {
  },
  components: {
    'search': Search,
    'scroll': Scroll,
    'position-box': PositionBox,
    'nav-list': NavList,
    'city-list': CityList,
    'mask-box': MaskBox
  }
}
</script>

<style lang="stylus">
@import 'common/stylus/index.styl'
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  color #2c3e50
</style>
