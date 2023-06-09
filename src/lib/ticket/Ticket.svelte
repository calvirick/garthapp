<script lang="ts">
  import { capitalizeFirstLetter } from '$lib/utils/capitalize';

  import Priority from './Priority.svelte';
  import AssignedUser from './AssignedUser.svelte';

  let statusColor = 'text-black';

  export let id: number;
  export let title: string;
  export let status: string;
  export let assigned_to: string;
  export let category: string;
  export let created_at: string;
  export let priority: string;

  const formattedDate = new Date(created_at)
    .toLocaleDateString('en-US', {
      month: '2-digit',
      day: '2-digit',
      year: '2-digit',
    })
    .replace(/\//g, '-');
</script>

<div class="floating flex-wrapper">
  <div id="select" class="ticket-cell with-border">
    <input type="checkbox" class="select-checkbox" />
  </div>
  <a href="/ticket/{id}" class="flex-wrapper w-full">
    <div id="ticket_id" class="ticket-cell with-border text-center w-[90px]">
      <strong>{id}</strong>
    </div>
    <div
      id="ticket-title"
      class="ticket-cell with-border flex flex-row items-center h-[2.6rem] flex-grow bg-lime-100 hover:bg-orange-100 font-roboto-condensed font-medium w-[40rem] justify-between"
    >
      {title}
      <div
        id="status"
        class="{status === 'new'
          ? 'text-white bg-purple-500'
          : status === 'replied'
          ? 'text-white bg-blue-500'
          : status === 'submitted'
          ? `text-white bg-green-600`
          : status === 'waiting'
          ? `${statusColor} bg-yellow-200`
          : status === 'hold'
          ? `${statusColor} bg-stone-300`
          : status === 'resolved'
          ? `${statusColor} bg-lime-300`
          : statusColor} rounded-full border border-solid border-black w-[115px] ml-10 text-center"
      >
        {#if status === 'resolved'}
          <i class="fa-solid fa-check" />
          {capitalizeFirstLetter(status)}
        {:else}
          {capitalizeFirstLetter(status)}
        {/if}
      </div>
    </div>
    <AssignedUser username={assigned_to} />
    <div
      id="category"
      class="ticket-cell with-border font-semibold italic w-[11rem] text-center whitespace-nowrap"
    >
      {category.toUpperCase()}
    </div>
    <div
      id="created_at"
      class="ticket-cell with-border font-roboto-condensed font-medium italic text-sm h-[2.6rem] justify-center items-center flex flex-row"
    >
      <span class="text-xs">DATE CREATED: &nbsp;</span>
      {formattedDate}
    </div>
    <div
      id="priority"
      class="ticket-cell with-border bg-slate-200 w-[149px] max-h-[42px] text-center"
    >
      <Priority {priority} />
    </div>
  </a>
</div>

<style lang="postcss">
  :root {
    --checkboxSize: 1.125rem;
  }

  input.select-checkbox {
    position: relative;
    top: 3px;
    width: var(--checkboxSize);
    height: var(--checkboxSize);
  }

  .ticket-cell {
    @apply p-2 px-5;
  }

  :global(.with-border) {
    @apply border border-solid border-black;
  }

  .flex-wrapper {
    @apply flex flex-row place-content-center;
  }

  .floating {
    @apply hover:bg-slate-200 hover:shadow-xl hover:transition-shadow;
  }

  .floating:hover {
    position: relative;
    bottom: 1px;
  }

  .floating:active {
    position: relative;
    bottom: 0px;
    @apply ring-4;
  }
</style>
