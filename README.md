# CRYPTOTRACKER
import { useState, useEffect } from 'react'
import { Search, TrendingUp, DollarSign, Percent, ArrowUpRight, ArrowDownRight, Moon, Sun, BarChart2, Coins, Database } from 'lucide-react'
import { Input } from "@/components/ui/input"
import { Button } from "@/components/ui/button"
import { Card, CardContent, CardDescription, CardFooter, CardHeader, CardTitle } from "@/components/ui/card"
import { Switch } from "@/components/ui/switch"
import { Dialog, DialogContent, DialogDescription, DialogHeader, DialogTitle, DialogTrigger } from "@/components/ui/dialog"
import { Table, TableBody, TableCell, TableHead, TableHeader, TableRow } from "@/components/ui/table"
import { LineChart, Line, XAxis, YAxis, Tooltip, ResponsiveContainer } from 'recharts'
import { ChartContainer, ChartTooltip, ChartTooltipContent } from "@/components/ui/chart"
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar"

// Function to generate random historical data
const generateHistoricalData = (basePrice, days = 30) => {
  const data = []
  let currentPrice = basePrice
  for (let i = days; i > 0; i--) {
    const date = new Date()
    date.setDate(date.getDate() - i)
    const change = (Math.random() - 0.5) * 0.05 // +/- 5% daily change
    currentPrice = currentPrice * (1 + change)
    data.push({
      date: date.toISOString().split('T')[0],
      price: currentPrice
    })
  }
  return data
}

// Expanded simulated cryptocurrency data
const cryptoData = [
  { id: 1, name: 'Bitcoin', symbol: 'BTC', price: 29324.52, change: 1.2, marketCap: 570000000000, volume: 15000000000, circulatingSupply: 19000000, totalSupply: 21000000, historicalData: generateHistoricalData(29324.52) },
  { id: 2, name: 'Ethereum', symbol: 'ETH', price: 1869.43, change: -0.8, marketCap: 224000000000, volume: 8000000000, circulatingSupply: 120000000, totalSupply: null, historicalData: generateHistoricalData(1869.43) },
  { id: 3, name: 'Cardano', symbol: 'ADA', price: 0.2781, change: 2.5, marketCap: 9700000000, volume: 200000000, circulatingSupply: 35000000000, totalSupply: 45000000000, historicalData: generateHistoricalData(0.2781) },
  { id: 4, name: 'Dogecoin', symbol: 'DOGE', price: 0.0624, change: -1.5, marketCap: 8700000000, volume: 300000000, circulatingSupply: 140000000000, totalSupply: null, historicalData: generateHistoricalData(0.0624) },
  { id: 5, name: 'Ripple', symbol: 'XRP', price: 0.4632, change: 0.7, marketCap: 24000000000, volume: 1000000000, circulatingSupply: 52000000000, totalSupply: 100000000000, historicalData: generateHistoricalData(0.4632) },
]

