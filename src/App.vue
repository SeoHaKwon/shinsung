<template>
  <div id="root">
    <div id="app" class="wrap" v-if="IRPAGE_TYPE === 'A' && IRPAGE_USE">
      <Gnb/>
      <go-top
      :max-width="0"
      :size="50"
      :right="15"
      :bottom="70"
      :radius="7"
      bg-color="#8E8E93"
      box-shadow="5px 5px 5px rgba(0, 0, 0, .5)">TOP</go-top>
    </div>
    <div id="app" v-on:mousewheel="onScroll($event)" v-else-if="IRPAGE_TYPE === 'B' && IRPAGE_USE">
      <GnbB :scrollTemp="scrollTemps"/>
    </div>
    <errpage v-if="IRPAGE_USE === false"/>
  </div>
</template>
<script>
import Gnb from '@/components/TypeA/Gnb'
import GnbB from '@/components/TypeB/Gnb'
import errpage from '@/components/404'

export default {
  name: 'App',
  components: {
    Gnb,
    GnbB,
    errpage
  },
  data: () => {
    return {
      IRPAGE_TYPE: '',
      IRPAGE_USE: true,
      scrollTemps: 0
    }
  },
  beforeCreate () {
    const _self = this
    const param = {
      'url': 'shinsungeng.irpage.co.kr'
    }
    this.$store.dispatch('GET_PAGETYPE', param)
      .then(res => {
        console.log(cordova.platformId)
        _self.IRPAGE_TYPE = res.IRPAGE_TYPE
        if (res.IRPAGE_USE_YN === 'Y') {
          _self.IRPAGE_USE = true
        } else {
          _self.IRPAGE_USE = false
        }
      })
  },
  methods: {
    onScroll (e) {
      const _self = this
      let _scrollTop = window.scrollY || document.documentElement.scrollTop
      if (_self.width > 899) {
        // console.log(_scrollTop, 'PC')
      } else {
        // console.log(_scrollTop, 'mobile')
      }
      _self.scrollTemps = _scrollTop
    }
  }
}
</script>
<style lang="scss">

</style>
