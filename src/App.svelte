<script lang="ts">
  import Tables from './lib/Tables.svelte'
  import Table from './lib/Table.svelte'
  import TableOrders from './lib/TableOrders.svelte'
  import Orders from './lib/Orders.svelte'
  import TableAdd from './lib/TableAdd.svelte'
  import DrinkAdd from './lib/DrinkAdd.svelte'
  import Router from 'svelte-spa-router'

  const routes = {
    // Exact path
    '/': Tables,
    '/orders': Orders,
    '/table/add': TableAdd,
    '/drink/add': DrinkAdd,

    // Using named parameters, with last being optional
    '/table/:id/': Table,

    '/table/orders/:id/': TableOrders,

    // Catch-all
    // This is optional, but if present it must be the last
    //'*': NotFound,
  }
</script>

<main>
  <Router {routes}/>
</main>

<style>

</style>
