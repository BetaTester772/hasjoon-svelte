<script>
  import "../app.css";

  import {onMount} from "svelte";

  onMount(() => {
    const problemsMenu = document.getElementById('problems-menu');
    const dropdown = document.getElementById('problems-dropdown');
    problemsMenu.addEventListener('click', function () {
      dropdown.toggleAttribute('hidden');
    });

    // 드롭다운 메뉴의 모든 링크에 대해 클릭 이벤트 핸들러 추가
    const nav_items = document.getElementsByClassName('relative')[0];
    const dropdownLinks = nav_items.querySelectorAll('a');
    dropdownLinks.forEach(link => {
      link.addEventListener('click', () => {
        dropdown.setAttribute('hidden', true);
      });
    });
  });
</script>

<div class="main-font-family flex flex-col min-h-screen">
    <nav>
        <div class="max-w-7xl mx-auto px-2 sm:px-6 lg:px-8">
            <div class="relative flex items-center justify-between h-16">
                <div class="flex-1 flex items-center justify-center sm:items-stretch sm:justify-start">
                    <div class="sm:block sm:ml-6">
                        <div class="flex space-x-4 text-base">
                            <a href="/"> <img src="/hasjoon_logo_white.png" alt="hasjoon logo" class="h-10 w-auto"></a>
                            <a href="/"
                               class="text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md font-medium">홈</a>

                            <div class="relative">
                                <button type="button"
                                        class="text-gray-300 hover:bg-gray-700 hover:text-white px-3 py-2 rounded-md font-medium"
                                        id="problems-menu" aria-haspopup="true">
                                    문제 <span>&#9662;</span>
                                </button>
                                <div class="absolute right-0 mt-2 w-48 rounded-md shadow-lg bg-white ring-1 ring-black ring-opacity-5 z-50"
                                     id="problems-dropdown" hidden>
<!--                                    TODO: make this page someday-->
<!--                                    <a href="/problem" class="block px-4 py-2 text-gray-700 hover:bg-gray-100">모든-->
<!--                                        문제</a>-->
                                    <a href="/problem/level"
                                       class="block px-4 py-2 text-gray-700 hover:bg-gray-100">레벨별
                                        문제</a>
                                    <a href="/problem/tag/1"
                                       class="block px-4 py-2 text-gray-700 hover:bg-gray-100">태그별
                                        문제</a>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </nav>
    <div class="flex-grow flex overflow-x-auto">
        <slot></slot>
    </div>

    <div class="footer flex p-4 text-sm w-full bg-gray-50">
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
    nav {
        background-color: #00664e;
    }

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

    .main-font-family {
        font-family: "Pretendard Variable", Pretendard, -apple-system, BlinkMacSystemFont, system-ui, Roboto, "Helvetica Neue", "Segoe UI", "Apple SD Gothic Neo", "Noto Sans KR", "Malgun Gothic", "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", sans-serif;
    }
</style>