<script>
    import Header from "$lib/Header.svelte";
    import "$lib/app.css";
    import { checkLogin } from "$lib/functions.js";

   
    checkLogin();
</script>
<Header />
<div class="container mx-auto">
    <slot />
</div>