<script>
    const menuList = [
        {
            label: "จัดการตึก",
            bg: "#FF922D",
            to: "/manage/another/rooms",
            icon: `
            <svg xmlns="http://www.w3.org/2000/svg" width="200" height="200"
                viewBox="0 0 24 24" fill="none" stroke="currentColor"
                stroke-width="2" stroke-linecap="round" stroke-linejoin="round"
                class="lucide lucide-building-2">
                <path d="M10 12h4" />
                <path d="M10 8h4" />
                <path d="M14 21v-3a2 2 0 0 0-4 0v3" />
                <path d="M6 10H4a2 2 0 0 0-2 2v7a2 2 0 0 0 2 2h16a2 2 0 0 0 2-2V9a2 2 0 0 0-2-2h-2" />
                <path d="M6 21V5a2 2 0 0 1 2-2h8a2 2 0 0 1 2 2v16" />
            </svg>`,
        },
        {
            label: "จัดการห้อง",
            bg: "#FF922D",
            to: "/manage/another/rooms",
            icon: `
           <svg xmlns="http://www.w3.org/2000/svg" width="200" height="200" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-door-open-icon lucide-door-open"><path d="M11 20H2"/><path d="M11 4.562v16.157a1 1 0 0 0 1.242.97L19 20V5.562a2 2 0 0 0-1.515-1.94l-4-1A2 2 0 0 0 11 4.561z"/><path d="M11 4H8a2 2 0 0 0-2 2v14"/><path d="M14 12h.01"/><path d="M22 20h-3"/></svg>`,
        },
        {
            label: "จัดการห้อง",
            bg: "#FF922D",
            to: "/manage/another/rooms",
            icon: `
          <svg xmlns="http://www.w3.org/2000/svg" width="200" height="200" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-user-icon lucide-user"><path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"/><circle cx="12" cy="7" r="4"/></svg>`,
        },

        // เพิ่มเมนูอื่นๆ ได้ เช่น
    ];
</script>

<h1 class="text-3xl mb-24">จัดการอื่นๆ</h1>
<div class="flex w-3/4 mx-auto h-[350px] items-center justify-between gap-12 p-2">
    {#each menuList as item}
        <a href={item.to}
            class={`transition bg-[${item.bg}] hover:bg-orange-500 rounded-2xl min-w-[400px] w-full h-full text-white p-3 flex flex-col items-center justify-center`}
        
        >
            {@html item.icon}
            <p class="text-xl">{item.label}</p>
        </a>
    {/each}
</div>
