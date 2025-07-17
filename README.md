# Warehouse Management System (WMS) MVP

## Overview
This is a full-stack MVP for a Warehouse Management System built with **Node.js (Express)** for the backend and **React.js** (with Tailwind CSS) for the frontend. It demonstrates file upload, dynamic SKU→MSKU mapping (including combo SKUs), and clean data visualization. The backend also supports exporting cleaned data for use in no-code relational databases like Baserow. The frontend now displays mapping/logging issues and allows users to download the cleaned CSV directly from the dashboard.

---

## 📦 App Flow (End-to-End)

1. **User opens the dashboard (frontend)**
2. **User uploads a sales data file** (Excel/CSV) via drag-and-drop or file picker
3. **Frontend sends the file** to the backend via an Axios POST request
4. **Backend parses the file** using `xlsx`, extracts SKU, MSKU, and quantity columns, and supports combo SKUs (comma-separated)
5. **Backend returns cleaned data and a mapping log** as JSON (with unknown MSKUs marked)
6. **Frontend displays the data** in a table, shows metrics (total quantity, unknown MSKUs), a bar chart by MSKU, and any mapping/logging issues
7. **User can download the cleaned CSV** directly from the dashboard for import into a no-code DB (e.g., Baserow)

---

## 🏗️ Project Structure

```
project-root/
├── Backend/
│   ├── index.js                # Express server entry point
│   ├── package.json            # Backend dependencies
│   └── src/
│       ├── routes/
│       │   └── upload.js       # File upload, parsing, and export route
│       ├── services/
│       │   └── mappingService.js # Cleans and maps uploaded data (combo support, logging)
│       └── uploads/            # Uploaded files and logs
├── Frontend/
│   ├── public/
│   │   └── index.html
│   ├── src/
│   │   ├── components/
│   │   │   └── UploadTable.jsx # Table for displaying mapped data
│   │   ├── App.js              # Main React app logic (handles cleaned data, logs, CSV download)
│   │   └── index.css           # Tailwind CSS imports
│   └── tailwind.config.js      # Tailwind config
└── README.md
```

---

## 🚀 How to Run

### 1. Backend
```bash
cd Backend
npm install
node index.js
```
- Runs on [http://localhost:4000](http://localhost:4000)

### 2. Frontend
```bash
cd Frontend
npm install
npm start
```
- Runs on [http://localhost:3000](http://localhost:3000)

---

## 📄 File Format & Combo SKUs
Upload an **Excel or CSV file** with columns:
- `SKU` (or `sku`) — supports comma-separated combos (e.g., `pen,notebook`)
- `MSKU` (or `msku`)
- `quantity` (or `qty`, `Qty`)

**Example:**
| SKU           | MSKU           | quantity |
|---------------|----------------|----------|
| pen           | cste-pen       | 10       |
| pen,notebook  | cste-combo     | 5        |
| pencil        |                | 7        |

- If `MSKU` is missing, it will be marked as `Unknown` in the results.
- Each SKU in a combo row is mapped individually (with the same MSKU and quantity for MVP simplicity).

---

## 🖥️ User Flow & UI Features (Frontend)
- Drag & drop or select a file
- Click **Upload**
- See a table of mapped data (SKU, MSKU, quantity)
- View total quantity, count of unknown MSKUs, and a bar chart of sales by MSKU
- **See mapping/logging issues** (e.g., missing SKUs, unknown MSKUs) in a clear alert section
- **Download the cleaned CSV** directly from the dashboard for import into a no-code DB

---

## 🛠️ Tech Stack
- **Backend:** Node.js, Express, Multer, xlsx, json2csv
- **Frontend:** React, Axios, Tailwind CSS, Recharts
- **No-Code DB:** [Baserow](https://baserow.io/) (recommended for relational DB and dashboard)

---

## 📝 Logging & Validation
- The backend logs mapping issues (missing SKUs, unknown MSKUs) to `Backend/src/uploads/mapping_log.json`.
- The API response includes a `log` array for immediate feedback, which is shown in the frontend.

---

## 📤 Export Cleaned Data for No-Code DB
- After uploading and processing a file, click **Download Cleaned CSV** in the dashboard, or GET `/upload/export/csv` from the backend:
  - [http://localhost:4000/upload/export/csv](http://localhost:4000/upload/export/csv)
- Import this CSV into Baserow, Teable.io, or NoCodeDB for further analysis and dashboarding.

---

## 🗄️ Using Baserow (or Similar)
1. [Sign up for Baserow](https://baserow.io/) (or run locally)
2. Create a new table and import the exported CSV
3. Use Baserow’s dashboard features to visualize, edit, and relate your data

---

## 👤 For Coders & Employers
- **Coders:** See `Backend/src/routes/upload.js` and `mappingService.js` for backend logic. Frontend logic is in `Frontend/src/App.js` and `UploadTable.jsx`.
- **Employers:** This MVP demonstrates file upload, dynamic mapping (including combos), logging, and modern UI/UX. It is easily extensible for real warehouse needs and integrates with no-code DBs for business users.
