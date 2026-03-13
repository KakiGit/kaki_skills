---
name: trip-planner
description: Search and compare flights and train/rail services for travel between China and Europe. Use when the user asks to find flights, search for train tickets, plan travel itineraries, compare travel prices, or needs trip recommendations between China and Europe. Handles both business and leisure travel planning with round-trip/one-way comparisons. Covers airlines, high-speed rail (China), and European rail services.
---

# Trip Planner Skill

Search and plan trips between China and Europe, comparing options across airlines, train services, and aggregators to find the best routes and prices.

## Supported Transportation

### Airlines

#### Chinese Airlines (中国航空公司)
| Airline | Code | Type | Website |
|---------|------|------|---------|
| Air China (国航) | CA | Full Service | airchina.com |
| China Eastern (东航) | MU | Full Service | ceair.com |
| China Southern (南航) | CZ | Full Service | csair.com |
| Xiamen Airlines (厦航) | MF | Full Service | xiamenair.com |
| Sichuan Airlines (川航) | 3U | Full Service | scal.com.cn |
| Hainan Airlines (海航) | HU | Full Service | hainanairlines.com |
| Juneyao Airlines (春秋航空) | 9C | Budget | juneyao.com |
| Spring Airlines (吉祥航空) | 9H | Budget | spring-airlines.com |

#### European Airlines (欧洲航空公司)
| Airline | Code | Type | Country |
|---------|------|------|---------|
| Lufthansa | LH | Full Service | Germany |
| Air France | AF | Full Service | France |
| British Airways | BA | Full Service | UK |
| KLM | KL | Full Service | Netherlands |
| Finnair | AY | Full Service | Finland |
| Swiss International | LX | Full Service | Switzerland |
| Austrian Airlines | OS | Full Service | Austria |
| Brussels Airlines | SN | Full Service | Belgium |
| Iberia | IB | Full Service | Spain |
| TAP Air Portugal | TP | Full Service | Portugal |
| Scandinavian Airlines | SK | Full Service | Scandinavia |
| LOT Polish Airlines | LO | Full Service | Poland |
| Ryanair | FR | Budget | Ireland/Europe |
| easyJet | U2 | Budget | UK/Europe |
| Wizz Air | W6 | Budget | Hungary |
| Vueling | VY | Budget | Spain |
| Eurowings | EW | Budget | Germany |
| Norwegian Air | DY | Budget | Norway |

### Train/Rail Services

#### China High-Speed Rail (中国高铁)
| Service | Code | Routes | Website |
|---------|------|--------|---------|
| China Railway (国铁) | CR | All domestic routes | 12306.com / ctrip.com |

**Note:** 12306 may block automated requests. Use 携程 (Ctrip) as primary source for Chinese train bookings and schedules.

#### European Rail Services
| Service | Country/Region | Website |
|---------|----------------|---------|
| Deutsche Bahn (DB) | Germany | bahn.com |
| SNCF | France | sncf.com |
| Eurostar | UK/France/Belgium | eurostar.com |
| Trenitalia | Italy | trenitalia.com |
| ÖBB | Austria | oebb.at |
| SBB | Switzerland | sbb.ch |
| NS | Netherlands | ns.nl |
| NMBS | Belgium | nmbs.be |
| Renfe | Spain | renfe.com |
| Comboios de Portugal | Portugal | cp.pt |
| PKP Intercity | Poland | pkp.pl |
| VR | Finland | vr.fi |
| MÁV-START | Hungary | mav.hu |
| Czech Railways (ČD) | Czech Republic | cd.cz |

#### European Rail Passes & Aggregators
| Service | Description | Website |
|---------|-------------|---------|
| Eurail Pass | Multi-country rail pass | eurail.com |
| Interrail | Multi-country rail pass (EU residents) | interrail.eu |
| Trainline | Europe-wide rail aggregator | trainline.eu |
| Rail Europe | Europe rail tickets | raileurope.com |
| Omio | Multi-modal travel search | omio.com |

## Trip Search Workflow

### Step 1: Parse User Input

Extract from user's travel plan:
- **Origin city** (出发城市) - e.g., 北京, Shanghai, Helsinki
- **Destination city** (目的城市) - e.g., 伦敦, Paris, 北京
- **Departure date** (出发日期) - format: YYYY-MM-DD or relative (next Monday)
- **Return date** (返程日期) - for round trip, or "one way" / "单程"
- **Passengers** (乘客数) - number of travelers
- **Travel class** (舱位/等级) - economy, business, first (default: economy)
- **Flexible dates** (灵活日期) - ±3 days flexibility
- **Preferred mode** - flight, train, or both (default: both)

### Step 1.5: Handle Unavailable Dates

**If no flights/trains available on requested dates:**

1. **Search nearby dates automatically:**
   - Try ±1 day from requested departure (yesterday/tomorrow)
   - Try ±3 days from requested departure
   - Try ±7 days from requested departure
   - For round trips, also adjust return date proportionally

2. **Check for seasonal closures:**
   - Some routes may be suspended seasonally
   - Note if route is not operating on requested dates

