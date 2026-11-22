# US container drayage rates (Transportli)

> Also on Hugging Face: <https://huggingface.co/datasets/Transportli/us-container-drayage-rates>. Canonical source and explanations: <https://transportli.com/blog/drayage-rates-study/>. Releases of this repository are archived on Zenodo with a DOI.

Five small tables of **measured** container drayage (port and rail-ramp trucking) prices from [Transportli](https://transportli.com/), a US marketplace that matches shipments with vetted carriers. They summarise 18,000 carrier-posted quotes at nine US terminal areas (11,065 of them for moves out of the terminal), with the latest quote dated November 18, 2026. The tables are aggregates only. Every row carries a `source_url` that points to the Transportli page that explains the numbers.

- Data through: **November 18, 2026**
- License: **CC BY 4.0**. Please credit **Transportli** and link to the `source_url` of the table you use (see Citation).
- Site: <https://transportli.com/> · Drayage guide: <https://transportli.com/drayage-services/> · Study: <https://transportli.com/blog/drayage-rates-study/> · Fees guide: <https://transportli.com/drayage-services/drayage-fees/> · Data license: <https://transportli.com/data-license/>
- Companion datasets: [Transportli/us-car-shipping-prices](https://huggingface.co/datasets/Transportli/us-car-shipping-prices) (car shipping) and [Transportli/us-ltl-freight-prices](https://huggingface.co/datasets/Transportli/us-ltl-freight-prices) (LTL freight)

## Tables

| Config | Rows | What a row is | Source page |
|---|---|---|---|
| `dray_distance_bands` | 6 | One distance band: quotes, median total price, 25th and 75th percentile, median price per mile | [Rates study](https://transportli.com/blog/drayage-rates-study/) |
| `dray_terminal_band_medians` | 51 | One terminal and distance band with at least 50 quotes: median total price | [Rates study](https://transportli.com/blog/drayage-rates-study/) |
| `dray_terminal_summary` | 9 | One terminal area: number of quotes, how many list no separate fuel surcharge, median fuel surcharge when listed, date range | [Rates study](https://transportli.com/blog/drayage-rates-study/) |
| `dray_fee_figures` | 6 | One accessorial fee type seen in quote notes: typical unit, number of figures, median and quartiles, most common amounts | [Fees guide](https://transportli.com/drayage-services/drayage-fees/) |
| `dray_summary` | 15 | One headline metric with its definition (price fit by distance, 20 ft versus 40 ft, fuel surcharge, spread between carriers) | [Rates study](https://transportli.com/blog/drayage-rates-study/) |

```python
# From Hugging Face (or read the CSVs in data/ directly)
from datasets import load_dataset
ds = load_dataset("Transportli/us-container-drayage-rates", "dray_distance_bands", split="train")
print(ds.to_pandas())
```

## How the numbers are made

The source is a public rate-history listing of drayage quotes that carriers posted for nine terminal areas: Los Angeles / Long Beach, Oakland / San Francisco, Seattle / Tacoma, Houston, New York / New Jersey, Savannah, Norfolk, Miami / Fort Lauderdale and Chicago rail ramps. We collected up to 2,000 of the newest quotes per terminal (18,000 in total, from about 1,700 anonymous carrier codes). Prices and distances are **not** adjusted. Quote-level rows, lane-level rows, carrier identifiers and the listing itself are **not** published here.

- **Total price** is the base rate plus any fuel surcharge the quote lists. Accessorials (chassis, pre-pull, storage, detention, tolls and so on) are separate and are in `dray_fee_figures`.
- **Distance** is the one-way road distance shown with each quote, in miles from the terminal to the delivery point. The tables use import moves (out of the terminal).
- `dray_terminal_band_medians` only includes terminal and distance-band cells with at least 50 quotes.
- `dray_summary`: the fit is total price = intercept + slope x miles for moves up to 250 miles, over lanes with at least 5 quotes from at least 3 carriers. The 20 ft versus 40 ft and return versus outbound ratios compare medians on the same lane. The spread figures use lanes with at least 6 carriers.
- `dray_fee_figures`: dollar figures found in the free-text notes of quotes. The notes are cut short, so the counts are floors, and the unit (per day, per hour, once) is the usual one for that fee, not read from each note.

## Limits

- These are **posted quotes, not executed loads or contracted rates.** A carrier's final price depends on the container, chassis, appointment, accessorials and the shipper's volume.
- About half of the quotes list no separate fuel surcharge, so compare all-in totals, not base rates.
- Terminals cover different date ranges (from February to November 2026 depending on the terminal), and the lane mix differs by terminal and by month, so month-to-month trends are **not** published and the tables should not be read as a time series.
- Lanes are limited to nine terminal areas, and the newest 2,000 quotes per terminal, so busy ports and short moves are over-represented.

## Citation

> Transportli, *US Container Drayage Rates*, data through November 18, 2026. <https://transportli.com/blog/drayage-rates-study/>

```bibtex
@misc{transportli_drayage_rates_2026,
  title  = {US Container Drayage Rates},
  author = {Transportli},
  year   = {2026},
  url    = {https://transportli.com/blog/drayage-rates-study/},
  note   = {Data through November 18, 2026. Hugging Face dataset Transportli/us-container-drayage-rates}
}
```

## About Transportli

Transportli is a marketplace that connects customers with independent, vetted carriers. It is not a carrier and does not move freight itself, and it earns revenue by matching a shipment with a carrier. Questions and corrections: <https://transportli.com/contact-us/>.

## Changelog

- November 18, 2026: first release (five tables).
