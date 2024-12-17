#CRYPTOTRACKER
Main Code (index.js)
import React, { useState } from "react";
import {
  LineChart,
  Line,
  XAxis,
  YAxis,
  ResponsiveContainer,
  Tooltip as ChartTooltip
} from "recharts";
import { Dialog, DialogTrigger, DialogContent, DialogHeader, DialogTitle, DialogDescription } from "@radix-ui/react-dialog";
import { ArrowUpRight, ArrowDownRight, DollarSign, Percent, BarChart2, TrendingUp, Database, Search } from "lucide-react"; // Assuming you use Lucide icons
import classNames from "classnames";

export default function CryptoTracker({ cryptoData }) {
  const [searchTerm, setSearchTerm] = useState("");

  const filteredCoins = cryptoData.filter((coin) =>
    coin.name.toLowerCase().includes(searchTerm.toLowerCase())
  );

  return (
    <div className="container mx-auto p-4">
      <div className="mb-4 relative">
        <input
          type="text"
          placeholder="Search for a coin..."
          value={searchTerm}
          onChange={(e) => setSearchTerm(e.target.value)}
          className="pl-10 bg-white dark:bg-gray-800 border-gray-300 dark:border-gray-700"
        />
        <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" size={20} />
      </div>

      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 mb-8">
        {filteredCoins.map((coin) => (
          <Dialog key={coin.id}>
            <DialogTrigger asChild>
              <div className="cursor-pointer hover:shadow-lg transition-shadow bg-white/80 dark:bg-gray-800/80 backdrop-blur-sm p-4">
                <div className="flex justify-between items-center">
                  <div className="flex items-center space-x-2">
                    <img src={`/placeholder.svg?height=32&width=32&text=${coin.symbol}`} alt={coin.name} className="h-8 w-8" />
                    <span>{coin.name} ({coin.symbol})</span>
                  </div>
                  {coin.change >= 0 ? (
                    <ArrowUpRight className="text-green-500" size={20} />
                  ) : (
                    <ArrowDownRight className="text-red-500" size={20} />
                  )}
                </div>
                <div>
                  <p className="text-2xl font-bold">${coin.price.toFixed(2)}</p>
                  <p
                    className={`text-sm ${coin.change >= 0 ? "text-green-500" : "text-red-500"}`}
                  >
                    {coin.change >= 0 ? "+" : ""}
                    {coin.change}%
                  </p>
                </div>
              </div>
            </DialogTrigger>
            <DialogContent>
              <DialogHeader>
                <DialogTitle>
                  {coin.name} ({coin.symbol})
                </DialogTitle>
                <DialogDescription>
                  Detailed information about {coin.name}.
                </DialogDescription>
              </DialogHeader>
              <div className="grid gap-4 py-4">
                {/* Coin details */}
              </div>
            </DialogContent>
          </Dialog>
        ))}
      </div>

      <div>
        <h2>Trending Coins</h2>
        <table>
          <thead>
            <tr>
              <th>Name</th>
              <th>Price</th>
              <th>24h Change</th>
              <th>Market Cap</th>
            </tr>
          </thead>
          <tbody>
            {cryptoData
              .sort((a, b) => b.change - a.change)
              .slice(0, 3)
              .map((coin) => (
                <tr key={coin.id}>
                  <td>
                    <div className="flex items-center space-x-2">
                      <img src={`/placeholder.svg?height=24&width=24&text=${coin.symbol}`} alt={coin.name} />
                      <span>{coin.name} ({coin.symbol})</span>
                    </div>
                  </td>
                  <td>${coin.price.toFixed(2)}</td>
                  <td className={coin.change >= 0 ? "text-green-500" : "text-red-500"}>
                    {coin.change >= 0 ? "+" : ""}
                    {coin.change}%
                  </td>
                  <td>${coin.marketCap}</td>
                </tr>
              ))}
          </tbody>
        </table>
      </div>
    </div>
  );
}

export async function getStaticProps() {
  // Replace with your actual API or data fetching logic
  const cryptoData = [
    // Mock data structure:
    {
      id: 1,
      name: "Bitcoin",
      symbol: "BTC",
      price: 45000,
      change: 5.5,
      marketCap: 850000000000,
      volume: 32000000000,
      circulatingSupply: 18500000,
    },
    {
      id: 2,
      name: "Ethereum",
      symbol: "ETH",
      price: 3500,
      change: -3.2,
      marketCap: 400000000000,
      volume: 20000000000,
      circulatingSupply: 116000000,
    },
    // Add more coins
  ];
  return { props: { cryptoData } };
}
package.json
{
  "name": "cryptotracker",
  "version": "1.0.0",
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "vercel-build": "npm run build"
  },
  "dependencies": {
    "next": "^13.0.0", // Replace with your desired version
    "react": "^18.0.0",
    "react-dom": "^18.0.0",
    "recharts": "^2.3.0", // Required for charts
    "@radix-ui/react-dialog": "^1.0.0", // For Dialog components
    "classnames": "^2.3.0" // For conditional class management
  }
}
