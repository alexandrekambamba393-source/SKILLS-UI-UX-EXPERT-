---
name: frictionless-ux-code
description: Production-ready HTML, CSS and vanilla JavaScript frictionless checkout-style flow with autofill-friendly fields, a smart default shipping choice, inline editing, a one-tap reorder action, an undo toast instead of a confirmation dialog and a clear single next step. Use when implementing a low-effort, high-clarity flow in plain HTML, CSS and JavaScript.
---

# Frictionless UX: HTML, CSS and JavaScript Implementation

```html
<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Quick Checkout</title>
<style>
  :root { --ink: #111418; --muted: #4a5160; --line: #d0d5dd; --accent: #1f4fd1; --surface: #fff; --bg: #f5f6f8; --ok: #067647; }
  * { box-sizing: border-box; }
  body { margin: 0; font-family: system-ui, sans-serif; color: var(--ink); background: var(--bg); line-height: 1.5; }
  :focus-visible { outline: 3px solid var(--accent); outline-offset: 3px; }

  .layout { max-width: 880px; margin: 40px auto; padding: 0 20px; display: grid; gap: 20px; grid-template-columns: 1.3fr 1fr; }
  @media (max-width: 780px) { .layout { grid-template-columns: 1fr; } }

  .card { background: var(--surface); border: 1px solid #e4e7ec; border-radius: 16px; padding: 24px; display: grid; gap: 16px; align-content: start; }
  h1 { font-size: 1.25rem; margin: 0; }
  h2 { font-size: 1rem; margin: 0; color: var(--muted); font-weight: 600; }

  .field { display: grid; gap: 6px; }
  label { font-weight: 600; font-size: 14px; }
  input, select { min-height: 44px; padding: 0 12px; font: inherit; border: 1.5px solid #8a93a3; border-radius: 10px; background: #fff; color: var(--ink); }

  .option { display: flex; gap: 12px; align-items: center; padding: 14px; border: 1.5px solid var(--line); border-radius: 12px; cursor: pointer; min-height: 52px; }
  .option:has(input:checked) { border-color: var(--accent); background: #f3f6ff; }
  .option input { width: 18px; height: 18px; margin: 0; }
  .option small { color: var(--muted); display: block; }

  .inline { display: flex; justify-content: space-between; align-items: center; gap: 12px; padding: 12px 14px; border-radius: 12px; background: #f7f8fa; }
  .inline p { margin: 0; }
  .link-btn { background: none; border: 0; color: var(--accent); font: inherit; font-weight: 600; cursor: pointer; min-height: 40px; padding: 0 6px; }

  .primary { min-height: 52px; border: 0; border-radius: 12px; background: var(--accent); color: #fff; font: inherit; font-weight: 700; cursor: pointer; }
  .primary:disabled { opacity: 0.6; cursor: wait; }
  .secondary { min-height: 44px; border: 1.5px solid var(--line); border-radius: 12px; background: #fff; color: var(--ink); font: inherit; font-weight: 600; cursor: pointer; }

  .summary dl { display: grid; grid-template-columns: 1fr auto; gap: 8px 12px; margin: 0; }
  .summary dt { color: var(--muted); }
  .summary dd { margin: 0; text-align: right; font-weight: 600; }
  .summary .total { border-top: 1px solid #e4e7ec; padding-top: 10px; margin-top: 4px; }

  .toast {
    position: fixed; left: 50%; bottom: 24px; transform: translateX(-50%);
    background: var(--ink); color: #fff; border-radius: 12px; padding: 12px 14px;
    display: flex; gap: 16px; align-items: center; box-shadow: 0 12px 28px rgba(0,0,0,0.25);
  }
  .toast[hidden] { display: none; }
  .toast button { background: none; border: 0; color: #9ec1ff; font: inherit; font-weight: 700; cursor: pointer; min-height: 40px; padding: 0 8px; }
  .toast button:focus-visible { outline: 2px solid #9ec1ff; }
</style>
</head>
<body>
  <main class="layout">
    <form class="card" id="checkout" autocomplete="on">
      <h1>Shipping</h1>

      <div class="field">
        <label for="name">Full name</label>
        <input id="name" name="name" autocomplete="name" value="Amara Otieno">
      </div>
      <div class="field">
        <label for="address">Street address</label>
        <input id="address" name="address" autocomplete="street-address" value="14 Kilimani Road">
      </div>

      <fieldset style="border:0;padding:0;margin:0;display:grid;gap:10px">
        <legend style="font-weight:600;font-size:14px;margin-bottom:6px">Delivery</legend>
        <label class="option">
          <input type="radio" name="delivery" value="standard" checked>
          <span>Standard<small>3 to 5 days, free</small></span>
        </label>
        <label class="option">
          <input type="radio" name="delivery" value="express">
          <span>Express<small>Next day, KES 500</small></span>
        </label>
      </fieldset>

      <div class="inline">
        <p>Paying with Visa ending 4242</p>
        <button class="link-btn" type="button">Change</button>
      </div>

      <button class="primary" type="submit" id="pay">Place order</button>
    </form>

    <aside class="card summary" aria-labelledby="sum-title">
      <h2 id="sum-title">Order summary</h2>
      <dl>
        <dt>Items (2)</dt><dd id="subtotal">KES 4,800</dd>
        <dt>Delivery</dt><dd id="delivery">Free</dd>
        <dt class="total">Total</dt><dd class="total" id="total">KES 4,800</dd>
      </dl>
      <button class="secondary" type="button" id="reorder">Reorder last purchase</button>
    </aside>
  </main>

  <div class="toast" id="toast" role="status" aria-live="polite" hidden>
    <span id="toast-text">Order placed.</span>
    <button type="button" id="undo">Undo</button>
  </div>

<script>
  const deliveryCost = { standard: 0, express: 500 };
  const subtotal = 4800;
  const form = document.getElementById("checkout");
  const toast = document.getElementById("toast");
  const toastText = document.getElementById("toast-text");
  let undoTimer = null;

  function updateTotals() {
    const method = form.elements.namedItem("delivery").value;
    const cost = deliveryCost[method];
    document.getElementById("delivery").textContent = cost === 0 ? "Free" : `KES ${cost.toLocaleString("en-US")}`;
    document.getElementById("total").textContent = `KES ${(subtotal + cost).toLocaleString("en-US")}`;
  }
  form.addEventListener("change", updateTotals);
  updateTotals();

  // Single-step placement with undo instead of a blocking confirmation dialog
  form.addEventListener("submit", (e) => {
    e.preventDefault();
    const pay = document.getElementById("pay");
    pay.disabled = true;
    setTimeout(() => {
      pay.disabled = false;
      toastText.textContent = "Order placed.";
      toast.hidden = false;
      document.getElementById("undo").focus();
      clearTimeout(undoTimer);
      undoTimer = setTimeout(() => { toast.hidden = true; }, 6000);
    }, 700);
  });

  document.getElementById("undo").addEventListener("click", () => {
    clearTimeout(undoTimer);
    toast.hidden = true;
    toastText.textContent = "Order cancelled.";
    toast.hidden = false;
    undoTimer = setTimeout(() => { toast.hidden = true; }, 3000);
  });

  // One-tap reorder using the last saved purchase
  document.getElementById("reorder").addEventListener("click", () => {
    toastText.textContent = "Last order added to your cart.";
    toast.hidden = false;
    clearTimeout(undoTimer);
    undoTimer = setTimeout(() => { toast.hidden = true; }, 3000);
  });
</script>
</body>
</html>
```

## Rules for implementation
- Use correct `autocomplete` values so browsers and password managers fill fields.
- Pre-select the most common option and show the consequence immediately (for example, the total updates).
- Prefer an undo toast to a confirmation dialog for reversible actions.
- Keep one clearly primary action per screen.
