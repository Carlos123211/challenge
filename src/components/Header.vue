<template>
<b-container>
    <b-row class="d-flex justify-content-between">
    <div class="HeaderText">
        Good Luck, {{name}} !
    </div>
    <div class="d-flex align-items-center">
        <p class="customText2"><b-icon-clock class="customClock"></b-icon-clock>  Your score: {{time}} seconds</p>
    </div>
    </b-row>
    <b-row  class="d-flex justify-content-between">
        <div class="customText">Pick up the rigth cards</div>
        <div class="customText">The faster the better!</div>
    </b-row>
</b-container>
</template>

<script>
export default {
  props: [
    'name',
    'time'
  ]

}
</script>

<style scoped>
    .customText{
        color: #ccc;
    }
    .customClock{
        fill: #41ebc5;
        stroke: #41ebc5;
    }
    .customText2{
        color: #3b0078;
    }
    .HeaderText{
        color:#333333;
        font-weight: bold;
    }
</style>
