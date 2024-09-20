3) Now Make CTEs of these fact Tables to get Count by Customer Id, Max Total, by Invoice, 



To create Common Table Expressions (CTEs) for these fact tables and derive counts by `CustomerId` and the maximum `Total` by `Invoice`, you can structure the SQL as follows:

### CTE for Count by CustomerId and Max Total by Invoice

```sql
WITH InvoiceStats AS (
    -- CTE for counting invoices per CustomerId and getting the Max Total per Invoice
    SELECT 
        CustomerId,
        COUNT(InvoiceId) AS InvoiceCount,
        MAX(Total) AS MaxInvoiceTotal
    FROM 
        INVOICES
    GROUP BY 
        CustomerId
),
InvoiceItemStats AS (
    -- CTE for counting line items and getting max unit price by invoice
    SELECT
        InvoiceId,
        COUNT(InvoiceLineId) AS ItemCount,
        MAX(UnitPrice * Quantity) AS MaxLineTotal
    FROM
        INVOICE_ITEMS
    GROUP BY
        InvoiceId
),
PlaylistTrackStats AS (
    -- CTE for counting the number of tracks per playlist
    SELECT
        PlaylistId,
        COUNT(TrackId) AS TrackCount
    FROM
        PLAYLIST_TRACK
    GROUP BY
        PlaylistId
)
-- You can join or select from these CTEs based on your needs:
SELECT 
    i.CustomerId,
    i.InvoiceCount,
    i.MaxInvoiceTotal,
    it.ItemCount,
    it.MaxLineTotal
FROM 
    InvoiceStats i
JOIN 
    InvoiceItemStats it ON i.CustomerId = it.InvoiceId
-- Optionally include playlist stats as well:
-- JOIN PlaylistTrackStats pt ON pt.PlaylistId = ...
;
```

### Explanation:
1. **InvoiceStats**: Counts the number of invoices per customer (`InvoiceCount`) and gets the maximum total per invoice (`MaxInvoiceTotal`).
   
2. **InvoiceItemStats**: Counts the number of line items per invoice (`ItemCount`) and calculates the maximum value for any individual line (`MaxLineTotal`, based on `UnitPrice * Quantity`).

3. **PlaylistTrackStats**: Counts the number of tracks in each playlist (`TrackCount`), which can be included depending on further queries.

### Query Output:
- The query will return the `CustomerId`, the number of invoices (`InvoiceCount`), the maximum invoice total (`MaxInvoiceTotal`), 
the number of items per invoice (`ItemCount`), and the maximum line item total (`MaxLineTotal`).
