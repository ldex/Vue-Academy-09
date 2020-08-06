<template>
  <div>
    <section v-if="error">
      {{error.message}}
    </section>
    <section v-else>
      <div v-if="loading">
          <h2>Loading...</h2>
      </div>
      <div v-else>
        <h2>{{product.name}}</h2>
        <img :src="product.imageUrl ? product.imageUrl : 'https://placeimg.com/200/200/tech'" width="200" style="float:right" />
        <h3>{{product.description}}</h3>
        <p>Price: {{product.price}}</p>
        <p>Fixed price? {{product.fixedPrice}}</p>
        <p>Discontinued? {{product.discontinued}}</p>
        <p>Modified date: {{product.modifiedDate | formatDate('MM/DD/YYYY hh:mm')  }}</p>              
        <p>
          <v-btn @click="deleteProduct">Delete</v-btn>
        </p>
      </div>
    </section>
  </div>
</template>

<script>
import { mapState } from 'vuex'

    export default {
        data () {
            return {
                error: null,               
                componentName: "Product Details"
            }
        },
        computed: {
            product() {
                return this.$store.state.product; 
            },
            ...mapState({loading:'isLoading'})
        },
        props: {
            id: {
                type:Number,
                required:true
            }
        },
        created() {
          this.fetchProduct(this.id);
        },
        methods: {
            fetchProduct(id) {
                this.$store.dispatch('fetchProduct', id);
            },
            deleteProduct() {
              if(window.confirm('Are you sure ??')) {
                this.$store.dispatch('deleteProduct', this.product.id)
                .then(
                  () => this.$router.push({ name: "products" })
                )
                .catch(
                  error => console.error('Could not delete product!' + error)
                )

              }
            }
        },
    }
</script>

<style lang="scss" scoped>

</style>