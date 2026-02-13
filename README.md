# 🕵️‍♂️ The Universal Form Listener Collection
> **"No Lead Left Behind."**

Welcome to the ultimate arsenal of JavaScript listeners for Google Tag Manager (GTM). This repository contains "universal" scripts designed to capture form submissions from popular marketing platforms—specifically tailored for the Brazilian market—without hardcoding IDs or constantly updating selectors.

These scripts are **greedy** (grab all data), **smart** (auto-detect forms), and **stealthy** (work quietly in the background).

---

## 🚀 Supported Platforms

| Platform | Technique Used | Data Layer Event | Complexity |
| :--- | :--- | :--- | :--- |
| **ActiveCampaign** | 🧬 DOM Mutation Observer | `activeCampaignFormSuccess` | Medium |
| **RD Station** | 👂 Native Event Listener | `rdStationFormSuccess` | Low (Cleanest) |
| **YayForms** | 📩 `postMessage` Listener | `YFSubmit` | Low |
| **GreatPages** | 📡 AJAX Interceptor | `greatPagesFormSuccess` | High (007 Style) |

---

## 🛠️ Installation Guide

1.  **Copy the Script:** Open the file for the platform you are using (e.g., `activecampaign_listener.js`).
2.  **Create Tag:** Go to Google Tag Manager > **Tags** > **New**.
3.  **Tag Type:** Select **Custom HTML**.
4.  **Paste:** Drop the code into the HTML box.
5.  **Trigger:** Set to **All Pages** (or specific pages where forms exist).
6.  **Publish:** Save and publish your container.

*The listener is now live and waiting for targets.*

---

## 📂 The Listeners Explained

### 1. ActiveCampaign 🧬
**Trigger:** DOM Change (Success Message Appearance)  
**Event:** `activeCampaignFormSuccess`  
**Data Captured:** `acFormData` (Object containing Email, Phone, Name, etc.)

ActiveCampaign doesn't give a clear "Success" signal, so we deploy a **MutationObserver**. This script watches the form's HTML. When it sees the specific "Thank You" message appear (Class: `_form-thank-you`), it fires.

### 2. RD Station 🇧🇷
**Trigger:** `window.addEventListener('rd:conversion')`  
**Event:** `rdStationFormSuccess`  
**Data Captured:** `rdFormData` (Greedy capture of `event.detail`).

RD Station broadcasts a standard JavaScript event when a conversion happens. We simply tune our radio to listen for `rd:conversion`.

### 3. YayForms 📝
**Trigger:** `window.onmessage` (Filtered for `YFSubmit`)  
**Event:** `YFSubmit`  
**Data Captured:** `yayData` (Contains `answers` object with email/phone).

YayForms talks to the window using `postMessage`. Our script sits in the middle of the conversation. It includes a **"Safety Shield"** to ignore noise from Facebook/Google pixels and only lets valid "YF" messages through.

### 4. GreatPages 📄
**Trigger:** Network Request 200 OK to `/conversion`  
**Event:** `greatPagesFormSuccess`  
**Data Captured:** `gpFormData` (Scrapes `nome`, `e-mail`, `telefone` directly from inputs).

GreatPages submits silently via AJAX without reloading. Our script uses an **AJAX Interceptor**. It wraps the browser's native `XMLHttpRequest` and inspects every network packet. If it sees a request to `/conversion` with a `200 OK` status, it scrapes the form inputs immediately.

---

## 🧪 Debugging & Testing

**Don't trust blindly! Always test.**

1.  Open your browser's **Console** (F12).
2.  Enable **GTM Preview Mode**.
3.  Fill out a form on your site.
4.  Look for the `dataLayer.push` in the GTM debug window OR type `window.dataLayer` in the console to see your shiny new data.

---

## ⚠️ Disclaimer

These scripts are designed to be "Universal," but web platforms update their code occasionally.
* **ActiveCampaign:** Relies on the class `_form-thank-you`.
* **GreatPages:** Relies on the endpoint `/conversion`.

*If the platforms change these specific identifiers, the scripts may need a quick update.*

Happy Tracking! 🎯
