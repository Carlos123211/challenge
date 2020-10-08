<template>
  <b-container :key="render" >
    <b-row  class="mt-4 mb-5">
        <b-col @click="getStart" v-bind:id="Cards.length > 0 ? 'div'+Cards[0].id : ''"  @dragover="allowDrop($event)" class="d-flex card card-white justify-content-center">
             <b-img v-bind:name="Cards.length > 0 ? Cards[0].val : ''" v-bind:id="Cards.length > 0 ? Cards[0].id : ''" draggable="true" @dragstart="drag" v-bind:src="Cards.length > 0 ? Cards[0].src : ''"></b-img>
        </b-col>
        <b-col @click="getStart" v-bind:id="Cards.length > 0 ? 'div'+Cards[1].id : ''"  @dragover="allowDrop($event)" class="d-flex card card-white justify-content-center">
             <b-img v-bind:name="Cards.length > 0 ? Cards[1].val : ''" v-bind:id="Cards.length > 0 ? Cards[1].id : ''" draggable="true" @dragstart="drag" v-bind:src="Cards.length > 0 ? Cards[1].src : ''"></b-img>
        </b-col>
        <b-col @click="getStart" v-bind:id="Cards.length > 0 ? 'div'+Cards[2].id : ''"  @dragover="allowDrop($event)" class="d-flex card card-white justify-content-center">
            <b-img  v-bind:name="Cards.length > 0 ? Cards[2].val : ''" v-bind:id="Cards.length > 0 ? Cards[2].id : ''" draggable="true" @dragstart="drag" v-bind:src="Cards.length > 0 ? Cards[2].src : ''"></b-img>
        </b-col>
        <b-col @click="getStart" v-bind:id="Cards.length > 0 ? 'div'+Cards[3].id : ''"  @dragover="allowDrop($event)" class="d-flex card card-white justify-content-center">
            <b-img  v-bind:name="Cards.length > 0 ? Cards[3].val : ''" v-bind:id="Cards.length > 0 ? Cards[3].id : ''" draggable="true" @dragstart="drag" v-bind:src="Cards.length > 0 ? Cards[3].src : ''"></b-img>
        </b-col>
        <b-col @click="getStart" v-bind:id="Cards.length > 0 ? 'div'+Cards[4].id : ''"  @dragover="allowDrop($event)" class="d-flex card card-white justify-content-center">
             <b-img v-bind:name="Cards.length > 0 ? Cards[4].val : ''" v-bind:id="Cards.length > 0 ? Cards[4].id : ''" draggable="true" @dragstart="drag" v-bind:src="Cards.length > 0 ? Cards[4].src : ''"></b-img>
        </b-col>
    </b-row>
    <b-row><b-col class="customText" cols="12">...and drop them here to make the logo great</b-col></b-row>
    <b-row v-if="correctAns"><b-col class="" cols="12">Your time was {{time}} seconds , congratulations. The game will restart in 10 seconds </b-col></b-row>
    <b-row id="row2" class="mt-5">
            <b-col id="div6" @drop="drop($event,2)" @dragover="allowDrop($event)" class="d-flex card customCards justify-content-center">
            </b-col>
            <b-col id="div7" @drop="drop($event,2)" @dragover="allowDrop($event)" class="d-flex card customCards justify-content-center">
            </b-col>
            <b-col id="div8" @drop="drop($event,2)" @dragover="allowDrop($event)" class="d-flex card customCards justify-content-center">
            </b-col>
            <b-col id="div9" @drop="drop($event,2)" @dragover="allowDrop($event)" class="d-flex card customCards justify-content-center">
            </b-col>
            <b-col id="div10" @drop="drop($event,2)" @dragover="allowDrop($event)" class="d-flex card customCards justify-content-center">
            </b-col>
    </b-row>
  </b-container>
</template>

