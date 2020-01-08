<template>
  <div class="sub-wrap">
    <div class="sub-visual-wrap">
      <SubVisual/>
      <div class="sub-info-txt">
        <div class="inner">
          <strong class="sub-title">실적발표</strong>
          <div class="history">
            <span class="icon-home" v-on:click="goHome">
              <img src="@/assets/images/icon/icon_home.png" alt="">
            </span>
            <span>실적발표</span>
          </div>
        </div>
      </div>
    </div>
    <div class="content-top">
      <AnnouncementTab/>
    </div>
    <div class="content-bottom" v-if="QaBoardView">
      <QaBoard v-bind:Contents="faqContents" v-bind:allData="allData" v-on:moreData="getFaqContent"/>
    </div>
    <Footer/>
  </div>
</template>

<script>
import SubVisual from '@/components/TypeA/SubVisual.vue'
import AnnouncementTab from '@/components/TypeA/AnnouncementTab.vue'
import QaBoard from '@/components/TypeA/QaBoard.vue'
import Footer from '@/components/TypeA/Footer.vue'
import { mapGetters } from 'vuex'

export default {
  name: 'announcement',
  components: {
    SubVisual,
    AnnouncementTab,
    QaBoard,
    Footer
  },
  data: () => {
    return {
      faqContents: [],
      allData: [],
      QaBoardView: true,
      qtype: ''
    }
  },
  computed: {
    ...mapGetters(['getCompSeq'])
  },
  watch: {
    getCompSeq () {
      const _self = this
      _self.getFaqContent()
    }
  },
  mounted () {
    const _self = this
    if (_self.getCompSeq) {
      _self.getFaqContent()
    }
  },
  methods: {
    moreFaqContents () {
      const _self = this
      _self.faqContents = _self.faqContents.concat(_self.allData.splice(0, 5))
    },
    getFaqContent () {
      const _self = this
      const aram = {
        seq: _self.getCompSeq
      }
      this.$store.dispatch('GET_FAQ', aram)
        .then(res => {
          if (res.length > 0) {
            _self.$store.commit('SET_QALEN', res.length)
            _self.allData = res
            _self.faqContents = res.splice(0, 5)
          } else if (res.length === 0) {
            _self.QaBoardView = false
          }
        })
    },
    goHome () {
      this.$router.push('/')
    }
  }
}
</script>
