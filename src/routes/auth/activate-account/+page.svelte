<script>
let activate_email = "";
let activate_code = "";
let loading = false;
let errorMsg = "";

let ACTIVE_EMAIL = "activate_email";
if (typeof localStorage !== "undefined") {
    activate_email = localStorage.getItem(ACTIVE_EMAIL);
    if(activate_email){

    }else{
        window.location.href = "/auth/login"
    }
}
const submitCode = () => {
    if(activate_code.length === 10){
        console.log(activate_code)
        console.log(loading)
    }
}
</script>

<svelte:head>
    <title>Authentication - Activate account</title> 
</svelte:head>

<div class="w-full h-screen flex flex-col p-4 items-center justify-center">
    <div
        class="w-fit max-w-sm mx-auto h-fit p-8 bg-white rounded-md"
        >
        <h3 class="font-bold text-center text-xl">Activate your account</h3>
        <p class="my-2 text-sm white text-center italic">Please check your email to get the code</p>
        <input 
        name="activate_code"
        id="activate_code"
        type="text"
        on:change={e => activate_code = e.target.value}
        maxlength={10}
        placeholder="X XXX XXX XXX"
        class="bg-slate-100 text-center text-xl px-4 py-2 w-full rounded-md border"/>
        <button 
        on:click={submitCode}
        disabled={loading}
        class={`w-full py-2 ${activate_code.length > 0 ? "shadow-md" : ""} rounded-md bg-white text-gray-500 my-3 border`}>
        {loading ? "Send" : "Loading..."}
        </button>
        <span class="w-full my-2 text-red-500 text-sm">{errorMsg}</span>
    </div>

</div>
