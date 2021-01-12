<template>
  <div>
    <v-container grid-list-xs>
      <StorageCalc/>
    </v-container>
  </div>
</template>

<script>
// @ is an alias to /src
import StorageCalc from '@/components/StorageCalc'

export default {
  name: 'Home',
  components: {
    StorageCalc
  }
}
</script>

<style lang="scss" scoped>

</style>