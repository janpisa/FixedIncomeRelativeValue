

# FixedIncomeRelativeValue
Python implementation of the Euro inflation-indexed bond market price decomposition proposed by Barclays. The resulting price is adjusted for the impact of seasonality and the value of the embedded deflation floor put option and, thus, given as
    
    Market Price + Seasonality - Deflation Floor = Adjusted Price

in order to provide direct comparability across bonds within the Euro linker fixed income class.

## Euro Linkers and Inflation Swaps
The Euro inflation-linked bonds (ILBs) track the [non-seasonally-adjusted Euro area HICPxT index](https://ec.europa.eu/eurostat/databrowser/view/ei_cphi_m__custom_22069970/default/table) - the identical index is designated as the underlying of the Euro area zero-coupon inflation swap market.

Issuers and obligations linked to the Euro are HICPxT index:

* France - [OAT€i](https://www.aft.gouv.fr/en/oateuroi-key-figures)

* Italy - [BTP€i](https://www.dt.mef.gov.it/en/debito_pubblico/titoli_di_stato/quali_sono_titoli/btpei/)

* Spain - [Obligación €i](https://www.tesoro.es/en/deuda-publica/valores-del-tesoro/emisiones-indexadas)

* Germany - [Inflation-Linked Federal Securities](https://www.deutsche-finanzagentur.de/en/federal-securities/types-of-federal-securities/inflation-linked-federal-securities)
## Seasonality
Nominal cash flows of ILBs are first derived from the index ratio forecast using the Euro inflation swap curve abstracting from the effect of seasonality and, afterwards, discounted by the quoted real yield-to-maturity of the bond. Second, the procedure is reiterated with the inflation swap curve capturing the impact of seasonality oscillations through a cumulative seasonality vector, consturcted using the [non-seasonally-adjusted (NSA) Euro area HICPxT index](https://ec.europa.eu/eurostat/databrowser/view/ei_cphi_m__custom_22069970/default/table) and the [seasonally-adjusted (SA) Euro area HICPxT index](https://data.ecb.europa.eu/data/datasets/HICP/HICP.M.U2.Y.X02300.4F0.INX?chart_props=W3sibm9kZUlkIjoiNDk5NzI2MSIsInByb3BlcnRpZXMiOlt7ImNvbG9ySGV4IjoiIiwiY29sb3JUeXBlIjoiIiwiY2hhcnRUeXBlIjoibGluZWNoYXJ0IiwibGluZVN0eWxlIjoiU29saWQiLCJsaW5lV2lkdGgiOiIxLjUiLCJheGlzUG9zaXRpb24iOiJsZWZ0Iiwib2JzZXJ2YXRpb25WYWx1ZSI6ZmFsc2UsImRhdGVzIjpbIjE5OTYtMDEtMzBUMjM6MDA6MDAuMDAwWiIsIjIwMjYtMDUtMzBUMjI6MDA6MDAuMDAwWiJdLCJpc1RkYXRhIjpmYWxzZSwibW9kaWZpZWRVbml0VHlwZSI6IiIsInllYXIiOiJmdWxsUmFuZ2UiLCJzdGFydERhdGUiOiIxOTk2LTAxLTMxIiwiZW5kRGF0ZSI6IjIwMjYtMDUtMzEiLCJzZXREYXRlIjp0cnVlLCJzaG93VGFibGVEYXRhIjpmYWxzZSwiY2hhbmdlTW9kZSI6ZmFsc2UsInNob3dNZW51U3R5bGVDaGFydCI6ZmFsc2UsImRpc3BsYXlNb2JpbGVDaGFydCI6dHJ1ZSwic2NyZWVuU2l6ZSI6Im1heCIsInNjcmVlbldpZHRoIjoxMjgwLCJzaG93VGRhdGEiOmZhbHNlLCJ0cmFuc2Zvcm1lZEZyZXF1ZW5jeSI6Im5vbmUiLCJ0cmFuc2Zvcm1lZFVuaXQiOiJub25lIiwiZnJlcXVlbmN5Ijoibm9uZSIsInVuaXQiOiJub25lIiwibW9kaWZpZWQiOiJmYWxzZSIsInNlcmllc0tleSI6Im1vbnRobHkiLCJzaG93dGFibGVTdGF0ZUJlZm9yZU1heFNjcmVlbiI6ZmFsc2UsImlzZGF0YWNvbXBhcmlzb24iOmZhbHNlLCJzZXJpZXNGcmVxdWVuY3kiOiJtb250aGx5IiwiaW50aWFsU2VyaWVzRnJlcXVlbmN5IjoibW9udGhseSIsIm1ldGFkYXRhRGVjaW1hbCI6IjIiLCJpc1RhYmxlU29ydGVkIjpmYWxzZSwiaXNZZWFybHlUZGF0YSI6ZmFsc2UsInJlc3BvbnNlRGF0YUVuZERhdGUiOiIyMDI2LTA1LTMxIiwiaXNpbml0aWFsQ2hhcnREYXRhIjp0cnVlLCJpc0RhdGVzRnJvbURhdGVQaWNrZXIiOnRydWUsImRhdGVQaWNrZXJFbmREYXRlIjoiMjAyNi0wNS0zMSIsImlzRGF0ZVBpY2tlckVuZERhdGUiOnRydWUsInNlcmllc2tleVNldCI6IiIsImRhdGFzZXRJZCI6IjQyNSIsImlzQ2FsbGJhY2siOmZhbHNlLCJpc1NsaWRlclRkYXRhIjp0cnVlLCJpc1NsaWRlckRhdGEiOnRydWUsImlzSW5pdGlhbENoYXJ0RGF0YUZyb21HcmFwaCI6dHJ1ZSwiY2hhcnRTZXJpZXNLZXkiOiJISUNQLk0uVTIuWS5YMDIzMDAuNEYwLklOWCIsInR5cGVPZiI6IiJ9XX1d), such that 

    monthly seasonality factor = NSA index/SA index.

The difference between the two present values comprises the seasonality adjustment.
## Deflation Floor
A deflation floor put option is embedded in the Euro ILBs granted by the fact that the inflation-indexed principal repayment amount cannot fall below the original par value (corresponding to the dated date of inflation index ratio of 1). The option is priced using the log-normal (1976) Black Scholes model.
## Complementary Files
Additionally, the repository includes a detailed Excel deflation floor put option value calculator (deflation_floor.xlsx) and a zero SOFR curve boostrapped from quoted SOFR swap contracts, replicating the Bloomberg Terminal methodology (ois_sofr_swap_curve.ipynb).


## References
The inflation-indexed products guide by Barclays, proposing the Euro linker relative value methodolgy:

    Pond, M. (2019). Global Inflation-Linked Products: A User’s Guide. Barclays Capital.
