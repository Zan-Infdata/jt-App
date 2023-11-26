<script lang="ts">
    import { onMount } from 'svelte';
    import Navbar from './Navbar.svelte'
    import Footer from './Footer.svelte'

    let data: any[] = [];

    let filter: string = "";
    let onlyFinished: boolean = false;


onMount(async () => {
    getOrders();
});

async function getOrders() {
    const url = `${import.meta.env.VITE_API_ROOT}/order/list/detail`;

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

}




</script>
<!-- Navigation-->
<Navbar active={1}/>
<!-- Header-->
<header class="bg-dark py-5">
    <div class="container px-4 px-lg-5 my-5">
        <div class="text-center text-white">
            <h1 class="display-4 fw-bolder">All of the orders</h1>
            <p class="lead fw-normal text-white-50 mb-0">Here you can see what all the orders are</p>

        </div>
    </div>
</header>
<!-- Section-->
<section class="py-5">
    <div class="container px-4 px-lg-5 mt-5">

        <div class="row gx-4 mb-4 ">
            <div class="col-md-4 mb">
                <div class="form-outline form-white">
                    <input bind:value={filter} type="text" placeholder="Filter by drink and table..." class="form-control"  />
                </div>
            </div>

            <div class="col-md-4 mb">
                <div class="form-outline form-white">
                    <input bind:checked={onlyFinished} id="chk" type="checkbox" class="form-check-input"  />
                    <label class="form-check-label" for="chk">
                        Only finished
                    </label>
                </div>
            </div>
        </div>

        <div class="row gx-4 gx-lg-5 row-cols-2 row-cols-md-3 row-cols-xl-4 ">

            

            <table class="table table-dark table-striped">
                <thead>
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Drink</th>
                    <th scope="col">Price</th>
                    <th scope="col">Table</th>
                    <th scope="col">Done</th>
                </tr>
                </thead>
                <tbody>
                {#each data as {T_ORDER_ID, MD_TABLE_CODE, MD_TABLE_NUMBER, MD_DRINNK_NAME, MD_DRINK_PRICE, MD_TABLE_ID, T_ORDER_ISACTIVE}, i}
                {#if MD_DRINNK_NAME.toLowerCase().includes(filter.toLowerCase()) || MD_TABLE_CODE.toLowerCase().includes(filter.toLowerCase())}
                
                {#if !onlyFinished || T_ORDER_ISACTIVE == "0"}
                <tr>
                    <th scope="row">{T_ORDER_ID}</th>
                    <td>{MD_DRINNK_NAME}</td>
                    <td>{MD_DRINK_PRICE+"€"}</td>
                    <td><a href="#/table/orders/{MD_TABLE_ID}">{"Miza "+ MD_TABLE_NUMBER +" :: [" + MD_TABLE_CODE +"]"}</a></td>
                    {#if T_ORDER_ISACTIVE == "1"}
                        <td><i class="bi bi-three-dots"></i></td>
                    {:else}
                        <td><i class="bi bi-check-all"></i></td>
                    {/if}

                </tr>
                {/if}
                {/if}
                {/each}
                </tbody>
            </table>


        </div>

    </div>
</section>

<Footer/>