<template>
  <div>
    <Nuxt />
  </div>
</template>

<style lang="sass">
html
  
  font-size: 18px
  word-spacing: 1px
  -ms-text-size-adjust: 100%
  -webkit-text-size-adjust: 100%
  -moz-osx-font-smoothing: grayscale
  -webkit-font-smoothing: antialiased
  box-sizing: border-box
  min-height: 100%

body, #__nuxt, #__layout
  min-height: 100vh

*,
*::before,
*::after 
  box-sizing: border-box
  margin: 0

.wrapper
  @include column
  width: 100%
  min-height: 100%
  justify-content: center

</style>

<script>
  
  export default {
  
}
</script>