<template>
  <TheNavbar/>
  <router-view/>
</template>

<script>
import TheNavbar from './components/TheNavbar.vue';
export default {
    components: { TheNavbar }
}
</script>

<style lang="scss">

</style>