<script>
import * as LetterZ from '@/assets/img/zoovu-z.svg'
import * as LetterO from '@/assets/img/zoovu-o.svg'
import * as LetterV from '@/assets/img/zoovu-v.svg'
import * as LetterU from '@/assets/img/zoovu-u.svg'
import _ from 'lodash'
export default {
  props: ['timeSend'],
  data () {
    return {
      Cards: [],
      correctCards: [],
      dragindex: 0,
      dropindex: 0,
      parentNode: '',
      time: 0,
      isRunning: false,
      interval: null,
      startCount: 0,
      row1: '',
      row2: '',
      render: 0,
      correctAns: false,
      timeOut: null
    }
  },
  watch: {
  },
  methods: {
    Penalty () {
      this.time += 10
    },
    CorrectOrder (index, val) {
      if (index === 0 && val === 'z') {
        return true
      } else if (index === 1 && val === 'o') {
        return true
      } else if (index === 2 && val === 'o') {
        return true
      } else if (index === 3 && val === 'v') {
        return true
      } else if (index === 4 && val === 'u') {
        return true
      } else {
        return false
      }
    },
    stopTimer () {
      clearInterval(this.interval)
    },
    startTimer () {
      this.interval = setInterval(this.incrementTime, 1000)
    },
    incrementTime () {
      this.time = parseInt(this.time) + 1
      this.$emit('update:timeSend', this.time)
    },
    drag (ev) {
      ev.dataTransfer.setData('text', ev.target.id)
      this.getStart()
      this.parentNode = ev.target.parentNode
    },
    getStart () {
      this.startCount++
      if (this.startCount === 1) {
        this.startTimer()
      }
    },
    drop (ev, num = 1) {
      ev.preventDefault()
      let orderValidator = 0
      const parentDrop = ev.target.parentNode
      var data = ev.dataTransfer.getData('text')
      if (!parentDrop.id.includes('row')) {
        parentDrop.replaceChild(document.getElementById(data), ev.target)
        this.parentNode.appendChild(ev.target)
      } else {
        ev.target.appendChild(document.getElementById(data))
      }
      const childNodes = document.getElementById('row2').childNodes
      for (let i = 0; i < childNodes.length; i++) {
        if (childNodes[i]?.children[0]?.name) {
          if (this.CorrectOrder(i, childNodes[i]?.children[0]?.name)) {
            orderValidator++
            this.correctCards[i] = true
          } else {
            this.correctCards[i] = false
            this.Penalty()
            break
          }
        }
      }

      if (orderValidator === 5) {
        this.correctAns = true
        clearInterval(this.interval)
        this.timeOut = setTimeout(this.restartGame, 10000)
      }
    },
    allowDrop (ev) {
      ev.preventDefault()
    },
    restartGame () {
      this.render++
      this.stopTimer()
      this.startCount = 0
      this.time = 0
      this.correctAns = false
      this.$emit('update:timeSend', this.time)
    }
  },
  mounted () {
    this.Cards = [
      { val: 'z', src: LetterZ, id: 1 },
      { val: 'o', src: LetterO, id: 2 },
      { val: 'o', src: LetterO, id: 3 },
      { val: 'v', src: LetterV, id: 4 },
      { val: 'u', src: LetterU, id: 5 }
    ]
    this.Cards = _.shuffle(this.Cards)
    clearTimeout(this.timeOut)
    clearInterval(this.interval)
  }
}
</script>

<style scoped>
.card {
    -webkit-box-shadow: 1px -1px 13px -2px rgba(0,0,0,0.5);
    -moz-box-shadow: 1px -1px 13px -2px rgba(0,0,0,0.5);
    box-shadow: 1px -1px 13px -2px rgba(0,0,0,0.5);
    height: 12em;
    max-width: 12.5em;
    margin-right: 1em;
    border-radius:16px;
}
.customText{
        color: #ccc;
        text-align: left;
    }
    .customCards{
        background: transparent;
        border: 4.5px dashed #41ebc5;
    }
    .card-white{
        background: white;
    }
</style>
