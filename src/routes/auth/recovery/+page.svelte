<script>
    import axios from "axios";
    import { goto } from "$app/navigation";
    import {fetchApiData} from "$lib/functions"
    import {
        URL_PREFIX,
        ACTIVATE_EMAIL,
        ACTIVATE_CODE,
    } from "$lib/constants.js";

    let recovery_email = "";
    let recovery_code = "";
    let isLoading = false;
    let isSuccess = false;
    let isSend = false;
    let errorMsg = "";
    let password = "";

    function isValidEmail(email) {
        const emailRegex = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$/;
        return emailRegex.test(email);
    }
    const getRecoveryCode = async () => {
        isLoading = true;
        if (isValidEmail(recovery_email)) {
            const result = await fetchApiData(
                `public/recovery/${recovery_email}`,
                null,
                "GET"
            );
            console.log(result)
            if (result.status === SUCCESS_RESULT) {
                isLoading = false;
                isSuccess = false;
                isSend = true;
                errorMsg = "";
            } else {
                errorMsg = result.msg;
                isLoading = false;
                isSend = false;
            }
        }
    };

    const checkTheCode = async (recovery_code) => {
        let config = {
            method: "GET",
            maxBodyLength: Infinity,
            url: `${URL_PREFIX}public/check-code/${recovery_code}`,
            headers: { "Content-Type": "application/json" },
        };
        const result = await axios.request(config);
        return result.data;
    };
    const submitCode = async () => {
        errorMsg = "";
        isSuccess = false;
        isLoading = true;
        if (recovery_code.length === 10) {
            if (checkTheCode(recovery_code)) {
                let data = JSON.stringify({
                    email: recovery_email,
                    password,
                });

                const result = await fetchApiData(
                    `public/recovery-password/${recovery_code}`,
                    null,
                    "GET"
                );
                if (result.status === SUCCESS_RESULT) {
                    isSuccess = true;
                } else {
                    errorMsg = error.response.data.msg;
                    isLoading = false;
                }
            }
        }
        isLoading = false;
    };
</script>

<svelte:head>
    <title>Authentication - Recovery password</title>
</svelte:head>

<div class="w-full h-screen flex flex-col p-4 items-center justify-center">
    <div class="w-fit max-w-sm mx-auto h-fit p-8 bg-white rounded-md moveLeft">
        <h3 class="font-bold text-center text-xl">Recovery your password</h3>
        <p
            class={`${
                isSuccess ? "block" : "hidden"
            } my-5 transition-all text-green-500 text-center`}
        >
            Your password is changed.
        </p>

        <div class={`${isSend ? "hidden" : "block"} transition-all`}>
            <p class="my-2 text-sm white text-center italic">
                Enter your email to get the code
            </p>
            <input
                name="recovery_email"
                id="recovery_email"
                type="email"
                on:change={(e) => (recovery_email = e.target.value)}
                placeholder="johnsmith2023@gmail.com"
                class="bg-slate-100 text-center text-lg px-4 py-2 w-full rounded-md border"
            />
            <button
                on:click={getRecoveryCode}
                class={`w-full py-2 ${
                    recovery_code.length > 0 ? "shadow-md" : ""
                } rounded-md bg-white text-gray-500 my-3 border`}
            >
                {!isLoading ? "Send" : "Loading..."}
            </button>
        </div>

        <div class={`${isSend ? "block" : "hidden"}  transition-all`}>
            <p class="my-2 text-sm white text-center italic">
                Enter your recovery code here:
            </p>
            <input
                name="recovery_code"
                id="recovery_code"
                type="text"
                on:change={(e) => (recovery_code = e.target.value)}
                maxlength={10}
                placeholder="X XXX XXX XXX"
                class="bg-slate-100 text-center text-xl px-4 py-2 w-full rounded-md border"
            />
            <button
                on:click={submitCode}
                class={`w-full py-2 ${
                    recovery_code.length > 0 ? "shadow-md" : ""
                } rounded-md bg-white text-gray-500 my-3 border`}
            >
                {!isLoading ? "Send" : "Loading..."}
            </button>
        </div>

        <span
            class="w-full my-3 text-center block font-bold text-red-500 text-sm"
            >{errorMsg}</span
        >

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
