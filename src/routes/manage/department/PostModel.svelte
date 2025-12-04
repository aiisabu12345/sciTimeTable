<script lang="ts">
    import { PUBLIC_API_URL } from "$env/static/public";

    export let onClose = () => {};
    export let reLoad = () => {};

    let name_th = "";
    let name_en = "";

    const resetForm = () => {
        name_th = "";
        name_en = "";
    };

    async function submit() {
        if (!name_th || !name_en) {
            alert("กรุณากรอกข้อมูลให้ครบทุกช่อง");
            return;
        }

        const payload = { name_th, name_en };

        try {
            const res = await fetch(`${PUBLIC_API_URL}/api/departments`, {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(payload),
            });

            if (!res.ok) throw new Error("ส่งข้อมูลไม่สำเร็จ");

            alert("เพิ่มภาควิชาสำเร็จ!");
            resetForm();
            reLoad();
            onClose();
        } catch (err: any) {
            alert("เกิดข้อผิดพลาด: " + err.message);
        }
    }
</script>

<!-- Wrapper ครอบทั้ง popup และ background -->
<div
    class="fixed inset-0 flex items-center justify-center z-50"
    on:click={onClose}  
>
    <!-- Background -->
    <div class="absolute inset-0 bg-black/40 backdrop-blur-sm"></div>

    <!-- Popup -->
    <div
        class="relative p-8 bg-[#FDEFE2] rounded-2xl w-full max-w-lg border border-orange-200 shadow-xl"
        on:click|stopPropagation 
    >
        <h2 class="text-2xl font-bold mb-6">เพิ่มภาควิชา</h2>

        <!-- ชื่อไทย -->
        <div class="items-center mb-4 flex w-full">
            <label class="w-[200px]">
                ชื่อภาควิชา (ไทย) <span class="text-red-500">*</span> :
            </label>
            <input
                type="text"
                bind:value={name_th}
                placeholder="ระบุชื่อภาควิชาภาษาไทย"
                class="flex-1 px-3 py-2 border rounded-md outline-none"
                required
            />
        </div>

        <!-- ชื่ออังกฤษ -->
        <div class="items-center mb-6 flex">
            <label class="w-[200px]">
                ชื่อภาควิชา (อังกฤษ) <span class="text-red-500">*</span> :
            </label>
            <input
                type="text"
                bind:value={name_en}
                placeholder="ระบุชื่อภาควิชาภาษาอังกฤษ"
                class="flex-1 px-3 py-2 border rounded-md outline-none"
                required
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
