<script lang="ts">
    import Navbar from './Navbar.svelte'
    import Footer from './Footer.svelte'


    let name:string = "";
    let desc:string = "";
    let price:number = 0.00; 
    let alert:boolean = false;
    let isSuccess:boolean = false;
    let alertTxt:string = "";

async function addDrink() {

    if(name.trim() == "" || desc.trim() == "" || price < 0){
        showAlert(false, "Bad inputs");
    }

    const url = `${import.meta.env.VITE_API_ROOT}/drink/add`;

    let body = {
        name: name,
        desc: desc,
        price: price
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
        showAlert(true, "Drink added successfully!");
        name = "";
        desc = "";
        price = 0.00;
    }
    else {
        showAlert(false, "Something went wrong!");
    }
}


function showAlert(success: boolean, txt: string){
    alertTxt = txt;
    isSuccess = success;
    alert= true;
}



</script>
<!-- Navigation-->
<Navbar active={2}/>

<!-- Section-->
<section class="py-5">
    <div class="container px-4 px-lg-5 mt-5">
    {#if alert}
        <div class="alert alert-dismissible fade show" class:alert-danger={!isSuccess} class:alert-success={isSuccess} role="alert">
            {alertTxt}
            <button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close"></button>
        </div>
    {/if}
        
        <div class="row gx-4 mb-4 ">
            <form>
                <div class="mb-3">
                  <label for="name" class="form-label">Drink Name</label>
                  <input bind:value={name} type="text" class="form-control" id="name" aria-describedby="emailHelp">
                </div>
                <div class="mb-3">
                  <label for="desc" class="form-label">Dedsc</label>
                  <textarea  bind:value={desc} class="form-control" id="desc"></textarea>
                </div>
                <div class="mb-3">
                    <label for="price" class="form-label">Price</label>
                    <input bind:value={price} type="number" class="form-control" id="price">
                </div>

                <button type="button" class="btn btn-primary" on:click={addDrink}>Add</button>
              </form>


        </div>


    </div>
</section>

<Footer/>