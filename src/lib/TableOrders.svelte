<script lang="ts">
    import { onMount } from 'svelte';
    import Navbar from './Navbar.svelte'
    import Footer from './Footer.svelte'

    let data: any[] = [];

    export let params: any = {};

onMount(async () => {
    getOrders();
});

async function getOrders() {
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
        data = result.data;
    }

    console.log(data);
}

async function finishOrder(id: number){
    const url = `${import.meta.env.VITE_API_ROOT}/order/finish`;

    let body = {
        oid: id
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

async function clearTable(){
    const url = `${import.meta.env.VITE_API_ROOT}/order/delete/table/${params.id}`;


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
<!-- Navigation-->
<Navbar active={0}/>
<!-- Header-->
<header class="bg-dark py-5">
    <div class="container px-4 px-lg-5 my-5">
        <div class="text-center text-white">
            <h1 class="display-4 fw-bolder">This is table {params.id}</h1>
            <p class="lead fw-normal text-white-50 mb-0">Here you can see what this table ordered</p>
            <hr class="mb-6"/>
            <div class="row justify-content-center">
                <div class="col-md-6">
                    <button class="btn btn-outline-warning" on:click={getOrders}>Refresh <i class="bi bi-arrow-clockwise"></i></button>
                </div>
                <div class="col-md-6">
                    <button class="btn btn-outline-danger" on:click={clearTable}>Clear Table <i class="bi bi-stars"></i></button>
                </div>
            </div>
        </div>
    </div>
</header>
<!-- Section-->
<section class="py-5">
    <div class="container px-4 px-lg-5 mt-5">
        <div class="row gx-4 gx-lg-5 row-cols-2 row-cols-md-3 row-cols-xl-4 justify-content-center">

            <p class="lead fw-normal text-black-50">Active orders:</p>

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
                    {#each data as {T_ORDER_ID, MD_DRINNK_NAME, MD_DRINK_PRICE, T_ORDER_ISACTIVE}, i}
                    {#if T_ORDER_ISACTIVE == "1"}
                        <tr class="mb-4">
                            <th scope="row">{i+1}</th>
                            <td>{MD_DRINNK_NAME}</td>
                            <td>{MD_DRINK_PRICE+"€"}</td>
                            <td><button class="btn btn-sm btn-outline-success" on:click={()=>finishOrder(T_ORDER_ID)}><i class="bi bi-check-lg"></i></button></td>
                        </tr>
                    {/if}
                    {/each}
                </tbody>
            </table>

        </div>

        <div class="row gx-4 gx-lg-5 row-cols-2 row-cols-md-3 row-cols-xl-4 justify-content-center">

            <p class="lead fw-normal text-black-50">All orders:</p>

            <table class="table table-dark table-striped">
                <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Drink</th>
                    <th scope="col">Price</th>
                    <th scope="col">Done</th>
                </tr>
                </thead>
                <tbody>
                    {#each data as {MD_DRINNK_NAME, MD_DRINK_PRICE, T_ORDER_ISACTIVE}, i}
                    <tr>
                        <th scope="row">{i+1}</th>
                        <td>{MD_DRINNK_NAME}</td>
                        <td>{MD_DRINK_PRICE+"€"}</td>
                        {#if T_ORDER_ISACTIVE == "1"}
                            <td><i class="bi bi-three-dots"></i></td>
                        {:else}
                            <td><i class="bi bi-check-all"></i></td>
                        {/if}

                    </tr>
                {/each}
                </tbody>
            </table>


        </div>

    </div>
</section>

<Footer/>