---
name: progress-indicators-code
description: Production-ready HTML, CSS and vanilla JavaScript progress indicators including an accessible determinate progress bar, a multi-step indicator with aria-current, an indeterminate spinner with text and a milestone live region. Use when implementing progress feedback for uploads, onboarding and processing in plain HTML, CSS and JavaScript.
---

# Progress Indicators: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Progress Indicators</title>
<style>
  :root { --ink: #111418; --muted: #5b6470; --line: #e4e7ec; --accent: #3b5bdb; --ok: #2b8a3e; --surface: #ffffff; }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: #f5f6f8; }

  .panel { max-width: 560px; margin: 48px auto; background: var(--surface); border: 1px solid var(--line); border-radius: 16px; padding: 28px; display: grid; gap: 24px; }
  .panel h2 { margin: 0; font-size: 1.25rem; }

  /* Determinate bar */
  .bar-label { display: flex; justify-content: space-between; font-size: 14px; color: var(--muted); margin-bottom: 8px; }
  progress {
    width: 100%; height: 10px; border: 0; border-radius: 999px; overflow: hidden;
    appearance: none; -webkit-appearance: none; background: #e9ecf2;
  }
  progress::-webkit-progress-bar { background: #e9ecf2; border-radius: 999px; }
  progress::-webkit-progress-value { background: var(--accent); border-radius: 999px; transition: width 300ms ease; }
  progress::-moz-progress-bar { background: var(--accent); border-radius: 999px; }

  /* Steps */
  .steps { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; list-style: none; padding: 0; margin: 0; }
  .steps li { font-size: 14px; color: var(--muted); padding-top: 10px; border-top: 3px solid var(--line); }
  .steps li[aria-current="step"] { color: var(--ink); font-weight: 600; border-top-color: var(--accent); }
  .steps li.done { color: var(--ok); border-top-color: var(--ok); }

  /* Indeterminate spinner */
  .busy { display: flex; align-items: center; gap: 12px; color: var(--muted); font-size: 15px; }
  .spinner { width: 18px; height: 18px; border-radius: 50%; border: 2.5px solid #d6dcea; border-top-color: var(--accent); animation: spin 800ms linear infinite; }
  @keyframes spin { to { transform: rotate(360deg); } }

  button { min-height: 44px; padding: 0 18px; border: 0; border-radius: 10px; background: var(--accent); color: #fff; font: inherit; font-weight: 600; cursor: pointer; justify-self: start; }
  button:disabled { opacity: 0.5; cursor: progress; }
  button:focus-visible { outline: 3px solid var(--ink); outline-offset: 3px; }

  @media (prefers-reduced-motion: reduce) {
    progress::-webkit-progress-value { transition: none; }
    .spinner { animation: none; border-top-color: #d6dcea; }
  }
  .sr-only { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); white-space: nowrap; }
</style>
</head>
<body>
  <section class="panel" aria-labelledby="upload-title">
    <h2 id="upload-title">Uploading report</h2>

    <div>
      <div class="bar-label"><span id="bar-name">report-q3.pdf</span><span id="bar-value">0%</span></div>
      <progress id="upload" max="100" value="0" aria-labelledby="upload-title bar-name"></progress>
    </div>

    <ol class="steps" aria-label="Setup steps">
      <li class="done">Account</li>
      <li aria-current="step">Workspace</li>
      <li>Invite team</li>
    </ol>

    <div class="busy" id="busy" hidden>
      <span class="spinner" aria-hidden="true"></span>
      <span>Processing your report, this can take a minute.</span>
    </div>

    <button id="start" type="button">Start upload</button>
    <p class="sr-only" id="live" role="status" aria-live="polite"></p>
  </section>

<script>
  const bar = document.getElementById("upload");
  const value = document.getElementById("bar-value");
  const live = document.getElementById("live");
  const busy = document.getElementById("busy");
  const start = document.getElementById("start");

  start.addEventListener("click", () => {
    start.disabled = true;
    busy.hidden = false;
    let done = 0;
    const total = 4;

    const timer = setInterval(() => {
      done += 1;
      const pct = Math.round((done / total) * 100);
      bar.value = pct;
      value.textContent = `${pct}%`;

      // Announce milestones only, not every change
      if (pct === 50 || pct === 100) live.textContent = `Upload ${pct} percent complete.`;

      if (done >= total) {
        clearInterval(timer);
        busy.hidden = true;
        live.textContent = "Upload complete.";
        start.disabled = false;
        start.textContent = "Upload again";
      }
    }, 900);
  });
</script>
</body>
</html>
```

## Rules for implementation
- Use the native `<progress>` element with an accessible name.
- Show the numeric value next to the bar and update it only from real progress.
- Announce milestones through `role="status"`, not every percentage change.
- Mark the current step with `aria-current="step"`.
