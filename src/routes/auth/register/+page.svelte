<script>
    import axios from "axios";
    import { goto } from "$app/navigation";
    import {URL_PREFIX, ACTIVATE_EMAIL, IS_REMEMBER, ACCESS_TOKEN, REFRESH_TOKEN, SUCCESS_RESULT} from "$lib/constants.js"
    import {fetchApiData} from "$lib/functions.js"

    let l_nameRef;
    let emailRef;
    let unameRef;
    let passwordRef;
    let submitRef;

    let errorMsgFullname = "";
    let errorMsgEmail = "";
    let errorMsgUsername = "";
    let errorMsgPassword = "";
    let errorMsgCommon = "";

    let f_name = "";
    let l_name = "";
    let email = "";
    let username = "";
    let password = "";
    let loading = false;
    let isError = false;

    function isValidEmail(email) {
        const emailRegex = /^[a-zA-Z0-9._-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,4}$/;
        return emailRegex.test(email);
    }
    const sendActivateCode = async (activate_email) => {
        let config = {
            method: "GET",
            url: `${URL_PREFIX}public/send-activate-mail?email=${activate_email}`,
            headers: { "Content-Type": "application/json" },
        };
        await axios.request(config);
        loading = false;
        goto("/auth/activate-account");
    };

    const checkInputRegister = async () => {
        loading = true;
        errorMsgCommon = "";
        if (f_name.length < 3 || l_name.length < 3) {
            errorMsgFullname = "Name must be more than 3 characters";
            isError = true;
        }

        if (email.length < 5 && !isValidEmail(email)) {
            errorMsgEmail = "Email format is not correct";
            isError = true;
        }

        if (username.length < 5) {
            errorMsgUsername = "Username must be more than 5 characters";
            isError = true;
        }
        if (password.length < 5) {
            errorMsgPassword = "Password must be more than 5 characters";
            isError = true;
        }
        if (isError) {
            errorMsgCommon = "Something went wrong !!";
        } else {
            let user = JSON.stringify({
                f_name,
                l_name,
                email,
                username,
                password,
            });

            
            const result = await fetchApiData("public/sign-up",null,"POST",user)
            if(result.status === SUCCESS_RESULT){
                await sendActivateCode(email);
                localStorage.setItem(ACTIVATE_EMAIL, email)
                goto("/auth/activate-account")
            }else{
                errorMsgCommon = result.data.msg
            }
        }
        loading = false;
    };

    const handleNextInput = (e, ref) => {
        if (e.key === "Enter") {
            ref.focus();
        }
    };
</script>

<svelte:head>
    <title>Authentication - Register</title>
</svelte:head>
<div
    class="w-full h-screen flex flex-col p-0 lg:p-2 items-center justify-center"
