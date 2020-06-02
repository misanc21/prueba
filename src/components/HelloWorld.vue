<template>
  <div class="hello">
    <h1>{{ titulo }}</h1>
    <hr>
    <b-row class="mt-5">
      <b-col md="12" lg="6" class="div-addArticle">
          <h3 class="pt-2">Add a new article</h3>
      </b-col>


      <b-col md="12" lg="6" class="div-purchase">
        <h3 class="pt-2">Purchase</h3>
        <b-row>
          <b-col>
            <b-table responsive :items="items" :fields="fieldsItem">
              <template v-slot:cell(index)="data">
                {{data.index+1}}
              </template>
              <template v-slot:cell(actions)="data">
                <b-icon-trash class="i-del" @click="deleteItem(data.item.index)"></b-icon-trash>
              </template>
            </b-table>
          </b-col>
        </b-row>
        <b-row align-v="stretch" >
          <b-col align-self="stretch">
            <b-button size="lg" variant="success"><b-icon-cart></b-icon-cart> Buy!</b-button>
          </b-col>
        </b-row>
      </b-col>
    </b-row>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HelloWorld',
  data () {
    return {
      titulo: 'Articles',
      articles: [],
      items: [],
      fieldsItem: ['index', 'sku', 'name', 'quantity', 'price', 'actions']
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

    },
    deleteItem(index){
      this.$swal.fire({
        title: '¿Are you sure?',
        text: "¿Do you want to delete this article?",
        icon: 'warning',
        showCancelButton: true,
        confirmButtonColor: '#3085d6',
        cancelButtonColor: '#d33',
        confirmButtonText: 'Yes, delete it!'
        }).then((result) => {
            //Send request to server
            if(result.value){
                this.items.splice(index, 1)
                swal.fire(
                'Deleted!',
                'this article Has been deleted.',
                'success'
                )
        }
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
.i-del{
  color: #42b983;
}
.div-addArticle{
  width: 100%;
  min-height: 35em;
  background: #e0e0e0;
}
.div-purchase{
  width: 100%;
  min-height: 35em;
  background: #f1eeee;
}
</style>
