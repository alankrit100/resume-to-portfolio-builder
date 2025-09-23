<!-- 
  File: src/routes/+page.svelte
  Description: This is the homepage, containing only the file upload UI.
  Upon successful upload, it navigates to the /portfolio_review page.
-->

<script>
  import { onMount } from 'svelte';
  import { quintOut } from 'svelte/easing';
  import { fly, fade } from 'svelte/transition';
  import { goto } from '$app/navigation';

  // --- STATE MANAGEMENT ---
  let resumeFile = null;
  let isUploading = false;
  let isGenerating = false;
  let isDarkMode = false;

  // --- FUNCTIONS ---
  function handleFileSelect(e) {
    resumeFile = e.target.files[0];
    if (resumeFile) {
      isUploading = true;
      setTimeout(() => {
        isUploading = false;
        isGenerating = true;
        setTimeout(() => {
          isGenerating = false;
          // Navigate to the portfolio editor page
          goto('/portfolio_review'); 
        }, 2500);
      }, 1500);
    }
  }

  function toggleTheme() {
    isDarkMode = !isDarkMode;
    if (isDarkMode) {
      document.documentElement.classList.add('dark');
    } else {
      document.documentElement.classList.remove('dark');
    }
  }
  
  onMount(() => {
    // Default to dark mode for the app's theme
    isDarkMode = true;
    document.documentElement.classList.add('dark');
  });

</script>

