<template>
  <div class="lists">
    <div v-for="citys in citylist" :key="citys[0]" :dataNum="citys[1].length">
      <p :ref="citys[0]" class="city-title">{{citys[0]}}</p>
      <p :ref="cityRef(citys[0],key)" class="city-item" v-for="(city,key) in citys[1]" :key="city.id" @click="changeCity(city.name)">{{city.name}}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'CityList',
  props: {
    citylist: Array,
    cityIndexList: Array,
    toCityGroup: String
  },
  data () {
    return {
    }
  },
  methods: {
    changeCity (name) {
      this.$emit('positionCity', name)
    },
    cityRef (str, num) {
      return `${str}${num}`
    },
    retrieval (text) {
      if (text.length === 1) {
        let reg = /^\w$/g
        if (text.match(reg)) {
          this.$emit('singleLetter', this.$refs[text.toUpperCase()][0])
        } else {
          let reg = new RegExp('^[\\u4E00-\\u9FFF]$', 'g')
          if (reg.test(text)) {
            for (let i = 0, len1 = this.citylist.length; i < len1; i++) {
              for (let j = 0, len2 = this.citylist[i][1].length; j < len2; j++) {
                if (this.citylist[i][1][j].name[0] === text) {
                  let str = this.cityIndexList[i]
                  this.$emit('singleLetter', this.$refs[`${str}${j}`][0])
                  return false
                }
              }
            }
          }
        }
      } else {
        let reg = /^\w{2,}$/g
        if (text.match(reg)) {
          for (let i = 0, len1 = this.citylist.length; i < len1; i++) {
            for (let j = 0, len2 = this.citylist[i][1].length; j < len2; j++) {
              let reg = new RegExp(`^${text}`, 'g')
              if (this.citylist[i][1][j].pinyin.match(reg) || this.citylist[i][1][j].acronym.match(reg)) {
                let str = this.cityIndexList[i]
                this.$emit('singleLetter', this.$refs[`${str}${j}`][0])
                return false
              }
            }
          }
        } else {
          let reg = new RegExp('^[\\u4E00-\\u9FFF]{2,}$', 'g')
          if (reg.test(text)) {
            for (let i = 0, len1 = this.citylist.length; i < len1; i++) {
              for (let j = 0, len2 = this.citylist[i][1].length; j < len2; j++) {
                let reg = new RegExp(`^${text}`, 'g')
                if (this.citylist[i][1][j].name.match(reg)) {
                  let str = this.cityIndexList[i]
                  this.$emit('singleLetter', this.$refs[`${str}${j}`][0])
                  return false
                }
              }
            }
          }
        }
      }
    }
  },
  watch: {
    toCityGroup (val) {
      if (!val) {
        return false
      }
      this.retrieval(val)
    }
  }
}
</script>

<style lang="stylus" scoped>
.lists
  width 100%
  background #fff
  div
    .city-title
      height 30px
      border-top 1px solid #ccc
      background #f0efed
      line-height 29px
      padding-left 10px
      box-sizing border-box
    .city-item
      margin 0px 10px
      height 35px
      line-height 35px
      border-bottom 1px solid #ccc
      box-sizing border-box
    .city-item:last-child
      border none
</style>
