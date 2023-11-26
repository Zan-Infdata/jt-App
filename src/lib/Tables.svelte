<script lang="ts">
    import { onMount } from 'svelte';
    import Navbar from './Navbar.svelte'
    import Footer from './Footer.svelte'

    let data: any[] = [];


onMount(async () => {
    
    console.log(import.meta.env.VITE_API_ROOT);

    const url = `${import.meta.env.VITE_API_ROOT}/table/list/detail`;

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


});


</script>
<!-- Navigation-->
<Navbar active={0}/>
<!-- Header-->
<header class="bg-dark py-5">
    <div class="container px-4 px-lg-5 my-5">
        <div class="text-center text-white">
            <h1 class="display-4 fw-bolder">Check all tables</h1>
            <p class="lead fw-normal text-white-50 mb-0">Here you can view the list of all the tables</p>
        </div>
    </div>
</header>
<!-- Section-->
<section class="py-5">
    <div class="container px-4 px-lg-5 mt-5">
        <div class="row gx-4 gx-lg-5 row-cols-2 row-cols-md-3 row-cols-xl-4 justify-content-center">


            {#each data as {MD_TABLE_ID, MD_TABLE_CODE, MD_TABLE_NUMBER, ORDR_NUM }, i }
                <div class="col mb-5">
                    <div class="card h-100">
                        <div class="badge bg-dark text-white position-absolute" style="top: 0.5rem; right: 0.5rem">Orders: {ORDR_NUM}</div>
                        <!--  Product details-->
                        <div class="card-body p-4">
                            <div class="text-center">
                                <!-- Product name-->
                                <h5 class="fw-bolder">Table {MD_TABLE_NUMBER}</h5>
                                <!-- Product price-->
                                {MD_TABLE_CODE}
                            </div>
                        </div>
                        <!-- Product actions-->
                        <div class="card-footer p-4 pt-0 border-top-0 bg-transparent">
                            <div class="text-center"><a class="btn btn-outline-dark mt-auto" href="#/table/orders/{MD_TABLE_ID}">View</a></div>
                        </div>
                    </div>
                </div>
            {/each}




        </div>
    </div>
</section>

<Footer/>