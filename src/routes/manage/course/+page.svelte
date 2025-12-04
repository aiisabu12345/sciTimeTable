<script lang="ts">
    import SearchInput from "$lib/compornent/SearchInput.svelte";
    import { PUBLIC_API_URL } from "$env/static/public";
    import Table from "$lib/compornent/Table.svelte";
    import PostModel from "./PostModel.svelte";
    import EditModal from "./EditModal.svelte";
    let name_th = "";
    let name_en = "";
    let num_years = "";
    let department_id = "";
    // GET พากวิชา
    let rows: any = [];
    let Depart: any = [];
    let Program: any = [];

    async function loadDepartments() {
        try {
            const res = await window.fetch(`${PUBLIC_API_URL}/api/departments`);
            const data = await res.json();
            Depart = data;
            mapProgramsWithDepartment();
        } catch (e) {
            console.error("โหลดข้อมูลภาควิชาล้มเหลว:", e);
        }
    }
    loadDepartments();
    let page = 1;

    let HaveLoad = true;
    $: if (search !== undefined) {
        page = 1;
        loadProgram(false);
    }
    // $: search,page , loadProgram();
    let loading = false;

    async function loadProgram(isLoadMore = false) {
        loading = true;
        try {
            let url = search
                ? `${PUBLIC_API_URL}/api/programs/search?q=${search}&limit=10&page=${page}`
                : `${PUBLIC_API_URL}/api/programs?limit=10&page=${page}`;

            const res = await window.fetch(url);
            const data = await res.json();

            const programsData = data.data ?? data; // ถ้า API คืน { data: [...] }

            if (isLoadMore) {
                if (programsData.length === 0) {
                    HaveLoad = false;
                }
                Program = [...Program, ...programsData];
            } else {
                Program = programsData;
                HaveLoad = true; // reset flag เวลาโหลดหน้าแรก
            }

            mapProgramsWithDepartment();
        } catch (e) {
            console.error("โหลดข้อมูลหลักสูตรล้มเหลว:", e);
        } finally {
            loading = false;
        }
    }

    function loadMore() {
        if (!HaveLoad) return;
        page++;
        loadProgram(true);
    }
    loadProgram(false);

    function mapProgramsWithDepartment() {
        if (!Program || !Depart) return;

        rows = Program.map((prog: any) => {
            // หา department ที่ตรงกับ department_id
            const dep = Depart.find((d: any) => d.id === prog.department_id);
            return {
                ...prog,
                department: dep ? dep.name_th : "-", // เพิ่ม field department
            };
        });
    }

    let search = "";
    const columns = [
        { key: "name_en", label: "ชื่อหลักสูตร (อังกฤษ)" },
        { key: "name_th", label: "ชื่อหลักสูตร (ไทย)" },
        { key: "num_years", label: "ชั้นปี" },
        { key: "department", label: "ภาควิชา" }, // แสดงชื่อ department
        { key: "manage", label: "จัดการ" },
    ];
    const resetForm = () => {
        name_th = "";
        name_en = "";
        num_years = "";
        department_id = "";
    };
    async function submit() {
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

            const responseText = await res.text();
            console.log("response:", responseText);

            if (!res.ok) throw new Error(responseText || "ส่งข้อมูลไม่สำเร็จ");

            loadProgram();
            alert("เพิ่มหลักสูตรสำเร็จ!");
            resetForm();
        } catch (err: any) {
            console.error(err);
            console.log(err.message);
            alert("เกิดข้อผิดพลาด: ");
        }
    }
    let openAdd = false;

    function openPopup() {
        openAdd = true;
    }
    function closePopup() {
        openAdd = false;
    }
    let dataEdit: any = null;
    const handleEdit = (row: any) => {
        dataEdit = { ...row };
    };

    async function deleteProgram(id: number) {
        const confirmDelete = confirm("คุณแน่ใจว่าจะลบหลักสูตรนี้?");
        if (!confirmDelete) return;

        try {
            const res = await fetch(`${PUBLIC_API_URL}/api/programs/${id}`, {
                method: "DELETE",
            });

            if (!res.ok) {
                const errText = await res.text();
                throw new Error(errText || "ลบหลักสูตรไม่สำเร็จ");
            }

            alert("ลบหลักสูตรสำเร็จ!");
            loadProgram(); // reload ตาราง
        } catch (err: any) {
            console.error(err);
            alert("เกิดข้อผิดพลาด: " + err.message);
        }
    }
</script>

