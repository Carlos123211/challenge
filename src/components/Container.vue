<template>
<b-container>
    <Header :name="name" :time="time" />
    <Body :timeSend.sync="time"/>
</b-container>
</template>

<script>
import Body from './Body.vue'
import Header from './Header.vue'
export default {
  props: {
    name: String
  },
  components: {
    Body,
    Header
  },
  data () {
    return {
      time: 0
    }
  },
  methods: {
    update (time) {
      this.time = time
    }
  }
}
</script>
