<script>
  import "../app.css";

  import {onMount} from "svelte";

  onMount(() => {
    document.getElementById('problems-menu').addEventListener('click', function () {
      document.getElementById('problems-dropdown').toggleAttribute('hidden');
    });
  })
</script>

<div class="flex flex-col min-h-screen">
    <nav class="bg-gray-800">
        <div class="max-w-7xl mx-auto px-2 sm:px-6 lg:px-8">
            <div class="relative flex items-center justify-between h-16">
                <div class="flex-1 flex items-center justify-center sm:items-stretch sm:justify-start">
                    <div class="hidden sm:block sm:ml-6">
                        <div class="flex space-x-4">
                            <a href="/"
                               class="text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium">홈</a>

                            <div class="relative">
                                <button type="button"
                                        class="text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md text-sm font-medium"
                                        id="problems-menu" aria-haspopup="true">
                                    문제 <span>&#9662;</span>
                                </button>
                                <div class="absolute right-0 mt-2 w-48 rounded-md shadow-lg bg-white ring-1 ring-black ring-opacity-5"
                                     id="problems-dropdown" hidden>
                                    <a href="/problem" class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100">모든
                                        문제</a>
                                    <a href="/problem/level"
                                       class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100">레벨별
                                        문제</a>
                                    <a href="/problem/tag"
                                       class="block px-4 py-2 text-sm text-gray-700 hover:bg-gray-100">태그별
                                        문제</a>
                                </div>
                            </div>

                        </div>
                    </div>
                </div>
            </div>
        </div>
    </nav>

    <slot></slot>

    <div class="footer flex p-4 text-sm w-full">
        <div class="footer-section flex-1 text-left">
            <p>All the information of problems, users and icons related to them in this site are borrowed from
                <a style="color: #0076c0;" href="https://www.acmicpc.net/" target="_blank">Baekjoon</a>
                and <a style="color: #17ce3a" href="https://solved.ac/" target="_blank">solved.ac</a>
            </p>
        </div>
        <div class="footer-section flex-1 text-center">
            <p>Contact</p>
            <a style="color: #2dd4ad" href="https://mail.google.com/mail/?view=cm&fs=1&to=has_22121@hana.hs.kr"
               target="_blank">has_22121@hana.hs.kr</a>
        </div>
        <div class="footer-section flex-1 text-right">
            <p>This site simply presents the information gathered from
                <a style="color: #0076c0;" href="https://www.acmicpc.net/" target="_blank">Baekjoon</a> and
                <a style="color: #17ce3a" href="https://solved.ac/" target="_blank">solved.ac</a>
                in an organized manner and is not officially affiliated with them.</p>
        </div>
    </div>
</div>

<style>
    .footer {
        @apply flex text-sm p-4 w-full;
    }

    .footer-section {
        @apply flex-1;
    }

    @media (max-width: 1024px) {
        .footer {
            @apply text-xs p-2 flex-col;
        }

        .footer-section {
            @apply flex-1 text-center mb-2;
        }
    }
</style>