<h1 class="text-3xl font-semibold mb-4">จัดการหลักสูตร</h1>
<div class="flex items-center space-x-2 mb-4">
    <SearchInput bind:search placeholder="ค้นหาหลักสูตร..." />
    <button
        class="bg-green-500 px-12 py-2 text-white rounded-2xl whitespace-nowrap"
        on:click={openPopup}
    >
        เพิ่มข้อมูล
    </button>
</div>
{#if dataEdit !== null}
    <EditModal
        data={dataEdit}
        reLoad={loadProgram}
        onClose={() => (dataEdit = null)}
    />
{/if}
{#if loading}
    <div class="flex justify-center py-8">
        <div
            class="animate-spin rounded-full h-10 w-10 border-b-2 border-orange-500"
        ></div>
    </div>
    {:else}
<Table {columns} {rows} onLoadMore={loadMore} {HaveLoad}>
    <div slot="manage" let:row class="flex gap-4 w-full justify-center">
        <button
            on:click={() => handleEdit(row)}
            class="p-2 bg-[#FF7A00] text-white rounded cursor-pointer"
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
                class="lucide lucide-pencil-icon lucide-pencil"
                ><path
                    d="M21.174 6.812a1 1 0 0 0-3.986-3.987L3.842 16.174a2 2 0 0 0-.5.83l-1.321 4.352a.5.5 0 0 0 .623.622l4.353-1.32a2 2 0 0 0 .83-.497z"
                /><path d="m15 5 4 4" /></svg
            >
        </button>

        <button
        on:click={() => deleteProgram(row.id)} class="p-2 bg-[#FF4141] text-white rounded cursor-pointer">
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
                class="lucide lucide-trash2-icon lucide-trash-2"
                ><path d="M10 11v6" /><path d="M14 11v6" /><path
                    d="M19 6v14a2 2 0 0 1-2 2H7a2 2 0 0 1-2-2V6"
                /><path d="M3 6h18" /><path
                    d="M8 6V4a2 2 0 0 1 2-2h4a2 2 0 0 1 2 2v2"
                /></svg
            >
        </button>
    </div>
</Table>
{/if}
{#if openAdd}
    <PostModel onClose={closePopup} reLoad={loadProgram} />
{/if}
<!-- <hr class="bg-black my-12" />
<div
    class="p-8 bg-[#FDEFE2] rounded-2xl w-full lg:w-2/4 mx-auto border border-orange-200"
>
    <h2 class="text-2xl font-bold mb-6">เพิ่มหลักสูตร</h2>

    <div class="items-center mb-4 flex w-full">
        <label class=" w-[200px]">
            ชื่อหลักสูตร (ไทย) <span class="text-red-500">*</span> :
        </label>

        <div class="flex items-center gap-2 mt-1 w-full">
            <input
                type="text"
                bind:value={name_th}
                placeholder="ระบุชื่อภาควิชาภาษาไทย"
                class="flex-1 px-3 py-2 border rounded-md outline-none"
            />
        </div>
    </div>

    <div class="mb-6 items-center flex">
        <label class="w-[200px]"
            >ชื่อหลักสูตร (อังกฤษ) <span class="text-red-500">*</span> :
        </label>
        <div class="flex items-center gap-2 mt-1 w-full">
            <input
                required
                type="text"
                bind:value={name_en}
                placeholder="ระบุชื่อภาควิชาภาษาอังกฤษ"
                class="flex-1 px-3 py-2 border rounded-md outline-none"
            />
        </div>
    </div>
    <div class="mb-6 items-center flex">
        <label class="w-[200px]">
            ภาควิชาที่สังกัด <span class="text-red-500">*</span> :
        </label>

        <div class="flex items-center gap-2 mt-1 w-full">
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
    </div>
    <div class="mb-6 items-center flex">
        <label class="w-[200px]"
            >ชั้นปี <span class="text-red-500">*</span> :
        </label>
        <div class="flex items-center gap-2 mt-1 w-full">
            <input
                type="number"
                bind:value={num_years}
                min="4"
                placeholder="ระบุชั้นปี"
                required
                class="flex-1 px-3 py-2 border rounded-md outline-none"
            />
        </div>
    </div>
    <div class="flex justify-center gap-6">
        <button
            class="px-10 py-2 bg-orange-500 text-white rounded-lg hover:bg-orange-600"
            on:click={submit}
        >
            ยืนยัน
        </button>

        <button
            class="px-10 py-2 bg-white border rounded-lg hover:bg-gray-200"
            on:click={resetForm}
        >
            ยกเลิก
        </button>
    </div>
</div> -->
