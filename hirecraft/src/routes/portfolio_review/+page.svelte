<!-- 
  File: src/routes/portfolio_review/+page.svelte
  Description: This is the dedicated portfolio editor page. The "Export" button
  now navigates to the new /export page. All modal logic has been removed.
-->

<script>
  import { onMount } from 'svelte';
  import { quintOut } from 'svelte/easing';
  import { fly, fade } from 'svelte/transition';
  import { goto } from '$app/navigation';

  // --- MOCK DATA ---
  const mockResumeData = {
    name: 'Alex Doe',
    title: 'Full-Stack Developer',
    location: 'San Francisco, CA',
    email: 'alex.doe@email.com',
    linkedin: 'linkedin.com/in/alexdoe',
    github: 'github.com/alexdoe',
    summary: 'A highly motivated and results-oriented Computer Science student with a passion for building elegant and functional web applications. Proficient in JavaScript, Python, and modern frameworks like React and Svelte. Eager to apply my skills in a challenging and collaborative environment.',
    experience: [
      {
        role: 'Software Engineer Intern',
        company: 'Innovatech Solutions',
        period: 'May 2023 - Aug 2023',
        description: 'Developed and maintained features for a client-facing web application using React and Node.js. Collaborated with a team of 5 engineers to ship code on a weekly basis.'
      },
      {
        role: 'Web Dev TA',
        company: 'State University',
        period: 'Jan 2023 - May 2023',
        description: 'Assisted students with coursework, graded assignments, and led lab sessions for an introductory web development course.'
      }
    ],
    education: {
      degree: 'B.S. in Computer Science',
      university: 'State University',
      period: '2021 - 2025'
    },
    projects: [
      {
        name: 'AI Portfolio Generator',
        description: 'A web app that uses AI to generate personal portfolio websites from a resume PDF. Built with SvelteKit, TailwindCSS, and a Python backend.',
        tech: ['SvelteKit', 'TailwindCSS', 'Python']
      },
      {
        name: 'Task Manager App',
        description: 'A simple and intuitive task management application to help users stay organized and productive.',
        tech: ['React', 'Node.js', 'MongoDB']
      }
    ],
    skills: ['JavaScript', 'TypeScript', 'React', 'Svelte', 'Node.js', 'Python', 'SQL', 'Docker', 'Git']
  };

  // --- STATE MANAGEMENT ---
  let isDarkMode = false;
  let selectedTemplate = 'glassmorphic'; 
  let userPrompt = ''; 
  let isRegenerating = false;

  // --- TEMPLATE DATA ---
  const templates = [
    { id: 'glassmorphic', name: 'Glass' },
    { id: 'gradient', name: 'Gradient' },
    { id: 'monochrome', name: 'Mono' }
  ];

  // --- FUNCTIONS ---
  function startOver() {
    goto('/');
  }

  function handleRegenerate() {
    isRegenerating = true;
    // Simulate AI regeneration
    setTimeout(() => {
        isRegenerating = false;
    }, 1500);
  }

  function handleExport() {
    // Navigate to the new export page
    goto('/export');
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
          <div class="flex items-center">
             <button on:click={toggleTheme} class="p-2 rounded-md text-gray-500 dark:text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-800 transition-colors"><svg class:hidden={isDarkMode} class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M21.752 15.002A9.718 9.718 0 0118 15.75c-5.385 0-9.75-4.365-9.75-9.75 0-1.33.266-2.597.748-3.752A9.753 9.753 0 003 11.25C3 16.635 7.365 21 12.75 21a9.753 9.753 0 009.002-5.998z" /></svg><svg class:hidden={!isDarkMode} class="h-5 w-5" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" d="M12 3v2.25m6.364.386l-1.591 1.591M21 12h-2.25m-.386 6.364l-1.591-1.591M12 18.75V21m-4.773-4.227l-1.591 1.591M5.25 12H3m4.227-4.773L5.636 5.636M15.75 12a3.75 3.75 0 11-7.5 0 3.75 3.75 0 017.5 0z" /></svg></button>
          </div>
        </div>
      </div>
    </header>

    <div class="container mx-auto px-4 sm:px-6 lg:px-8 pt-24 pb-16 md:pt-32 md:pb-24">
      <div class="max-w-7xl mx-auto" in:fade={{ duration: 500 }}>
        <div class="lg:grid lg:grid-cols-12 lg:gap-8">
          <aside class="lg:col-span-3">
            <div class="sticky top-24 space-y-6">
              <div>
                <h2 class="text-xl font-bold text-gray-900 dark:text-blue-200">Portfolio Editor</h2>
                <p class="text-sm text-gray-500 dark:text-gray-400 mt-1">Refine your portfolio with a prompt.</p>
              </div>
              <div class="space-y-2">
                <label class="text-sm font-semibold text-gray-700 dark:text-gray-300">Template</label>
                <div class="grid grid-cols-3 gap-2">
                  {#each templates as template}
                    <button on:click={() => selectedTemplate = template.id} class="text-center px-3 py-2 rounded-md text-sm font-semibold transition-all border" class:bg-white={selectedTemplate === template.id} class:dark:bg-gray-800={selectedTemplate === template.id} class:text-blue-600={selectedTemplate === template.id} class:dark:text-blue-300={selectedTemplate === template.id} class:border-blue-500={selectedTemplate === template.id} class:dark:border-blue-400={selectedTemplate === template.id} class:text-gray-600={selectedTemplate !== template.id} class:dark:text-gray-400={selectedTemplate !== template.id} class:border-gray-200={selectedTemplate !== template.id} class:dark:border-gray-700={selectedTemplate !== template.id} class:hover:border-blue-400={selectedTemplate !== template.id} class:dark:hover:border-blue-400={selectedTemplate !== template.id} class:shadow-sm={selectedTemplate === template.id}>{template.name}</button>
                  {/each}
                </div>
              </div>
              <div class="space-y-2">
                  <label for="prompt" class="text-sm font-semibold text-gray-700 dark:text-gray-300">Prompt</label>
                  <textarea bind:value={userPrompt} id="prompt" rows="4" class="w-full p-2 text-sm bg-white dark:bg-gray-900/80 border border-gray-300 dark:border-gray-700 rounded-md focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition" placeholder="e.g., Make it professional with a dark theme..."></textarea>
              </div>
              <div class="space-y-3 border-t border-gray-200 dark:border-gray-700 pt-6">
                  <button on:click={handleRegenerate} disabled={isRegenerating} class="w-full px-4 py-2 text-sm font-semibold bg-gray-600 text-white rounded-md hover:bg-gray-700 transition-colors flex items-center justify-center">
                    {#if isRegenerating}
                      <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24"><circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle><path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path></svg>
                      Regenerating...
                    {:else}
                      Regenerate
                    {/if}
                  </button>
                  <button on:click={handleExport} class="w-full px-4 py-2 text-sm font-semibold bg-blue-600 text-white rounded-md hover:bg-blue-700 transition-colors shadow">Export</button>
                  <button on:click={startOver} class="w-full text-sm font-semibold text-gray-600 hover:text-gray-800 dark:text-gray-400 dark:hover:text-white transition-colors">Start Over</button>
              </div>
            </div>
          </aside>

          <main class="lg:col-span-9 mt-8 lg:mt-0">
            <div class="bg-gray-800/30 p-4 rounded-xl border border-gray-200 dark:border-gray-800">
              <div class="aspect-[16/10] w-full bg-gray-900 rounded-lg shadow-inner overflow-y-auto">
                {#if selectedTemplate === 'glassmorphic'}
                  <div class="p-10 md:p-12 font-sans text-gray-300 bg-gray-900/80"><header class="text-center"><h1 class="text-5xl font-bold text-white">{mockResumeData.name}</h1><p class="text-xl text-blue-400 mt-2">{mockResumeData.title}</p></header><main class="mt-12 space-y-10"><section class="bg-black/20 backdrop-blur-md border border-white/10 rounded-lg p-6"><h2 class="text-lg font-semibold text-white uppercase tracking-widest">About</h2><p class="mt-3 text-gray-400 leading-relaxed">{mockResumeData.summary}</p></section><section><h2 class="text-lg font-semibold text-white uppercase tracking-widest mb-4">Experience</h2><div class="space-y-6">{#each mockResumeData.experience as job}<div class="bg-black/20 backdrop-blur-md border border-white/10 rounded-lg p-6"><h3 class="font-bold text-blue-400 text-xl">{job.role}</h3><p class="font-semibold text-gray-400">{job.company} <span class="text-gray-500 font-normal">| {job.period}</span></p><p class="mt-2 text-gray-400">{job.description}</p></div>{/each}</div></section></main></div>
                {/if}
                {#if selectedTemplate === 'gradient'}
                  <div class="p-10 md:p-12 font-sans bg-gray-900"><header class="text-center"><h1 class="text-6xl font-extrabold bg-gradient-to-r from-purple-400 to-blue-400 text-transparent bg-clip-text">{mockResumeData.name}</h1><p class="text-xl text-gray-400 mt-2">{mockResumeData.title}</p></header><main class="mt-12 space-y-10"><div><h2 class="text-2xl font-bold text-white">About Me</h2><p class="mt-3 text-gray-400 leading-relaxed border-l-4 border-blue-500 pl-4">{mockResumeData.summary}</p></div><div><h2 class="text-2xl font-bold text-white">Experience</h2><div class="mt-4 space-y-6">{#each mockResumeData.experience as job}<div><h3 class="font-semibold text-xl text-white">{job.role}</h3><p class="text-gray-500">{job.company} | {job.period}</p><p class="mt-2 text-gray-400">{job.description}</p></div>{/each}</div></div><div><h2 class="text-2xl font-bold text-white">Skills</h2><div class="mt-4 flex flex-wrap gap-3">{#each mockResumeData.skills as skill}<span class="bg-gradient-to-r from-purple-500 to-blue-500 text-white text-sm font-medium px-4 py-1.5 rounded-full">{skill}</span>{/each}</div></div></main></div>
                {/if}
                {#if selectedTemplate === 'monochrome'}
                  <div class="p-10 md:p-12 font-sans bg-black text-gray-400"><header class="text-center pb-10 border-b border-gray-800"><h1 class="text-5xl font-semibold text-white">{mockResumeData.name}</h1><p class="text-xl text-gray-500 mt-2 tracking-widest">{mockResumeData.title}</p></header><main class="mt-10 grid md:grid-cols-2 gap-10"><section><h2 class="text-lg font-medium text-white uppercase tracking-wider">About</h2><p class="mt-3 text-gray-500 leading-relaxed">{mockResumeData.summary}</p></section><section><h2 class="text-lg font-medium text-white uppercase tracking-wider">Contact</h2><div class="mt-3 space-y-1 text-gray-500"><p>{mockResumeData.email}</p><p>{mockResumeData.linkedin}</p><p>{mockResumeData.github}</p></div></section><section class="md:col-span-2"><h2 class="text-lg font-medium text-white uppercase tracking-wider">Experience</h2><div class="mt-4 border-t border-gray-800">{#each mockResumeData.experience as job, i}<div class="py-4 {i > 0 ? 'border-t border-gray-800' : ''}"><h3 class="font-semibold text-lg text-white">{job.role}</h3><p class="text-sm text-gray-600">{job.company} | {job.period}</p></div>{/each}</div></section></main></div>
                {/if}
              </div>
            </div>
          </main>
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
