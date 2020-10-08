<template>
  <div class="home">
    <b-container v-if="!nameCompleted" class="d-flex justify-content-center align-items-center h-100">
      <b-row class="justify-content-md-center">
      <Welcome class="col-12" msg="Hello friend, tell me your name..." />
        <div class="d-flex align-item col-12 justify-content-md-center">
          <b-form-input @keyup.enter.prevent="getName(name)" class=" mb-2 mt-4 customInput w-50 text-center" v-model="name" placeholder="Your name here"></b-form-input>
        </div>
        <b-button @click="getName(name)"   class="mt-4 d-flex align-items-center customButton" variant="light"
        >Let's go <b-icon-arrow-right-short></b-icon-arrow-right-short>
      </b-button>
      </b-row>
    </b-container>
    <b-container v-else-if="nameCompleted">
      <Container :name="name" />
    </b-container>
  </div>
</template>

<script>
// @ is an alias to /src
import Welcome from '@/components/Welcome.vue'
import Container from '@/components/Container.vue'
export default {
  name: 'Home',
  data () {
    return {
      name: '',
      nameCompleted: false
    }
  },
  components: {
    Welcome,
    Container
  },
  methods: {
    getName (name) {
      console.log(name)
      if (name.trim() !== '') {
        this.nameCompleted = true
      } else {
        this.nameCompleted = false
      }
    }
  }
}
</script>

<style scoped>
.customInput {
  background: transparent;
  border: none;
  border-bottom: 1px solid rgba(0, 0, 0, 0.2);
  border-radius: 0px;
}
.home {
  height: 100%;
}
.customButton {
  border-radius: 2em;
  color: #3b0078;
  font-weight: 600;
  -webkit-box-shadow: 0px 4px 5px -2px rgba(0, 0, 0, 0.75);
  -moz-box-shadow: 0px 4px 5px -2px rgba(0, 0, 0, 0.75);
  box-shadow: 0px 4px 5px -2px rgba(0, 0, 0, 0.75);
  font-size: 14px;
  border: 1px rgba(0, 0, 0, 0, 0.5);
}
</style>
