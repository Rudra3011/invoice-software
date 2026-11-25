# GST Invoice Pro - Business Invoicing & Billing Software (with SQLite Database)

A modern, responsive, and completely self-contained business invoice software with a **connected persistent SQLite database**. Designed specifically for Indian businesses with full GST compliance (optional Non-GST mode), live real-time preview, instant PDF download/print, dynamic UPI QR code payments, and dual-mode storage (Server SQLite Database with seamless browser fallback).

---

## 🗄️ Database Connection & Architecture

Your data is stored securely in a **persistent SQLite database** (`data/invoices.db`) on the server.
- **Central Storage**: Invoices and company profiles are saved directly to `data/invoices.db`. When hosted on a server or shared across a local network, any device connected to the server accesses the same centralized database.
- **Dual-Mode Sync (Resilient)**:
  - When running via the backend server (`node server.js` or `python run_server.py`), the app automatically connects to the SQLite REST API and shows a green **`● Database: SQLite Connected`** badge.
  - If opened standalone (`index.html` via `file://`), it gracefully falls back to browser `localStorage` and shows **`○ Database: Offline (Local)`**, ensuring you can still generate invoices even without the server running.
- **Automatic Migration**: Database tables (`invoices`, `profile`, `settings`) are created automatically on first run.

---

## 🔒 User Authentication & Security

The software is protected by a secure login screen:
- **User ID**: `rudra`
- **Password**: `Rudra@3011`
- **Features**:
  - Secure credential validation on both backend API and client.
  - "Keep me logged in" session memory so you don't have to re-enter on every page refresh.
  - Show / Hide password toggle icon.
  - 1-Click **Logout** button located in the top navigation bar.

---

## 🚀 Quick Start (How to Run)

### Option 1: 1-Click Launcher (Windows)
- Simply double-click **`start.bat`**.
- It starts the Node.js backend server (or falls back to Python automatically) and opens `http://localhost:3000` in your default browser.
- Data is continuously stored in `data/invoices.db`.

### Option 2: Node.js Server (Recommended for Hosting)
```powershell
npm start
# OR
node server.js
```
- Starts the lightweight HTTP & SQLite REST server on port `3000`.
- Zero external npm dependencies (uses Node.js built-in `node:sqlite` and HTTP modules).
- Visit `http://localhost:3000`.

### Option 3: Python Server
```powershell
python run_server.py
```
- Starts the Python SQLite REST server on port `8000`.
- Zero external pip dependencies (uses built-in `sqlite3` and `http.server`).
- Visit `http://localhost:8000`.

---

## 🌐 How to Host Online

When you host this project online on any cloud provider or Virtual Private Server (VPS), your database persists across sessions:

