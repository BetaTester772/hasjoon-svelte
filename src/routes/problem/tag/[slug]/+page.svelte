<script>
  import {onMount} from "svelte";
  import {page} from "$app/stores";

  let problemList = [];
  let maxProblemCount = 0;
  let totalPages = 0;

  const calculateTotalWidth = (total, maxTotal) => {
    if (total > maxTotal) {
      total = maxTotal;
    }
    return (total / maxTotal) * 100 + '%';
  };


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

    // Sort the problemList based on the count in descending order
    const sortedProblemList = [...problemList].sort((a, b) => b.count - a.count);

    maxProblemCount = sortedProblemList[0].count;
  });

  // 전체 페이지 수 계산을 20개 기준으로 변경
  $: if (problemList.length > 0) {
    totalPages = Math.ceil(problemList.length / 20);
  }

  $: page_num = parseInt($page.params.slug || '1');
  // 데이터 슬라이싱 범위를 20개 기준으로 변경
  $: slicedData = problemList.slice(20 * (page_num - 1), 20 * page_num);

  // Update maxProblemCount based on slicedData
  $: if (slicedData.length > 0) {
    const sortedSlicedData = [...slicedData].sort((a, b) => b.count - a.count);
    maxProblemCount = sortedSlicedData[0].count;
  }

  const calculateWidth = (solved, total) => (solved / total) * 100 + '%';
</script>

<div class="container mx-auto">
    {#if problemList.length > 0}
        <table class="min-w-full bg-white text-left">
            <thead class="bg-gray-800 text-white">
            <tr>
                <th class="w-1/3 py-3 px-4 uppercase font-semibold text-sm">레벨</th>
                <th class="w-1/12 py-3 px-4 uppercase font-semibold text-sm">미해결</th>
                <th class="w-1/12 py-3 px-4 uppercase font-semibold text-sm">해결</th>
                <th class="w-1/12 py-3 px-4 uppercase font-semibold text-sm">전체</th>
                <th class="w-1/6 py-3 px-4 uppercase font-semibold text-sm">진행도</th>
            </tr>
            </thead>
            <tbody class="text-gray-700 ">
            {#each problemList.slice(20 * (page_num - 1), 20 * page_num) as problem}
                <tr>
                    <td class="w-1/3 py-3 px-4"><span class="inline-flex items-center"><p>{problem.ko}</p>&nbsp;<p
                            class="text-gray-400">{problem.en}</p></span></td>
                    <td class="w-1/12 py-3 px-4">{problem.count - problem.solved_count}</td>
                    <td class="w-1/12 py-3 px-4">{problem.solved_count}</td>
                    <td class="w-1/12 py-3 px-4">{problem.count}</td>
                    <td class="w-1/6 py-3 px-4">
                        <div class="relative pt-1"
                             style="width: {calculateTotalWidth(problem.count, maxProblemCount)};">
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

        <div class="flex justify-center space-x-2">
            {#each Array(totalPages) as _, i}
                <a href={`/problem/tag/${i+1}`}>
                    <button class="{page_num === (i + 1) ? 'pagination-btn-active' : 'pagination-btn'} sm:text-xs">
                        {i + 1}
                    </button>
                </a>
            {/each}
        </div>
    {:else}
        <div class="flex justify-center items-center">
            <div class="text-lg text-gray-700">Loading...</div>
        </div>
    {/if}
</div>

<style>
    .pagination-btn {
        padding: 10px 15px; /* 크기 조정 */
        margin: 5px; /* 마진 추가 */
        border: 1px solid #ddd; /* 윤곽선 추가 */
        border-radius: 5px; /* 윤곽선 둥글게 */
        background-color: white; /* 배경색 */
        color: black; /* 글자색 */
        font-size: 16px; /* 글자 크기 */
        cursor: pointer; /* 커서 변경 */
        transition: all 0.3s ease; /* 부드러운 전환 효과 */
    }

    .pagination-btn:hover {
        background-color: #f0f0f0; /* 호버 시 배경색 변경 */
    }

    .pagination-btn:disabled {
        color: #999; /* 비활성화 시 글자색 변경 */
        cursor: not-allowed; /* 커서 변경 */
    }

    .pagination-btn-active {
        padding: 10px 15px; /* 크기 조정 */
        margin: 5px; /* 마진 추가 */
        border: 1px solid #ddd; /* 윤곽선 추가 */
        border-radius: 5px; /* 윤곽선 둥글게 */
        color: black; /* 글자색 */
        font-size: 16px; /* 글자 크기 */
        cursor: pointer; /* 커서 변경 */
        transition: all 0.3s ease; /* 부드러운 전환 효과 */
        background-color: #1d886f; /* Example: Change background color */
        color: white; /* Example: Change text color */
    }

    /* Adjust styles for smaller screens using media queries */
    @media (max-width: 640px) { /* Tailwind's 'sm' breakpoint */
        .pagination-btn, .pagination-btn-active {
            padding: 2px 2px; /* Smaller padding */
            font-size: 12px; /* Smaller font size */
        }
    }
</style>