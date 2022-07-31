<template>
  <div>
    <h1>V1</h1>
    {{ tips }}
  </div>
</template>

<script>
import tipsData from "../treasurytips.json";

export default {
  data () {
    return {
      info: null,
      tips: tipsData
    }
  },
  mounted () {
    // this.$axios
    //   .get('https://polkadot.subsquare.io')
    //   .then(response => (this.info = response))
  },
  components: {
  }
}
</script>