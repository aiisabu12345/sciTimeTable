<script lang="ts">
    import Table from "$lib/compornent/Table.svelte";
    import { PUBLIC_API_URL } from "$env/static/public";
    import EditModal from "./EditModal.svelte";

    // กำหนด columns ของตาราง (ฟิลด์ตาม API)
    import { onMount } from "svelte";
    import SearchInput from "$lib/compornent/SearchInput.svelte";
    import PostModel from "./PostModel.svelte";
    const columns = [
        { key: "name_th", label: "ชื่อภาควิชา (TH)" },
        { key: "name_en", label: "ชื่อภาควิชา (EN)" },
        {
            key: "manage",
            label: "จัดการ",
        },
    ];

    let rows: any = [];
    let page = 1;

    let search = "";
    $: search, page, loadDepartments();
    let loading = false;
    async function loadDepartments() {
        loading = true;
        try {
            let url = "";
            if (search === "") {
                url = `${PUBLIC_API_URL}/api/departments?limit=10&page=${page}`;
            } else {
                url = `${PUBLIC_API_URL}/api/departments/search?q=${search}&limit=10&page=${page}`;
            }
            const res = await fetch(url);
            const data = await res.json();
            rows = data;
        } catch (e) {
            console.error("โหลดข้อมูลภาควิชาล้มเหลว:", e);
        } finally {
            loading = false; // ปิดโหลด
        }
    }
    let HaveLoad = true
    function loadMore() {
        page++;
        loadDepartments();
    }

    loadDepartments();

    // POST
    // state ของฟอร์ม
    let name_th = "";
    let name_en = "";

    // รีเซ็ตฟอร์ม
    const resetForm = () => {
        name_th = "";
        name_en = "";
    };

    // ส่งข้อมูลไป backend
    async function submit() {
        const payload = {
            name_th,
            name_en,
        };

        try {
            const res = await fetch(`${PUBLIC_API_URL}/api/departments`, {
                method: "POST",
                headers: { "Content-Type": "application/json" },
                body: JSON.stringify(payload),
            });

            if (!res.ok) throw new Error("ส่งข้อมูลไม่สำเร็จ");
            loadDepartments();
            alert("เพิ่มภาควิชาสำเร็จ!");
            resetForm();
        } catch (err: any) {
            console.error(err);
            alert("เกิดข้อผิดพลาด: " + err.message);
        }
    }
    let dataEdit: any = null;
    const closeModal = () => {
        dataEdit = null;
    };
    const handleEdit = (row: any) => {
        // clone row เพื่อไม่ให้แก้ไขตรงๆ ใน table
        dataEdit = { ...row };
    };
    const handleDelete = async (row: any) => {
        const confirmDelete = window.confirm(
            `คุณต้องการลบภาควิชา "${row.name_th}" ใช่หรือไม่?`,
        );
        if (!confirmDelete) return; // ถ้าไม่ confirm ก็ออกเลย

        try {
            const res = await fetch(
                `${PUBLIC_API_URL}/api/departments/${row.id}`,
                {
                    method: "DELETE",
                    headers: { "Content-Type": "application/json" },
                },
            );
            if (!res.ok) throw new Error("ลบข้อมูลไม่สำเร็จ");
            loadDepartments();
            alert("ลบภาควิชาสำเร็จ!");
        } catch (err: any) {
            console.error(err);
            alert("เกิดข้อผิดพลาด: " + err.message);
        }
    };

    let openAdd = false;

    function openPopup() {
        openAdd = true;
    }
    function closePopup() {
        openAdd = false;
    }
    
</script>

<h1 class="text-3xl font-semibold mb-4">จัดการภาควิชา</h1>
<div class="flex items-center space-x-2 mb-4">
    <SearchInput bind:search placeholder="ค้นหาภาควิชา..." />
    <button
        class="bg-green-500 px-12 py-2 text-white rounded-2xl whitespace-nowrap"
        on:click={openPopup}
    >
        เพิ่มข้อมูล
    </button>
</div>
{#if dataEdit !== null}
    <EditModal data={dataEdit} reLoad={loadDepartments} onClose={closeModal} />
{/if}
{#if loading}
    <div class="flex justify-center py-8">
        <div
            class="animate-spin rounded-full h-10 w-10 border-b-2 border-orange-500"
        ></div>
    </div>
{:else}
    <Table {columns} {rows} onLoadMore={loadMore} HaveLoad = {HaveLoad}>
        <div slot="manage" let:row class="flex gap-4 w-full justify-center">
            <button
                class="p-2 bg-[#FF7A00] text-white rounded cursor-pointer"
                on:click={() => handleEdit(row)}
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
                class="p-2 bg-[#FF4141] text-white rounded cursor-pointer"
                on:click={() => handleDelete(row)}
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
    <PostModel onClose={closePopup} reLoad={loadDepartments} />
{/if}
<!-- <hr class="bg-black my-12" />
<div
    class="p-8 bg-[#FDEFE2] rounded-2xl w-full lg:w-2/4 mx-auto border border-orange-200"
>
    <h2 class="text-2xl font-bold mb-6">เพิ่มภาควิชา</h2>

    <div class="items-center mb-4 flex w-full">
        <label class=" w-[200px]">
            ชื่อภาควิชา (ไทย) <span class="text-red-500">*</span> :
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
            >ชื่อภาควิชา (อังกฤษ) <span class="text-red-500">*</span> :
        </label>
        <div class="flex items-center gap-2 mt-1 w-full">
            <input
                type="text"
                bind:value={name_en}
                placeholder="ระบุชื่อภาควิชาภาษาอังกฤษ"
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
