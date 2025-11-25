// pages/index.tsx

import Link from "next/link";
import { supabase } from "../lib/supabaseClient";
import { useEffect, useState } from "react";

export default function Dashboard() {
  const [user, setUser] = useState<any>(null);

  useEffect(() => {
    supabase.auth.getUser().then(({ data }) => setUser(data?.user));
  }, []);

  return (
    <main className="min-h-screen bg-gray-100 p-8">
      {/* Top Bar */}
      <header className="flex justify-between items-center mb-10">
        <h1 className="text-3xl font-bold">GST Accounting Dashboard</h1>
        <div>
          {user ? (
            <button
              onClick={async () => {
                await supabase.auth.signOut();
                location.reload();
              }}
              className="px-4 py-2 bg-red-600 text-white rounded"
            >
              Logout
            </button>
          ) : (
            <Link href="/auth">
              <a className="px-4 py-2 bg-blue-600 text-white rounded">
                Login
              </a>
            </Link>
          )}
        </div>
      </header>

      {/* Cards Section */}
      <section className="grid grid-cols-1 md:grid-cols-3 gap-6 mb-10">
        <div className="p-6 bg-white shadow rounded">
          <h2 className="text-lg font-semibold mb-2">Total Invoices</h2>
          <p className="text-3xl font-bold text-blue-600">27</p>
        </div>

        <div className="p-6 bg-white shadow rounded">
          <h2 className="text-lg font-semibold mb-2">Pending Reconciliations</h2>
          <p className="text-3xl font-bold text-yellow-600">5</p>
        </div>

        <div className="p-6 bg-white shadow rounded">
          <h2 className="text-lg font-semibold mb-2">GST Payable</h2>
          <p className="text-3xl font-bold text-red-600">₹14,800</p>
        </div>
      </section>

      {/* Quick Tools */}
      <section>
        <h2 className="text-2xl font-semibold mb-4">Quick Tools</h2>

        <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
          {/* Invoice Upload / OCR */}
          <Link href="/invoices">
            <a className="p-6 bg-white rounded shadow hover:shadow-lg transition">
              <h3 className="text-xl font-semibold mb-2">📄 Invoice OCR Entry</h3>
              <p className="text-gray-700">
                Upload invoice photos and extract data using OCR.
              </p>
            </a>
          </Link>

          {/* Bank Reconciliation */}
          <Link href="/api/reconcile">
            <a className="p-6 bg-white rounded shadow hover:shadow-lg transition">
              <h3 className="text-xl font-semibold mb-2">🏦 Bank Reconciliation</h3>
              <p className="text-gray-700">
                Match bank statements with invoice records.
              </p>
            </a>
          </Link>

          {/* Accountant Access */}
          <Link href="/accountant">
            <a className="p-6 bg-white rounded shadow hover:shadow-lg transition">
              <h3 className="text-xl font-semibold mb-2">🧑‍💼 Accountant Panel</h3>
              <p className="text-gray-700">
                Manage client records, invoices, and GST filings.
              </p>
            </a>
          </Link>

          {/* Add Invoice Manually */}
          <Link href="/invoices/new">
            <a className="p-6 bg-white rounded shadow hover:shadow-lg transition">
              <h3 className="text-xl font-semibold mb-2">➕ Create Invoice</h3>
              <p className="text-gray-700">Manually create GST invoices.</p>
            </a>
          </Link>

          {/* GST Reports */}
          <Link href="/gst/reports">
            <a className="p-6 bg-white rounded shadow hover:shadow-lg transition">
              <h3 className="text-xl font-semibold mb-2">📊 GST Reports</h3>
              <p className="text-gray-700">Generate monthly & quarterly GST summaries.</p>
            </a>
          </Link>

          {/* File Upload */}
          <Link href="/api/upload">
            <a className="p-6 bg-white rounded shadow hover:shadow-lg transition">
              <h3 className="text-xl font-semibold mb-2">📁 PDF / CSV Upload</h3>
              <p className="text-gray-700">
                Upload bank statements or invoices for processing.
              </p>
            </a>
          </Link>
        </div>
      </section>
    </main>
  );
}
