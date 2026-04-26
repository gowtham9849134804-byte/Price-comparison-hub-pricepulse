# Price-comparison-hub-pricepulse
{
  "name": "price-compare-hub",
  "private": true,
  "version": "1.0.0",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "recharts": "^2.8.0"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.0.0",
    "vite": "^5.0.0",
    "tailwindcss": "^3.4.0",
    "postcss": "^8.4.0",
    "autoprefixer": "^10.4.0"
  }
}
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
})
export default {
  content: ["./index.html", "./src/**/*.{js,jsx}"],
  theme: { extend: {} },
  plugins: [],
}
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}

<!DOCTYPE html>
<html>
  <head>
    <title>Price Compare Hub</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>
JSX
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "./index.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
@tailwind base;
@tailwind components;
@tailwind utilities;
import {
  BarChart,
  Bar,
  XAxis,
  YAxis,
  Tooltip,
  PieChart,
  Pie,
  Cell,
  ResponsiveContainer,
  Legend
} from "recharts";

export default function Dashboard({ data }) {
  const colors = ["#22c55e", "#3b82f6", "#f59e0b"];

  return (
    <div className="grid md:grid-cols-2 gap-6 mt-10">
      
      <div className="bg-gray-800 p-4 rounded">
        <h2 className="mb-2">Price Comparison</h2>
        <ResponsiveContainer width="100%" height={250}>
          <BarChart data={data}>
            <XAxis dataKey="site" stroke="#fff" />
            <YAxis stroke="#fff" />
            <Tooltip />
            <Bar dataKey="price" fill="#3b82f6" />
          </BarChart>
        </ResponsiveContainer>
      </div>

      <div className="bg-gray-800 p-4 rounded">
        <h2 className="mb-2">Ratings</h2>
        <ResponsiveContainer width="100%" height={250}>
          <PieChart>
            <Pie data={data} dataKey="rating" nameKey="site" outerRadius={80} label>
              {data.map((_, i) => (
                <Cell key={i} fill={colors[i % colors.length]} />
              ))}
            </Pie>
            <Legend />
          </PieChart>
        </ResponsiveContainer>
      </div>

    </div>
  );
}
import { useState } from "react";
import Dashboard from "./components/Dashboard";

const mockData = [
  {
    name: "iPhone 14",
    price: 70000,
    site: "Amazon",
    rating: 4.5,
    reviews: 1200
  },
  {
    name: "iPhone 14",
    price: 68000,
    site: "Flipkart",
    rating: 4.4,
    reviews: 900
  },
  {
    name: "iPhone 14",
    price: 72000,
    site: "Myntra",
    rating: 4.2,
    reviews: 600
  }
];

export default function App() {
  const [search, setSearch] = useState("");
  const [results, setResults] = useState([]);

  const handleSearch = () => {
    const filtered = mockData
      .filter(p => p.name.toLowerCase().includes(search.toLowerCase()))
      .sort((a, b) => a.price - b.price);

    setResults(filtered);
  };

  return (
    <div className="min-h-screen bg-gray-900 text-white p-6">

      <h1 className="text-3xl text-center mb-6">
        Real-Time Price Comparison Hub
      </h1>

      <div className="flex justify-center gap-2 mb-6">
        <input
          className="p-2 rounded text-black"
          placeholder="Search product..."
          onChange={(e) => setSearch(e.target.value)}
        />
        <button
          onClick={handleSearch}
          className="bg-blue-500 px-4 py-2 rounded"
        >
          Search
        </button>
      </div>

      <div className="grid md:grid-cols-2 gap-4">
        {results.map((p, i) => (
          <div key={i} className="bg-gray-800 p-4 rounded">
            <h2>{p.name}</h2>
            <p>₹{p.price}</p>
            <p>{p.site}</p>
            <p>⭐ {p.rating} ({p.reviews})</p>
            {i === 0 && <span className="text-green-400">Best Deal</span>}
          </div>
        ))}
      </div>

      {results.length > 0 && <Dashboard data={results} />}

    </div>
  );
}
