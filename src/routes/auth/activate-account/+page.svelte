<script>
    import axios from "axios";
    import { goto } from "$app/navigation";
    import { URL_PREFIX, ACTIVATE_EMAIL,SUCCESS_RESULT } from "$lib/constants.js";
    import {fetchApiData} from "$lib/functions"

    let activate_email = "";
    let activate_code = "";
    let isLoading = false;
    let isSuccess = false;
    let errorMsg = "";

    const getActivateCode = async (activate_email) => {
        isLoading = true;
        const result = await fetchApiData(`public/send-activate-mail?email=${activate_email}`,null,"GET")
                if (result.status === SUCCESS_RESULT) {
                    isLoading = false;
                    isSuccess = false;
                    errorMsg = "";
                } else {
                    errorMsg = result.msg;
                    isLoading = false;
                }
    };

    const onLoadActivate = async () => {
        if (typeof localStorage !== "undefined") {
            activate_email = localStorage.getItem(ACTIVATE_EMAIL);
            if (!activate_email) {
                goto("/auth/login");
            }
        }
    };
    const submitCode = async () => {
        errorMsg = "";
        isSuccess = false;
        isLoading = true;
        if (activate_code.length === 10) {
            let data = JSON.stringify({
                email: activate_email,
                code: activate_code,
            });

            const result = await fetchApiData(
                `public/activate-account`,
                null,
                "POST",
                data
            );
            if (result.status === SUCCESS_RESULT) {
                localStorage.removeItem(ACTIVATE_EMAIL);
                isSuccess = true;
            } else {
                errorMsg = result.data.msg;
                isLoading = false;
            }
        }
        isLoading = false;
    };
    onLoadActivate();
</script>

<svelte:head>
    <title>Authentication - Activate account</title>
</svelte:head>

<div class="w-full h-screen flex flex-col p-4 items-center justify-center">
    <div class="w-fit max-w-sm mx-auto h-fit p-8 bg-white rounded-md moveLeft">
        <h3 class="font-bold text-center text-xl">Activate your account</h3>
        <p
            class={`${
                isSuccess ? "block" : "hidden"
            } my-5 transition-all text-green-500 text-center`}
        >
            Your account is activated!
        </p>

        <div
            class={`${
                isSuccess || errorMsg ? "hidden" : "block"
            }  transition-all`}
        >
            <p class="my-2 text-sm white text-center italic">
                Please check your email to get the code
            </p>
            <input
                name="activate_code"
                id="activate_code"
                type="text"
                on:change={(e) => (activate_code = e.target.value)}
                maxlength={10}
                placeholder="X XXX XXX XXX"
                class="bg-slate-100 text-center text-xl px-4 py-2 w-full rounded-md border"
            />
            <button
                on:click={submitCode}
                class={`w-full py-2 ${
                    activate_code.length > 0 ? "shadow-md" : ""
                } rounded-md bg-white text-gray-500 my-3 border`}
            >
                {!isLoading ? "Send" : "Loading..."}
            </button>
        </div>

        <span
            class="w-full my-5 text-center block font-bold text-red-500 text-sm"
            >{errorMsg}</span
        >
        <button
            on:click={getActivateCode(activate_email)}
            class={`${errorMsg ? "flex" : "hidden"} w-full py-2 ${
                activate_code.length > 0 ? "shadow-md" : ""
            } rounded-md bg-white text-gray-500 my-3 border justify-center`}
        >
            {!isLoading ? "Re-send email" : "Loading..."}
        </button>
        <a
            href="/auth/login"
            class="text-center flex gap-1 items-center mx-auto w-full justify-center"
        >
            <svg
                class="w-5 h-5"
                viewBox="0 0 512 512"
                xmlns="http://www.w3.org/2000/svg"
                ><title /><polyline
                    points="112 352 48 288 112 224"
                    style="fill:none;stroke:#000;stroke-linecap:square;stroke-miterlimit:10;stroke-width:32px"
                /><polyline
                    points="64 288 464 288 464 160"
                    style="fill:none;stroke:#000;stroke-linecap:square;stroke-miterlimit:10;stroke-width:32px"
                /></svg
            >
            Back to login
        </a>
    </div>
</div>