<div class="abstract-background bg-white dark:bg-gray-950 min-h-screen w-full transition-colors duration-300">
  <main class="relative z-10 font-sans text-gray-900 dark:text-gray-200">
    <!-- ====================================================================== -->
    <!-- COMPONENT: Navbar.svelte                                               -->
    <!-- ====================================================================== -->
    <header class="bg-white/80 dark:bg-gray-950/80 backdrop-blur-lg sticky top-0 z-50 border-b border-gray-200 dark:border-b-gray-600">
      <div class="container mx-auto px-4 sm:px-6 lg:px-8">
        <div class="flex items-center justify-between h-16">
          <div class="flex items-center space-x-2.5">
             <svg class="h-6 w-6 text-gray-900 dark:text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="square" stroke-linejoin="round" d="M9 12h3.75M9 15h3.75M9 18h3.75m3 .75H18a2.25 2.25 0 002.25-2.25V6.108c0-1.135-.845-2.098-1.976-2.192a48.424 48.424 0 00-1.123-.08m-5.801 0c-.065.21-.1.433-.1.664 0 .414.336.75.75.75h4.5a.75.75 0 00.75-.75 2.25 2.25 0 00-.1-.664m-5.8 0A2.251 2.251 0 0113.5 2.25H15c1.012 0 1.867.668 2.15 1.586m-5.8 0c-.376.023-.75.05-1.124.08C9.095 4.01 8.25 4.973 8.25 6.108V8.25m0 0H4.875c-.621 0-1.125.504-1.125 1.125v11.25c0 .621.504 1.125 1.125 1.125h9.75c.621 0 1.125-.504 1.125-1.125V9.375c0-.621-.504-1.125-1.125-1.125H8.25zM6.75 12h.008v.008H6.75V12zm0 3h.008v.008H6.75V15zm0 3h.008v.008H6.75V18z" /></svg>
            <h1 class="text-lg font-semibold tracking-tight text-gray-900 dark:text-blue-300">Portfolio Generator</h1>
          </div>
          <nav class="hidden md:flex items-center space-x-8">
            <a href="/" class="text-sm font-medium text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-blue-300 transition-colors">Home</a>
            <a href="/portfolio_review" class="text-sm font-medium text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-blue-300 transition-colors">Templates</a>
            <a href="/" class="text-sm font-medium text-gray-600 dark:text-gray-400 hover:text-gray-900 dark:hover:text-blue-300 transition-colors">GitHub</a>
          </nav>
          <div class="flex items-center">
             <button on:click={toggleTheme} class="p-2 rounded-md text-gray-500 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors"><svg class:hidden={isDarkMode} class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M21.752 15.002A9.718 9.718 0 0118 15.75c-5.385 0-9.75-4.365-9.75-9.75 0-1.33.266-2.597.748-3.752A9.753 9.753 0 003 11.25C3 16.635 7.365 21 12.75 21a9.753 9.753 0 009.002-5.998z" /></svg><svg class:hidden={!isDarkMode} class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M12 3v2.25m6.364.386l-1.591 1.591M21 12h-2.25m-.386 6.364l-1.591-1.591M12 18.75V21m-4.773-4.227l-1.591 1.591M5.25 12H3m4.227-4.773L5.636 5.636M15.75 12a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0z" /></svg></button>
          </div>
        </div>
      </div>
    </header>

    <div class="container mx-auto px-4 sm:px-6 lg:px-8 pt-24 pb-16 md:pt-32 md:pb-24">
      <!-- ====================================================================== -->
      <!-- SECTION: UPLOAD PAGE                                                   -->
      <!-- ====================================================================== -->
      <div class="max-w-2xl mx-auto text-center" in:fly={{ y: 20, duration: 500, easing: quintOut }}>
        <h2 class="text-4xl font-bold tracking-tight text-gray-900 dark:text-blue-200 sm:text-5xl">Build Your Professional Portfolio, Instantly!</h2>
        <p class="mt-6 text-lg leading-8 text-gray-600 dark:text-gray-400">Upload your resume and sit back while we create a sleek portfolio for you!!</p>
        <div class="mt-16 max-w-xl mx-auto">
          <div class="bg-gray-50/50 dark:bg-gray-900/50 backdrop-blur-sm rounded-lg border border-gray-200 dark:border-gray-800">
            <div class="p-6 md:p-8">
              <label for="resume-upload" class="relative block w-full p-10 text-center cursor-pointer bg-white/80 dark:bg-gray-900/60 rounded-md border-2 border-dashed border-gray-300 dark:border-gray-700/60 hover:border-blue-300 dark:hover:border-blue-300 transition-colors duration-100">
                <input type="file" id="resume-upload" class="absolute inset-0 w-full h-full opacity-0" accept=".pdf" on:change={handleFileSelect} disabled={isUploading || isGenerating} />
                <div class="flex flex-col items-center justify-center space-y-3">
                  <svg class="h-10 w-10 text-gray-400 dark:text-gray-500" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M12 16.5V9.75m0 0l-3.75 3.75M12 9.75l3.75 3.75M3 17.25V6.75A2.25 2.25 0 015.25 4.5h13.5A2.25 2.25 0 0121 6.75v10.5A2.25 2.25 0 0118.75 21H5.25A2.25 2.25 0 013 17.25z" /></svg>
                  <p class="text-sm font-semibold text-gray-700 dark:text-gray-300"><span class="text-blue-600 dark:text-blue-500">Click to upload</span> or drag and drop</p>
                  <p class="text-xs text-gray-500 dark:text-gray-400">PDF (MAX. 5MB)</p>
                </div>
              </label>
            </div>
            <div class="h-14 px-6 flex items-center justify-center">
              {#if isUploading}
                <div class="flex items-center space-x-2" transition:fade><div class="w-4 h-4 border-2 border-t-blue-600 border-r-blue-600 border-b-blue-600 border-l-transparent rounded-full animate-spin"></div><span class="text-sm text-gray-500 dark:text-gray-400">Uploading...</span></div>
              {:else if isGenerating}
                <div class="flex items-center space-x-2" transition:fade><div class="w-4 h-4 border-2 border-t-blue-600 border-r-blue-600 border-b-blue-600 border-l-transparent rounded-full animate-spin"></div><span class="text-sm text-gray-500 dark:text-gray-400">Generating portfolio...</span></div>
              {/if}
            </div>
          </div>
        </div>
      </div>
    </div>
  </main>
</div>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Lora:wght@400;700&display=swap');
  
  :global(html) {
    font-family: 'Inter', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }
  
  .font-serif {
    font-family: 'Lora', serif;
  }

  .abstract-background {
    position: relative;
    overflow-x: hidden;
  }

  .abstract-background::before {
    content: '';
    position: absolute;
    inset: 0;
    z-index: 0;
    background-image: url("data:image/svg+xml,%3Csvg width='100' height='100' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M0 50 Q 25 25, 50 50 T 100 50' stroke='%23e5e7eb' fill='none'/%3E%3C/svg%3E");
    background-size: 70px 70px;
    opacity: 0.5;
  }

  :global(html.dark) .abstract-background::before {
    background-image: url("data:image/svg+xml,%3Csvg width='100' height='100' xmlns='http://www.w3.org/2000/svg'%3E%3Cpath d='M0 50 Q 25 25, 50 50 T 100 50' stroke='%23374151' fill='none'/%3E%3C/svg%3E");
    opacity: .8;
  }
</style>
