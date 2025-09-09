
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
        <div className="cq-title">Ops Checker Queue</div>
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
      </div>
      <hr className="cq-divider" />

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
                  <td className="cq-cell txn bold">{row.txnRef}</td>
                  <td className="cq-cell loan bold">{row.loanId}</td>
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

/* Font family */
.checker-queue-wrap {
  font-family: "Trebuchet MS", Verdana, sans-serif;
}

/* Header */
.cq-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 6px;
}
.cq-title {
  font-size: 20px;
  font-weight: 700;
  color: #053a89;
}

/* Divider */
.cq-divider {
  border: none;
  border-top: 3px solid #053a89;
  margin-bottom: 16px;
}

/* Search box */
.cq-search {
  border: 2px solid #053a89;
  color: #053a89;
  font-weight: 500;
  padding: 6px 10px;
  border-radius: 4px;
  outline: none;
}
.cq-search::placeholder {
  color: #3d6ab7;
}

/* Bold cells */
.cq-cell.bold {
  font-weight: 700;
}
/* Table header */
.cq-thead th {
  font-size: 12px;   /* smaller font for header cells */
  padding: 8px 10px; /* more compact rows */
}

/* Table body cells */
.cq-cell {
  font-size: 12px;   /* smaller font for table rows */
  padding: 8px 10px;
}

/* Bold cells */
.cq-cell.bold {
  font-weight: 700;
}
```





