<script>
    import "$lib/app.css";
    import { checkLogin } from "$lib/functions.js";

    checkLogin();
</script>

<svelte:head>
    <title>Authentication</title>
</svelte:head>
<slot />
<div class="bg-auth absolute top-0 left-0" />
