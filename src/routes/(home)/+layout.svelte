<script>
    import Header from "$lib/Header.svelte";
    import "$lib/app.css";
    import { checkToken, checkLogin } from "$lib/functions.js";
    import { goto } from "$app/navigation";
    import {
        SELECTED_SORT,
        SELECTED_TASK_SORT,
        ACCESS_TOKEN,
        URL_PREFIX,
        LOCAL_USER,
        REFRESH_TOKEN,
    } from "$lib/constants.js";

   
    checkLogin();
</script>
<Header />
<slot />