3. **Report findings clearly:**
   - Inform user that original dates have no availability
   - Show best options from nearby dates
   - Highlight price differences between dates
   - Suggest alternative nearby airports/stations if relevant

### Step 2: Initial Search via Aggregators

Use these platforms first for comprehensive results:

**For Flights:**
1. **Google Flights** (google.com/flights)
   - Enter origin and destination
   - Set dates and passengers
   - Note: shows multiple airlines and prices

2. **Skyscanner** (skyscanner.com)
   - Good for comparing budget carriers
   - Use "Whole month" view for best prices

3. **携程 (Ctrip)** (ctrip.com)
   - Best for Chinese domestic flights and China-EU routes
   - Shows Chinese airline options clearly

**For Trains:**

1. **携程 (Ctrip)** (ctrip.com) - PRIMARY for China
   - Use for Chinese train schedules and bookings
   - 12306 may block requests; Ctrip is more reliable

2. **Trainline** (trainline.eu) - PRIMARY for Europe
   - Aggregates European rail services
   - Shows prices across multiple operators

3. **Omio** (omio.com)
   - Good for comparing train vs flight prices
   - Covers both European and some international routes

4. **Rail Europe** (raileurope.com)
   - Good for international European routes
   - Also sells rail passes

### Step 3: Deep Dive on Regional Providers

**For Flights:**
- Check specific airlines based on routes for better deals
- China → Europe: Air China, China Eastern, China Southern, Hainan Airlines
- Europe → China: Lufthansa, Air France, British Airways, Finnair

**For Trains:**
- Check national railway websites for better deals
- Germany: Deutsche Bahn
- France: SNCF
- UK: Eurostar
- China: Use 携程 (Ctrip) as primary, 12306 backup

### Step 4: Compare Flight vs Train

For routes where both options exist, compare:
- **Price** - train often cheaper for similar travel times
- **Duration** - flights faster for long distances
- **Convenience** - city center vs airport location
- **Environmental impact** - trains more eco-friendly

### Step 5: Compare Round Trip vs One Way

**Always calculate both:**
1. Round trip ticket price (往返票)
2. Two one-way tickets (两张单程票)

For flights: Sometimes combining different airlines for outbound and return is cheaper.
For trains: Single tickets sometimes cheaper than return, or different operators.

## Report Format

Generate a clear markdown report:

**If original dates unavailable, start with this notice:**

```markdown
> ⚠️ **No availability on requested dates ([Date Range])**
> Showing options for nearby dates. Best alternatives below:
```

```markdown
# 🛫 Trip Planner Results

## Trip Overview
- **Route:** [Origin] → [Destination]
- **Dates:** [Departure] → [Return]
- **Passengers:** [N]
- **Class:** [Economy/Business]

---

## ✈️ Flight Options (按价格排序)

### Option 1: [Airline Name] - $XXX
- **Type:** Direct / Connecting (X stops)
- **Duration:** Xh Xm
- **Route:** [Flight path]
- **Book here:** [Link]

### Option 2: [Airline Name] - $YYY
- ...

---

## 🚄 Train Options (按价格排序)

### Option 1: [Rail Service] - €XXX
- **Type:** High-speed / Regional / Night
- **Duration:** Xh Xm
- **Route:** [Train path]
- **Departure/Arrival:** [Times]
- **Book here:** [Link]

### Option 2: [Rail Service] - €YYY
- ...

---

## ✈️ vs 🚄 Comparison

| Option | Price | Duration | Notes |
|--------|-------|----------|-------|
| Flight | $XXX | Xh Xm | Faster, airport transfer |
| Train | €XXX | Xh Xm | City center to center |

---

## Round Trip vs One Way

| Option | Price | Notes |
|--------|-------|-------|
| Round Trip | $XXX | Best value |
| 2x One Way | $YYY | Different operators |

---

## Tips & Notes
- [Any booking tips, visa requirements, etc.]
- Prices subject to change
- Check baggage allowance (flights)
- Rail passes may be worth it for multiple journeys
- **Date flexibility tip:** Prices can vary significantly by ±1-3 days; mid-week travel often cheaper

---

*Generated on [Date] | Prices in [Currency]*
```

## Important Notes

1. **Always include booking links** in the report - this is critical
2. **For Chinese trains:** Use 携程 (Ctrip) as primary source since 12306 may block automated requests
3. **Verify prices** - aggregator prices may differ from operator direct
4. **Check baggage allowance** - especially for budget carriers and some trains
5. **Visa requirements** - mention if transit visa needed for connections
6. **Currency** - show prices in both local currency and USD/EUR for reference
7. **Consider overnight trains** - can save on accommodation while traveling

## Tools Used

- **web_browser**: Navigate travel websites and aggregators
- **web_fetch**: Get additional information from URLs

## Success Criteria

The skill should produce a report that:
1. Clearly shows flight options with prices and booking links
2. Clearly shows train options with prices and booking links
3. Compares flight vs train tradeoffs
4. Includes both round trip and one-way options
5. Is easy to scan and understand at a glance
