---
name: command-palette-code-react-tailwind
description: Production-ready React and Tailwind CSS command palette with a global Ctrl or Cmd plus K shortcut, an ARIA combobox and listbox, ranked keyword search, keyboard navigation, focus return and an empty state. Use when adding a keyboard-driven command palette to a React application.
---

# Command Palette: React and Tailwind Implementation

## Command data and ranking (commands.ts)
```ts
export interface Command {
  id: string;
  group: "Actions" | "Pages";
  label: string;
  keywords: string;
  shortcut?: string;
  run: () => void;
}

export function rank(commands: Command[], query: string): Command[] {
  const q = query.trim().toLowerCase();
  if (!q) return commands;
  return commands
    .map((c) => {
      const label = c.label.toLowerCase();
      const score = label.startsWith(q) ? 3 : label.includes(q) ? 2 : c.keywords.toLowerCase().includes(q) ? 1 : 0;
      return { c, score };
    })
    .filter((x) => x.score > 0)
    .sort((a, b) => b.score - a.score)
    .map((x) => x.c);
}
```

## Palette component (CommandPalette.tsx)
```tsx
import { useEffect, useId, useRef, useState } from "react";
import { rank, type Command } from "./commands";

export function CommandPalette({ commands }: { commands: Command[] }) {
  const [open, setOpen] = useState(false);
  const [query, setQuery] = useState("");
  const [active, setActive] = useState(0);
  const returnRef = useRef<HTMLElement | null>(null);
  const inputRef = useRef<HTMLInputElement>(null);
  const listId = useId();

  const results = rank(commands, query);

  // Global shortcut
  useEffect(() => {
    const onKey = (e: KeyboardEvent) => {
      if ((e.ctrlKey || e.metaKey) && e.key.toLowerCase() === "k") {
        e.preventDefault();
        setOpen((v) => !v);
      }
    };
    document.addEventListener("keydown", onKey);
    return () => document.removeEventListener("keydown", onKey);
  }, []);

  // Focus management
  useEffect(() => {
    if (open) {
      returnRef.current = document.activeElement as HTMLElement;
      setQuery("");
      setActive(0);
      requestAnimationFrame(() => inputRef.current?.focus());
    } else {
      returnRef.current?.focus();
    }
  }, [open]);

  if (!open) return null;

  const close = () => setOpen(false);
  const run = (cmd?: Command) => {
    if (!cmd) return;
    close();
    cmd.run();
  };

  function onKeyDown(e: React.KeyboardEvent<HTMLInputElement>) {
    if (e.key === "ArrowDown") { e.preventDefault(); setActive((a) => (results.length ? (a + 1) % results.length : 0)); }
    else if (e.key === "ArrowUp") { e.preventDefault(); setActive((a) => (results.length ? (a - 1 + results.length) % results.length : 0)); }
    else if (e.key === "Home") { e.preventDefault(); setActive(0); }
    else if (e.key === "End") { e.preventDefault(); setActive(Math.max(0, results.length - 1)); }
    else if (e.key === "Enter") { e.preventDefault(); run(results[active]); }
    else if (e.key === "Escape") { e.preventDefault(); close(); }
  }

  const activeCmd = results[active];

  return (
    <div
      className="fixed inset-0 z-50 flex items-start justify-center bg-ink/45 pt-[12vh]"
      onMouseDown={(e) => { if (e.target === e.currentTarget) close(); }}
      onKeyDown={(e) => { if (e.key === "Tab") { e.preventDefault(); inputRef.current?.focus(); } }}
    >
      <div role="dialog" aria-modal="true" aria-label="Command palette" className="w-[min(620px,calc(100%-32px))] overflow-hidden rounded-2xl bg-white shadow-2xl">
        <label htmlFor={`${listId}-input`} className="sr-only">Search commands</label>
        <input
          id={`${listId}-input`}
          ref={inputRef}
          role="combobox"
          aria-expanded="true"
          aria-controls={listId}
          aria-autocomplete="list"
          aria-activedescendant={activeCmd ? `${listId}-${activeCmd.id}` : undefined}
          autoComplete="off"
          value={query}
          onChange={(e) => { setQuery(e.target.value); setActive(0); }}
          onKeyDown={onKeyDown}
          placeholder="Type a command or search..."
          className="w-full border-0 border-b border-slate-200 px-4 py-4 text-base outline-none"
        />

        {results.length === 0 ? (
          <p className="p-5 text-center text-body">No commands match "{query}". Try "project" or "invite".</p>
        ) : (
          <ul id={listId} role="listbox" aria-label="Commands" className="max-h-[360px] overflow-y-auto p-2">
            {results.map((cmd, i) => (
              <li
                key={cmd.id}
                id={`${listId}-${cmd.id}`}
                role="option"
                aria-selected={i === active}
                onMouseMove={() => setActive(i)}
                onClick={() => run(cmd)}
                className={`flex min-h-11 cursor-pointer items-center justify-between gap-3 rounded-lg px-3 ${
                  i === active ? "bg-[#eef2ff] shadow-[inset_3px_0_0_#3b5bdb]" : ""
                }`}
              >
                <span>{cmd.label}</span>
                {cmd.shortcut && <kbd className="rounded-md border border-slate-200 px-1.5 text-xs text-body">{cmd.shortcut}</kbd>}
              </li>
            ))}
          </ul>
        )}

        <p role="status" aria-live="polite" className="sr-only">
          {results.length} {results.length === 1 ? "command" : "commands"}
        </p>
      </div>
    </div>
  );
}
```

## Usage (App.tsx)
```tsx
import { CommandPalette } from "./CommandPalette";
import type { Command } from "./commands";

const commands: Command[] = [
  { id: "new-project", group: "Actions", label: "New project", keywords: "create add make", shortcut: "N", run: () => console.log("create") },
  { id: "invite", group: "Actions", label: "Invite teammate", keywords: "add member people", shortcut: "I", run: () => console.log("invite") },
  { id: "export", group: "Actions", label: "Export report", keywords: "download csv pdf", shortcut: "E", run: () => console.log("export") },
  { id: "dashboard", group: "Pages", label: "Go to dashboard", keywords: "home overview", run: () => console.log("dashboard") },
  { id: "billing", group: "Pages", label: "Go to billing", keywords: "invoices plan payment", run: () => console.log("billing") },
];

export default function App() {
  return (
    <main className="mx-auto max-w-3xl p-8 font-sans text-ink">
      <h1 className="text-2xl font-bold">Workspace</h1>
      <p className="mt-2 text-body">
        Press Ctrl + K (or Cmd + K) to open the command palette. Every command is also available in the menus.
      </p>
      <CommandPalette commands={commands} />
    </main>
  );
}
```

## Rules for implementation
- Implement the ARIA combobox pattern with `aria-activedescendant`.
- Open on Ctrl or Cmd plus K and restore focus to the previous element on close.
- Keep focus trapped inside the palette while open.
- Keep every command reachable through the normal interface as well.
