<template>
  <view>
    <view>
      <u-popup v-model="show" mode="bottom" height="160px">
        <view>出淤泥而不染，濯清涟而不妖</view>
      </u-popup>
      <u-button @click="show = true">打开</u-button>
    </view>
  </view>
</template>

<script>
export default {
  data() {
    return {
      title: 'uView ui',
      show: false
    }
  },
  onLoad() {

  },
  methods: {

  }
}
</script>

<style lang="scss">

</style>