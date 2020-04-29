<template>
  <div
    class="spinner-border spinner"
    role="status"
    style="width: 2rem; height: 2rem;"
  >
    <span class="sr-only">Loading...</span>
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
.spinner {
  position: fixed;
  top: 50%;
  left: 50%;
  z-index: 9999;
}
</style>
