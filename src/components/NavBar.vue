<template>

<nav class="navbar bg-body-tertiary custom-navbar">
  <div class="container-fluid">
            <a class="navbar-brand" href="#">
                <img src="../../public/caixa.png" alt="CEF" width="34" height="24">
            </a>
    <span class="navbar-text title fs-5 text-light fw-bold">
      PSI CEDIP Assistente Sênior - Elson Júnio Cadete Pedra (c143249)
    </span>
  </div>
</nav>

</template>

<script>
export default {

}
</script>
<style scoped>
.title{
    margin: 0 auto;
}
.custom-navbar{
    background: linear-gradient(to right, #63ffa942, rgba(0,92,169,0.9));
}

@media (max-width: 720px){
    .title{
        display: none;
    }
}
</style>