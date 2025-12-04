<script lang="ts">
    import { PUBLIC_API_URL } from "$env/static/public";

    export let onClose = () => {};
    export let reLoad = () => {};

    // ฟิลด์ทั้งหมด
    let name_th = "";
    let name_en = "";
    let department_id = "";
    let num_years = "";

    let Depart: any[] = []; // เก็บข้อมูลภาควิชา

    // โหลดภาควิชา
    async function loadDepartments() {
        try {
            const res = await fetch(`${PUBLIC_API_URL}/api/departments`);
            const data = await res.json();
            Depart = data;
        } catch (err) {
            console.error("โหลดข้อมูลภาควิชาล้มเหลว:", err);
        }
    }

    loadDepartments();

    const resetForm = () => {
        name_th = "";
        name_en = "";
        department_id = "";
        num_years = "";
    };

    async function submit() {
        if (!name_th || !name_en || !department_id || !num_years) {
            alert("กรุณากรอกข้อมูลให้ครบทุกช่อง");
            return;
        }

        const payload = {
            name_th,
            name_en,
            department_id: Number(department_id),
            num_years: Number(num_years),
        };

        try {
            const res = await fetch(`${PUBLIC_API_URL}/api/programs`, {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(payload),
            });

            if (!res.ok) {
                const errText = await res.text();
                throw new Error(errText || "ส่งข้อมูลไม่สำเร็จ");
            }

            alert("เพิ่มหลักสูตรสำเร็จ!");
            resetForm();
            reLoad();
            onClose();
        } catch (err: any) {
            console.error(err);
            alert("เกิดข้อผิดพลาด: " + err.message);
        }
    }
</script>

<!-- Background คลิกปิด -->
<div
    class="fixed inset-0 bg-black/40 backdrop-blur-sm z-50"
    on:click={onClose}
>
<!-- Popup กล่อง -->
<div class="fixed inset-0 flex items-center justify-center z-50">
    <div
        class="relative p-8 bg-[#FDEFE2] rounded-2xl w-full max-w-lg border border-orange-200 shadow-xl"
        on:click|stopPropagation
    >
        <h2 class="text-2xl font-bold mb-6">เพิ่มหลักสูตร</h2>

        <!-- ชื่อไทย -->
        <div class="items-center mb-4 flex w-full">
            <label class="w-[200px]"
                >ชื่อหลักสูตร (ไทย) <span class="text-red-500">*</span> :</label
            >
            <input
                type="text"
                bind:value={name_th}
                placeholder="ระบุชื่อหลักสูตรภาษาไทย"
                class="flex-1 px-3 py-2 border rounded-md outline-none"
            />
        </div>

        <!-- ชื่ออังกฤษ -->
        <div class="items-center mb-4 flex w-full">
            <label class="w-[200px]"
                >ชื่อหลักสูตร (อังกฤษ) <span class="text-red-500">*</span> :</label
            >
            <input
                type="text"
                bind:value={name_en}
                placeholder="ระบุชื่อหลักสูตรภาษาอังกฤษ"
                class="flex-1 px-3 py-2 border rounded-md outline-none"
            />
        </div>

        <!-- เลือกภาควิชา -->
        <div class="items-center mb-4 flex w-full">
            <label class="w-[200px]"
                >ภาควิชาที่สังกัด <span class="text-red-500">*</span> :</label
            >
            <select
                bind:value={department_id}
                class="flex-1 px-3 py-2 border rounded-md outline-none"
            >
                <option value="">-- เลือกภาควิชา --</option>
                {#each Depart as dep}
                    <option value={dep.id}>{dep.name_th}</option>
                {/each}
            </select>
        </div>

        <!-- ชั้นปี -->
        <div class="items-center mb-6 flex w-full">
            <label class="w-[200px]"
                >ชั้นปี (มากกว่า 3 ปี) <span class="text-red-500">*</span> :</label
            >
            <input
                type="number"
                bind:value={num_years}
                min="1"
                placeholder="ระบุชั้นปี"
                class="flex-1 px-3 py-2 border rounded-md outline-none"
            />
        </div>

        <!-- ปุ่ม -->
        <div class="flex justify-center gap-6">
            <button
                class="px-10 py-2 bg-orange-500 text-white rounded-lg hover:bg-orange-600"
                on:click={submit}
            >
                ยืนยัน
            </button>
            <button
                class="px-10 py-2 bg-white border rounded-lg hover:bg-gray-200"
                on:click={onClose}
            >
                ยกเลิก
            </button>
        </div>
    </div>
</div>
</div>

