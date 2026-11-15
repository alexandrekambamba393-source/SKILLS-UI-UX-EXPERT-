---
name: command-palette-code
description: Production-ready HTML, CSS and vanilla JavaScript command palette with a Ctrl or Cmd plus K shortcut, an ARIA combobox and listbox, keyword and prefix ranking, arrow and Enter keyboard handling, focus return and an empty state. Use when implementing a keyboard-driven command palette in plain HTML, CSS and JavaScript.
---

# Command Palette: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Command Palette</title>
<style>
  :root { --ink: #111418; --muted: #5b6470; --line: #e4e7ec; --accent: #3b5bdb; --surface: #fff; --bg: #f5f6f8; --active: #eef2ff; }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: var(--bg); min-height: 100vh; }
  :focus-visible { outline: 3px solid var(--accent); outline-offset: 2px; }

  .intro { max-width: 720px; margin: 48px auto; padding: 0 24px; }
  .intro kbd { font: inherit; font-size: 13px; background: #fff; border: 1px solid var(--line); border-bottom-width: 2px; border-radius: 6px; padding: 1px 6px; }

  .backdrop { position: fixed; inset: 0; background: rgba(17, 20, 24, 0.45); display: flex; align-items: flex-start; justify-content: center; padding-top: 12vh; z-index: 50; }
  .backdrop[hidden] { display: none; }

  .palette { width: min(620px, calc(100% - 32px)); background: var(--surface); border-radius: 16px; box-shadow: 0 24px 60px rgba(0,0,0,0.25); overflow: hidden; }
  .input { width: 100%; border: 0; border-bottom: 1px solid var(--line); padding: 16px 18px; font: inherit; font-size: 16px; outline: none; }
  .input:focus-visible { outline: none; }

  .list { list-style: none; margin: 0; padding: 8px; max-height: 360px; overflow-y: auto; }
  .group { font-size: 12px; font-weight: 700; text-transform: uppercase; letter-spacing: 0.06em; color: var(--muted); padding: 10px 10px 4px; }
  .option { display: flex; justify-content: space-between; align-items: center; gap: 12px; min-height: 44px; padding: 0 12px; border-radius: 10px; cursor: pointer; }
  .option[aria-selected="true"] { background: var(--active); box-shadow: inset 3px 0 0 var(--accent); }
  .shortcut { font-size: 12px; color: var(--muted); border: 1px solid var(--line); border-radius: 6px; padding: 1px 6px; }
  .empty { padding: 20px 12px; color: var(--muted); text-align: center; }
  .sr-only { position: absolute; width: 1px; height: 1px; overflow: hidden; clip: rect(0 0 0 0); white-space: nowrap; }
</style>
</head>
<body>
  <main class="intro">
    <h1>Workspace</h1>
    <p>Press <kbd>Ctrl</kbd> + <kbd>K</kbd> (or <kbd>Cmd</kbd> + <kbd>K</kbd>) to open the command palette. Every command is also available in the menus.</p>
    <button type="button" id="open-btn">Open command palette</button>
  </main>

  <div class="backdrop" id="backdrop" hidden>
    <div class="palette" role="dialog" aria-modal="true" aria-label="Command palette">
      <label for="cmd" class="sr-only">Search commands</label>
      <input
        id="cmd"
        class="input"
        type="text"
        role="combobox"
        aria-autocomplete="list"
        aria-expanded="true"
        aria-controls="cmd-list"
        aria-activedescendant=""
        placeholder="Type a command or search..."
        autocomplete="off"
      >
      <ul id="cmd-list" class="list" role="listbox" aria-label="Commands"></ul>
      <p id="count" class="sr-only" role="status" aria-live="polite"></p>
    </div>
  </div>

<script>
  const commands = [
    { id: "new-project", group: "Actions", label: "New project", keywords: "create add make", shortcut: "N", run: () => alert("Create project") },
    { id: "invite", group: "Actions", label: "Invite teammate", keywords: "add member people", shortcut: "I", run: () => alert("Invite") },
    { id: "export", group: "Actions", label: "Export report", keywords: "download csv pdf", shortcut: "E", run: () => alert("Export") },
    { id: "delete", group: "Actions", label: "Delete project", keywords: "remove trash destroy", run: () => alert("Delete") },
    { id: "dashboard", group: "Pages", label: "Go to dashboard", keywords: "home overview", run: () => alert("Dashboard") },
    { id: "billing", group: "Pages", label: "Go to billing", keywords: "invoices plan payment", run: () => alert("Billing") },
    { id: "settings", group: "Pages", label: "Go to settings", keywords: "preferences account profile", run: () => alert("Settings") },
  ];

  const backdrop = document.getElementById("backdrop");
  const input = document.getElementById("cmd");
  const list = document.getElementById("cmd-list");
  const count = document.getElementById("count");
  const openBtn = document.getElementById("open-btn");
  let filtered = [];
  let active = 0;
  let returnFocus = null;

  function score(cmd, q) {
    const label = cmd.label.toLowerCase();
    const keys = cmd.keywords.toLowerCase();
    if (!q) return 1;
    if (label.startsWith(q)) return 3;
    if (label.includes(q)) return 2;
    if (keys.includes(q)) return 1;
    return 0;
  }

  function render() {
    const q = input.value.trim().toLowerCase();
    filtered = commands
      .map((c) => ({ c, s: score(c, q) }))
      .filter((x) => x.s > 0)
      .sort((a, b) => b.s - a.s)
      .map((x) => x.c);

    list.innerHTML = "";
    if (filtered.length === 0) {
      const li = document.createElement("li");
      li.className = "empty";
      li.textContent = `No commands match "${input.value}". Try "project" or "invite".`;
      list.appendChild(li);
      input.removeAttribute("aria-activedescendant");
      count.textContent = "No results";
      return;
    }

    let lastGroup = null;
    filtered.forEach((cmd, i) => {
      if (cmd.group !== lastGroup && !q) {
        const h = document.createElement("li");
        h.className = "group";
        h.setAttribute("role", "presentation");
        h.textContent = cmd.group;
        list.appendChild(h);
        lastGroup = cmd.group;
      }
      const li = document.createElement("li");
      li.id = `opt-${cmd.id}`;
      li.className = "option";
      li.setAttribute("role", "option");
      li.setAttribute("aria-selected", String(i === active));
      li.innerHTML = `<span>${cmd.label}</span>${cmd.shortcut ? `<span class="shortcut">${cmd.shortcut}</span>` : ""}`;
      li.addEventListener("mousemove", () => setActive(i));
      li.addEventListener("click", () => run(i));
      list.appendChild(li);
    });

    active = Math.min(active, filtered.length - 1);
    syncActive();
    count.textContent = `${filtered.length} ${filtered.length === 1 ? "command" : "commands"}`;
  }

  function syncActive() {
    const options = list.querySelectorAll('[role="option"]');
    options.forEach((el, i) => el.setAttribute("aria-selected", String(i === active)));
    const current = filtered[active];
    if (current) {
      input.setAttribute("aria-activedescendant", `opt-${current.id}`);
      document.getElementById(`opt-${current.id}`)?.scrollIntoView({ block: "nearest" });
    }
  }

  function setActive(i) {
    active = i;
    syncActive();
  }

  function run(i) {
    const cmd = filtered[i];
    if (!cmd) return;
    close();
    cmd.run();
  }

  function open() {
    returnFocus = document.activeElement;
    backdrop.hidden = false;
    input.value = "";
    active = 0;
    render();
    input.focus();
  }

  function close() {
    backdrop.hidden = true;
    returnFocus?.focus();
  }

  input.addEventListener("input", () => { active = 0; render(); });

  input.addEventListener("keydown", (e) => {
    if (e.key === "ArrowDown") { e.preventDefault(); setActive((active + 1) % filtered.length); }
    else if (e.key === "ArrowUp") { e.preventDefault(); setActive((active - 1 + filtered.length) % filtered.length); }
    else if (e.key === "Home") { e.preventDefault(); setActive(0); }
    else if (e.key === "End") { e.preventDefault(); setActive(filtered.length - 1); }
    else if (e.key === "Enter") { e.preventDefault(); run(active); }
    else if (e.key === "Escape") { e.preventDefault(); close(); }
  });

  // Keep focus inside the palette while it is open
  backdrop.addEventListener("keydown", (e) => {
    if (e.key === "Tab") { e.preventDefault(); input.focus(); }
  });
  backdrop.addEventListener("click", (e) => { if (e.target === backdrop) close(); });

  openBtn.addEventListener("click", open);
  document.addEventListener("keydown", (e) => {
    if ((e.ctrlKey || e.metaKey) && e.key.toLowerCase() === "k") {
      e.preventDefault();
      backdrop.hidden ? open() : close();
    }
  });
</script>
</body>
</html>
```

## Rules for implementation
- Implement the ARIA combobox pattern with `aria-activedescendant` pointing at the active option.
- Open with Ctrl or Cmd plus K, focus the input immediately and return focus on close.
- Rank exact and prefix matches first; include keywords and synonyms.
- Keep every command reachable through the normal interface as well.
