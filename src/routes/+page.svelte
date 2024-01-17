<script>
  import {onMount} from 'svelte';
  import Chart from 'chart.js/auto';

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
    fetch("http://127.0.0.1:8000/organization")
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
          labels: ['푼 문제', '만점에 도전하는 문제', '시도한 문제', '기타'],
          datasets: [{
            data: [solved, tried, failed, notTried],
            backgroundColor: ['#2dd4ad', '#FFCE56', '#FF6384', '#36A2EB'],
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
<main>
    <div class="container mx-auto flex flex-col min-h-screen">
        <div class="content flex flex-row flex-grow">
            <div class="text-container flex-1">
                <h1><strong>오늘까지 {Static.name}는</strong></h1>
                <h1><strong>{Static.members}명</strong> 구성원이</h1>
                <h1><strong>{Static.problemsSolved}개</strong> 문제를 풀었고</h1>
                <h1><strong>AC rating {Static.acRating}</strong>로</h1>
                <h1>전체 <strong>{Static.rank}등</strong>,</h1>
                <h1>고등학교 <strong>{Static.hsRank}등</strong> 입니다.</h1>
            </div>
            <div class="graph-container flex-1">
                <canvas id="graphCanvas"></canvas>
            </div>
        </div>
        <div class="footer flex p-4 text-sm w-full">
            <div class="footer-section flex-1 text-left">
                <p>All the information of problems, users and icons related to them in this site are borrowed from
                    <a style="color: #0076c0;" href="https://www.acmicpc.net/" target="_blank">Baekjoon</a>
                    and <a style="color: #17ce3a" href="https://solved.ac/" target="_blank">solved.ac</a>
                </p>
            </div>
            <div class="footer-section flex-1 text-center">
                <p>Contact</p>
                <a href="https://mail.google.com/mail/?view=cm&fs=1&to=has_22121@hana.hs.kr" target="_blank">has_22121@hana.hs.kr</a>
            </div>
            <div class="footer-section flex-1 text-right">
                <p>This site simply presents the information gathered from
                    <a style="color: #0076c0;" href="https://www.acmicpc.net/" target="_blank">Baekjoon</a> and
                    <a style="color: #17ce3a" href="https://solved.ac/" target="_blank">solved.ac</a>
                    in an organized manner and is not officially affiliated with them.</p>
            </div>
        </div>
    </div>
</main>

<style>
    .container {
        @apply flex flex-col min-h-screen mx-auto;
    }

    .content {
        @apply flex-grow flex flex-row;
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

    .footer {
        @apply flex text-sm p-4 w-full;
    }

    .footer-section {
        @apply flex-1;
    }
</style>

