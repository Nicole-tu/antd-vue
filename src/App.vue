<template>
  <div id="app">
    <div class="main-wrapper">
      <div class="ant-row">
        <tab-panel />
      </div>
    </div>
    <a-back-top />
  </div>
</template>

<script>
import TabPanel from './components/TabPanel.vue'
export default {
  name: 'app',
  components: {
    TabPanel
  }
}
</script>