<template>
  <q-page class="flex flex-center">
    <q-img
      src="statics/office.jpg"
      class="header-image absolute-top"
    />
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex'
}
</script>

<style lang="scss">
.header-image {
  height: 100%;
  z-index: -1;
  opacity: 0.8;
}
</style>