### 1. Render / Railway / Fly.io / Koyeb (PaaS)
1. Push this folder to a GitHub repository.
2. Create a new **Web Service** on [Render.com](https://render.com) or [Railway.app](https://railway.app).
3. Set:
   - **Build Command**: `npm install` (or leave empty)
   - **Start Command**: `npm start`
   - **Environment Variable**: `PORT=3000`
4. Attach a **Persistent Disk** mounted at `/data` if using ephemeral cloud containers so `data/invoices.db` is preserved between deployments.

### 2. VPS (Ubuntu / Debian / DigitalOcean / AWS EC2)
1. Copy the project folder to the server.
2. Install Node.js (v20+ or v22+).
3. Use `pm2` to run the server in the background:
   ```bash
   npm install -g pm2
   pm2 start server.js --name "invoice-app"
   pm2 save
   pm2 startup
   ```
4. Point your Nginx reverse proxy to `http://localhost:3000`.

---

## ✨ Key Features

### 1. 🇮🇳 Optional GST Mode & Flexible Invoicing
- **One-Click GST / Non-GST Switch**: Switch between **GST Tax Invoice** and **Non-GST Invoice** with a single click.
- **Non-GST Mode Features**:
  - Hides GST% and HSN columns in the items table.
  - No CGST / SGST / IGST tax calculations (0% tax).
  - Automatically hides the HSN Tax Summary table.
  - GSTIN fields become completely optional.
  - Invoice header cleanly switches to **"INVOICE"** (or choose *"Bill of Supply"*, *"Proforma Invoice"*, *"Commercial Invoice"*, or custom title).
- **GST Mode Features**:
  - Includes all 36 Indian States and Union Territories with official 2-digit GST codes (01 to 38).
  - Entering a 15-digit GSTIN automatically extracts the state code and selects the state.
  - Auto-calculates **CGST (50%) + SGST (50%)** for Intra-State or **IGST (100%)** for Inter-State.
  - Full HSN / SAC summary breakdown table.

### 2. ⚡ Clean Items & Flexible Round Off
- **Unit Column Removed**: Clean and spacious layout for descriptions, rates, and totals.
- **Optional Discount Column**: A checkbox **"Enable Discount"** allows you to show or completely hide the Discount column. When disabled, no discount column appears in either the editor or the final printed invoice.
- **Flexible Round Off (Auto, Manual, None)**:
  - **Auto**: Automatically rounds to the nearest whole Rupee.
  - **Manual Round Off**: Enter exact adjustment value (e.g., if total is `123.45`, choose **"- Deduct"** and enter `3.45` to round the invoice to exactly `120.00`).
  - **No Round Off**: Keeps exact paise.
  - Works seamlessly in **both GST and Non-GST modes**!

### 3. 📱 Dynamic UPI QR Code Generation
- Generates standard UPI QR codes (`upi://pay?pa=...&pn=...&am=...&cu=INR`).
- Clients can scan the QR code with **Google Pay, PhonePe, Paytm, BHIM** to pay the exact invoice amount instantly.
- Generates purely on the client side using pure JavaScript with zero external network required.

### 4. 🎨 3 Professional Invoice Templates & Color Customization
- **Modern Minimalist**: Contemporary layout with clean typography, accent bars, and sleek card layout.
- **Classic GST Tax Invoice**: Formal government-style bordered format with copies checkbox (*Original for Recipient / Duplicate for Transporter / Triplicate for Supplier*).
- **Executive Corporate**: Bold top banner header, executive feel, ideal for IT, services, and corporate billing.
- **5 Accent Color Themes**: Royal Navy, Emerald Teal, Deep Indigo, Crimson, and Slate Graphite.

### 5. 🖨️ 1-Click PDF Download & Crisp A4 Printing
- Dedicated print stylesheet (`css/print.css`) configured for exact A4 dimensions (`210mm x 297mm`).
- Razor-sharp vector text and colors with no UI clutter or buttons in the printout.
- Click **"Print / Save PDF"** and select **"Save as PDF"** or choose any connected printer.

### 6. 💾 Database Persistence, History & Backup
- **SQLite Database (`data/invoices.db`)**: Invoices and profile are stored centrally on disk.
- **Save as Default Profile**: Save your company name, GSTIN, address, bank details, and logo once—all new invoices start pre-filled!
- **Invoice Archive (History)**: Save multiple past invoices, search by client/invoice number/date, reload, duplicate, or delete.
- **JSON Backup & Restore**: One-click download of all data as a backup JSON file, with easy one-click restore.

---

## 📁 File Structure

```
Invoice Software/
├── index.html              # Main application user interface
├── server.js               # Node.js backend server with SQLite REST API
├── run_server.py           # Python fallback server with SQLite REST API
├── package.json            # Node.js project configuration (npm start)
├── start.bat               # 1-click Windows launcher
├── README.md               # User guide and documentation
├── data/
│   └── invoices.db         # Persistent SQLite Database file
├── css/
│   ├── style.css           # App interface styles & theme
│   └── print.css           # A4 print and PDF stylesheet
└── js/
    ├── gst-states.js       # Indian States & GST codes directory
    ├── number-to-words.js  # Indian currency amount to words converter
    ├── qrcode.js           # Pure JS UPI QR code generator
    ├── templates.js        # Modern, Classic & Corporate invoice templates
    └── app.js              # Application logic, calculations & SQLite ApiClient
```
