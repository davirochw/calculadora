<template>
  <button class="button"
      @click="$emit('onClick', label)">
      {{ label }}
  </button>
</template>

<script>
export default {
  props:{ 
    label: {}
  }
}
</script>

<style>

</style>