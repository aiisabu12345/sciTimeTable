<script>
    import logo from "../../../static/kmitl-logo.jpg";
    export let username = "Username";
</script>

<nav
    class="w-full px-12 bg-[#FF7A00] text-white py-3 flex items-center justify-between"
>
    <div class="flex  space-x-2 items-center h-full">
        <img
            class="h-auto max-w-[55px]"
            src="https://img5.pic.in.th/file/secure-sv1/kmitl-seal.webp"
            alt=""
        />
        <h1 class="text-2xl font-semibold">Sci Time Table</h1>
    </div>

    <div class="flex items-center gap-3 text-2xl">
        <div
            class="w-8 h-8 rounded-full bg-white/30 flex items-center justify-center"
        >
            <svg
                xmlns="http://www.w3.org/2000/svg"
                width="24"
                height="24"
                viewBox="0 0 24 24"
                fill="none"
                stroke="currentColor"
                stroke-width="2"
                stroke-linecap="round"
                stroke-linejoin="round"
                class="lucide lucide-user-icon lucide-user"
                ><path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2" /><circle
                    cx="12"
                    cy="7"
                    r="4"
                /></svg
            >
        </div>
        <span>{username}</span>
    </div>
</nav>
