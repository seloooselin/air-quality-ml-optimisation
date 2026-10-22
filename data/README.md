# Data access and provenance

No dataset is redistributed. The notebook expects an authorised coursework workbook at `data/sensor_data.xlsx`. Keep that file local and ignored. Obtain the exact file only from your own retained coursework materials or an authorised university source; the package does not offer a public mirror.

The inspected workbook has 3,304 rows and these columns:

`CO(GT), PT08.S1(CO), C6H6(GT), PT08.S2(NMHC), NOx(GT), PT08.S3(NOx), NO2(GT), PT08.S4(NO2), PT08.S5(O3), T, RH, AH`.

CO(GT) is the target. There are 224 values above 4.5, no missing targets, 182 missing NOx(GT) values and 184 missing NO2(GT) values, marked -999. These counts describe the private source, not newly generated model results. Its SHA-256 is recorded in the provenance notes for local identification.

The schema resembles [UCI Air Quality](https://archive.ics.uci.edu/dataset/360/air+quality), credited there to S. Vito (2008), DOI [10.24432/C59K5F](https://doi.org/10.24432/C59K5F). The university subset's selection, ordering and modification process were not recovered. UCI describes 9,358 observations, additional columns and a -200 missing marker. **Do not rename a UCI download to this workbook and assume equivalent experiments or results.**

For a separate exploratory dataset, use the official UCI page's Download control and retain its citation and terms. That is not an exact reproduction route for this coursework. As reviewed on 22 October 2026, the UCI page states CC BY 4.0 but also retains research-only/noncommercial wording in its description. This package does not resolve that inconsistency or infer redistribution rights over the university-modified workbook.
