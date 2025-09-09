
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


```
import React, { useState, useMemo } from "react";
import "./CheckerQueue.css";

const MOCK_ROWS = [
  {
    txnRef: "TXN1001",
    loanId: "LN001",
    applicant: "John Doe",
    amount: 50000,
    currency: "USD",
    createdAt: "2025-09-01T10:15:00Z",
    currStep: "CHECKER",
    status: "PENDING",
    assignedTo: "user_001",
    makerCompletedAt: "2025-09-01T11:00:00Z",
  },
  {
    txnRef: "TXN1002",
    loanId: "LN002",
    applicant: "Alice Smith",
    amount: 75000,
    currency: "EUR",
    createdAt: "2025-09-01T09:00:00Z",
    currStep: "CHECKER",
    status: "APPROVED",
    assignedTo: "user_123",
    makerCompletedAt: "2025-09-01T09:40:00Z",
  },
  {
    txnRef: "TXN1003",
    loanId: "LN003",
    applicant: "Rahul Kumar",
    amount: 60000,
    currency: "INR",
    createdAt: "2025-08-30T14:20:00Z",
    currStep: "CHECKER",
    status: "REJECTED",
    assignedTo: "user_045",
    makerCompletedAt: "2025-08-30T15:00:00Z",
  },
  {
    txnRef: "TXN1004",
    loanId: "LN004",
    applicant: "Priya Verma",
    amount: 120000,
    currency: "USD",
    createdAt: "2025-09-02T08:30:00Z",
    currStep: "CHECKER",
    status: "PENDING",
    assignedTo: "user_002",
    makerCompletedAt: "2025-09-02T09:10:00Z",
  },
];

function formatDateIso(iso) {
  if (!iso) return "";
  const d = new Date(iso);
  const day = String(d.getDate()).padStart(2, "0");
  const month = d.toLocaleString("default", { month: "short" });
  const year = d.getFullYear();
  return `${day}-${month}-${year}`;
}

export default function CheckerQueue() {
  const [query, setQuery] = useState("");

  const filtered = useMemo(() => {
    const q = (query || "").trim().toLowerCase();
    if (!q) return MOCK_ROWS;
    return MOCK_ROWS.filter((r) => String(r.txnRef).toLowerCase().includes(q));
  }, [query]);

  return (
    <div className="checker-queue-wrap">
      <div className="cq-header">
        <div className="cq-search-wrap">
          <input
            type="search"
            className="cq-search"
            placeholder="Search (Transaction Ref No)"
            value={query}
            onChange={(e) => setQuery(e.target.value)}
            aria-label="Search by transaction reference number"
          />
        </div>

        <div className="cq-title">Ops Checker Queue</div>
      </div>

      <div className="cq-table-wrap">
        <table className="cq-table" role="table" aria-label="Ops Checker Queue table">
          <thead className="cq-thead">
            <tr>
              <th>Transaction Ref No</th>
              <th>Loan ID</th>
              <th>Applicant Name</th>
              <th>Amount</th>
              <th>Currency</th>
              <th>Created At</th>
              <th>Curr Step</th>
              <th>Status</th>
              <th>Assigned To</th>
              <th>Maker Completed At</th>
            </tr>
          </thead>

          <tbody>
            {filtered.length === 0 ? (
              <tr className="cq-empty-row">
                <td colSpan="10" className="cq-empty-cell">
                  No matching transactions.
                </td>
              </tr>
            ) : (
              filtered.map((row) => (
                <tr key={row.txnRef} className="cq-row" tabIndex={0}>
                  <td className="cq-cell txn">{row.txnRef}</td>
                  <td className="cq-cell loan">{row.loanId}</td>
                  <td className="cq-cell applicant">{row.applicant}</td>
                  <td className="cq-cell amount">{row.amount.toLocaleString()}</td>
                  <td className="cq-cell currency">{row.currency}</td>
                  <td className="cq-cell created">{formatDateIso(row.createdAt)}</td>
                  <td className="cq-cell step">{row.currStep}</td>
                  <td className={`cq-cell status ${row.status.toLowerCase()}`}>
                    {row.status}
                  </td>
                  <td className="cq-cell assigned">{row.assignedTo}</td>
                  <td className="cq-cell makerCompleted">{formatDateIso(row.makerCompletedAt)}</td>
                </tr>
              ))
            )}
          </tbody>
        </table>
      </div>
    </div>
  );
}
```


```
/* CheckerQueue.css - custom styles for CheckerQueue.js */

/* Container */
.checker-queue-wrap {
  max-width: 1100px;
  margin: 28px auto;
  background: #ffffff;
  border-radius: 8px;
  padding: 18px;
  box-shadow: 0 6px 18px rgba(10, 35, 77, 0.08);
  font-family: system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
}

/* Header: search (left) and title on same line */
.cq-header {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 14px;
  flex-wrap: nowrap;
}

/* Search box on left side of heading */
.cq-search-wrap {
  flex: 0 0 320px; /* fixed width for search */
}
.cq-search {
  width: 100%;
  padding: 8px 10px;
  border: 1px solid #d0d7e0;
  border-radius: 4px;
  font-size: 14px;
  outline: none;
  transition: box-shadow 0.15s ease, border-color 0.15s ease;
}
.cq-search:focus {
  box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.12);
  border-color: #5b9cff;
}

/* Title to the right of search (same line) */
.cq-title {
  font-size: 18px;
  font-weight: 600;
  color: #16325c;
  margin-left: 6px;
}

/* Table wrapper for horizontal scroll on small screens */
.cq-table-wrap {
  overflow-x: auto;
}

/* Table base */
.cq-table {
  width: 100%;
  border-collapse: collapse;
  min-width: 1000px; /* ensure horizontal scroll when needed */
}

/* Table head */
.cq-thead th {
  background-color: #053a89; /* deep blue header */
  color: #ffffff;
  text-align: left;
  font-size: 13px;
  font-weight: 600;
  padding: 10px 12px;
  border-bottom: 2px solid rgba(0,0,0,0.06);
  white-space: nowrap;
}

/* Table body cells */
.cq-table td {
  padding: 10px 12px;
  border-bottom: 1px solid #eef1f6;
  font-size: 14px;
  color: #1f2d3d;
  vertical-align: middle;
  white-space: nowrap;
}

/* Make the whole row change on hover */
.cq-row {
  cursor: pointer;
  transition: background-color 0.12s ease;
}
.cq-row:hover td {
  background-color: #f4f9ff; /* light blue on hover — change if needed */
}

/* Optional: keyboard focus look */
.cq-row:focus {
  outline: 2px solid rgba(5, 58, 137, 0.18);
  outline-offset: -2px;
}

/* Empty state row */
.cq-empty-row td {
  padding: 28px 12px;
  text-align: center;
  color: #6b7280;
  font-style: italic;
}

/* Small responsive tweak */
@media (max-width: 720px) {
  .cq-search-wrap {
    flex: 1 1 48%;
  }
  .cq-title {
    font-size: 16px;
  }
  .cq-table {
    min-width: 900px;
  }
}
/* Status colors */
.cq-cell.status.pending {
  color: #856404;
  background-color: #fff3cd;
  font-weight: 600;
}

.cq-cell.status.approved {
  color: #155724;
  background-color: #d4edda;
  font-weight: 600;
}

.cq-cell.status.rejected {
  color: #721c24;
  background-color: #f8d7da;
  font-weight: 600;
} 
```





