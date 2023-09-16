<script>
    let isRememberMe = false;
    const toggleSwitch = () => {
        isRememberMe = !isRememberMe;
    };

</script>

<div class="w-full h-screen flex flex-col items-center justify-center">
    <div
        class="flex w-full lg:max-w-2xl rounded-lg lg:shadow-lg overflow-hidden m-auto self-center bg-white bg-opacity-70"
    >
        <div class="w-1/2 p-8 bg-form" />

        <!-- Phần form đăng nhập bên phải -->
        <div class="w-1/2 p-8">
            <div class="text-center">
                <img
                    src="your-logo.png"
                    alt="Logo"
                    class="w-16 h-16 mx-auto mb-4"
                />
                <h1 class="text-2xl font-semibold mb-2">Đăng nhập</h1>
            </div>

            <div>
                <div class="mb-4">
                    <label
                        for="username"
                        class="block text-gray-600 text-sm font-medium mb-1"
                        >Tên người dùng</label
                    >
                    <input
                        type="text"
                        id="username"
                        name="username"
                        class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:border-blue-500"
                        placeholder="johnsmith2023"
                        required
                    />
                </div>

                <div class="mb-4">
                    <label
                        for="password"
                        class="block text-gray-600 text-sm font-medium mb-1"
                        >Mật khẩu</label
                    >
                    <input
                        type="password"
                        id="password"
                        name="password"
                        class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:border-blue-500"
                        placeholder="***"
                        required
                    />
                </div>

                <div
                    class="mb-4 flex gap-2 items-center w-fit cursor-pointer justify-between"
                >
                    <button
                        id="remember"
                        on:click={toggleSwitch}
                        class={`${
                            isRememberMe ? "switch_active" : ""
                        } w-12 h-6 remember-switch-bg border rounded-full justify-center flex items-center`}
                    >
                        <span
                            class={`button-switch h-5 w-5 rounded-full ${
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

                <div class="mb-4">
                    <button
                        type="submit"
                        class="w-full bg-blue-500 text-white font-semibold px-4 py-2 rounded-lg hover:bg-blue-600"
                        >Đăng nhập</button
                    >
                </div>

                <div class="text-center">
                    <p class="text-gray-600 text-sm">
                        Chưa có tài khoản? <a
                            href="/auth/register"
                            class="text-blue-500 hover:underline">Đăng ký</a
                        >
                    </p>
                </div>
            </div>
        </div>
    </div>
</div>

<style>
    .remember-switch-bg {
        background-color: rgb(200, 200, 200);
        transition: background 0.5s ease-in-out;
    }

    .switch_active {
        background-color: rgb(0, 128, 58);
    }
    .button-switch {
        background-color: yellow;
        display: inline-block;
        transition: all 0.3s ease-in-out;
    }

    .switch-on {
        transform: translateX(12px);
        background-color: white;
    }
    .switch-off {
        transform: translateX(-12px);
    }
</style>
