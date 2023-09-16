<script>
    import axios from "axios";
    const USERNAME_REMEMBER = "username_remember";
    const IS_REMEMBER = "isRemember";
    const ACCESS_TOKEN = "actk";
    const REFRESH_TOKEN = "rftk";
    const URL_PREFIX = "http://192.168.1.19:8080/";
    let unameRef;
    let passwordRef;
    let submitRef;
    let localRememebr = false;
    let errorMsgUsername = "";
    let errorMsgPassword = "";
    let errorMsgCommon = "";
    let username = "";
    let password = "";
    let loading = false;
    let isError = false;

    if (typeof localStorage !== "undefined") {
        localRememebr = localStorage.getItem(IS_REMEMBER);
        if (localRememebr) {
            username = localStorage.getItem(USERNAME_REMEMBER);
        }
    }
    let isRememberMe = localRememebr;
    const toggleSwitch = () => {
        isRememberMe = !isRememberMe;
    };

    const checkInputLogin = async () => {
        loading = true;
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
            if (isRememberMe) {
                localStorage.setItem(IS_REMEMBER, isRememberMe);
                localStorage.setItem(USERNAME_REMEMBER, username);
            } else {
                localStorage.removeItem(IS_REMEMBER);
                localStorage.removeItem(USERNAME_REMEMBER);
            }
            errorMsgUsername = "";
            errorMsgPassword = "";
            errorMsgCommon = "";
            let user = JSON.stringify({ username, password });

            let config = {
                method: "post",
                maxBodyLength: Infinity,
                url: `${URL_PREFIX}public/sign-in`,
                headers: {
                    "Content-Type": "application/json",
                },
                data: user,
            };

            await axios
                .request(config)
                .then((response) => {
                    if (response.status === 200) {
                        const result = response.data.content;
                        localStorage.setItem(ACCESS_TOKEN, result.access_token);
                        localStorage.setItem(
                            REFRESH_TOKEN,
                            result.refresh_token
                        );
                    } else {
                        errorMsgCommon = "Username or Password is incorrect";
                    }
                })
                .catch((error) => {
                    errorMsgCommon = "Username or Password is incorrect";
                })
                .finally(() => {
                    loading = false;
                });
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
    <title>Authentication - Login</title>
</svelte:head>
<div class="w-full h-screen flex flex-col p-4 items-center justify-center">
    <div
        class="flex w-full lg:max-w-2xl rounded-lg lg:shadow-lg overflow-hidden m-auto self-center moveRight"
    >
        <div class="w-1/2 p-8 bg-form hidden lg:flex">
            <div
                class="text-center m-auto rounded-md border bg-black bg-opacity-50 p-4"
            >
                <p class="text-white text-sm">
                    Chưa có tài khoản? <a
                        href="/auth/register"
                        class="text-white lg:hover:underline font-bold"
                        >Đăng ký</a
                    >
                </p>
            </div>
        </div>

        <!-- Phần form đăng nhập bên phải -->
        <div
            class="w-96 mx-auto h-full lg:h-fit lg:w-1/2 p-8 bg-white rounded-md lg:rounded-none bg-opacity-70 lg:bg-opacity-50"
        >
            <div class="text-center mb-4">
                <h1 class="text-2xl font-semibold mb-2">Đăng nhập</h1>
            </div>

            <div>
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

                <div
                    class="mb-4 flex gap-2 items-center w-fit cursor-pointer justify-between"
                >
                    <button
                        id="remember"
                        on:click={toggleSwitch}
                        class={`${
                            isRememberMe ? "switch_active" : ""
                        } w-12 h-6 remember-switch-bg border-2 rounded-full justify-center flex items-center`}
                    >
                        <span
                            class={`button-switch h-2 w-2 rounded-full ${
                                isRememberMe ? "switch-on" : "switch-off"
                            }`}
                        />
                    </button>
                    <label
                        for="remember"
                        class="text-gray-600 text-sm font-medium cursor-pointer"
                        >Ghi nhớ tài khoản</label
                    >
                </div>

                <div class="mb-4 flex flex-col items-center justify-center">
                    <button
                        on:click={checkInputLogin}
                        bind:this={submitRef}
                        type="submit"
                        class={`${
                            errorMsgCommon ? "shake-button" : ""
                        } w-full bg-white text-slate-600 font-semibold px-4 py-2 rounded-lg lg:hover:text-black`}
                    >
                        <span>{loading ? "Loading..." : "Đăng nhập"}</span>
                    </button>
                    <span
                        class="h-3 mt-3 font-bold text-red-600 text-sm errorMsg"
                        >{errorMsgCommon}</span
                    >
                </div>
                <div class="text-center block lg:hidden mb-4">
                    <p class="text-gray-600 text-sm">
                        Chưa có tài khoản? <a
                            href="/auth/register"
                            class="text-slate-800 lg:lg:hover:underline font-bold"
                            >Đăng ký</a
                        >
                    </p>
                </div>
                <div class="w-full bg-slate-400" style="height: .5px;" />
                <div class="text-center mt-3">
                    <a
                        href="/auth/recovery"
                        class="text-black font-bold lg:hover:underline"
                        >Quên mật khẩu ?</a
                    >
                </div>
            </div>
        </div>
    </div>
</div>

<style>
    .remember-switch-bg {
        border-color: rgb(255, 255, 255);
        transition: border 0.5s ease-in-out;
    }

    .switch_active {
        border-color: rgb(0, 128, 58);
    }
    .button-switch {
        background-color: rgb(255, 255, 255);
        transition: all 0.3s ease-in-out;
    }

    .switch-on {
        transform: translateX(12px);
        background-color: rgb(0, 128, 58);
    }
    .switch-off {
        transform: translateX(-12px);
    }


</style>
