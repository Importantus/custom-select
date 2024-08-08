<script lang="ts">
  import { fly } from "svelte/transition";

  export let placeholder = "Select option";
  export let value: Option | null = null;
  export let options: Option[];

  let currentFocusIndex = 0;
  let showDropdown = false;

  function toggleDropdown() {
    showDropdown = !showDropdown;
    if (showDropdown) {
      currentFocusIndex = value != null ? options.indexOf(value) : 0;
    }
  }

  function handleItemSelection(option: Option) {
    value = option;
    toggleDropdown();
  }

  function handleListKeydown(event: KeyboardEvent) {
    if (event.key === "Enter") {
      event.preventDefault();
      handleItemSelection(options[currentFocusIndex]);
    }

    if (event.key === "ArrowDown" || event.key === "ArrowUp") {
      currentFocusIndex =
        event.key === "ArrowDown"
          ? Math.min(currentFocusIndex + 1, options.length - 1)
          : Math.max(currentFocusIndex - 1, 0);
    }

    if (event.key === "Escape") {
      toggleDropdown();
    }
  }

  function handleFocusOut(event: FocusEvent) {
    const currentTarget = event.currentTarget as HTMLElement;
    if (
      !event.relatedTarget ||
      !currentTarget?.contains(event.relatedTarget as Node)
    ) {
      showDropdown = false;
    }
  }
</script>

<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
<div
  class="w-80"
  role="dialog"
  on:focusout={handleFocusOut}
  on:keydown={handleListKeydown}
>
  <button
    class="w-full border-2 hover:border-gray-400 focus:border-blue-600 p-2 rounded-lg flex justify-between items-center transition-all duration-200"
    aria-expanded={showDropdown}
    on:click={toggleDropdown}
  >
    <p class="{value == null ? 'text-gray-500' : 'font-semibold'} ">
      {value != null ? value.value : placeholder}
    </p>
    <div
      class="text-gray-500 transition-transform duration-300 {showDropdown
        ? 'rotate-180'
        : ''}"
    >
      <svg
        xmlns="http://www.w3.org/2000/svg"
        width="24"
        height="24"
        viewBox="0 0 24 24"
        fill="none"
        stroke="currentColor"
        stroke-width="1.5"
        stroke-linecap="round"
        stroke-linejoin="round"
        class="lucide lucide-chevron-down"><path d="m6 9 6 6 6-6" /></svg
      >
    </div>
  </button>
  <div class="relative">
    {#if showDropdown}
      <ul
        transition:fly={{
          duration: 400,
          opacity: 0,
          y: "-5%",
        }}
        class="absolute z-10 bg-white flex flex-col w-full max-h-52 overflow-y-auto overflow-x-hidden rounded-md shadow mt-2 cursor-pointer"
      >
        {#each options as option, index}
          <button
            class="focus:outline-none"
            on:click={() => handleItemSelection(option)}
            on:mouseenter={() => (currentFocusIndex = index)}
          >
            <li
              in:fly|global={{
                delay: 100 * index,
                duration: 300,
                opacity: 0,
                x: "-5%",
              }}
              class="px-4 py-2 text-left
                {option.id === value?.id
                ? 'bg-blue-600 text-white'
                : index === currentFocusIndex
                  ? 'bg-gray-200'
                  : ''}
                "
            >
              {option.value}
            </li>
          </button>
        {/each}
      </ul>
    {/if}
  </div>
</div>
