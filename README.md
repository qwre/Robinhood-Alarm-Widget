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

<img width="689" height="472" alt="image" src="https://github.com/user-attachments/assets/9743aef7-65b0-40e8-b5bb-d6a97edf1cd3" />


