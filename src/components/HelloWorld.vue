<template>
  <div class="hello">
    <h1>{{ titulo }}</h1>
    <div id="div-compra">
      <table id="tbla-compra">
        <thead>
          <th>SKU</th>
          <th>Name</th>
          <th>Quantity</th>
          <th>Price</th>
        </thead>
        <tbody>
          <tr v-for>

          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  data () {
    return {
      titulo: 'Articulos para comprar',
      articles: [],
      items: []
    }
  },
  methods: {
    getArticles(){
      const config = {
        headers:{
          Authorization:`Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJkM2NIVUVibVJoc1EzeXhNbzV2VnliSTFzaDZCSDJZRCIsImlhdCI6MTU4NTkzMjYzNDU0OH0.tMSht_M3ryQl5IqCirhYR1gb8j3FQ26vILT4Qpx4XrdFz-zUmqbgFYiKTaZHPpB85etRIMhxVoZf6tOrHy0fnA`
        }
      }
      axios.get(`https://eshop-deve.herokuapp.com/api/v2/orders/2195721781388`,
                config)
      .then(({data})=>{
        this.articles = data
        this.items = this.articles.order.items
        })

    }
  },
  mounted(){
    this.getArticles();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