>
    <div
        class="flex w-full lg:max-w-2xl rounded-lg lg:shadow-lg overflow-hidden m-auto self-center moveLeft"
    >
        <div class="w-1/2 p-2 bg-form hidden lg:flex">
            <div
                class="text-center m-auto rounded-md border bg-black bg-opacity-50 p-4"
            >
                <p class="text-white text-sm">
                    Đã có tài khoản? <a
                        href="/auth/login"
                        class="text-white lg:hover:underline font-bold"
                        >Đăng nhập ngay</a
                    >
                </p>
            </div>
        </div>

        <!-- Phần form đăng nhập bên phải -->
        <div
            class="w-96 mx-auto h-full lg:h-fit lg:w-1/2 p-8 bg-white rounded-md lg:rounded-none bg-opacity-70 lg:bg-opacity-50"
        >
            <div class="text-center mb-4">
                <h1 class="text-2xl font-semibold mb-2">Đăng ký</h1>
            </div>

            <div>
                <div class="flex flex-col">
                    <div class="flex gap-2">
                        <div class="flex flex-col">
                            <label
                                for="fName"
                                class="block text-gray-600 text-sm font-medium mb-1"
                                >Họ</label
                            >
                            <input
                                type="text"
                                id="fName"
                                name="fName"
                                on:keypress={(e) =>
                                    handleNextInput(e, l_nameRef)}
                                value={f_name}
                                on:change={(e) => (f_name = e.target.value)}
                                class={`w-full px-4 py-2 border rounded-lg focus:outline-none ${
                                    errorMsgFullname ? "border-red-500" : ""
                                } focus:border-blue-500`}
                                placeholder="John"
                            />
                        </div>
                        <div class="flex flex-col">
                            <label
                                for="lName"
                                class="block text-gray-600 text-sm font-medium mb-1"
                                >Tên</label
                            >
                            <input
                                type="text"
                                id="lName"
                                name="lName"
                                bind:this={l_nameRef}
                                on:keypress={(e) =>
                                    handleNextInput(e, emailRef)}
                                value={l_name}
                                on:change={(e) => (l_name = e.target.value)}
                                class={`w-full px-4 py-2 border rounded-lg focus:outline-none ${
                                    errorMsgFullname ? "border-red-500" : ""
                                } focus:border-blue-500`}
                                placeholder="Smith"
                            />
                        </div>
                    </div>
                    <span class="h-5 text-red-600 text-sm errorMsg"
                        >{errorMsgFullname}</span
                    >
                </div>
                <div class="mb-4 flex flex-col">
                    <label
                        for="email"
                        class="block text-gray-600 text-sm font-medium mb-1"
                        >Email</label
                    >
                    <input
                        type="email"
                        id="email"
                        name="email"
                        bind:this={emailRef}
                        on:keypress={(e) => handleNextInput(e, unameRef)}
                        value={email}
                        on:change={(e) => (email = e.target.value)}
                        class={`w-full px-4 py-2 border rounded-lg focus:outline-none ${
                            errorMsgEmail ? "border-red-500" : ""
                        } focus:border-blue-500`}
                        placeholder="johnsmith2023@gmail.com"
                        required
                    />
                    <span class="h-5 text-red-600 text-sm errorMsg"
                        >{errorMsgEmail}</span
                    >
                </div>

                <div class="mb-4 flex flex-col">
                    <label
                        for="username"
                        class="block text-gray-600 text-sm font-medium mb-1"
                        >Tên người dùng</label
                    >
                    <input
                        type="text"
                        id="username"
                        name="username"
                        bind:this={unameRef}
                        on:keypress={(e) => handleNextInput(e, passwordRef)}
                        value={username}
                        on:change={(e) => (username = e.target.value)}
                        class={`w-full px-4 py-2 border rounded-lg focus:outline-none ${
                            errorMsgUsername ? "border-red-500" : ""
                        } focus:border-blue-500`}
                        placeholder="johnsmith2023"
                        required
                    />
                    <span class="h-5 text-red-600 text-sm errorMsg"
                        >{errorMsgUsername}</span
                    >
                </div>

                <div class="mb-4 flex flex-col">
                    <label
                        for="password"
                        class="block text-gray-600 text-sm font-medium mb-1"
                        >Mật khẩu</label
                    >
                    <input
                        type="password"
                        id="password"
                        name="password"
                        on:keypress={(e) => handleNextInput(e, submitRef)}
                        bind:this={passwordRef}
                        on:change={(e) => (password = e.target.value)}
                        class={`w-full px-4 py-2 border rounded-lg  ${
                            errorMsgPassword ? "border-red-500" : ""
                        } focus:outline-none focus:border-blue-500`}
                        placeholder="***"
                        required
                    />
                    <span class="h-5 text-red-600 text-sm errorMsg"
                        >{errorMsgPassword}</span
                    >
                </div>

                <div class="mb-2 flex flex-col items-center justify-center">
                    <button
                        on:click={checkInputRegister}
                        bind:this={submitRef}
                        type="submit"
                        class={`${
                            errorMsgCommon ? "shake-button" : ""
                        } w-full bg-white text-slate-600 font-semibold px-4 py-2 rounded-lg lg:hover:text-black`}
                    >
                        <span>{loading ? "Loading..." : "Go"}</span>
                    </button>
                    <span
                        class="h-3 mt-3 font-bold text-red-600 text-sm errorMsg"
                        >{errorMsgCommon}</span
                    >
                </div>
                <div class="text-center my-3 block lg:hidden">
                    <p class="text-gray-600 text-sm">
                        Đã có tài khoản? <a
                            href="/auth/login"
                            class="text-slate-800 lg:hover:underline font-bold"
                            >Đăng nhập ngay</a
                        >
                    </p>
                </div>
            </div>
        </div>
    </div>
</div>

<style>
</style>
