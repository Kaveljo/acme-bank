<template>
  <Header></Header>
  <router-view class="content"/>
</template>

<script>
import Header from './components/layout/Header.vue'

export default {
  components:{
    Header
  }
}
</script>


<style>
@import url('https://fonts.googleapis.com/css2?family=Open+Sans:wght@300;400;600;700&display=swap');
html{
  font-family: 'Open Sans', sans-serif;
}

body{
  margin: 0;
}

p{
  margin: 0; 
}

h1,h2,h3,h4,h5,h6{
  margin: 0;
}
.content{
  max-width: 1440px;
  margin: 20px auto 0;
}
</style>
