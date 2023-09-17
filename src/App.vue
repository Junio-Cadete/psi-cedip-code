<template>
  <div>
    <NavBar />
    <Tabela />
    
  </div>
</template>

<script>
import Tabela from './components/Tabela.vue'
import NavBar from './components/NavBar.vue'

export default {
  name: 'App',
  components: {
    Tabela,
    NavBar
  }
}
</script>

<style>

</style>
