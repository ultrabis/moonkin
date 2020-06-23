<template> <div></div></template>

<style scoped></style>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'

const Props = Vue.extend({
  props: {
    data: Object
  }
})

@Component
export default class MyComponent extends Props {
}
</script>
