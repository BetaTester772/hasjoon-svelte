<script>
  import {page} from "$app/stores";
  import {onMount} from "svelte";

  $: hs_name = $page.params.slug;

  const url = "https://api.hasjoon.net"

  let hasData = {
    "id": null,
    "name": "null",
    "members": null,
    "problemsSolved": null,
    "acRating": null,
    "hsRank": null,
    "rank": null
  }
  let opponentData = {
    "id": null,
    "name": "null",
    "members": null,
    "problemsSolved": null,
    "acRating": null,
    "hsRank": null,
    "rank": null,
    "color": ""
  }

  function getDiffStr(a, b) {
    if (a > b) {
      return `+${a - b}`
    } else if (a < b) {
      return `-${b - a}`
    } else {
      return `0`
    }
  }

  function getDiffColor(a, b) {
    if (a > b) {
      return `#2dd4ad`
    } else if (a < b) {
      return `#d73a49`
    } else {
      return `#6a737d`
    }
  }

  onMount(() => {
    fetch(url + `/vs/high_school?hs_name=${hs_name}`)
      .then(res => {
        if (res.status === 404) {
          window.location.href = '/'; // Redirect to the home page
          alert("해당 학교가 존재하지 않습니다.");
        }
        return res.json();
      })
      .then(data => {
        hasData.id = data.us.organization_id;
        hasData.name = data.us.name;
        hasData.members = data.us.user_count;
        hasData.problemsSolved = data.us.solved_count;
        hasData.acRating = data.us.rating;
        hasData.hsRank = data.us.rank;
        hasData.rank = data.us.global_rank;

        opponentData.id = data.opponent.organization_id;
        opponentData.name = data.opponent.name;
        opponentData.members = data.opponent.user_count;
        opponentData.problemsSolved = data.opponent.solved_count;
        opponentData.acRating = data.opponent.rating;
        opponentData.hsRank = data.opponent.rank;
        opponentData.rank = data.opponent.global_rank;
        if (opponentData.name == "하나고등학교") {
          opponentData.color = "#1d886f"
        } else {
          opponentData.color = data.opponent.color;
        }
      })
  });
</script>

<div class="flex-grow flex items-center">
    <div class="content container flex flex-row mx-auto">
        <div class="text-container flex-1">
            <h1><strong>오늘까지 <span style="color: #1d886f;"><a href="https://solved.ac/ranking/o/{hasData.id}"
                                                              target="_blank">{hasData.name}</a></span></strong></h1>
            <a id="vs" href="/"><i
                    class="fas fa-arrows-alt-h text-black hover:text-blue-500"/></a>
            <h1><strong><span style="color: {opponentData.color};"><a
                    href="https://solved.ac/ranking/o/{opponentData.id}"
                    target="_blank">{opponentData.name}</a></span>는</strong>
            </h1>
            <h1><span style="color: {getDiffColor(opponentData.members, hasData.members)}"><strong>
                {opponentData.members}명</strong> ({getDiffStr(opponentData.members, hasData.members)})</span> 구성원이</h1>
            <h1><span style="color: {getDiffColor(opponentData.problemsSolved, hasData.problemsSolved)}"><strong>
                {opponentData.problemsSolved}개</strong>
                ({getDiffStr(opponentData.problemsSolved, hasData.problemsSolved)})</span> 문제를 풀었고</h1>
            <h1><span
                    style="color: {getDiffColor(opponentData.acRating, hasData.acRating)}"><strong>AC rating {opponentData.acRating}</strong>
                ({getDiffStr(opponentData.acRating, hasData.acRating)})</span></h1>
            <h1>고등학교 <span style="color: {getDiffColor(hasData.hsRank, opponentData.hsRank)}"><strong>
                 {opponentData.hsRank}등</strong>&nbsp;({getDiffStr(hasData.hsRank, opponentData.hsRank)}) </span>입니다.
            </h1>
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
        @apply p-5 ml-10 flex-col justify-center text-5xl text-left leading-snug;
        word-break: keep-all;
    }

    @media (max-width: 1024px) {
        .text-container {
            @apply text-3xl;
        }
    }

    @media (max-width: 768px) {
        .content {
            @apply flex-col;
        }

        .text-container {
            @apply text-2xl;
        }
    }
</style>