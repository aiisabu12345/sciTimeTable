<script>
  // Section 1: ภาพรวมข้อมูลในระบบ
  const systemOverview = [
    { label: "ภาควิชาทั้งหมด", value: "x", suffix: "ภาควิชา" },
    { label: "หลักสูตรทั้งหมด", value: "x", suffix: "หลักสูตร" },
    { label: "รายวิชาทั้งหมด", value: "x", suffix: "วิชา" }
  ];

  // Section 2: สถานะการจัดตารางเรียน
  const scheduleStatus = [
    { label: "ทั้งหมด", value: "x" },
    { label: "บันทึกสำเร็จแล้ว", value: "x" },
    { label: "ยังไม่บันทึก", value: "x" }
  ];

  // Section 3: เมนูลัด
  const menuItems = [
    {
      id: 1,
      label: "จัดตารางเรียน",
      iconPath: `<rect x="3" y="3" width="7" height="7"></rect>
                 <rect x="14" y="3" width="7" height="7"></rect>
                 <rect x="14" y="14" width="7" height="7"></rect>
                 <rect x="3" y="14" width="7" height="7"></rect>`
    },
    {
      id: 2,
      label: "จัดการรายวิชาปัจจุบัน",
      iconPath: ` <path d="M2 11.5V5a2 2 0 0 1 2-2h3.9c.7 0 1.3.3 1.7.9l.8 1.2c.4.6 1 .9 1.7.9H20a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2h-9.5"/><path d="M11.378 13.626a1 1 0 1 0-3.004-3.004l-5.01 5.012a2 2 0 0 0-.506.854l-.837 2.87a.5.5 0 0 0 .62.62l2.87-.837a2 2 0 0 0 .854-.506z"/>`
    },
    {
      id: 3,
      label: "จัดการหลักสูตร",
      iconPath: ` <path d="M2 11.5V5a2 2 0 0 1 2-2h3.9c.7 0 1.3.3 1.7.9l.8 1.2c.4.6 1 .9 1.7.9H20a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2h-9.5"/><path d="M11.378 13.626a1 1 0 1 0-3.004-3.004l-5.01 5.012a2 2 0 0 0-.506.854l-.837 2.87a.5.5 0 0 0 .62.62l2.87-.837a2 2 0 0 0 .854-.506z"/>`
    },
    {
      id: 4,
      label: "จัดการภาควิชา",
      iconPath: ` <path d="M2 11.5V5a2 2 0 0 1 2-2h3.9c.7 0 1.3.3 1.7.9l.8 1.2c.4.6 1 .9 1.7.9H20a2 2 0 0 1 2 2v10a2 2 0 0 1-2 2h-9.5"/><path d="M11.378 13.626a1 1 0 1 0-3.004-3.004l-5.01 5.012a2 2 0 0 0-.506.854l-.837 2.87a.5.5 0 0 0 .62.62l2.87-.837a2 2 0 0 0 .854-.506z"/>`
    }
  ];
</script>

<div class="min-h-screen  bg-white text-gray-900 ">
  <div class=" mx-auto space-y-10">

    <!-- Header Section -->
    <header class="flex flex-col md:flex-row md:items-end justify-between gap-4">
      <h1 class="text-3xl font-bold tracking-tight">ข้อมูลภาพรวม</h1>
      <div class="text-gray-700 text-lg">
        ปีการศึกษา : x | ภาคเรียนที่ : x
      </div>
    </header>

    <!-- Section 1 -->
    <section class="space-y-4">
      <h2 class="text-xl font-medium">ภาพรวมข้อมูลในระบบ :</h2>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        {#each systemOverview as item}
          <div class="border border-gray-200 rounded-lg p-6 flex items-center justify-center text-lg shadow-sm bg-white">
            {item.label} : {item.value} {item.suffix}
          </div>
        {/each}
      </div>
    </section>

    <!-- Section 2 -->
    <section class="space-y-4">
      <h2 class="text-xl font-medium">สถานะการจัดตารางเรียนปีการศึกษาปัจจุบัน :</h2>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
        {#each scheduleStatus as item}
          <div class="border border-gray-200 rounded-lg p-6 flex items-center justify-center text-lg shadow-sm bg-white">
            {item.label} : {item.value}
          </div>
        {/each}
      </div>
    </section>

    <!-- Section 3: เมนูลัด -->
    <section class="space-y-6 pt-4">
      <h2 class="text-3xl font-bold">เมนูลัด</h2>

      <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-8">
        {#each menuItems as menu}
          <button
            class="
              group flex flex-col items-center justify-center 
              h-64 rounded-2xl bg-white 
              shadow-[0_4px_20px_rgba(0,0,0,0.15)] 
              hover:shadow-[0_8px_25px_rgba(0,0,0,0.2)] 
              border border-gray-100
              transition-all duration-300 transform hover:-translate-y-1
            "
          >
            <div class="mb-6 text-black group-hover:text-orange-600 transition-colors">
              <svg 
                width="80" 
                height="80" 
                viewBox="0 0 24 24" 
                fill="none" 
                stroke="currentColor" 
                stroke-width="2.5" 
                stroke-linecap="round" 
                stroke-linejoin="round"
              >
                {@html menu.iconPath}
              </svg>
            </div>

            <span class="text-xl font-medium text-gray-900 group-hover:text-orange-600">
              {menu.label}
            </span>
          </button>
        {/each}
      </div>
    </section>

  </div>
</div>
