<script>
  import {onMount} from 'svelte';
  import Chart from 'chart.js/auto';

  let chart;


  let Static = {
    name: "",
    members: 0,
    problemsSolved: 0,
    acRating: 0,
    rank: 0,
    solved: 0,
    tried: 0,
    failed: 0,
    notTried: 0,
  };


  onMount(() => {
    fetch("http://127.0.0.1:8000/organization")
      .then(res => res.json())
      .then(data => {
        Static.name = data.name;
        Static.members = data.user_count;
        Static.problemsSolved = data.solved_count;
        Static.acRating = data.rating;
        Static.rank = data.rank;

        const solved = data.solved_count;
        const tried = 0;
        const failed = 0;
        const notTried = data.count - data.solved_count;

        const graphData = {
          labels: ['푼 문제', '만점에 도전하는 문제', '시도한 문제', '풀 문제'],
          datasets: [{
            data: [solved, tried, failed, notTried],
            backgroundColor: ['#36A2EB', '#FFCE56', '#FF6384', '#4BC0C0'],
            borderWidth: 1
          }]
        };

        const canvas = document.getElementById('graphCanvas');
        const ctx = canvas.getContext('2d');

        chart = new Chart(ctx, {
          type: 'doughnut',
          data: graphData,
          options: {
            responsive: true,
            maintainAspectRatio: false
          }
        });
      })
      .catch(err => console.log(err));
  });
</script>


<div class="container mx-auto flex">
    <div class="text-container flex-1">
        <h1><strong>오늘까지 {Static.name}는</strong></h1>
        <h1><strong>{Static.members}명</strong> 구성원이</h1>
        <h1><strong>{Static.problemsSolved}개</strong> 문제를 풀었고</h1>
        <h1><strong>AC rating {Static.acRating}</strong>로</h1>
        <h1>전체 <strong>{Static.rank}등</strong>입니다.</h1>
    </div>
    <div class="graph-container flex-1">
        <canvas id="graphCanvas"></canvas>
    </div>
</div>


<style>
    .container {
        @apply flex mx-auto max-w-7xl;
    }

    .text-container {
        @apply p-5 ml-10 flex flex-col justify-center text-5xl text-left leading-snug;
        word-break: keep-all;
    }

    @media (max-width: 1024px) {
        .text-container {
            @apply text-3xl;
        }

        #graphCanvas {
            @apply max-w-md max-h-60;
        }
    }

    @media (max-width: 768px) {
        .text-container {
            @apply text-2xl;
        }

        #graphCanvas {
            @apply max-w-sm max-h-40;
        }
    }


    .graph-container {
        @apply p-5 flex justify-center items-center;
    }

    #graphCanvas {
        @apply w-full h-full max-w-lg max-h-96;
    }
</style>

