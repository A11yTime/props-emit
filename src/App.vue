<template>
<ProductList 
:products="products"
 @add-product="addProduct"
 @delete-product="deleteProduct"
/>
</template>
<script>
import ProductList from './components/ProductList.vue';
export default {
  components: {
    ProductList
  },
  data() {
    return {
      products: [
        { id: 1, name: 'Laptop', price: 1000, available: true },
        { id: 2, name: 'Phone', price: 500, available: false },
        { id: 3, name: 'Headphones', price: 150, available: true }
      ]
    }
  },
methods: {
  addProduct(newProduct){
  if(!newProduct.name || newProduct.price <=0) {
    alert('Please provide valid products details')
      return;
  }
  const newProductWithId = {
    ...newProduct,
    id: this.products.length + 1
      }
      this.products.push(newProductWithId)
    },
   updateProduct(updatedProduct) {
        const index = this.products.findIndex(product => product.id === updatedProduct.id);
        if (index !== -1) {
          this.products.splice(index, 1, updatedProduct);
        }
      },
   deleteProduct(productId) {
        this.products = this.products.filter(product => product.id !== productId);
      }
  }
  }
</script>
<style scoped>
</style>