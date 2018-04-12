<template>
  <div class="hello">
    <ViewPage>
      <div class="one">1</div>
      <div class="two">2</div>
      <div class="three">3</div>
      <div class="four">4</div>
      <div class="four">5</div>
      <div class="four">6</div>
      
    </ViewPage>
  </div>
</template>

<script>

  import ViewPage from './ViewPage.vue'

  export default {
    name: 'HelloWorld',
    components: {
      ViewPage,
    },
    data() {
      return {}
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .one {
    width: 7.5rem;
    height: 3rem;
    background-color: #cc00ff;
  }

  .two {
    width: 7.5rem;
    height: 3rem;
    background-color: #00ccff;
  }

  .three {
    width: 7.5rem;
    height: 3rem;
    background-color: #aacc00;
  }

  .four {
    width: 7.5rem;
    height: 3rem;
    background-color: #ff0000;
  }
</style>
