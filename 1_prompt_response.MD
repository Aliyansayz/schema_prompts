1)  Analyze given schema and find me possible fact tables in the following schema:-


ALBUMS
AlbumId INTEGER
Title NVARCHAR(160)
ArtistId INTEGER


ARTISTS
ArtistId INTEGER
Name NVARCHAR(120)


CUSTOMERS
CustomerId INTEGER
FirstName NVARCHAR(40)
LastName NVARCHAR(20)
Company NVARCHAR(80)
Address NVARCHAR(70)
City NVARCHAR(40)
State NVARCHAR(40)
Country NVARCHAR(40)
PostalCode NVARCHAR(10)
Phone NVARCHAR(24)
Fax NVARCHAR(24)
Email NVARCHAR(60)
SupportRepId INTEGER


EMPLOYEES
EmployeeId INTEGER
LastName NVARCHAR(20)
FirstName NVARCHAR(20)
Title NVARCHAR(30)
ReportsTo INTEGER
BirthDate DATETIME
HireDate DATETIME
Address NVARCHAR(70)
City NVARCHAR(40)
State NVARCHAR(40)
Country NVARCHAR(40)
PostalCode NVARCHAR(10)
Phone NVARCHAR(24)
Fax NVARCHAR(24)
Email NVARCHAR(60)


GENRES
GenreId INTEGER
Name NVARCHAR(120)


INVOICES
InvoiceId INTEGER
CustomerId INTEGER
InvoiceDate DATETIME
BillingAddress NVARCHAR(70)
BillingCity NVARCHAR(40)
BillingState NVARCHAR(40)
BillingCountry NVARCHAR(40)
BillingPostalCode NVARCHAR(10)
Total NUMERIC(10,2)


INVOICE_ITEMS
InvoiceLineId INTEGER
InvoiceId INTEGER
TrackId INTEGER
UnitPrice NUMERIC(10,2)
Quantity INTEGER


MEDIA_TYPES
MediaTypeId INTEGER
Name NVARCHAR(120)


PLAYLISTS
PlaylistId INTEGER
Name NVARCHAR(120)


PLAYLIST_TRACK
PlaylistId INTEGER
TrackId INTEGER


TRACKS
TrackId INTEGER
Name NVARCHAR(200)
AlbumId INTEGER
MediaTypeId INTEGER
GenreId INTEGER
Composer NVARCHAR(220)
Milliseconds INTEGER
Bytes INTEGER
UnitPrice NUMERIC(10,2)




To identify potential fact tables in the given schema, we need to look for tables that record transactional or measurable events that can be analyzed. Fact tables often have a large number of records, 
include foreign keys referencing dimension tables, and contain numeric fields representing measurable data (e.g., prices, quantities). 

Based on the provided schema, here are the potential fact tables:

### 1. **INVOICES**
   - **Reason**: The `INVOICES` table records transactions between customers and the company. It contains transactional information such as `InvoiceId`, `CustomerId`, `InvoiceDate`, and `Total`, which are typical of fact tables.
   - **Measurable data**: `Total` (the total amount of the invoice) is a key numeric field used in analysis.

### 2. **INVOICE_ITEMS**
   - **Reason**: The `INVOICE_ITEMS` table records line items for each invoice. It contains foreign keys to the `INVOICES` and `TRACKS` tables, as well as fields like `UnitPrice` and `Quantity`, which can be aggregated and analyzed.
   - **Measurable data**: `UnitPrice` and `Quantity` are key numeric fields used in analysis. This table allows more granular analysis than the `INVOICES` table (e.g., specific items purchased per invoice).

### 3. **PLAYLIST_TRACK**
   - **Reason**: The `PLAYLIST_TRACK` table records associations between playlists and tracks, and although it doesn't have numeric fields, it serves as a transactional table in the context of user interaction with music tracks.
   - **Measurable data**: This table allows analysis of track counts per playlist, track popularity, and user engagement, though it does not directly contain monetary values.

### Supporting Dimension Tables:
- **ALBUMS** (connected via `AlbumId` in `TRACKS`)
- **ARTISTS** (connected via `ArtistId` in `ALBUMS`)
- **CUSTOMERS** (connected via `CustomerId` in `INVOICES`)
- **EMPLOYEES** (connected via `SupportRepId` in `CUSTOMERS`)
- **GENRES** (connected via `GenreId` in `TRACKS`)
- **MEDIA_TYPES** (connected via `MediaTypeId` in `TRACKS`)
- **TRACKS** (connected via `TrackId` in `INVOICE_ITEMS` and `PLAYLIST_TRACK`)
- **PLAYLISTS** (connected via `PlaylistId` in `PLAYLIST_TRACK`)

### Summary of Potential Fact Tables:
1. **INVOICES** – Records customer transactions (invoices).
2. **INVOICE_ITEMS** – Records details of individual items in invoices.
3. **PLAYLIST_TRACK** – Records associations between tracks and playlists for engagement analysis.

These tables can be analyzed with different grain levels, from overall invoice totals to individual track purchases or playlist interactions.


