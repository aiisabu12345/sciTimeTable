<script lang="ts">
  export let columns: {
    key: string;
    label: string;
    render?: (row: any) => any;
  }[] = [];
  export let rows: any[] = [];

  $: loading = rows.length == 0 ? true : false;
  export let onLoadMore: () => void;
  export let HaveLoad:boolean

  $: limitReached = rows.length >= 10 ? true : false;
</script>


 
{#if loading}
 <div class="flex justify-center py-8">
    <p class="text-orange-400 ">ไม่พบข้อมูล</p>
    </div>
    {:else}
  <div class="w-full overflow-x-auto">
    <table class="min-w-max border-collapse w-full">
      <thead class="p-4">
        <tr class="bg-[#FF922D] border-[#FF922D] border text-white">
          {#each columns as col}
            <th class="p-2">{col.label}</th>
          {/each}
        </tr>
      </thead>

      <tbody>
        {#each rows as row}
          <tr class="hover:bg-gray-100">
            {#each columns as col}
              <td class="p-2 border border-orange-400">
                {#if col.key === "manage"}
                  <slot name="manage" {row}></slot>
                {:else}
                  {row[col.key]}
                {/if}
              </td>
            {/each}
          </tr>
        {/each}
      </tbody>
    </table>
  </div>
  {#if limitReached && HaveLoad}
    <div class="flex justify-center mt-4">
      <button
        on:click={onLoadMore}
        class="px-4 py-2 bg-orange-500 text-white rounded"
      >
        โหลดเพิ่ม
      </button>
    </div>
  {/if}
  {/if}

