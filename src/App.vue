<template lang="pug">
.background(:class="{active : mask}")
  .search
    .search_icon
    input.search_input(v-model.trim="searchText")
    .search_close
      .close_icon(@click="searchText = ''")
  .adv(v-if="advImgName" :style="{backgroundImage:`url('${require(`./images/adv/${advImgName.name}`)}')`}")
    .adv_btn_left(@click="advImgLeft()") {{'<'}}
    //- .adv_btn
    .adv_btn_right(@click="advImgRight()") {{'>'}}
  .time {{'倒數 ' + hours + ':' + minutes + ':' + seconds}}
  .commodity
    .commodity_menu
      .commodity_menu_item(v-for="item in attributes" :key="item.id" @click="typeTag = item.id" :class="{active : typeTag === item.id}") {{item.typeName}}
    .commodity_group
      .commodity_item(v-if="item.type.includes(typeTag)" v-for="item in searchPokemon" :key="item.name" @click="pop(item)")
        .commodity_item_img(:style="{backgroundImage:`url('${require(`./images/item/${item.imgName}`)}')`}")
        .commodity_item_name {{ item.pokemonName }}
  .pop(v-if="mask && pop_img && pop_name")
    .pop_mask(@click="mask = false")
      .pop_group
        .pop_img(:style="{backgroundImage:`url('${require(`./images/item/${pop_img}`)}')`}")
        .pop_name {{pop_name}}
  .gotop_btn(@click="goTopHandler" :class="{ active : activeGoTop }") {{'GOTOP'}}
</template>
<script>
export default {
  name: 'protest',

  data() {
    return {
      attributes: [
        { id: 0, typeName: '全部' }, 
        { id: 1, typeName: '水系' }, 
        { id: 2, typeName: '火系' }, 
        { id: 3, typeName: '草系' },
        { id: 4, typeName: '幽靈系' },
        { id: 5, typeName: '電系' }, 
      ],
      advImg: [
        { id: 0, name: '000.jpg' }, 
        { id: 1, name: '001.jpg' }, 
        { id: 2, name: '002.jpg' }, 
        { id: 3, name: '003.jpg' },
        { id: 4, name: '004.jpg' },
        { id: 5, name: '005.jpg' }, 
        { id: 6, name: '006.jpg' }, 
        { id: 7, name: '007.jpg' }, 
        { id: 8, name: '008.jpg' }, 
        { id: 9, name: '009.jpg' }, 
      ],
      commodity_item: [
        { type: [ 0, 3 ], pokemonName: '妙蛙種子', imgName: 'pm001.gif' },
        { type: [ 0, 3 ], pokemonName: '妙蛙草', imgName: 'pm002.gif' },
        { type: [ 0, 3 ], pokemonName: '妙蛙花', imgName: 'pm003.gif' },
        { type: [ 0, 2 ], pokemonName: '小火龍', imgName: 'pm004.gif' },
        { type: [ 0, 2 ], pokemonName: '火恐龍', imgName: 'pm005.gif' },
        { type: [ 0, 2 ], pokemonName: '噴火龍', imgName: 'pm006.gif' },
        { type: [ 0, 1 ], pokemonName: '傑尼龜', imgName: 'pm007.gif' },
        { type: [ 0, 1 ], pokemonName: '卡咪龜', imgName: 'pm008.gif' },
        { type: [ 0, 1 ], pokemonName: '水箭龜', imgName: 'pm009.gif' },
        { type: [ 0, 5 ], pokemonName: '皮卡丘', imgName: 'pm025.gif' },
        { type: [ 0, 5 ], pokemonName: '雷丘', imgName: 'pm026.gif' },
        { type: [ 0, 1 ], pokemonName: '可達鴨', imgName: 'pm054.gif' },
        { type: [ 0, 1 ], pokemonName: '哥達鴨', imgName: 'pm055.gif' },
        { type: [ 0, 2 ], pokemonName: '卡蒂狗', imgName: 'pm058.gif' },
        { type: [ 0, 2 ], pokemonName: '風速狗', imgName: 'pm059.gif' },
        { type: [ 0, 4 ], pokemonName: '鬼斯', imgName: 'pm092.gif' },
        { type: [ 0, 4 ], pokemonName: '鬼斯通', imgName: 'pm093.gif' },
        { type: [ 0, 4 ], pokemonName: '耿鬼', imgName: 'pm094.gif' },
        { type: [ 0, 1 ], pokemonName: '乘龍', imgName: 'pm131.gif' },
        { type: [ 0, 1 ], pokemonName: '水精靈', imgName: 'pm134.gif' },
        { type: [ 0, 5 ], pokemonName: '雷精靈', imgName: 'pm135.gif' },
        { type: [ 0, 2 ], pokemonName: '火精靈', imgName: 'pm136.gif' },

      ],
      advImgName: '',
      advImgTag: 0,
      mask: false,
      pop_img: '',
      pop_name: '',
      typeTag: 0,
      searchText: '',
      activeGoTop: false,
      timeTotal: 86400000,
      hours: 0,
      minutes: 0,
      seconds: 0,
    }
  },
  computed: {
    searchPokemon () {
      if(this.searchText.length) {
        return this.commodity_item.filter(item => {
        return item.pokemonName == this.searchText
      })
      } else {
        return this.commodity_item
      }
    },
  },

  mounted () {
    this.advImgName = this.advImg[this.advImgTag]
    let vm = this
    window.addEventListener('scroll', function () {
      if (window.scrollY === 0) vm.activeGoTop = false
      else vm.activeGoTop = true
    })
    this.reciprocal()
  },

  methods: {
    advImgLeft() {
      if(this.advImgTag === 0) return ''
      else this.advImgTag = this.advImgTag - 1
      this.advImgName = this.advImg[this.advImgTag]
      return ''
    },
    advImgRight() {
      if(this.advImgTag === this.advImg.length - 1) return ''
      else this.advImgTag = this.advImgTag + 1 
      this.advImgName = this.advImg[this.advImgTag]
      return ''
    },
    pop(item) {
      this.pop_img = item.imgName
      this.pop_name = item.pokemonName
      this.mask = true
    },
    goTopHandler () {
      window.scroll(0, 0)
    },
    reciprocal () {
      if(this.timeTotal >= 0) {
        this.hours = Math.floor( (this.timeTotal/(1000*60*60)) % 24 )
        this.minutes = Math.floor( (this.timeTotal/1000/60) % 60 )
        this.seconds = Math.floor((this.timeTotal/1000) % 60 )
        this.timeTotal = this.timeTotal - 1000;
        setTimeout(this.reciprocal, 1000);
      }
    },
  }
}
</script>
<style lang="scss" scoped>
  @import './style.scss';
</style>
