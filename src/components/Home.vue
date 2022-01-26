<template>
  <div >
    <div>
      <img  src="../assets/img/bmg/2.jpg" alt="?" style=" height: 100%;width: 100%;position: absolute;opacity:0.7">
    </div>
    <song-audio />
      <the-header></the-header>
      <the-aside
      ></the-aside>
      <div style="background: url('../assets/img/bmg/4.png')"  class="content-box" :class="{'content-collapse':collapse}">
        <div style="background: url('../assets/img/bmg/1.jpg')" class="content">
          <router-view style="background: url('../assets/img/bmg/4.png')"></router-view>
        </div>
      </div>

  </div>
</template>

<script>
import SongAudio from './SongAudio'
import TheHeader from './TheHeader.vue'
import TheAside from './TheAside.vue'
import _ctrEvent from '../assets/js/ctr-event'

export default {
  components: {
    SongAudio,
    TheHeader,
    TheAside
  },
  data () {
    return {
      collapse: false
    }
  },
  created () {
    _ctrEvent.$on('collapse', msg => {
      this.collapse = msg
    })
  }
}
</script>

<style>
</style>
