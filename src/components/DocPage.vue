<template>
  <section class="doc-main-container">
    <div ref="doc" >
      <slot>
        You can place the component inside the doc balises
      </slot>
    </div>
  </section>
</template>

<script>
export default {

  // MOUNTED
  mounted() {
    this.getDocBody()
  }

  // METHODS
  ,methods: {
    getDocBody(){
      console.log(this.$slots.default());
    } 
  },
  
}
</script>

<style scoped>

</style>