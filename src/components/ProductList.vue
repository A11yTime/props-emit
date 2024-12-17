<template>
    <h2 class="header">Product List</h2>
  
    <!-- Add product form -->
    <div class="add-product-form">
      <input
        v-model="newProduct.name"
        type="text"
        placeholder="Enter product name"
        class="input-field"
        aria-label="Product Name"
      />
      <input
        v-model.number="newProduct.price"
        type="number"
        placeholder="Enter product price"
        class="input-field"
        aria-label="Product Price"
      />
      <label class="checkbox-label">
        Available:
        <input v-model="newProduct.available" type="checkbox" />
      </label>
      <button @click="onAddProduct" class="add-btn">Add Product</button>
    </div>
  
    <!-- Product list -->
    <div class="product-list">
      <ul>
        <li v-for="(product, index) in products" :key="index" class="product-item">
          <div class="product-card">
            <div v-if="product.isEditing" class="product-edit-form">
              <!-- Editable form for product -->
              <form @submit.prevent="saveEdit(product)" aria-labelledby="edit-product-form">
                <label for="edit-name-{{ product.id }}">Product Name</label>
                <input
                  id="edit-name-{{ product.id }}"
                  v-model="product.name"
                  type="text"
                  placeholder="Enter new product name"
                  class="input-field"
                  required
                  aria-required="true"
                />
  
                <label for="edit-price-{{ product.id }}">Product Price</label>
                <input
                  id="edit-price-{{ product.id }}"
                  v-model.number="product.price"
                  type="number"
                  placeholder="Enter new product price"
                  class="input-field"
                  required
                  aria-required="true"
                />
  
                <label for="edit-available-{{ product.id }}">Available</label>
                <input
                  id="edit-available-{{ product.id }}"
                  v-model="product.available"
                  type="checkbox"
                  class="input-field"
                  aria-checked="product.available"
                />
  
                <button type="submit" class="save-btn">Save Changes</button>
                <button @click="cancelEdit(product)" type="button" class="cancel-btn">Cancel</button>
              </form>
            </div>
  
            <div v-else class="product-info">
              <h3 class="product-name">{{ product.name }}</h3>
              <p class="product-price">${{ product.price }}</p>
              <p class="product-availability">
                <span :class="product.available ? 'available' : 'out-of-stock'">
                  {{ product.available ? 'Available' : 'Out of Stock' }}
                </span>
              </p>
              <div class="product-actions">
                <button @click="startEdit(product)" class="edit-btn">Edit</button>
                <button @click="deleteProduct(product.id)" class="delete-btn">Delete</button>
              </div>
            </div>
          </div>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    name: "ProductList",
    props: {
      products: {
        type: Array,
        required: true,
      },
    },
    emits: ["add-product", "update-product"],
    data() {
      return {
        newProduct: {
          name: "",
          price: null,
          available: false,
        },
      };
    },
    methods: {
      // Add a new product
      onAddProduct() {
        if (this.newProduct.name && this.newProduct.price > 0) {
          this.$emit("add-product", { ...this.newProduct });
          this.newProduct.name = "";
          this.newProduct.price = null;
          this.newProduct.available = false;
        } else {
          alert("Please fill out all fields correctly.");
        }
      },
  
      // Start editing a product
      startEdit(product) {
        product.isEditing = true;
      },
  
      // Cancel editing a product
      cancelEdit(product) {
        product.isEditing = false;
        // Restore original product data if editing is canceled
        this.$nextTick(() => {
          product.name = product.originalName;
          product.price = product.originalPrice;
          product.available = product.originalAvailable;
        });
      },
  
      // Save changes to a product
      saveEdit(product) {
        this.$emit("update-product", { ...product });
        product.isEditing = false;
      },
      deleteProduct(productId) {
        this.$emit('delete-product', productId)
      }
    },
  };
  </script>
  
  <style scoped>
  .header {
    text-align: center;
    font-size: 2rem;
    color: #333;
    margin-bottom: 20px;
  }
  
  .add-product-form {
    display: flex;
    flex-direction: column;
    align-items: center;
    margin-bottom: 20px;
  }
  
  .input-field {
    padding: 10px;
    margin: 10px;
    width: 250px;
    font-size: 16px;
    border: 1px solid #ddd;
    border-radius: 5px;
    outline: none;
    transition: border-color 0.3s ease;
  }
  
  .input-field:focus {
    border-color: #6c757d;
  }
  
  .checkbox-label {
    font-size: 16px;
    margin-bottom: 10px;
  }
  
  .add-btn {
    padding: 10px 20px;
    background-color: #28a745;
    color: white;
    border: none;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  .add-btn:hover {
    background-color: #218838;
  }
  
  .product-list {
    display: flex;
    flex-direction: column;
    gap: 15px;
    padding: 20px;
  }
  
  .product-item {
    list-style-type: none;
  }
  
  .product-card {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #fff;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
    transition: transform 0.3s ease;
  }
  
  .product-card:hover {
    transform: scale(1.05);
  }
  
  .product-info {
    flex: 1;
  }
  
  .product-name {
    font-size: 1.25rem;
    font-weight: bold;
    color: #333;
  }
  
  .product-price {
    font-size: 1.1rem;
    color: #555;
  }
  
  .product-availability {
    font-size: 1rem;
    font-weight: bold;
  }
  
  .available {
    color: #28a745;
  }
  
  .out-of-stock {
    color: #dc3545;
  }
  
  .product-actions {
    display: flex;
    gap: 10px;
  }
  
  .edit-btn {
    padding: 5px 10px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    font-size: 14px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  .edit-btn:hover {
    background-color: #0056b3;
  }
  
  .product-edit-form {
    width: 100%;
  }
  
  .product-edit-form input {
    width: 100%;
    margin-bottom: 10px;
  }
  
  .save-btn,
  .cancel-btn {
    padding: 10px 20px;
    font-size: 14px;
    border-radius: 5px;
    cursor: pointer;
    transition: background-color 0.3s ease;
  }
  
  .save-btn {
    background-color: #28a745;
    color: white;
  }
  
  .save-btn:hover {
    background-color: #218838;
  }
  
  .cancel-btn {
    background-color: #dc3545;
    color: white;
  }
  
  .cancel-btn:hover {
    background-color: #c82333;
  }
  </style>
  