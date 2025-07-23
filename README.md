# HHS npi parser
Extracts meaningful provider (individual and organization) records from a National Provider Identifer (NPI) csv data file provided by the HHS National Plan and Provider Enumeration System (NPPES). See https://download.cms.gov/nppes/NPI_Files.html, specifically under the section titled "Monthly NPPES Downloadable File".

The csv appears to be an amalgamation of database tables yielding spurious tuples and extensive data bloat in the form of empty columns and cells creating mostly empty rows.  The csv file has close to 330 columns and as of June 2025 it's about 10GB!!! This software parses that file and cherry picks a subgroup of rows and columns or cells within those rows based on customizable business logic and produces xlsx files containing the results.
For example, you may only want to see individual providers (or vice versa), we can filter out all of the organizational providers.  You may also want to see individual providers by state, we can create an excel file of providers where each sheet represents the state associated with one of the many addresses provided in the csv.

Aapplication logic/features
===========================================
- Drop all rows missing an entity type code.
- Drop all rows with a non US country code.
- Drop all rows missing both a clinic address state and "provider license number state code_1".
- We allocate providers to states based on either the state abbreviation of the provider clinic address state or the provider's first license number state in the case that the provider business state is not a recognized state.  Currently, if we can't find a state we drop the provider row.
-- If we find the state, we put the provider on a sheet associated with the state represented by the value in the csv state column (provider business or first identifier state TODO verify).
- We create two xlsx files, one for individual providers and one for organizational providers.

# NBER npi parser
Extracts meaningful provider (individual and organization) records from NBER data that was correlated (via NPI) with state provider license data, HHS NPPES data and provider taxonomy data.  See https://www.nber.org/research/data/national-plan-and-provider-enumeration-system-nppes.
The NBER data is richer than the HHS NPPES data but older.  HHS NPPES is published monthly whereas the NBER data was last published May of 2024.  Unlike the HHS NPPES data, the NBER data captures provider state licensing and sole proprietor status, I believe both capture provider taxonom(ies).

Application logic/features
===========================================
- We load all NPI to taxonomy records where the taxonomy code matches the subset of codes we care about.
- We load all NPI to licensed state records where the NPI is also associated with a taxonomy we care about.
- Only capture providers that are registered with taxonomy codes we care about.
- Only capture providers that are licensed to practice in at least one state.  Providers can appear under more than one state worksheet.
- We create two xlsx files, one for captured individual providers and one for captured organizational providers.
