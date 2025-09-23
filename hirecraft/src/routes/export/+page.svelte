<!-- 
  File: src/routes/export/+page.svelte
  Description: This is the new, dedicated page for user feedback and export options.
  This version adds a final portfolio preview next to the feedback controls.
-->

<script>
  import { onMount } from 'svelte';
  import { goto } from '$app/navigation';
  import { fly } from 'svelte/transition';

  // --- STATE MANAGEMENT ---
  let isDarkMode = false;
  let userLikesDesign = null; // null, true, or false
  let feedbackPrompt = '';
  let isRegenerating = false;
  // We'll assume a template is selected for the preview. In a real app, this would be passed from the previous page.
  let selectedTemplate = 'glassmorphic'; 

  // --- MOCK DATA (for download function and preview) ---
  const mockResumeData = {
    name: 'Alex Doe',
    title: 'Full-Stack Developer',
    summary: 'A highly motivated and results-oriented Computer Science student with a passion for building elegant and functional web applications...',
    experience: [
      { role: 'Software Engineer Intern', company: 'Innovatech Solutions', period: 'May 2023 - Aug 2023', description: 'Developed and maintained features for a client-facing web application using React and Node.js.' },
    ],
    email: 'alex.doe@email.com',
    linkedin: 'linkedin.com/in/alexdoe',
    github: 'github.com/alexdoe',
  };

  // --- FUNCTIONS ---
  function handleRegenerate() {
    isRegenerating = true;
    // Simulate AI regeneration based on feedback
    setTimeout(() => {
        isRegenerating = false;
        // After regenerating, send user back to the editor to see the changes
        goto('/portfolio_review');
    }, 2000);
  }

  function downloadHTMLFile() {
      const htmlContent = `<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>${mockResumeData.name}'s Portfolio</title>
    <script src="https://cdn.tailwindcss.com"><\/script>
</head>
<body class="bg-gray-900">
    <h1 class="text-white text-4xl text-center p-12">Portfolio for ${mockResumeData.name}</h1>
    <!-- The full HTML for the selected template would be generated here -->
</body>
</html>`;
      const blob = new Blob([htmlContent], { type: 'text/plain' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = `${mockResumeData.name.toLowerCase().replace(' ', '_')}_portfolio.txt`;
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
      URL.revokeObjectURL(url);
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
    isDarkMode = true;
    document.documentElement.classList.add('dark');
  });
</script>

<div class="abstract-background bg-white dark:bg-gray-950 min-h-screen w-full transition-colors duration-300">
  <main class="relative z-10 font-sans text-gray-900 dark:text-gray-200">
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
        </div>
      </div>
    </header>

    <div class="container mx-auto px-4 sm:px-6 lg:px-8 pt-24 pb-16 md:pt-32 md:pb-24">
      <div class="max-w-6xl mx-auto" in:fly={{ y: 20, duration: 500 }}>
        <div class="text-center mb-12">
          <h2 class="text-4xl font-bold tracking-tight text-gray-900 dark:text-blue-200 sm:text-5xl">Final Step: Feedback & Export</h2>
          <p class="mt-4 text-lg leading-8 text-gray-600 dark:text-gray-400">Confirm your design, then download or deploy your new portfolio.</p>
        </div>

        <div class="grid lg:grid-cols-2 gap-8 lg:gap-12">
          <!-- Left Column: Final Preview -->
          <div>
            <h3 class="font-semibold text-lg text-gray-900 dark:text-white mb-4">Final Preview</h3>
            <div class="bg-gray-800/30 p-4 rounded-xl border border-gray-200 dark:border-gray-800">
              <div class="aspect-[16/10] w-full bg-gray-900 rounded-lg shadow-inner overflow-y-auto">
                <!-- TEMPLATE: Glassmorphic (as an example) -->
                <div class="p-10 md:p-12 font-sans text-gray-300 bg-gray-900/80">
                  <header class="text-center">
                    <h1 class="text-5xl font-bold text-white">{mockResumeData.name}</h1>
                    <p class="text-xl text-blue-400 mt-2">{mockResumeData.title}</p>
                  </header>
                  <main class="mt-12 space-y-10">
                    <section class="bg-black/20 backdrop-blur-md border border-white/10 rounded-lg p-6">
                      <h2 class="text-lg font-semibold text-white uppercase tracking-widest">About</h2>
                      <p class="mt-3 text-gray-400 leading-relaxed">{mockResumeData.summary}</p>
                    </section>
                    <section>
                      <h2 class="text-lg font-semibold text-white uppercase tracking-widest mb-4">Experience</h2>
                      <div class="space-y-6">
                        {#each mockResumeData.experience as job}
                          <div class="bg-black/20 backdrop-blur-md border border-white/10 rounded-lg p-6">
                            <h3 class="font-bold text-blue-400 text-xl">{job.role}</h3>
                            <p class="font-semibold text-gray-400">{job.company} <span class="text-gray-500 font-normal">| {job.period}</span></p>
                            <p class="mt-2 text-gray-400">{job.description}</p>
                          </div>
                        {/each}
                      </div>
                    </section>
                  </main>
                </div>
              </div>
            </div>
          </div>

          <!-- Right Column: Feedback and Export Controls -->
          <div class="text-left p-8 bg-gray-50/50 dark:bg-gray-900/50 backdrop-blur-sm rounded-lg border border-gray-200 dark:border-gray-800 space-y-6">
            <div>
              <p class="font-semibold text-gray-900 dark:text-white">Are you happy with the design?</p>
              <div class="mt-3 grid grid-cols-2 gap-4">
                <button on:click={() => userLikesDesign = true} class="p-4 rounded-lg text-center transition-colors border-2 {userLikesDesign === true ? 'bg-blue-500/10 border-blue-500' : 'border-gray-300 dark:border-gray-700 hover:border-blue-400'}">
                  <span class="text-2xl">👍</span>
                  <p class="font-semibold mt-1 text-gray-800 dark:text-gray-200">Yes, I like it!</p>
                </button>
                <button on:click={() => userLikesDesign = false} class="p-4 rounded-lg text-center transition-colors border-2 {userLikesDesign === false ? 'bg-red-500/10 border-red-500' : 'border-gray-300 dark:border-gray-700 hover:border-red-400'}">
                  <span class="text-2xl">👎</span>
                  <p class="font-semibold mt-1 text-gray-800 dark:text-gray-200">No, I want changes.</p>
                </button>
              </div>
            </div>

            <!-- Conditional Blocks based on user feedback -->
            {#if userLikesDesign === true}
              <div in:fly={{ y: 10, duration: 300 }}>
                <h3 class="font-semibold text-gray-900 dark:text-white">Great! How would you like to proceed?</h3>
                <div class="mt-3 space-y-3">
                  <button on:click={downloadHTMLFile} class="w-full flex items-center justify-center gap-3 p-3 text-sm font-semibold bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700 rounded-lg transition-colors">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" /></svg>
                    Download as HTML
                  </button>
                  <a href="https://app.netlify.com/drop" target="_blank" rel="noopener noreferrer" class="w-full flex items-center justify-center gap-3 p-3 text-sm font-semibold bg-gray-100 dark:bg-gray-800 hover:bg-gray-200 dark:hover:bg-gray-700 rounded-lg transition-colors">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2"><path stroke-linecap="round" stroke-linejoin="round" d="M7 16a4 4 0 01-.88-7.903A5 5 0 1115.9 6L16 6a5 5 0 011 9.9M15 13l-3-3m0 0l-3 3m3-3v12" /></svg>
                    Deploy with Netlify
                  </a>
                </div>
              </div>
            {/if}

            {#if userLikesDesign === false}
              <div in:fly={{ y: 10, duration: 300 }}>
                <h3 class="font-semibold text-gray-900 dark:text-white">What changes would you like to see?</h3>
                <p class="text-sm text-gray-500 dark:text-gray-400">Describe the changes and we'll regenerate the portfolio for you.</p>
                <textarea bind:value={feedbackPrompt} rows="4" class="mt-2 w-full p-2 text-sm bg-white dark:bg-gray-900/80 border border-gray-300 dark:border-gray-700 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition" placeholder="e.g., Use a different color for the headings, make the about section shorter..."></textarea>
                <button on:click={handleRegenerate} disabled={isRegenerating} class="w-full mt-3 px-4 py-2 text-sm font-semibold bg-blue-600 text-white rounded-md hover:bg-blue-700 transition-colors flex items-center justify-center">
                  {#if isRegenerating}
                    <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path></svg>
                    Regenerating...
                  {:else}
                    Submit Feedback & Regenerate
                  {/if}
                </button>
              </div>
            {/if}
          </div>
        </div>
      </div>
    </div>
  </main>
</div>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap');
  
  :global(html) {
    font-family: 'Inter', sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
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
