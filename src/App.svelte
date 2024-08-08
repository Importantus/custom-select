<script lang="ts">
  import CustomSelect from "./lib/CustomSelect.svelte";
  let selectedOption: Option;
</script>

<main class="flex justify-center items-center w-full h-screen flex-col gap-10">
  <div>
    <h1 class="text-2xl font-bold">Custom Dropdown Select</h1>
    <p class="text-gray-600">
      {selectedOption != null
        ? `Selected: ${selectedOption.value}`
        : "Nothing selected"}
    </p>
  </div>
  <div class="card">
    <CustomSelect
      options={[
        {
          id: "1",
          value: "Option 1",
        },
        {
          id: "2",
          value: "Option 2",
        },
        {
          id: "3",
          value: "Option 3",
        },
        {
          id: "4",
          value: "Option 4",
        },
        {
          id: "5",
          value: "Option 5",
        },
      ]}
      placeholder="Ich halte einen Platz frei"
      bind:value={selectedOption}
    />
  </div>
</main>
