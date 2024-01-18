<script>
  import {onMount} from "svelte";

  let problemList = [];

  // const url = "http://localhost:8000"
  const url = "https://api.hasjoon.net"

  onMount(async () => {
    try {
      const response = await fetch(url + "/problem/tag");
      if (response.ok) {
        const data = await response.json();
        problemList = data.problem_tag;
      } else {
        console.error("Error fetching problem list:", response.statusText);
      }
    } catch (error) {
      console.error("Network error:", error);
    }
  });

  const calculateWidth = (solved, total) => (solved / total) * 100 + '%';
</script>

<div class="container mx-auto">
    {#if problemList.length > 0}
        <table class="min-w-full bg-white text-left">
            <thead class="bg-gray-800 text-white">
            <tr>
                <th class="w-1/4 py-3 px-4 uppercase font-semibold text-sm">레벨</th>
                <th class="w-1/12 py-3 px-4 uppercase font-semibold text-sm">미해결</th>
                <th class="w-1/12 py-3 px-4 uppercase font-semibold text-sm">해결</th>
                <th class="w-1/12 py-3 px-4 uppercase font-semibold text-sm">전체</th>
                <th class="w-1/2 py-3 px-4 uppercase font-semibold text-sm">진행도</th>
            </tr>
            </thead>
            <tbody class="text-gray-700 ">
            {#each problemList as problem}
                <tr>
                    <td class="w-1/4 py-3 px-4">{problem.ko}{problem.en}</td>
                    <td class="w-1/12 py-3 px-4">{problem.count}</td>
                    <td class="w-1/12 py-3 px-4">{problem.solved_count}</td>
                    <td class="w-1/12 py-3 px-4">{problem.count - problem.solved_count}</td>
                    <td class="w-1/2 py-3 px-4">
                        <div class="relative pt-1">
                            <div class="overflow-hidden h-2 text-xs flex rounded bg-gray-200">
                                <div style="width: {calculateWidth(problem.solved_count, problem.count)}; background-color: #1d886f;"
                                     class="shadow-none flex flex-col text-center whitespace-nowrap text-white justify-center transition-all duration-300 ease-in-out"></div>
                            </div>
                        </div>
                    </td>
                </tr>
            {/each}
            </tbody>
        </table>
    {:else}
        <div class="flex justify-center items-center">
            <div class="text-lg text-gray-700">Loading...</div>
        </div>
    {/if}
</div>

<style>
    /* Local styles if needed */
</style>