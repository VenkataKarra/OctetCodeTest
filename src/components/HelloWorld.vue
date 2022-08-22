<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <p>
      This page is displaying customer list from data/customers.json file.      
    </p>    
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>


