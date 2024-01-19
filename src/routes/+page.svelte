<script>
  import {onMount} from 'svelte';
  import Chart from 'chart.js/auto';

  // const url = "http://localhost:8000"
  const url = "https://api.hasjoon.net"

  let Static = {
    name: "{학교명}",
    members: null,
    problemsSolved: null,
    acRating: null,
    rank: null,
    hsRank: null,
    solved: null,
    tried: null,
    failed: null,
    notTried: null,
  };


  onMount(() => {
    fetch(url + "/organization")
      .then(res => res.json())
      .then(data => {
        Static.name = data.organization_data.name;
        Static.members = data.organization_data.user_count;
        Static.problemsSolved = data.organization_data.solved_count;
        Static.acRating = data.organization_data.rating;
        Static.rank = data.organization_data.rank;
        Static.hsRank = data.organization_data.rank_high_school;

        const solved = data.organization_data.solved_count;
        const tried = 0;
        const failed = 0;
        const notTried = 29168 - data.organization_data.solved_count;

        const graphData = {
          labels: ['푼 문제', '아직 풀기 전인 문제'], //'만점에 도전하는 문제', '시도한 문제',
          datasets: [{
            data: [solved, notTried],
            backgroundColor: ['#2dd4ad', '#36A2EB'], //  '#FFCE56', '#FF6384',
            borderWidth: 1
          }]
        };

        const canvas = document.getElementById('graphCanvas');
        const ctx = canvas.getContext('2d');

        let chart = new Chart(ctx, {
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

<div class="flex-grow flex items-center">
    <div class="content container flex flex-row mx-auto">
        <div class="text-container flex-1">
            <h1><strong>오늘까지 <span style="color: #1d886f;"><a href="https://solved.ac/ranking/o/804" target="_blank">{Static.name}</a></span>는</strong></h1>
            <h1><strong>{Static.members}명</strong>의 구성원이</h1>
            <h1><strong>{Static.problemsSolved}개</strong>의 문제를 풀었고</h1>
            <h1><strong>AC rating {Static.acRating}</strong>로</h1>
            <h1>고등학교 <strong>{Static.hsRank}등</strong>,</h1>
            <h1>전체 <strong>{Static.rank}등</strong>입니다.</h1>
        </div>
        <div class="graph-container flex-1">
            <canvas id="graphCanvas"></canvas>
        </div>
    </div>
</div>


<style>
    .container {
        @apply mx-auto;
    }

    .content {
        @apply flex flex-row mx-auto;
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
        .content {
            @apply flex-col;
        }

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