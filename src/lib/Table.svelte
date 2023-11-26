<script lang="ts">
    import { onMount } from 'svelte';
    import Navbar from './Navbar.svelte'
    import Footer from './Footer.svelte'

    let data: any[] = [];
    let drinks: any[] = [];
    export let params: any = {};

onMount(async () => {
    

    const url = `${import.meta.env.VITE_API_ROOT}/drink/list`;

    const headers: Headers = new Headers();
    headers.set('Content-Type', 'application/json');
    headers.set('Accept', 'application/json');

    const request: RequestInfo = new Request(url, {
        method: 'GET',
    });

    const res = await fetch(request);
    const result = await res.json();

    if(result.status == 200){
        data = result.data;
    }

    getOrders();


});


async function getOrders(){
    const url = `${import.meta.env.VITE_API_ROOT}/order/tablelist/all?tid=${params.id}`;

    const headers: Headers = new Headers();
    headers.set('Content-Type', 'application/json');
    headers.set('Accept', 'application/json');

    const request: RequestInfo = new Request(url, {
        method: 'GET',
    });

    const res = await fetch(request);
    const result = await res.json();

    if(result.status == 200){
        drinks = result.data;
    }
    console.log(drinks);
    drinks = drinks;
}


async function orderDrink(id: number){
    const url = `${import.meta.env.VITE_API_ROOT}/order/add`;

    let body = {
        tid: params.id,
        did: id
    }

    const headers: Headers = new Headers();
    headers.set('Content-Type', 'application/json');
    headers.set('Accept', 'application/json');

    const request: RequestInfo = new Request(url, {
        method: 'POST',
        headers: headers,
        body: JSON.stringify(body)
    });

    const res = await fetch(request);
    const result = await res.json();

    if(result.status == 200){
        getOrders();
    }
}


async function cancelOrder(id: number){
    const url = `${import.meta.env.VITE_API_ROOT}/order/delete/${id}`;

    const headers: Headers = new Headers();
    headers.set('Content-Type', 'application/json');
    headers.set('Accept', 'application/json');

    const request: RequestInfo = new Request(url, {
        method: 'DELETE',
        headers: headers
    });

    const res = await fetch(request);
    const result = await res.json();

    if(result.status == 200){
        getOrders();
    }
}


</script>

<!-- Header-->
<header class="bg-dark py-5">
    <div class="container px-4 px-lg-5 my-5">
        <div class="text-center text-white">
            <h1 class="display-4 fw-bolder">This is table {params.id}</h1>

            

            <p class="lead fw-normal text-white-50 mb-0">Here you can order drinks for your table</p>
            <hr class="mb-6"/>

            
            <div class="row justify-content-center d-flex mt-6 mb-0">
                <div class="col-md-6">
                    
                    <p class="lead fw-normal text-white-50">Your current orders</p>
                </div>

                <div class="col-md-6">
                    
                    <p class="lead fw-normal text-white-50">All of your orders:  
                        <button class="btn btn-outline-warning" on:click={getOrders}>Refresh <i class="bi bi-arrow-clockwise"></i></button>
                    </p>
                </div>
            </div>

            <div class="row justify-content-center d-flex mt-6 mb-0">
                <div class="col-md-6">
                    <table class="table table-dark table-striped">
                        <thead>
                        <tr>
                            <th scope="col">#</th>
                            <th scope="col">Drink</th>
                            <th scope="col">Price</th>
                            <th scope="col"></th>
                        </tr>
                        </thead>
                        <tbody>
                            {#each drinks as {T_ORDER_ID, MD_DRINNK_NAME, MD_DRINK_PRICE, T_ORDER_ISACTIVE}, i}
                            {#if T_ORDER_ISACTIVE == "1"}
                                <tr class="mb-4">
                                    <th scope="row">{i+1}</th>
                                    <td>{MD_DRINNK_NAME}</td>
                                    <td>{MD_DRINK_PRICE+"€"}</td>
                                    <td><button class="btn btn-sm btn-outline-danger" on:click={()=>cancelOrder(T_ORDER_ID)}><i class="bi bi-x-circle"></i></button></td>
                                </tr>
                            {/if}
                            {/each}
                        </tbody>
                    </table>
                </div>
                <div class="col-md-6">
                    <table class="table table-secondary table-striped">
                        <thead>
                        <tr>
                            <th scope="col">#</th>
                            <th scope="col">Drink</th>
                            <th scope="col">Price</th>
                        </tr>
                        </thead>
                        <tbody>

                            {#each drinks as {MD_DRINNK_NAME, MD_DRINK_PRICE, T_ORDER_ISACTIVE}, i}
                                <tr>
                                    <th scope="row">{i+1}</th>
                                    <td>{MD_DRINNK_NAME}</td>
                                    <td>{MD_DRINK_PRICE+"€"}</td>
                                </tr>
                            {/each}

                        </tbody>
                    </table>
                </div>

            </div>


        </div>
    </div>
</header>
<!-- Section-->
<section class="py-5">
    <div class="container px-4 px-lg-5 mt-5">
        <div class="row gx-4 gx-lg-5 row-cols-2 row-cols-md-3 row-cols-xl-4 justify-content-center">


            {#each data as {MD_DRINK_ID, MD_DRINNK_NAME, MD_DRINK_DESC ,MD_DRINK_PRICE }, i }
                <div class="col mb-5">
                    <div class="card h-100">
                        <div class="badge bg-dark text-white position-absolute" style="top: 0.5rem; right: 0.5rem">{MD_DRINK_ID}</div>
                        <!-- Product details-->
                        <div class="card-body p-4">
                            <div class="text-center">
                                <!-- Product name-->
                                <h5 class="fw-bolder">{MD_DRINNK_NAME}</h5>
                                <!-- Product price-->
                                {MD_DRINK_DESC}
                            </div>
                        </div>
                        <!-- Product actions-->
                        <div class="card-footer p-4 pt-0 border-top-0 bg-transparent">
                            <div class="text-center"><button class="btn btn-outline-dark mt-auto" on:click={()=>{orderDrink(MD_DRINK_ID)}}>Order</button></div>
                        </div>
                    </div>
                </div>
            {/each}




        </div>
    </div>
</section>

<Footer/>