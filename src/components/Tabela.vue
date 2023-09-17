<template>
  <div class="container text-center tabela">
    <div class="row">
      <div class="col mb-5">
        <p class="text-start">Foram copiados os dados da página 'http://www.cedip.caixa/aplicativos/des/psi-13608/files/psi-13608.json' e inseridos no arquivo de nome 'lancamentos.json' dentro da pasta 'public'. O código completo da aplicação encontra-se <a target="_blank" href="https://github.com/Junio-Cadete/psi-cedip-code"> aqui</a>.</p>
      </div>
    </div>
    <div class="row">
      <div class="col">
        <table class="table">
          <thead>
            <tr class="altura-linha">
              <th scope="col" v-for="(titulo,i) in cabecalhoTabela" :key="i">{{titulo}}</th>
            </tr>
          </thead>
          <tbody>
            <tr class="altura-linha" v-for="(lancamento,index) in lancamentosTrataveis" :key="index">
              <td>{{lancamento.dataEfetiva}} </td>
              <td>{{lancamento.valor}} </td>
              <td>-{{lancamento.valor}} </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import dadosLancamentos from '../../public/lancamentos.json'

export default {
  name: 'CompPsi',
  data(){
    return{
      lancamentos: [],
      cabecalhoTabela: ['Data Efetiva', 'Crédito', 'Débito'],
      lancamentosTrataveis: []
    }
  },
  computed:{
    lancamentosTrataveis(){
      return this.obterDadosTrataveis (this.lancamentos)
    }
  },  
   methods:{
    carregarDados(){
      try{
        this.lancamentos = dadosLancamentos
      }
      catch(erro){
        console.log("Erro ao carregar os dados", erro )
      }
    },
    obterDadosTrataveis(data){
      const trataveis = [];
      const tratados = [];

      const dados = data.map( d => { 
        const {dataEfetiva, valor} = d
        const int = valor.replace(',','.')
        const data = dataEfetiva.split('-').reverse().join('/')
          return {'dataEfetiva': data, 'valor':  parseFloat(int) }})     
      
        dados.forEach((lancamento, index) => {
          if (tratados.includes(index)) {         
            return;  // Se o lançamento já foi tratado, pule para o próximo..
          }    
          const { dataEfetiva, valor } = lancamento;            
          const lancamentoInvertido = dados.find((l, i) => {
            return i !== index && l.dataEfetiva === dataEfetiva && l.valor === -valor && !tratados.includes(i);
          }); // Verifica se há um lançamento correspondente com sinal invertido
      
          if (lancamentoInvertido) { // Se encontrarmos um lançamento invertido, adicionamos ambos aos tratados           
            tratados.push(index);
            tratados.push(dados.indexOf(lancamentoInvertido));
            trataveis.push(lancamento);
            trataveis.push(lancamentoInvertido);
            
          }
        });

        const lancamentosTrataveisPositivos = trataveis.filter( l => l.valor > 0)
        const lancamentosTrataveisFinal = lancamentosTrataveisPositivos.map( l => { 
          const {valor} = l
          const valor1 = valor.toFixed(2).toString().replace('.',',')
          return {'dataEfetiva': l.dataEfetiva, 'valor': valor1 }} ) ;
        
        return lancamentosTrataveisFinal;
    }
  },  
  created(){
    this.carregarDados()
  } 
}
</script>

<style scoped>
  .altura-linha{
    height: 60px;
  }
  .altura-linha th, .altura-linha td{
    font-size: 35px;
  }
  .tabela{
    margin-top: 100px;
  }
</style>