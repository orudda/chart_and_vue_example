<template>
  <body>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" 
    rel="stylesheet" 
    integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" 
    crossorigin="anonymous">
    <Navbar>
    </Navbar> 
    <div class="d-flex justify-content-center">
      <Title msg="Sales by month for:" />
    </div>
      <div class="d-flex justify-content-around">
        <div>
          <b-form-select v-model="categoria" :options="categorias"></b-form-select>
        </div>
          
        <div>
          <b-form-select v-model="produto" :options="produtos"></b-form-select>
        </div>
        <div>
          <b-form-select v-model="marca" :options="marcas"></b-form-select>
        </div>
      </div>
    <div>
      <canvas id="myChart" width="100" height="100"></canvas>
    </div>
  </body>
</template>
<script>
import Title from "./components/Title.vue";
import Navbar from "./components/Navbar.vue";
import Chart from 'chart.js';

export default {
  name: "App",
  components: {
    Title,
    Navbar
  },

  data() {
    return {
      data_set: [],
      mounth:['Januery', 'February', 'March', 'April'],
      categoria: '',
      categorias: ['comida','roupa','gedget'],
      produtos:[],
      produto: '',
      marca: '',
      marcas: []
    };
  },
  mounted(){
    this.data_set = this.set_dataset(); //inicia data_set
    this.init_chart([0]); //inicia bar_chart
  },
  watch:{ //responsável por vigiar os valores. Se categoria ou produto mudar
  //  tem que atualizar os elementos da cascata
    categoria(){
      this.produtos = this.get_produto();
    },
    produto(){
      this.marcas = this.get_marca();
    },
    marca(){//atualiza barchart
      let data = this.get_sales();
      this.init_chart(data);
    }
  },
  methods:{
    onlyUnique(value, index, self) {
      return self.indexOf(value) === index;
    },
    get_produto(){
      let result = [];
      this.data_set.map(item => {
        if(item.categoria == this.categoria){
          result.push(item.producto)
        }
      })
      return result.filter(this.onlyUnique);
    },
    get_marca(){
      let result = [];
      this.data_set.map(item => {
        if(item.producto == this.produto){
          result.push(item.marca)
        }
      })
      return result.filter(this.onlyUnique);
    },
    get_sales(){
      let result = [];
      this.data_set.map(item => {
        if(item.marca == this.marca){
          result.push(item.January);
          result.push(item.February);
          result.push(item.March);
          result.push(item.April);
        }
      })
      return result.filter(this.onlyUnique);

    },
    set_dataset(){//escolhi colocar números aleatórios para não ter que ficar digitando. No final das contas os números variam mas a lógica é a mesma
      let result = [];
      result.push({categoria: 'comida', producto: 'arroz', marca:'sepe'});
      result.push({categoria: 'comida', producto: 'arroz', marca:'prato fino'});
      result.push({categoria: 'comida', producto: 'feijao', marca:'branco'});
      result.push({categoria: 'comida', producto: 'feijao', marca:'preto'});
      result.push({categoria: 'comida', producto: 'batata', marca:'baroa'});
      result.push({categoria: 'comida', producto: 'batata', marca:'doce'});
      result.push({categoria: 'roupa', producto: 'blusa', marca:'nike'});
      result.push({categoria: 'roupa', producto: 'blusa', marca:'adidas'});
      result.push({categoria: 'roupa', producto: 'calça', marca:'moletom'});
      result.push({categoria: 'roupa', producto: 'calça', marca:'jeans'});
      result.push({categoria: 'roupa', producto: 'saia', marca:'curta'});
      result.push({categoria: 'roupa', producto: 'saia', marca:'longa'});
      result.push({categoria: 'gedget', producto: 'telefone', marca:'sanmsung'});
      result.push({categoria: 'gedget', producto: 'telefone', marca:'apple'});
      result.push({categoria: 'gedget', producto: 'fone', marca:'edifier'});
      result.push({categoria: 'gedget', producto: 'fone', marca:'jbl'});
      result.push({categoria: 'gedget', producto: 'monitor', marca:'lg'});
      result.push({categoria: 'gedget', producto: 'monitor', marca:'positivo'});
      for(let i=0; i<result.length; i++){
        result[i]['January'] = Math.floor((Math.random() * 100) + 1);
        result[i]['February'] = Math.floor((Math.random() * 100) + 1);
        result[i]['March'] = Math.floor((Math.random() * 100) + 1);
        result[i]['April'] = Math.floor((Math.random() * 100) + 1);
      }
      return result;
    },
    
    init_chart(data_){
      const ctx = document.getElementById('myChart')
      this.data_set = this.set_dataset();
      const myChart = new Chart(ctx, {
        type: 'bar',
        data: {
          labels: this.mounth,
          datasets: [{
            label: '# of Sales',
            data: data_,
            backgroundColor: [
              'rgba(255, 99, 132, 0.2)',
              'rgba(54, 162, 235, 0.2)',
              'rgba(255, 206, 86, 0.2)',
              'rgba(75, 192, 192, 0.2)'
            ],
            borderColor: [
              'rgba(255, 99, 132, 1)',
              'rgba(54, 162, 235, 1)',
              'rgba(255, 206, 86, 1)',
              'rgba(75, 192, 192, 1)'
            ],
            borderWidth: 1
          }]
        },
        options: {
          scales: {
            y: {
              beginAtZero: true
            }
          }
        }
      });
      myChart;
    }
  }
};
</script>

<style>
</style>