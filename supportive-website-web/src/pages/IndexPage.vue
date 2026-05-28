<template>
  <q-page class="row items-top justify-evenly" style="background-color: #F8E8C7;">
    <div> 
      <img src="../assets/kitty.webp" style="height: 200px; width: auto;">
      <div class="title">A cozy place for you</div>
    </div>
  </q-page>

    <div class="red-circle"></div>
    <div class="red-circle2"></div>
</template>

<script setup lang="ts">

</script>

<style lang="css">
.red-circle {
  width: 400px;
  height: 400px;
  background-color: #F77E53;
  position: absolute;
  top: 70%;
  left: -5%;
  border-radius: 200px;
  z-index: 0;
}

.red-circle2 {
  width: 80px;
  height: 80px;
  background-color: #F77E53;
  position: absolute;
  top: 50%;
  left: 80%;
  border-radius: 200px;
  z-index: 0;
}

.title {
  margin: 0;
  padding: 0;
  position: relative;
  z-index: 1;

  font-size: clamp(35px, 4vw, 50px);

  white-space: normal;
  overflow-wrap: break-word;
  word-break: break-word;

  max-width: 100%;
}
</style>
