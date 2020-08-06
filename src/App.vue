<template>
  <div id="app">
    <div class="siimple-h2">
      {{ title }}
    </div>
    <div class="body">
      <NewLink/>
      <Feed/>
    </div>
  </div>
</template>

<script>
import NewLink from '../src/components/NewLink'
import Feed from '../src/components/Feed'

export default {
  name: 'app',
  data () {
    return {
      title: 'Share links in realtime, because why not?'
    }
  },
  components: {
    NewLink,
    Feed
  }
}
</script>

<style>
 #app {
    display: flex;
    align-items: center;
    flex-direction: column;
 }
 .body {
   margin-top: 4em;
   display: flex;
   justify-content: space-between;
   width: 100%;
 }

</style>

