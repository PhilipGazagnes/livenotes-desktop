<template>
  <div
    class="container"
    @click="$emit('click')"
  >
    <img class="logo" src="../assets/logo-fun-live.svg">
  </div>
</template>

<style lang="postcss">
.container {
  position: fixed;
  height: 100vh;
  width: 100%;
  top: 0;
  left: 0;
  background: #010223;
  display: flex;
  justify-content: center;
  align-items: center;
}
.logo {
  width: 60%;
}
</style>