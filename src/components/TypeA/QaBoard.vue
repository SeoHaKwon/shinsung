<template>
    <div class="board-section">
      <p class="title-wrap">
        <strong>자주하는 질문과 답변</strong>
      </p>
      <ul class="list qa">
        <li v-for="(item, idx) in Contents" v-bind:key="idx" v-on:click="ViewPage(idx)">
          <strong class="board-title">
            {{ item.QUESTION }}
          </strong>
          <a href="javascript:void(0)" class="link"></a>
        </li>
      </ul>
      <div class="btn-wrap center" v-if="allData.length > 0">
        <button type="button" class="btn_more" v-on:click="moreData">
          <span>more</span>
        </button>
      </div>
    </div>
</template>

<script>
export default {
  name: 'QaBoard',
  props: ['Contents', 'allData'],
  methods: {
    ViewPage (idx) {
      const _self = this
      _self.$router.push({ name: 'announcementView', params: { 'index': idx } })
    },
    moreData () {
      const _self = this
      _self.$emit('moreData')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
</style>
