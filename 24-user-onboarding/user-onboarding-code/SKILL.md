---
name: user-onboarding-code
description: Production-ready HTML, CSS and vanilla JavaScript onboarding with a persistent setup checklist that completes through real actions, a skippable three-step welcome tour with keyboard support, an empty state with a clear first action and localStorage progress tracking. Use when implementing a first-run experience and setup checklist in plain HTML, CSS and JavaScript.
---

# User Onboarding: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Get Started</title>
<style>
  :root { --ink: #111418; --muted: #5b6470; --line: #e4e7ec; --accent: #3b5bdb; --ok: #2b8a3e; --surface: #ffffff; --bg: #f5f6f8; }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: var(--bg); line-height: 1.6; }
  :focus-visible { outline: 3px solid var(--accent); outline-offset: 3px; }

  .layout { max-width: 880px; margin: 40px auto; padding: 0 20px; display: grid; gap: 24px; }

  .checklist { background: var(--surface); border: 1px solid var(--line); border-radius: 16px; padding: 24px; }
  .checklist h2 { margin: 0 0 4px; font-size: 1.25rem; }
  .checklist .progress { color: var(--muted); font-size: 14px; margin: 0 0 16px; }
  .checklist ol { list-style: none; padding: 0; margin: 0; display: grid; gap: 10px; }
  .step { display: flex; align-items: center; justify-content: space-between; gap: 12px; padding: 14px 16px; border: 1px solid var(--line); border-radius: 12px; }
  .step.done { background: #f1faf3; border-color: #b9e3c6; }
  .step .label { display: flex; align-items: center; gap: 12px; font-weight: 600; }
  .mark { width: 22px; height: 22px; border-radius: 50%; border: 2px solid #b8c0cc; display: grid; place-items: center; font-size: 13px; color: #fff; }
  .step.done .mark { background: var(--ok); border-color: var(--ok); }
  .step.done .mark::after { content: "✓"; }
  .step button { min-height: 40px; padding: 0 14px; border: 1.5px solid var(--accent); border-radius: 10px; background: #fff; color: var(--accent); font: inherit; font-weight: 600; cursor: pointer; }
  .step.done button { border-color: var(--line); color: var(--muted); }

  .empty { background: var(--surface); border: 1px dashed #b8c0cc; border-radius: 16px; padding: 40px 24px; text-align: center; }
  .empty h3 { margin: 0 0 8px; font-size: 1.25rem; }
  .empty p { margin: 0 auto 20px; color: var(--muted); max-width: 46ch; }
  .primary { min-height: 44px; padding: 0 20px; border: 0; border-radius: 10px; background: var(--accent); color: #fff; font: inherit; font-weight: 600; cursor: pointer; }

  /* Tour */
  .tour { position: fixed; right: 20px; bottom: 20px; width: min(340px, calc(100% - 40px)); background: var(--ink); color: #fff; border-radius: 14px; padding: 20px; box-shadow: 0 16px 40px rgba(0,0,0,0.25); }
  .tour[hidden] { display: none; }
  .tour h3 { margin: 0 0 6px; font-size: 1.05rem; }
  .tour p { margin: 0 0 14px; color: rgba(255,255,255,0.85); font-size: 15px; }
  .tour .row { display: flex; justify-content: space-between; align-items: center; gap: 8px; }
  .tour button { min-height: 40px; padding: 0 14px; border-radius: 9px; border: 0; font: inherit; font-weight: 600; cursor: pointer; }
  .tour .next { background: #fff; color: var(--ink); }
  .tour .skip { background: transparent; color: #cfd6e4; }
  .tour .dots { color: #9aa3ae; font-size: 13px; }
</style>
</head>
<body>
  <main class="layout">
    <section class="checklist" aria-labelledby="setup-title">
      <h2 id="setup-title">Set up your workspace</h2>
      <p class="progress" id="progress" role="status" aria-live="polite">0 of 3 complete</p>
      <ol id="steps">
        <li class="step" data-key="profile"><span class="label"><span class="mark" aria-hidden="true"></span>Complete your profile</span><button type="button">Start</button></li>
        <li class="step" data-key="project"><span class="label"><span class="mark" aria-hidden="true"></span>Create your first project</span><button type="button">Start</button></li>
        <li class="step" data-key="invite"><span class="label"><span class="mark" aria-hidden="true"></span>Invite a teammate</span><button type="button">Start</button></li>
      </ol>
    </section>

    <section class="empty" aria-labelledby="empty-title">
      <h3 id="empty-title">No projects yet</h3>
      <p>Projects hold your files, tasks and team activity. Create one to see your dashboard come to life.</p>
      <button class="primary" type="button" data-complete="project">Create a project</button>
    </section>
  </main>

  <aside class="tour" id="tour" role="dialog" aria-labelledby="tour-title" aria-describedby="tour-body" hidden>
    <h3 id="tour-title"></h3>
    <p id="tour-body"></p>
    <div class="row">
      <span class="dots" id="tour-dots" aria-live="polite"></span>
      <div class="row" style="gap:6px">
        <button class="skip" type="button" id="tour-skip">Skip tour</button>
        <button class="next" type="button" id="tour-next">Next</button>
      </div>
    </div>
  </aside>

<script>
  const KEY = "onboarding-v1";
  const state = (() => {
    try { return JSON.parse(localStorage.getItem(KEY)) || {}; } catch { return {}; }
  })();
  const save = () => { try { localStorage.setItem(KEY, JSON.stringify(state)); } catch {} };

  const steps = Array.from(document.querySelectorAll(".step"));
  const progress = document.getElementById("progress");

  function render() {
    steps.forEach((el) => {
      const done = !!state[el.dataset.key];
      el.classList.toggle("done", done);
      const btn = el.querySelector("button");
      btn.textContent = done ? "Done" : "Start";
      btn.disabled = done;
    });
    const count = steps.filter((el) => state[el.dataset.key]).length;
    progress.textContent = `${count} of ${steps.length} complete`;
  }

  function complete(key) {
    if (state[key]) return;
    state[key] = true;
    save();
    render();
  }

  // Items complete through real actions, not checkbox clicks.
  steps.forEach((el) => el.querySelector("button").addEventListener("click", () => {
    complete(el.dataset.key);
  }));
  document.querySelector("[data-complete]").addEventListener("click", (e) => {
    complete(e.currentTarget.dataset.complete);
  });

  // Three-step tour, skippable and shown once.
  const tourSteps = [
    { title: "Projects", body: "Group related work in projects to keep teammates aligned." },
    { title: "Tasks", body: "Add tasks to a project and assign owners to track progress." },
    { title: "Invites", body: "Bring your team in once your first project is ready." },
  ];
  const tour = document.getElementById("tour");
  let index = 0;

  function showTour() {
    tour.hidden = false;
    renderTour();
    document.getElementById("tour-next").focus();
  }
  function renderTour() {
    const s = tourSteps[index];
    document.getElementById("tour-title").textContent = s.title;
    document.getElementById("tour-body").textContent = s.body;
    document.getElementById("tour-dots").textContent = `${index + 1} of ${tourSteps.length}`;
    document.getElementById("tour-next").textContent = index === tourSteps.length - 1 ? "Finish" : "Next";
  }
  function endTour() {
    tour.hidden = true;
    state.tourSeen = true;
    save();
  }

  document.getElementById("tour-next").addEventListener("click", () => {
    if (index < tourSteps.length - 1) { index += 1; renderTour(); } else endTour();
  });
  document.getElementById("tour-skip").addEventListener("click", endTour);
  document.addEventListener("keydown", (e) => { if (e.key === "Escape" && !tour.hidden) endTour(); });

  render();
  if (!state.tourSeen) showTour();
</script>
</body>
</html>
```

## Rules for implementation
- Complete checklist items through real product actions, never through standalone checkboxes.
- Make every tour skippable and show it only once, storing that state.
- Keep the empty state actionable with a clear primary button.
- Keep tour dialogs keyboard-operable with Escape to dismiss, and move focus into the dialog when it opens.
