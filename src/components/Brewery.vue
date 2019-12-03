<template>
  <div>
    {{ brewery.name }}
  </div>
</template>

<script>
export default {
  name: 'Brewery',
  props: {
    brewery: Object,
  },
}
</script>

<style>

</style>