export default function Component() {
  const [searchTerm, setSearchTerm] = useState('')
  const [isDarkMode, setIsDarkMode] = useState(false)

  useEffect(() => {
    if (isDarkMode) {
      document.documentElement.classList.add('dark')
    } else {
      document.documentElement.classList.remove('dark')
    }
  }, [isDarkMode])

  const filteredCoins = cryptoData.filter(coin => 
    coin.name.toLowerCase().includes(searchTerm.toLowerCase()) ||
    coin.symbol.toLowerCase().includes(searchTerm.toLowerCase())
  )

  const formatLargeNumber = (num) => {
    if (num >= 1000000000) {
      return (num / 1000000000).toFixed(2) + 'B'
    } else if (num >= 1000000) {
      return (num / 1000000).toFixed(2) + 'M'
    } else {
      return num.toLocaleString()
    }
  }

  return (
    <div className={`min-h-screen ${isDarkMode ? 'dark' : ''}`}>
      <div className="min-h-screen bg-gradient-to-br from-blue-100 via-white to-purple-100 dark:from-gray-900 dark:via-gray-800 dark:to-gray-900 transition-colors duration-500">
        <div className="container mx-auto p-4">
          <div className="flex justify-between items-center mb-4">
            <h1 className="text-3xl font-bold text-gray-800 dark:text-white">Digital Coin Information</h1>
            <div className="flex items-center space-x-2">
              <Sun className="h-4 w-4 text-yellow-500 dark:text-yellow-300" />
              <Switch
                checked={isDarkMode}
                onCheckedChange={setIsDarkMode}
                aria-label="Toggle dark mode"
              />
              <Moon className="h-4 w-4 text-gray-500 dark:text-gray-300" />
            </div>
          </div>
          
          <div className="mb-4 relative">
            <Input
              type="text"
              placeholder="Search for a coin..."
              value={searchTerm}
              onChange={(e) => setSearchTerm(e.target.value)}
              className="pl-10 bg-white dark:bg-gray-800 border-gray-300 dark:border-gray-700"
            />
            <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 text-gray-400" size={20} />
          </div>

          <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 mb-8">
            {filteredCoins.map(coin => (
              <Dialog key={coin.id}>
                <DialogTrigger asChild>
                  <Card className="cursor-pointer hover:shadow-lg transition-shadow bg-white/80 dark:bg-gray-800/80 backdrop-blur-sm">
                    <CardHeader>
                      <CardTitle className="flex justify-between items-center">
                        <div className="flex items-center space-x-2">
                          <Avatar className="h-8 w-8">
                            <AvatarImage src={`/placeholder.svg?height=32&width=32&text=${coin.symbol}`} alt={coin.name} />
                            <AvatarFallback>{coin.symbol}</AvatarFallback>
                          </Avatar>
                          <span>{coin.name} ({coin.symbol})</span>
                        </div>
                        {coin.change >= 0 ? (
                          <ArrowUpRight className="text-green-500" size={20} />
                        ) : (
                          <ArrowDownRight className="text-red-500" size={20} />
                        )}
                      </CardTitle>
                    </CardHeader>
                    <CardContent>
                      <p className="text-2xl font-bold">${coin.price.toFixed(2)}</p>
                      <p className={`text-sm ${coin.change >= 0 ? 'text-green-500' : 'text-red-500'}`}>
                        {coin.change >= 0 ? '+' : ''}{coin.change}%
                      </p>
                      <div className="h-20 mt-2">
                        <ChartContainer config={{
                          price: {
                            label: "Price",
                            color: coin.change >= 0 ? "hsl(var(--chart-1))" : "hsl(var(--chart-2))",
                          },
                        }}>
                          <ResponsiveContainer width="100%" height="100%">
                            <LineChart data={coin.historicalData}>
                              <Line type="monotone" dataKey="price" stroke={`var(--color-price)`} strokeWidth={2} dot={false} />
                              <XAxis dataKey="date" hide />
                              <YAxis hide domain={['dataMin', 'dataMax']} />
                            </LineChart>
                          </ResponsiveContainer>
                        </ChartContainer>
                      </div>
                    </CardContent>
                  </Card>
                </DialogTrigger>
                <DialogContent className="sm:max-w-[425px]">
                  <DialogHeader>
                    <DialogTitle className="flex items-center space-x-2">
                      <Avatar className="h-10 w-10">
                        <AvatarImage src={`/placeholder.svg?height=40&width=40&text=${coin.symbol}`} alt={coin.name} />
                        <AvatarFallback>{coin.symbol}</AvatarFallback>
                      </Avatar>
                      <span>{coin.name} ({coin.symbol})</span>
                    </DialogTitle>
                    <DialogDescription>Detailed information about {coin.name}</DialogDescription>
                  </DialogHeader>
                  <div className="grid gap-4 py-4">
                    <div className="grid grid-cols-2 items-center gap-4">
                      <DollarSign className="justify-self-end" />
                      <div>
                        <p className="font-semibold">Price</p>
                        <p className="text-lg">${coin.price.toFixed(2)}</p>
                      </div>
                    </div>
                    <div className="grid grid-cols-2 items-center gap-4">
                      <Percent className="justify-self-end" />
                      <div>
                        <p className="font-semibold">24h Change</p>
                        <p className={`text-lg ${coin.change >= 0 ? 'text-green-500' : 'text-red-500'}`}>
                          {coin.change >= 0 ? '+' : ''}{coin.change}%
                        </p>
                      </div>
                    </div>
                    <div className="grid grid-cols-2 items-center gap-4">
                      <BarChart2 className="justify-self-end" />
                      <div>
                        <p className="font-semibold">Market Cap</p>
                        <p className="text-lg">${formatLargeNumber(coin.marketCap)}</p>
                      </div>
                    </div>
                    <div className="grid grid-cols-2 items-center gap-4">
                      <Coins className="justify-self-end" />
                      <div>
                        <p className="font-semibold">Volume (24h)</p>
                        <p className="text-lg">${formatLargeNumber(coin.volume)}</p>
                      </div>
                    </div>
                    <div className="grid grid-cols-2 items-center gap-4">
                      <Database className="justify-self-end" />
                      <div>
                        <p className="font-semibold">Circulating Supply</p>
                        <p className="text-lg">{formatLargeNumber(coin.circulatingSupply)} {coin.symbol}</p>
                      </div>
                    </div>
                    {coin.totalSupply && (
                      <div className="grid grid-cols-2 items-center gap-4">
                        <Database className="justify-self-end" />
                        <div>
                          <p className="font-semibold">Total Supply</p>
                          <p className="text-lg">{formatLargeNumber(coin.totalSupply)} {coin.symbol}</p>
                        </div>
                      </div>
                    )}
                    <div className="col-span-2 h-40">
                      <ChartContainer config={{
                        price: {
                          label: "Price",
                          color: coin.change >= 0 ? "hsl(var(--chart-1))" : "hsl(var(--chart-2))",
                        },
                      }}>
                        <ResponsiveContainer width="100%" height="100%">
                          <LineChart data={coin.historicalData}>
                            <Line type="monotone" dataKey="price" stroke={`var(--color-price)`} strokeWidth={2} />
                            <XAxis 
                              dataKey="date" 
                              tickFormatter={(value) => value.split('-')[2]}
                              tick={{ fontSize: 12 }}
                            />
                            <YAxis 
                              domain={['dataMin', 'dataMax']} 
                              tickFormatter={(value) => `$${value.toFixed(2)}`}
                              tick={{ fontSize: 12 }}
                              width={80}
                            />
                            <ChartTooltip 
                              content={<ChartTooltipContent hideLabel />} 
                              cursor={false}
                            />
                          </LineChart>
                        </ResponsiveContainer>
                      </ChartContainer>
                    </div>
                  </div>
                </DialogContent>
              </Dialog>
            ))}
          </div>

          <Card className="bg-white/90 dark:bg-gray-800/90 backdrop-blur-sm">
            <CardHeader>
              <CardTitle className="flex items-center"><TrendingUp className="mr-2" /> Trending Coins</CardTitle>
              <CardDescription>Top performing cryptocurrencies in the last 24 hours</CardDescription>
            </CardHeader>
            <CardContent>
              <Table>
                <TableHeader>
                  <TableRow>
                    <TableHead>Name</TableHead>
                    <TableHead>Price</TableHead>
                    <TableHead>24h Change</TableHead>
                    <TableHead>Market Cap</TableHead>
                  </TableRow>
                </TableHeader>
                <TableBody>
                  {cryptoData.sort((a, b) => b.change - a.change).slice(0, 3).map(coin => (
                    <TableRow key={coin.id}>
                      <TableCell className="flex items-center space-x-2">
                        <Avatar className="h-6 w-6">
                          <AvatarImage src={`/placeholder.svg?height=24&width=24&text=${coin.symbol}`} alt={coin.name} />
                          <AvatarFallback>{coin.symbol}</AvatarFallback>
                        </Avatar>
                        <span>{coin.name} ({coin.symbol})</span>
                      </TableCell>
                      <TableCell>${coin.price.toFixed(2)}</TableCell>
                      <TableCell className={coin.change >= 0 ? 'text-green-500' : 
                       'text-red-500'}>
                        {coin.change >= 0 ? '+' : ''}{coin.change}%
                      </TableCell>
                      <TableCell>${formatLargeNumber(coin.marketCap)}</TableCell>
                    </TableRow>
                  ))}
                </TableBody>
              </Table>
            </CardContent>
          </Card>
        </div>
      </div>
    </div>
  )
}
