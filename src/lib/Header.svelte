<script>
    let menu_items = [
        { name: "Home", url: "/" },
        { name: "About", url: "/about" },
        { name: "My account", url: "/account" },
    ];

    let isOpenMenu = false;

    const toggleMenu = () => (isOpenMenu = !isOpenMenu);
</script>

<header class="w-full bg-white">
    <div
        class="container mx-auto flex justify-between items-center flex-col lg:flex-row relative py-2"
    >
        <div
            class="py-2 relative w-full lg:w-fit flex items-center justify-center"
        >
            <button
                on:click={toggleMenu}
                class="absolute left-2 top-2 flex lg:hidden"
            >
                <svg
                    class={`hamburger-menu w-8 h-8 ${
                        isOpenMenu ? "menu-open" : ""
                    }`}
                    style="enable-background:new 0 0 24 24;"
                    version="1.1"
                    viewBox="0 0 24 24"
                    xmlns="http://www.w3.org/2000/svg"
                >
                    <path
                        d="M5,7h14c0.6,0,1-0.4,1-1s-0.4-1-1-1H5C4.4,5,4,5.4,4,6S4.4,7,5,7z"
                    />
                    <path
                        d="M5,13h14c0.6,0,1-0.4,1-1s-0.4-1-1-1H5c-0.6,0-1,0.4-1,1S4.4,13,5,13z"
                    />
                    <path
                        d="M5,19h14c0.6,0,1-0.4,1-1s-0.4-1-1-1H5c-0.6,0-1,0.4-1,1S4.4,19,5,19z"
                    /></svg
                >
            </button>
            <a href="/" class="text-xl font-bold">Logo</a>
        </div>
        <div
            class={`m-menu ${
                isOpenMenu ? " menu-expanded" : ""
            } max-h-0 lg:max-h-fit overflow-hidden absolute lg:relative bg-white lg:bg-transparent top-14 lg:top-0 left-0 px-2 pt-2 lg:p-2 w-full lg:w-fit flex flex-col lg:flex-row lg:items-center gap-2`}
        >
            <div
                class="w-3/4 mx-auto lg:w-xl flex gap-2 border rounded-full px-2 border-black"
            >
                <input
                    type="search"
                    placeholder="Tìm kiếm gì đó ..."
                    class="w-full px-2 py-1 outline-none focus:outline-none bg-transparent"
                />
                <button>
                    <svg
                        class="w-5 h-5"
                        xmlns="http://www.w3.org/2000/svg"
                        viewBox="0 0 24 24"
                        id="search"
                    >
                        <g data-name="Layer 2">
                            <path
                                d="m20.71 19.29-3.4-3.39A7.92 7.92 0 0 0 19 11a8 8 0 1 0-8 8 7.92 7.92 0 0 0 4.9-1.69l3.39 3.4a1 1 0 0 0 1.42 0 1 1 0 0 0 0-1.42zM5 11a6 6 0 1 1 6 6 6 6 0 0 1-6-6z"
                                data-name="search"
                            /></g
                        >
                    </svg>
                </button>
            </div>

            <ul
                class="flex flex-col lg:flex-row px-2 bg-whitew-full lg:w-fit lg:relative"
            >
                {#each menu_items as item}
                    <li
                        class="font-bold px-2 w-full lg:w-fit h-fit py-4 border-b lg:border-b-0 whitespace-nowrap"
                    >
                        <a href={item.url}>{item.name}</a>
                    </li>
                {/each}
            </ul>
        </div>
    </div>
</header>

<style>
    .hamburger-menu path {
        transition: all 0.3s ease-in-out;
    }
    .hamburger-menu.menu-open path:nth-child(2) {
        opacity: 0;
        transition: opacity 0.3s ease-in-out;
    }
    .hamburger-menu.menu-open path:nth-child(1) {
        transform: rotate(45deg) translate(6px, -6px);
    }
    .hamburger-menu.menu-open path:nth-child(3) {
        transform: rotate(-45deg) translate(-12px, 0px);
    }
    @media (max-width: 1024px) {
        .m-menu {
            transition: max-height 0.4s ease-in-out;
        }
    }
    .m-menu.menu-expanded {
        max-height: 100vh;
    }
</style>
