<template>
  <div id="app">
    <MainBody/>
  </div>
</template>

<script>
  import MainBody from "./components/MainBody";

  export default {
    name: 'App',
    components: {
      MainBody
    }
  }
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Orbitron&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Lexend+Deca&family=Lobster+Two&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Raleway:wght@500&display=swap');
@import url('https://fonts.googleapis.com/css2?family=Grenze+Gotisch&family=Raleway&display=swap');

/* @import "~video-react/styles/scss/video-react"; */

img
{
  object-fit: contain;
}
</style>
