
# Talk-A-Tive

Talk-a-tive is a Full Stack Chatting App.
Uses Socket.io for real time communication and stores user details in encrypted format in Mongo DB Database.
## Tech Stack

**Client:** React JS

**Server:** Node JS, Express JS

**Database:** Mongo DB
  
## Demo


## Run Locally

Clone the project

```bash
  git clone https://github.com/piyush-eon/mern-chat-app
```

Go to the project directory

```bash
  cd mern-chat-app
```

Install dependencies

```bash
  npm install
```

```bash
  cd frontend/
  npm install
```

Start the server

```bash
  npm run start
```
Start the Client

```bash
  //open now terminal
  cd frontend
  npm start
```
#########

import React, { useState, useMemo, useRef, useEffect } from "react";
import "bootstrap/dist/css/bootstrap.min.css";

// CheckerInboxTable.js
// Component for Ops Checker Inbox
// - 4 columns: Transaction Ref No, Curr Step Name, Last Step Name, Process Date
// - Global search and Show/Hide Columns using Bootstrap dropdown

const MOCK_DATA = [
  {
    txnRef: "SCW1575G10A1110419074465",
    currStep: "Pending Checker",
    lastStep: "Maker Completed",
    processDate: "2019-04-11T08:38:04Z",
  },
  {
    txnRef: "SCW1575G10A1110419074466",
    currStep: "Pending Checker",
    lastStep: "Maker Completed",
    processDate: "2019-04-11T08:38:10Z",
  },
  {
    txnRef: "SCW1575G10A1110419074467",
    currStep: "Pending Checker",
    lastStep: "Maker Completed",
    processDate: "2019-04-11T08:38:15Z",
  },
  {
    txnRef: "SCW1575G10A1110419074469",
    currStep: "Pending Checker",
    lastStep: "Maker Completed",
    processDate: "2019-04-11T08:43:09Z",
  },
];

const COLUMNS = [
  { key: "txnRef", label: "Transaction Ref No" },
  { key: "currStep", label: "Curr Step Name" },
  { key: "lastStep", label: "Last Step Name" },
  { key: "processDate", label: "Process Date" },
];

function formatDate(iso) {
  if (!iso) return "";
  const d = new Date(iso);
  const day = String(d.getDate()).padStart(2, "0");
  const month = d.toLocaleString("default", { month: "short" });
  const year = d.getFullYear();
  const time = d.toTimeString().split(" ")[0];
  return `${day}-${month}-${year} ${time}`;
}

export default function CheckerInboxTable() {
  const [data] = useState(MOCK_DATA);
  const [query, setQuery] = useState("");
  const [visibleCols, setVisibleCols] = useState(() => {
    const map = {};
    COLUMNS.forEach((c) => (map[c.key] = true));
    return map;
  });

  const [isDropdownOpen, setIsDropdownOpen] = useState(false);
  const dropdownRef = useRef(null);

  useEffect(() => {
    function onDocClick(e) {
      if (dropdownRef.current && !dropdownRef.current.contains(e.target)) {
        setIsDropdownOpen(false);
      }
    }
    document.addEventListener("mousedown", onDocClick);
    return () => document.removeEventListener("mousedown", onDocClick);
  }, []);

  const toggleColumn = (key) => {
    setVisibleCols((prev) => ({ ...prev, [key]: !prev[key] }));
  };

  const filtered = useMemo(() => {
    const q = (query || "").trim().toLowerCase();
    if (!q) return data;
    return data.filter((row) =>
      COLUMNS.some((col) => {
        if (!visibleCols[col.key]) return false;
        const val = String(row[col.key] ?? "").toLowerCase();
        return val.includes(q);
      })
    );
  }, [data, query, visibleCols]);

  const allHidden = Object.values(visibleCols).every((v) => v === false);

  return (
    <div className="container my-4">
      <div className="card shadow-sm">
        <div className="card-body">
          <div className="d-flex justify-content-between align-items-center mb-3 flex-nowrap">
            {/* Search - placed inline */}
            <input
              type="search"
              className="form-control form-control-sm me-2"
              placeholder="Search (txn, step, date...)"
              value={query}
              onChange={(e) => setQuery(e.target.value)}
              style={{ minWidth: 260 }}
            />

            {/* Show/Hide Columns as Bootstrap dropdown in one line */}
            <div className={`dropdown ${isDropdownOpen ? "show" : ""}`} ref={dropdownRef}>
              <button
                type="button"
                className="btn btn-sm btn-outline-secondary dropdown-toggle"
                onClick={() => setIsDropdownOpen((v) => !v)}
                aria-expanded={isDropdownOpen}
              >
                Show / Hide Columns
              </button>

              <ul className={`dropdown-menu dropdown-menu-end ${isDropdownOpen ? "show" : ""}`} style={{ minWidth: 220 }}>
                <li className="px-3 py-2">
                  {COLUMNS.map((col) => (
                    <div className="form-check" key={col.key}>
                      <input
                        className="form-check-input"
                        type="checkbox"
                        id={`chk-${col.key}`}
                        checked={!!visibleCols[col.key]}
                        onChange={() => toggleColumn(col.key)}
                      />
                      <label className="form-check-label ms-2" htmlFor={`chk-${col.key}`}>
                        {col.label}
                      </label>
                    </div>
                  ))}
                </li>
              </ul>
            </div>
          </div>

          {allHidden ? (
            <div className="alert alert-warning my-4">
              All columns are hidden. Use <strong>Show / Hide Columns</strong> to display columns.
            </div>
          ) : (
            <div className="table-responsive">
              <table className="table table-sm table-bordered align-middle mb-0">
                <thead>
                  <tr style={{ backgroundColor: "#004085", color: "#fff" }}>
                    {COLUMNS.map((col) => visibleCols[col.key] && <th key={col.key}>{col.label}</th>)}
                  </tr>
                </thead>
                <tbody>
                  {filtered.length === 0 ? (
                    <tr>
                      <td
                        colSpan={Math.max(Object.values(visibleCols).filter(Boolean).length, 1)}
                        className="text-center py-4 text-muted"
                      >
                        No results found.
                      </td>
                    </tr>
                  ) : (
                    filtered.map((row) => (
                      <tr key={row.txnRef}>
                        {visibleCols.txnRef && <td className="text-break">{row.txnRef}</td>}
                        {visibleCols.currStep && <td>{row.currStep}</td>}
                        {visibleCols.lastStep && <td>{row.lastStep}</td>}
                        {visibleCols.processDate && <td className="text-nowrap">{formatDate(row.processDate)}</td>}
                      </tr>
                    ))
                  )}
                </tbody>
              </table>
            </div>
          )}
        </div>
      </div>
    </div>
  );
}

  




  
