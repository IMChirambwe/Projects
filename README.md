# Asian Arithmetic Option Pricing: Johnson Moment Matching

Implementation of the Posner & Milevsky (1998) four-moment state-price density approximation for pricing arithmetic Asian call options. Arithmetic averages of lognormal prices are not themselves lognormal, so standard Black-Scholes has no clean closed form. This project fits a Johnson distribution to the first four moments of the arithmetic average and prices via Gauss-Hermite quadrature.

## Method Explained

1. Compute the first four raw moments of the arithmetic average analytically under risk-neutral GBM.
2. Convert to mean, variance, skewness, and kurtosis.
3. Fit a Johnson SU or shifted lognormal (SL) distribution by matching skewness and kurtosis; recover location-scale parameters to match mean and standard deviation.
4. Price the call as a discounted expectation under the fitted Johnson SPD using 80-point Gauss-Hermite quadrature.
5. Benchmark against Monte Carlo.

## Results

Daily-monitored ATM call (S=100, K=100, r=5%, σ=20%, T=1yr, 252 dates, 100k paths):

| Method | Price |
|---|---:|
| Monte Carlo | 5.766916 |
| Johnson 4M | 5.739409 |
| Error | 0.48% |

Replication of Posner & Milevsky Tables 2 and 4 shows errors below 1% across strike and volatility levels. Johnson SU is selected in every case.

## Repository

```
.
├── Asian Options SPD.ipynb          # Full implementation
├── Asian Option Pricing with SPD.pdf  # Project report
└── README.md
```

## Notebook structure

| Section | What it does |
|---|---|
| Gauss-Hermite Quadrature | Sets up 80-point quadrature nodes and weights |
| GBM Product Moments | Analytical product moments via combinations with replacement |
| Johnson Moment Matching | Fits SU/SL by optimising skewness and kurtosis residuals |
| 4M Price | Discounted expectation under fitted Johnson SPD |
| Monte Carlo Benchmark | GBM path simulation with antithetic-free baseline |
| Table 2 Replication | Weekly averaging, σ ∈ {30%, 50%}, K ∈ {90, 100, 110} |
| Table 4 Replication | Quarterly and monthly averaging, currency option parameters |
| Daily Example | ATM call, 252 monitoring dates |
| Johnson Fit Plot | Fitted density vs. simulated arithmetic average histogram |

## Setup

```bash
pip install numpy pandas matplotlib scipy jupyter
jupyter notebook
```

Open `Asian Options SPD.ipynb` and run top to bottom. 
## Reference

Posner, S. E. and Milevsky, M. A. (1998). *Valuing Exotic Options by Approximating the SPD with Higher Moments*. Journal of Financial Engineering, 7(2), 109–125.
