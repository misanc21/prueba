<template>
  <div class="hello">
    <h1>{{ titulo }}</h1>
    <hr>
    <b-row class="mt-5">
      <b-col md="12" lg="6" class="div-addArticle">
          <h3 class="pt-2">Add a new article</h3>
          <b-row>
            <b-col>
              <b-form @submit="addArticle()" @reset="resetForm()" class="p-2">
                <b-form-group id="input-group-1" label="SKU" label-for="lbl-sku">
                  <b-form-input id="ipt-sku" v-model="form.sku" type="text"  placeholder="SKU" :state="validationSku"></b-form-input>
                  <b-form-invalid-feedback :state="validationSku">
                    you need to fill this field (At least 3 characters)
                  </b-form-invalid-feedback>
                  <b-form-valid-feedback :state="validationSku">
                    Looks Good.
                  </b-form-valid-feedback>
                </b-form-group>
                <b-form-group id="input-group-2" label="Name" label-for="lbl-name" description="Add the name">
                  <b-form-input id="ipt-name" v-model="form.name" type="text"  placeholder="Name" :state="validationName"></b-form-input>
                  <b-form-invalid-feedback :state="validationName">
                    you need to fill this field (At least 5 characters)
                  </b-form-invalid-feedback>
                  <b-form-valid-feedback :state="validationName">
                    Good Name :).
                  </b-form-valid-feedback>
                </b-form-group>
                <b-row>
                  <b-col>
                    <b-form-group id="input-group-3" label="Quantity" label-for="lbl-quantity" description="Add the quantity">
                      <b-form-input id="ipt-quantity" v-model="form.quantity" type="number"  placeholder="quantity" :state="validationNumbers"></b-form-input>
                      <b-form-invalid-feedback :state="validationNumbers">
                        you need to fill this field (more than zero)
                      </b-form-invalid-feedback>
                      <b-form-valid-feedback :state="validationNumbers">
                        Looks good.
                      </b-form-valid-feedback>
                    </b-form-group>
                  </b-col>
                  <b-col>
                    <b-form-group id="input-group-4" label="price" label-for="lbl-price" description="Add the price">
                      <b-form-input id="ipt-price" v-model="form.price" type="number"  placeholder="quantity" :state="validationPrice"></b-form-input>
                      <b-form-invalid-feedback :state="validationPrice">
                        you need to fill this field (more than zero)
                      </b-form-invalid-feedback>
                      <b-form-valid-feedback :state="validationPrice">
                        Looks good.
                      </b-form-valid-feedback>
                    </b-form-group>
                  </b-col>
                </b-row>
                <b-button type="reset" variant="danger">Reset</b-button>
                <b-button type="submit" variant="primary">Add to cart</b-button>
              </b-form>
            </b-col>
          </b-row>
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
      purchasenumber: '',
      items: [],
      fieldsItem: ['index', 'sku', 'name', 'quantity', 'price', 'actions'],
      form:{
        sku: '',
        name: '',
        quantity: 0,
        price: 0
      }
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
                  this.$swal.fire(
                  'Deleted!',
                  'this article Has been deleted.',
                  'success'
                  )
          }
      })
    },
    resetForm(){
      this.form ={
        sku: '',
        name: '',
        quantity: 0,
        price: 0
      }
    },
    addArticle(){
      let {sku, name, quantity, price} = this.form
      if(sku.length < 3 || name.length < 6 || quantity *1 <= 0 || price *1 <= 0 ){
        this.$swal.fire(
          `Your article couldn't be added`,
          `Verify all the required fields`,
          'error'
        )
      }else{
        this.items.push(this.form)
        this.$swal.fire(
          `Your article has been added`,
          `continue with the purchase`,
          'success'
        )
      }
    }
  },
  computed: {
      validationSku() {
        return this.form.sku.length >= 3
      },
      validationName() {
        return this.form.name.length >= 5
      },
      validationNumbers() {
        return this.form.quantity.length > 0 && this.form.quantity > 0
      },
      validationPrice() {
        return this.form.price.length > 0 && this.form.price > 0
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
