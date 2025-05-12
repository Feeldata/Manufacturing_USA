# Manufacturing_USA
How vulnerable the biggest Manufacturing Sectors are, and what are their biggest risks.

-- 
# Manufacturing Sector Analysis SQL Queries

This repository contains SQL queries used to analyze key U.S. manufacturing sectors for supply chain resilience assessment.

## Table of Contents
- [Overview](#overview)
- [Sector Input Analysis](#sector-input-analysis)
- [Top Inputs Table Creation](#top-inputs-table-creation)

## Overview

These SQL queries analyze five key manufacturing sectors:
- Food Manufacturing (NAICS 311)
- Chemical Manufacturing (NAICS 325)
- Fabricated Metal Product Manufacturing (NAICS 332)
- Machinery Manufacturing (NAICS 333)
- Transportation Equipment Manufacturing (NAICS 336)

## Sector Input Analysis

### Food Manufacturing (311)
```sql
-- Find the top 3 Inputs for Food Manufacturing 311
SELECT Code, `commodity description`, `total use of products`
FROM top_input
WHERE Code LIKE '311%'
ORDER BY CAST(`total use of products` AS UNSIGNED) DESC;
```

### Transportation Equipment Manufacturing (336)
```sql
-- For Transportation Equipment Manufacturing 336
SELECT Code, `commodity description`, `total use of products`
FROM top_input
WHERE Code LIKE '336%'
ORDER BY CAST(`total use of products` AS UNSIGNED) DESC;
```

### Fabricated Metal Product Manufacturing (332)
```sql
-- For Fabricated Metal Product Manufacturing 332
SELECT Code, `commodity description`, `total use of products`
FROM top_input
WHERE Code LIKE '332%'
ORDER BY CAST(`total use of products` AS UNSIGNED) DESC;
```

### Machinery Manufacturing (333)
```sql
-- For Machinery Manufacturing 333
SELECT Code, `commodity description`, `total use of products`
FROM top_input
WHERE Code LIKE '333%'
ORDER BY CAST(`total use of products` AS UNSIGNED) DESC;
```

### Chemical Manufacturing (325)
```sql
-- For Chemical Manufacturing 325
SELECT Code, `commodity description`, `total use of products`
FROM top_input
WHERE Code LIKE '325%'
ORDER BY CAST(`total use of products` AS UNSIGNED) DESC;
```

## Top Inputs Table Creation

```sql
-- Create a table with top inputs for the 5 sectors
CREATE TABLE top_inputs_by_subsector AS
SELECT
    code AS naics_code,
    `Commodity Description`,
    CAST(REPLACE(`Total use of products`, ',', '') AS UNSIGNED) AS total_qty
FROM (
    SELECT
        code,
        `Commodity Description`,
        `Total use of products`,
        ROW_NUMBER() OVER (
            PARTITION BY LEFT(code, 3)
            ORDER BY CAST(REPLACE(`Total use of products`, ',', '') AS UNSIGNED) DESC
        ) AS rn
    FROM
        top_input
    WHERE
        LEFT(code, 3) IN ('311', '336', '332', '333', '325')
) ranked
WHERE rn  'United States'
ORDER BY 
    CAST(Establishments AS UNSIGNED) DESC;
```

## Notes

- The concordance mapping was found to be incomplete and will be supplemented with manual checks using the HTS tool (hts.usitc.gov).
- A data flow map was created to better visualize data relationships and identify key retrieval points.

---

![DATA FLOW](https://github.com/user-attachments/assets/4f83d47c-1b98-4d02-ad21-545be6197c6b)

The final result is this

![MANUFACTURING INFOGRAPHIC](https://github.com/user-attachments/assets/997f9f49-24e1-47e2-8e3b-1630c3c6b9b1)
--
![336](https://github.com/user-attachments/assets/0fceb21f-4522-4761-acda-80dc50521530)

--
And here is an insight into my logic for the project as well as the work log.

# Worklog

## Project Timeline and Progress

### Monday, April 21, 2025
Started by establishing key questions from stakeholder requirements on manufacturing resilience. Given the current trade climate with tariffs, I aimed to analyze how resilient the biggest manufacturing sectors in the US are in the face of trade tensions.

- Obtained manufacturing sector data with NAICS codes from Census Bureau
- Filtered by employment, isolated USA data, and selected five largest sectors by employment:
  - Transportation Equipment Manufacturing (336)
  - Food Manufacturing (311)
  - Fabricated Metal Product Manufacturing (332)
  - Machinery Manufacturing (333)
  - Chemical Manufacturing (325)

### Wednesday, April 23, 2025
- Gathered National Risk Index data from resilience.climate.gov
- Downloaded KML file for geographic visualization
- Collected trade data from USITC DataWeb (general imports 2020-2025)
- Obtained tariff rates for 2025 from USITC
- Retrieved Annual Survey of Manufactures Benchmark data
- Developed methodology to calculate domestic production and import reliance
- Classified data into raw data and project data buckets
- Defined key KPIs for analysis:
  - Domestic production (total cost materials + value added)
  - Import/export values
  - Import reliance percentage
  - Concentration index
  - Top imports
  - National Risk Index
  - Sector size
  - Tariffs
  - Input dependency metrics

### Thursday, April 24, 2025
- Encountered issues importing large files into SQL
- Trimmed data in Excel to focus on relevant NAICS codes
- Successfully transferred data to SQL environment
- Set up database and prepared for analysis

### Friday, April 25, 2025
- Completed importing all Excel files into SQL
- Began learning about HS codes and mapping methodology
- Planned approach for establishing reliance relationships

### Tuesday, April 29, 2025
- Created Excel sheet for trade/tariff data including:
  - Imports, exports, inputs, HS codes, tariff rates, major partners
- Developed methodology for mapping imports to HS codes using:
  - USITC NAICS-HS Concordance
  - Product description validation
  - USITC Trade DataWeb
  - BEA's Use Table
  - HS Database verification
  - Customs rulings consultation
- Established process for mapping inputs to HS codes and calculating tariff impacts

### Monday, May 5, 2025
- Analyzed "Use of Commodities by Industry" data to identify top inputs
- Found domestic quantity, import quantity, and total inputs in supply data
- Prepared to calculate import reliance and tariff impacts
- Imported both top input and domestic supply data to SQL

### Tuesday, May 6, 2025
- Listed top inputs for each manufacturing sector
- Began mapping inputs to HS codes to determine tariff impacts
- Encountered challenges with concordance mapping (one-to-multiple or one-to-none relationships)
- Pivoted to manual verification using HTS tool

## Data Sources Utilized

- Census Bureau manufacturing data
- USITC trade and tariff information
- BEA Input-Output accounts
- FEMA/NOAA climate risk assessments
- Industry concentration metrics
