<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld msg="Welcome to Your Vue.js App"/>
  <ul v-for="customer in customers" :key="customer.id">
    <li>{{customer.name}} - {{isHealthy(customer.invoices) ? 'Yes' : 'No'}}</li>
  </ul>  
</template>

<script>
import customers from './data/customers.json'
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },  
  setup() {

    const diffDays = (invoiceDate) => {
      const date1 = new Date(invoiceDate);
      const date2 = new Date();
      const timeDiff = Math.abs(date2 - date1);
      const daysDiff = Math.ceil(timeDiff / (1000 * 60 * 60 * 24));
      return daysDiff;
    }

    const isSumOfInvocesGreaterThan100k = (invoices) => {
      const sum = invoices.map(i => i.invoiceOriginalAmount).reduce((a, c) => { return a + c });
      return sum > 100000;
    }

    const isAnyOutStandingInvoicesOlderThan90Days = (invoices) => {
      const outStandingInvoices = invoices?.filter((i) => 
        i.outstandingAmount > 0 && diffDays(i.invoiceIssueDate) > 90);
      return outStandingInvoices?.length > 0;
    }

    const isHealthy = (invoices) => {    
      return isSumOfInvocesGreaterThan100k(invoices)
        && !isAnyOutStandingInvoicesOlderThan90Days(invoices);
    }

    return {
      customers,
      isSumOfInvocesGreaterThan100k,
      isAnyOutStandingInvoicesOlderThan90Days,
      isHealthy,
    }

  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;  
  color: #2c3e50;
  margin-top: 60px;
}
</style>
