<script>
  import {onMount} from "svelte";

  let problemList = [];

  const rankDict = {
    0: {"name": "Unrated", "color": "rgb(45, 45, 45)"},
    1: {"name": "Bronze V", "color": "rgb(157, 73, 0)"},
    2: {"name": "Bronze IV", "color": "rgb(157, 73, 0)"},
    3: {"name": "Bronze III", "color": "rgb(157, 73, 0)"},
    4: {"name": "Bronze II", "color": "rgb(157, 73, 0)"},
    5: {"name": "Bronze I", "color": "rgb(157, 73, 0)"},
    6: {"name": "Silver V", "color": "rgb(56, 84, 110)"},
    7: {"name": "Silver IV", "color": "rgb(56, 84, 110)"},
    8: {"name": "Silver III", "color": "rgb(56, 84, 110)"},
    9: {"name": "Silver II", "color": "rgb(56, 84, 110)"},
    10: {"name": "Silver I", "color": "rgb(56, 84, 110)"},
    11: {"name": "Gold V", "color": "rgb(210, 133, 0)"},
    12: {"name": "Gold IV", "color": "rgb(210, 133, 0)"},
    13: {"name": "Gold III", "color": "rgb(210, 133, 0)"},
    14: {"name": "Gold II", "color": "rgb(210, 133, 0)"},
    15: {"name": "Gold I", "color": "rgb(210, 133, 0)"},
    16: {"name": "Platinum V", "color": "rgb(0, 199, 139)"},
    17: {"name": "Platinum IV", "color": "rgb(0, 199, 139)"},
    18: {"name": "Platinum III", "color": "rgb(0, 199, 139)"},
    19: {"name": "Platinum II", "color": "rgb(0, 199, 139)"},
    20: {"name": "Platinum I", "color": "rgb(0, 199, 139)"},
    21: {"name": "Diamond V", "color": "rgb(0, 158, 229)"},
    22: {"name": "Diamond IV", "color": "rgb(0, 158, 229)"},
    23: {"name": "Diamond III", "color": "rgb(0, 158, 229)"},
    24: {"name": "Diamond II", "color": "rgb(0, 158, 229)"},
    25: {"name": "Diamond I", "color": "rgb(0, 158, 229)"},
    26: {"name": "Ruby V", "color": "rgb(224, 0, 76)"},
    27: {"name": "Ruby IV", "color": "rgb(224, 0, 76)"},
    28: {"name": "Ruby III", "color": "rgb(224, 0, 76)"},
    29: {"name": "Ruby II", "color": "rgb(224, 0, 76)"},
    30: {"name": "Ruby I", "color": "rgb(224, 0, 76)"}
  };

  // const url = "http://localhost:8000"
  const url = "https://api.hasjoon.net"

  // New function to calculate the total width of the progress bar
  const calculateTotalWidth = (total, maxTotal) => {
    if (total > maxTotal) {
      total = maxTotal;
    }
    return (total / maxTotal) * 100 + '%';
  };

  // Modify the onMount function to find the max count
  let maxProblemCount = 0;

  onMount(async () => {
    try {
      const response = await fetch(url + "/problem/level");
      if (response.ok) {
        const data = await response.json();
        problemList = data.problem_list;
      } else {
        console.error("Error fetching problem list:", response.statusText);
      }
    } catch (error) {
      console.error("Network error:", error);
    }

    // Sort the problemList based on the count in descending order
    const sortedProblemList = [...problemList].sort((a, b) => b.count - a.count);

    // Check if level 0 has the highest count
    if (sortedProblemList[0].level === 0) {
      // Use the count of the next level as the maxProblemCount, or the level 0 count if there's only one level
      maxProblemCount = sortedProblemList[1] ? sortedProblemList[1].count : sortedProblemList[0].count;
    } else {
      // If level 0 is not the highest, use the highest count
      maxProblemCount = sortedProblemList[0].count;
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
                    <td class="w-1/4 py-3 px-4" style="color: {rankDict[problem.level]['color']}">
                        <!--                    TODO: <a href="/problem/level/{problem.level}">-->
                        <span class="inline-flex items-center">
                        <img class="h-5 w-5 mr-2" src="https://static.solved.ac/tier_small/{problem.level}.svg"
                             alt="{problem.level}">
                            {rankDict[problem.level]["name"]}
                        </span>
                        <!--                    </a>-->
                    </td>
                    <td class="w-1/12 py-3 px-4">{problem.count}</td>
                    <td class="w-1/12 py-3 px-4">{problem.solved_count}</td>
                    <td class="w-1/12 py-3 px-4">{problem.count - problem.solved_count}</td>
                    <td class="py-3 px-4">
                        <div class="relative pt-1"
                             style="width: {calculateTotalWidth(problem.count, maxProblemCount)};">
                            <!-- Updated progress bar width -->
                            <div class="overflow-hidden h-2 text-xs flex rounded bg-gray-200">
                                <div style="width: {calculateWidth(problem.solved_count, problem.count)}; background-color: {rankDict[problem.level]['color']};"
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