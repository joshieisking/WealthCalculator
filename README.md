# Wealth Planner

A personal finance projection tool for planning your path to retirement and financial independence.

## What it does

Enter your financial details and get an interactive chart showing how your wealth grows over time, along with key metrics:

- **Wealth at retirement** — projected portfolio value when you stop working
- **Years to target** — how long until you hit your wealth goal
- **Buffer vs target** — how far ahead or behind you are

## Features

- **Growth chart** — visualizes your portfolio trajectory year by year, with milestone bands and a target line
- **Coast FI lens** — calculates the age at which you can stop contributing and still coast to your retirement goal
- **Withdrawal rate lens** — shows your expected portfolio duration and real (inflation-adjusted) returns in retirement
- **Scenario comparison** — compare your base plan against an alternative (different contribution, retirement age, or return rate)
- **Loan awareness** — accounts for mortgage and car loan end dates, which affect your contribution capacity
- **Education costs** — factors in per-child university expenses across a 4-year window
- **Persistent inputs** — all your numbers are saved to localStorage, so they're there when you come back

## Inputs

| Field | Description |
|---|---|
| Current age / Retirement age | Your timeline |
| Current portfolio | Starting wealth |
| Monthly contribution | How much you invest each month |
| Target wealth | Your FI number |
| Per-child uni cost | University fees per child over 4 years |
| Home mortgage / Car loan end | Years when loan repayments stop |
| Annual return | Expected nominal return rate |
| Inflation | Expected inflation rate |

## Stack

Single-file HTML/CSS/JS app. No build step. Uses [Chart.js](https://www.chartjs.org/) for the chart and Google Fonts for typography.

## Running locally

Open `index.html` in a browser — no server required.

## Deployment

Hosted on GitHub Pages from the `main` branch.
