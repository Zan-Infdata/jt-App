<script lang="ts">
    import Navbar from './Navbar.svelte'
    import Footer from './Footer.svelte'


    let code:string = "";
    let number:number = 0;
    let alert:boolean = false;
    let isSuccess:boolean = false;
    let alertTxt:string = "";

async function addTable() {

    if(code.trim() == "" || number < 0){
        showAlert(false, "Bad inputs");
    }

    const url = `${import.meta.env.VITE_API_ROOT}/table/add`;

    let body = {
        code: code,
        number: number
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
        showAlert(true, "Table added successfully!");
        code = "";
        number = 0;
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
                  <label for="name" class="form-label">Code</label>
                  <input bind:value={code} type="text" placeholder="tbl_XX" class="form-control" id="name" aria-describedby="emailHelp">
                </div>
                <div class="mb-3">
                    <label for="price" class="form-label">Table number:</label>
                    <input bind:value={number} step=1 type="number" class="form-control" id="price">
                </div>

                <button type="button" class="btn btn-primary" on:click={addTable}>Add</button>
              </form>


        </div>


    </div>
</section>

<Footer/>