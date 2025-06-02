<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Neon Pulse Timer with Sound</title>

  <script src="https://cdn.tailwindcss.com"></script>
  <link
    rel="stylesheet"
    href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css"
  />
  <style>
    @import url("https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700&display=swap");

    :root {
      --neon-blue: #08f;
      --neon-pink: #f0f;
      --neon-purple: #90f;
      --neon-green: #0f0;
    }

    body {
      font-family: "Orbitron", sans-serif;
      background: linear-gradient(135deg, #0a0a20 0%, #1a1a30 100%);
      color: white;
      overflow-x: hidden;
      margin: 0;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 1.5rem;
    }

    .neon-text {
      text-shadow: 0 0 6px var(--neon-blue), 0 0 12px var(--neon-blue),
        0 0 30px var(--neon-purple);
    }

    .neon-border {
      box-shadow: 0 0 8px var(--neon-blue), 0 0 15px var(--neon-blue),
        0 0 30px var(--neon-purple), inset 0 0 8px var(--neon-blue),
        inset 0 0 15px var(--neon-blue), inset 0 0 30px var(--neon-purple);
      border: 1px solid var(--neon-blue);
    }

    .pulse {
      animation: pulse 3s infinite ease-in-out alternate;
    }

    @keyframes pulse {
      0% {
        box-shadow: 0 0 8px var(--neon-blue), 0 0 15px var(--neon-blue);
      }
      100% {
        box-shadow: 0 0 25px var(--neon-blue), 0 0 45px var(--neon-blue),
          0 0 60px var(--neon-purple);
      }
    }

    .timer-digit {
      position: relative;
      width: 2.8ch;
      height: 3.8rem;
      overflow: hidden;
      display: inline-block;
      vertical-align: middle;
      font-weight: 700;
    }

    .digit-wrapper {
      position: absolute;
      width: 100%;
      left: 0;
      top: 0;
      transition: transform 0.4s cubic-bezier(0.4, 0, 0.2, 1);
      text-align: center;
    }

    .digit-container {
      position: relative;
      height: 3.8rem;
      overflow: hidden;
      display: inline-block;
      width: 2.8ch;
    }

    .timer-digit span {
      display: block;
      font-size: 3.5rem;
      line-height: 3.8rem;
      color: var(--neon-blue);
      text-shadow: 0 0 8px var(--neon-blue), 0 0 20px var(--neon-purple);
      user-select: none;
    }

    .colon {
      font-size: 3.5rem;
      line-height: 3.8rem;
      color: var(--neon-blue);
      text-shadow: 0 0 8px var(--neon-blue), 0 0 20px var(--neon-purple);
      user-select: none;
      margin: 0 0.1rem;
    }

    .progress-ring {
      transform: rotate(-90deg);
      width: 100%;
      height: 100%;
      overflow: visible;
    }

    .progress-ring__circle {
      fill: transparent;
      stroke-linecap: round;
      transition: stroke-dashoffset 0.5s ease;
      transform-origin: 50% 50%;
    }

    .glow {
      filter: drop-shadow(0 0 10px var(--neon-blue));
    }

    .particle {
      position: absolute;
      background-color: var(--neon-blue);
      border-radius: 50%;
      pointer-events: none;
      z-index: -1;
      opacity: 0.85;
      box-shadow: 0 0 8px var(--neon-blue), 0 0 12px var(--neon-blue);
      will-change: transform, opacity;
    }

    input[type="number"] {
      appearance: textfield;
      -moz-appearance: textfield;
      -webkit-appearance: textfield;
    }
  </style>
</head>
<body>
  <div
    class="absolute top-0 left-0 w-full h-full overflow-hidden -z-10"
    id="particles"
  ></div>

  <div class="text-center mb-8">
    <h1 class="text-4xl md:text-6xl font-bold neon-text mb-2">
      NEON PULSE TIMER
    </h1>
    <p class="text-lg md:text-xl text-gray-300">Count every moment with style</p>
  </div>

  <div
    class="w-full max-w-md bg-black bg-opacity-50 rounded-2xl p-8 neon-border pulse mb-8"
  >
    <div class="flex justify-center items-center mb-8 relative w-64 h-64 mx-auto">
      <svg class="progress-ring w-full h-full" viewBox="0 0 100 100" aria-hidden="true">
        <circle
          class="progress-ring__circle text-blue-900 opacity-30"
          stroke-width="4"
          r="40"
          cx="50"
          cy="50"
        ></circle>
        <circle
          id="progress-circle"
          class="progress-ring__circle text-blue-500 glow"
          stroke-width="4"
          stroke-linecap="round"
          r="40"
          cx="50"
          cy="50"
          stroke-dasharray="251.2"
          stroke-dashoffset="251.2"
        ></circle>
      </svg>

      <div
        class="absolute inset-0 flex flex-col items-center justify-center select-none"
        aria-live="polite"
      >
        <div
          id="timer-display"
          class="text-5xl md:text-7xl font-bold neon-text flex"
          aria-label="Timer"
        >
          <div class="digit-container" aria-hidden="true">
            <div class="digit-wrapper" id="hours-digit-wrapper" style="transform: translateY(0);">
              <span id="hours-digit-top">00</span>
              <span id="hours-digit-bottom" style="display:none;">00</span>
            </div>
          </div>
          <span class="colon">:</span>
          <div class="digit-container" aria-hidden="true">
            <div class="digit-wrapper" id="minutes-digit-wrapper" style="transform: translateY(0);">
              <span id="minutes-digit-top">00</span>
              <span id="minutes-digit-bottom" style="display:none;">00</span>
            </div>
          </div>
          <span class="colon">:</span>
          <div class="digit-container" aria-hidden="true">
            <div class="digit-wrapper" id="seconds-digit-wrapper" style="transform: translateY(0);">
              <span id="seconds-digit-top">00</span>
              <span id="seconds-digit-bottom" style="display:none;">00</span>
            </div>
          </div>
        </div>
        <div
          class="text-lg text-gray-300 mt-2"
          id="timer-state"
          aria-live="polite"
          aria-atomic="true"
          >Ready</div
        >
      </div>
    </div>

    <div class="grid grid-cols-3 gap-4 mb-6">
      <div>
        <label
          for="hours-input"
          class="block text-sm text-gray-300 mb-1"
          >Hours</label
        >
        <input
          type="number"
          id="hours-input"
          min="0"
          max="23"
          value="0"
          class="w-full bg-gray-900 bg-opacity-50 text-white rounded-lg p-2 text-center neon-border focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
      </div>
      <div>
        <label
          for="minutes-input"
          class="block text-sm text-gray-300 mb-1"
          >Minutes</label
        >
        <input
          type="number"
          id="minutes-input"
          min="0"
          max="59"
          value="0"
          class="w-full bg-gray-900 bg-opacity-50 text-white rounded-lg p-2 text-center neon-border focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
      </div>
      <div>
        <label
          for="seconds-input"
          class="block text-sm text-gray-300 mb-1"
          >Seconds</label
        >
        <input
          type="number"
          id="seconds-input"
          min="0"
          max="59"
          value="0"
          class="w-full bg-gray-900 bg-opacity-50 text-white rounded-lg p-2 text-center neon-border focus:outline-none focus:ring-2 focus:ring-blue-500"
        />
      </div>
    </div>

    <div class="flex justify-center gap-4">
      <button
        id="start-btn"
        class="bg-blue-700 hover:bg-blue-800 px-6 py-2 rounded-full font-semibold tracking-wider neon-border transition"
      >
        <i class="fa-solid fa-play"></i> Start
      </button>
      <button
        id="pause-btn"
        class="bg-yellow-700 hover:bg-yellow-800 px-6 py-2 rounded-full font-semibold tracking-wider neon-border transition"
        disabled
      >
        <i class="fa-solid fa-pause"></i> Pause
      </button>
      <button
        id="reset-btn"
        class="bg-red-700 hover:bg-red-800 px-6 py-2 rounded-full font-semibold tracking-wider neon-border transition"
        disabled
      >
        <i class="fa-solid fa-rotate-left"></i> Reset
      </button>
    </div>
  </div>

  <!-- Sound Effects -->
  <audio id="tick-sound" src="https://actions.google.com/sounds/v1/alarms/beep_short.ogg" preload="auto"></audio>
  <audio id="bell-sound" src="https://actions.google.com/sounds/v1/alarms/alarm_clock.ogg" preload="auto"></audio>

  <script>
    const hoursInput = document.getElementById("hours-input");
    const minutesInput = document.getElementById("minutes-input");
    const secondsInput = document.getElementById("seconds-input");

    const startBtn = document.getElementById("start-btn");
    const pauseBtn = document.getElementById("pause-btn");
    const resetBtn = document.getElementById("reset-btn");

    const timerState = document.getElementById("timer-state");

    const hoursWrapper = document.getElementById("hours-digit-wrapper");
    const minutesWrapper = document.getElementById("minutes-digit-wrapper");
    const secondsWrapper = document.getElementById("seconds-digit-wrapper");

    const hoursTop = document.getElementById("hours-digit-top");
    const hoursBottom = document.getElementById("hours-digit-bottom");

    const minutesTop = document.getElementById("minutes-digit-top");
    const minutesBottom = document.getElementById("minutes-digit-bottom");

    const secondsTop = document.getElementById("seconds-digit-top");
    const secondsBottom = document.getElementById("seconds-digit-bottom");

    const progressCircle = document.getElementById("progress-circle");
    const radius = progressCircle.r.baseVal.value;
    const circumference = 2 * Math.PI * radius;
    progressCircle.style.strokeDasharray = circumference;
    progressCircle.style.strokeDashoffset = circumference;

    const particlesContainer = document.getElementById("particles");

    // Sound elements
    const tickSound = document.getElementById("tick-sound");
    const bellSound = document.getElementById("bell-sound");

    let totalSeconds = 0;
    let remainingSeconds = 0;
    let timerInterval = null;
    let isRunning = false;

    function format(num) {
      return num.toString().padStart(2, "0");
    }

    function animateDigit(wrapper, topSpan, bottomSpan, newValue) {
      if (topSpan.textContent === newValue) return;

      bottomSpan.textContent = newValue;
      bottomSpan.style.display = "block";

      wrapper.style.transition = "transform 0.4s cubic-bezier(0.4, 0, 0.2, 1)";
      wrapper.style.transform = "translateY(-100%)";

      setTimeout(() => {
        topSpan.textContent = newValue;
        wrapper.style.transition = "none";
        wrapper.style.transform = "translateY(0)";
        bottomSpan.style.display = "none";
      }, 400);
    }

    function updateDisplay(hours, minutes, seconds) {
      animateDigit(hoursWrapper, hoursTop, hoursBottom, format(hours));
      animateDigit(minutesWrapper, minutesTop, minutesBottom, format(minutes));
      animateDigit(secondsWrapper, secondsTop, secondsBottom, format(seconds));
    }

    function updateProgress() {
      if (totalSeconds === 0) {
        progressCircle.style.strokeDashoffset = circumference;
        return;
      }
      const offset = circumference * (1 - remainingSeconds / totalSeconds);
      progressCircle.style.strokeDashoffset = offset;
    }

    class Particle {
      constructor() {
        this.size = Math.random() * 4 + 2;
        this.x = Math.random() * window.innerWidth;
        this.y = Math.random() * window.innerHeight;
        this.vx = (Math.random() - 0.5) * 0.3;
        this.vy = (Math.random() - 0.5) * 0.3;
        this.el = document.createElement("div");
        this.el.className = "particle";
        this.el.style.width = this.size + "px";
        this.el.style.height = this.size + "px";
        this.el.style.left = this.x + "px";
        this.el.style.top = this.y + "px";
        particlesContainer.appendChild(this.el);
      }

      move() {
        this.x += this.vx;
        this.y += this.vy;

        if (this.x < 0 || this.x > window.innerWidth) this.vx = -this.vx;
        if (this.y < 0 || this.y > window.innerHeight) this.vy = -this.vy;

        this.el.style.transform = `translate(${this.x}px, ${this.y}px)`;
      }
    }

    const particles = [];
    for (let i = 0; i < 40; i++) {
      particles.push(new Particle());
    }

    function animateParticles() {
      particles.forEach((p) => p.move());
      requestAnimationFrame(animateParticles);
    }
    animateParticles();

    function startTimer() {
      if (isRunning) return;

      const h = Math.min(Math.max(parseInt(hoursInput.value) || 0, 0), 23);
      const m = Math.min(Math.max(parseInt(minutesInput.value) || 0, 0), 59);
      const s = Math.min(Math.max(parseInt(secondsInput.value) || 0, 0), 59);

      totalSeconds = h * 3600 + m * 60 + s;
      if (totalSeconds === 0) {
        timerState.textContent = "Please set a time greater than 0.";
        return;
      }
      remainingSeconds = totalSeconds;

      hoursInput.disabled = true;
      minutesInput.disabled = true;
      secondsInput.disabled = true;

      startBtn.disabled = true;
      pauseBtn.disabled = false;
      resetBtn.disabled = false;

      timerState.textContent = "Running...";
      isRunning = true;

      updateDisplay(h, m, s);
      updateProgress();

      timerInterval = setInterval(() => {
        remainingSeconds--;
        if (remainingSeconds < 0) {
          clearInterval(timerInterval);
          timerState.textContent = "Time's up!";
          isRunning = false;
          startBtn.disabled = false;
          pauseBtn.disabled = true;
          resetBtn.disabled = false;
          updateProgress();

          // Play bell sound on end
          bellSound.currentTime = 0;
          bellSound.play();

          return;
        }

        const rh = Math.floor(remainingSeconds / 3600);
        const rm = Math.floor((remainingSeconds % 3600) / 60);
        const rs = remainingSeconds % 60;

        updateDisplay(rh, rm, rs);
        updateProgress();

        // Play tick sound each second
        tickSound.currentTime = 0;
        tickSound.play();
      }, 1000);
    }

    function pauseTimer() {
      if (!isRunning) return;
      clearInterval(timerInterval);
      timerState.textContent = "Paused";
      isRunning = false;
      startBtn.disabled = false;
      pauseBtn.disabled = true;
      resetBtn.disabled = false;
    }

    function resetTimer() {
      clearInterval(timerInterval);
      isRunning = false;
      remainingSeconds = totalSeconds;

      hoursInput.disabled = false;
      minutesInput.disabled = false;
      secondsInput.disabled = false;

      startBtn.disabled = false;
      pauseBtn.disabled = true;
      resetBtn.disabled = true;

      timerState.textContent = "Ready";

      updateDisplay(
        parseInt(hoursInput.value) || 0,
        parseInt(minutesInput.value) || 0,
        parseInt(secondsInput.value) || 0
      );
      updateProgress();
    }

    startBtn.addEventListener("click", startTimer);
    pauseBtn.addEventListener("click", pauseTimer);
    resetBtn.addEventListener("click", resetTimer);

    updateDisplay(0, 0, 0);
    updateProgress();
  </script>
</body>
</html>





