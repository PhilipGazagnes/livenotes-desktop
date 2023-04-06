<template>
  <div
    class="container"
    @click="$emit('click')"
  >
    <img class="logo" src="../assets/logo-fun-live.svg">
    <div class="website">www.fun-live.fr</div>
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
  flex-direction: column;
}
.logo {
  width: 60%;
}
.website {
  color: white;
  font-size: 6em;
}
</style>