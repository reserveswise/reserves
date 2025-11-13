<div id="overlayContainer"></div>

<script>
  // OVERLAY LOAD + TIMER START
  fetch('overlay.html')
    .then(res => res.text())
    .then(html => {
      document.getElementById('overlayContainer').innerHTML = html;

      // AB JS CHALAO — DOM MEIN HAI AB
      const TIMER_KEY = 'bindingTimerSeconds';
      let totalSeconds = parseInt(localStorage.getItem(TIMER_KEY)) || 48 * 60 * 60;
      let timerInterval;

      function startTimer() {
        const overlay = document.getElementById('timerOverlay');
        const timerEl = document.getElementById('countdownTimer');
        if (!overlay || !timerEl) return;

        overlay.style.display = 'flex';

        timerInterval = setInterval(() => {
          if (totalSeconds <= 0) {
            clearInterval(timerInterval);
            overlay.style.display = 'none';
            localStorage.removeItem(TIMER_KEY);
            new Audio('data:audio/wav;base64,UklGRnoGAABXQVZFZm10IBAAAAABAAEAQB8AAEAfAAABAAgAZGF0YQoGAACBhYqFbF1fdJivrJBhNjVgodDbq2EcBQ==').play();
            return;
          }
          totalSeconds--;
          localStorage.setItem(TIMER_KEY, totalSeconds);

          const h = String(Math.floor(totalSeconds / 3600)).padStart(2, '0');
          const m = String(Math.floor((totalSeconds % 3600) / 60)).padStart(2, '0');
          const s = String(totalSeconds % 60).padStart(2, '0');
          timerEl.textContent = `${h}:${m}:${s}`;
        }, 1000);

        // Initial update
        const h = String(Math.floor(totalSeconds / 3600)).padStart(2, '0');
        const m = String(Math.floor((totalSeconds % 3600) / 60)).padStart(2, '0');
        const s = String(totalSeconds % 60).padStart(2, '0');
        timerEl.textContent = `${h}:${m}:${s}`;
      }

      if (totalSeconds > 0) startTimer();
    });
</script>
