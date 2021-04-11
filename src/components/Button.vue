<template>
  <button
    :class="['btn', color]"
    @click='click()'
  >
    {{ text }}
  </button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    text: String,
    color: String
  },
  methods: {
    click () {
      this.$emit('btn-click')
    }
  }
}
</script>
