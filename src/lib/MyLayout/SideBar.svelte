<script lang="ts">
  import { page } from "$app/stores";

  let openExcelMenu = false; // toggle menu

  const menu = [
    {
      name: "หน้าแรก",
      path: "/",
      icon: `<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-house-icon lucide-house"><path d="M15 21v-8a1 1 0 0 0-1-1h-4a1 1 0 0 0-1 1v8"/><path d="M3 10a2 2 0 0 1 .709-1.528l7-6a2 2 0 0 1 2.582 0l7 6A2 2 0 0 1 21 10v9a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2z"/></svg>`,
    },
    {
      name: "ค้นหารายวิชา",
      path: "/search",
      icon: `<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-search-icon lucide-search"><path d="m21 21-4.34-4.34"/><circle cx="11" cy="11" r="8"/></svg>`,
    },
    {
      name: "จัดการข้อมูล",
      path: "/manage",
      icon: `<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-folder-pen-icon lucide-folder-pen"><path d="M2 11.5V5a2 2 0 0 1 2-2h3.9c.7 0 1.3.3 1.7.9l.8 1.2c.4.6 1 .9 1.7.9H20a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2h-9.5"/><path d="M11.378 13.626a1 1 0 1 0-3.004-3.004l-5.01 5.012a2 2 0 0 0-.506.854l-.837 2.87a.5.5 0 0 0 .62.62l2.87-.837a2 2 0 0 0 .854-.506z"/></svg>`,
      children: [
        { name: "จัดการภาควิชา", path: "/manage/Department" },
        { name: "จัดการหลักสูตร", path: "/manage/course" },
        { name: "จัดการรายวิชา", path: "/manage/subject" },
      ],
    },
    {
      name: "ประวัติการบันทึก",
      path: "/history",
      icon: `<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-history-icon lucide-history"><path d="M3 12a9 9 0 1 0 9-9 9.75 9.75 0 0 0-6.74 2.74L3 8"/><path d="M3 3v5h5"/><path d="M12 7v5l4 2"/></svg>`,
    },
  ];
</script>

<aside
  class="min-w-60 bg-white border-r border-gray-100 shadow-lg h-screen p-4 flex flex-col gap-2"
>
  {#each menu as item}
    {#if item.children}
      <div>
        <!-- ปุ่มเปิด/ปิดเมนูย่อย -->
        <button
          on:click={() => (openExcelMenu = !openExcelMenu)}
          class={`w-full flex items-center justify-between p-3 rounded-lg transition
            ${
              $page.url.pathname.startsWith(item.path)
                ? "bg-[#FF7A00] text-white"
                : "hover:bg-orange-100 text-black"
            }`}
        >
          <div class="flex items-center gap-2">
            {@html item.icon}
            <span>{item.name}</span>
          </div>

          <svg
            class={`transition-transform ${openExcelMenu ? "rotate-90" : ""}`}
            xmlns="http://www.w3.org/2000/svg"
            width="20"
            height="20"
            fill="none"
            stroke="currentColor"
            stroke-width="2"
            stroke-linecap="round"
            stroke-linejoin="round"
          >
            <path d="M6 9l6 6 6-6" />
          </svg>
        </button>

        {#if openExcelMenu}
          <div class="ml-10 mt-1 flex flex-col gap-1">
            {#each item.children as sub}
              <a
                href={sub.path}
                on:click={() => (openExcelMenu = true)}
                class={`p-2 rounded transition
                  ${
                    $page.url.pathname === sub.path
                      ? "text-[#FF7A00] font-semibold"
                      : "text-gray-700 hover:text-[#FF7A00]"
                  }`}
              >
                {sub.name}
              </a>
            {/each}
          </div>
        {/if}
      </div>
    {:else}
      <a
        href={item.path}
        on:click={() => (openExcelMenu = false)}
        class={`flex items-center gap-2 p-3 rounded-lg transition
          ${
            $page.url.pathname === item.path
              ? "bg-[#FF7A00] text-white"
              : "hover:bg-orange-100 text-black"
          }`}
      >
        {@html item.icon}
        <span>{item.name}</span>
      </a>
    {/if}
  {/each}

  <button
    class="gap-2 p-3 rounded-lg border-t border-gray-200 pt-4 flex items-center hover:text-white hover:bg-red-500 cursor-pointer transition-all space-x-2 mt-4"
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
      class="lucide lucide-log-out-icon lucide-log-out"
      ><path d="m16 17 5-5-5-5" /><path d="M21 12H9" /><path
        d="M9 21H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h4"
      /></svg
    >
    <p>ออกจากระบบ</p>
  </button>
</aside>
