# Robinhood Alarm Widget

Small, client-side alarm widget for **Robinhood Web**.  
It runs entirely in the browser, reads prices from the existing page DOM, and plays a sound when your target price is hit.

No API keys. No extra requests to Robinhood servers.  
Everything happens locally in your own tab.

---

## Features

- Add price alarms for any symbol visible on Robinhood Web  
- Uses only the HTML/JS that is already loaded in your browser  
- No backend, no database, no extra infrastructure  
- Sound alert when the target price is reached  
- Lightweight UI with a dark, gradient background

---

## How it works

1. You open a Robinhood stock page in your browser.  
2. The widget reads the current price from the DOM (no API calls).  
3. When the live price crosses your target price, a sound is played.  

This project does **not**:
- Send extra requests to Robinhood  
- Bypass security or modify your account  
- Store or upload your data

Everything is local.

---

## Live demo

Once GitHub Pages is enabled for this repo:

👉 **Demo:**  
`https://qwre.github.io/Robinhood-Alarm-Widget/`

(The page uses the same gradient background and a small “Alarms” panel like in the preview.)

---

## Contacts

<div align="left">

  <div style="
      font-size:18px;
      font-weight:600;
      margin-bottom:8px;
      color:#ffffff;
    ">
    Contacts
  </div>

  <p style="display:flex; gap:10px; align-items:center;">

    <!-- Telegram -->
    <a href="https://t.me/KULLANICI_ADIN" target="_blank" style="display:inline-block;">
      <img 
        src="https://img.icons8.com/?size=100&id=25n4hOEoY7ss&format=png&color=000000"
        width="42"
        style="vertical-align:middle;"
      />
    </a>

    <!-- Instagram -->
    <a href="https://instagram.com/KULLANICI_ADI" target="_blank" style="display:inline-block;">
      <img 
        src="https://img.icons8.com/?size=100&id=WRrWnPAq4WVx&format=png&color=000000"
        width="42"
        style="vertical-align:middle;"
      />
    </a>

  </p>

</div>
