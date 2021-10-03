<template>
  <div>
    <h1>{{ text }}</h1>
  </div>
</template>

<script lang="ts">
import { ref, defineComponent } from '@nuxtjs/composition-api'

export default defineComponent({
  setup() {
    const text = 'Hello, World!test'
    const text2 = ref('Hello, World!')

    return {
      text,
    }
  },
})
</script>
