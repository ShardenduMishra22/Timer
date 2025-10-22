<script lang="ts">
  let timer = $state(5000);
  let initialTime = $state(5000);
  let isRunning = $state(false);
  let hours = $state(0);
  let minutes = $state(0);
  let seconds = $state(0);
  let milliseconds = $state(0);
  
  $effect(() => {
    if (isRunning && timer > 0) {
      const id = setInterval(() => {
        timer -= 10;
        
        if (timer <= 0) {
          timer = 0;
          isRunning = false;
        }
      }, 10);
      
      return () => clearInterval(id);
    }
  });
  
  function startTimer() {
    if (timer > 0) isRunning = true;
  }
  
  function pauseTimer() {
    isRunning = false;
  }
  
  function resetTimer() {
    isRunning = false;
    timer = initialTime;
  }
  
  // @ts-ignore
  function setTime(ms) {
    isRunning = false;
    timer = ms;
    initialTime = ms;
  }
  
  function calculateTotalMs() {
    return hours * 3600000 + minutes * 60000 + seconds * 1000 + milliseconds;
  }
  
  function formatTime(ms: number) {
    const hours = Math.floor(ms / 3600000);
    const minutes = Math.floor((ms % 3600000) / 60000);
    const seconds = Math.floor((ms % 60000) / 1000);
    const milliseconds = ms % 1000;
    return `${hours}:${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}:${milliseconds.toString().padStart(3, '0')}`;
  }
</script>

<div class="space-y-4">
  <p class="text-2xl font-bold">
    Timer: {formatTime(timer)}
    {#if timer === 0}
      <span class="text-red-600">⏰ Time's Up!</span>
    {/if}
  </p>
  
  <div class="flex gap-2 mb-4">
    <button 
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
      onclick={() => setTime(3000)}
    >
      3s
    </button>
    <button 
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
      onclick={() => setTime(5000)}
    >
      5s
    </button>
    <button 
      class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
      onclick={() => setTime(10000)}
    >
      10s
    </button>
  </div>
  
  <div class="flex gap-2 items-center mb-4">
    <div class="flex flex-col">
      <label for="hours" class="text-sm">Hours</label>
      <input id="hours" type="number" bind:value={hours} min="0" step="1" class="border rounded px-2 py-1 w-16">
    </div>
    <div class="flex flex-col">
      <label for="minutes" class="text-sm">Minutes</label>
      <input id="minutes" type="number" bind:value={minutes} min="0" max="59" step="1" class="border rounded px-2 py-1 w-16">
    </div>
    <div class="flex flex-col">
      <label for="seconds" class="text-sm">Seconds</label>
      <input id="seconds" type="number" bind:value={seconds} min="0" max="59" step="1" class="border rounded px-2 py-1 w-16">
    </div>
    <div class="flex flex-col">
      <label for="milliseconds" class="text-sm">Milliseconds</label>
      <input id="milliseconds" type="number" bind:value={milliseconds} min="0" max="999" step="1" class="border rounded px-2 py-1 w-20">
    </div>
    <button 
      class="bg-purple-500 hover:bg-purple-700 text-white font-bold py-2 px-4 rounded"
      onclick={() => setTime(calculateTotalMs())}
    >
      Set Time
    </button>
  </div>
  
  <div class="flex gap-4">
    <button 
      class="bg-green-500 hover:bg-green-700 text-white font-bold py-2 px-4 rounded"
      onclick={startTimer}
      disabled={isRunning || timer === 0}
    >
      Start
    </button>
    
    <button 
      class="bg-yellow-500 hover:bg-yellow-700 text-white font-bold py-2 px-4 rounded"
      onclick={pauseTimer}
      disabled={!isRunning}
    >
      Pause
    </button>
    
    <button 
      class="bg-red-500 hover:bg-red-700 text-white font-bold py-2 px-4 rounded"
      onclick={resetTimer}
    >
      Reset
    </button>
  </div>
</div>
