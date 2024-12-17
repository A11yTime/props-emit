<template>

 <h2>{{ msg }}</h2>
 <p>Count: {{ count }}</p>
 <button @click="incrementCount">Increment Count</button>
 <button @click="updateMessage">Update Message</button>
</template>
<script>
export default {
 props: ['msg', 'count'],
 emits: ['updateData'],
 
 methods: {
  incrementCount() {
    this.$emit('updateData', {msg: this.msg, count: this.count + 1})
  },
  updateMessage() {
    this.$emit('updateData', {msg: 'Hello from HelloWorld!', count: this.count})
  }
 }
  
}

</script>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
