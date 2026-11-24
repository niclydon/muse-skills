# Receipt Matcher

## Purpose

Explain charges on a bank or card statement by matching each transaction against email evidence, and surface what cannot be explained.

## Workflow

1. Set the window. Default to the last 30 days unless the user names a window or a specific charge. Confirm the window and which accounts are covered before starting.

2. Pull transactions for the window using the Plaid connector. Keep the account name, posted date, merchant name, amount, pending flag, and currency per transaction. If the read reports that some banks or card providers failed to return data, note which ones are missing and run the read again if the connector supports it.

3. Search Gmail for receipts in the same window, widened by 7 days on each side, because receipts often arrive before or after a charge posts. Use receipt-style searches in Gmail search syntax:
   - `subject:(receipt OR invoice OR "order confirmation" OR "booking confirmation") after:YYYY/M/D before:YYYY/M/D`
   - `subject:(charge OR payment OR "thank you for your order") after:YYYY/M/D before:YYYY/M/D`
   - Merchant-specific follow-ups for the largest unmatched amounts: `from:<merchant> after:YYYY/M/D before:YYYY/M/D`

   Open promising candidates and take the amount, date, and merchant exactly as stated in the message body. Prefer transactional mail over promotional mail, and use the event or charge time stated in the message, not the message's own timestamp.

4. Match on three signals, all three required for a confident match: the amount equals the transaction amount, the date is within a few days of posting, and the merchant or description is plausibly the same party. Call out rather than smooth over: pending versus posted amount differences, currency conversion differences (receipt in one currency, charge in another), and amounts that differ by a tip or fee.

5. Bucket every transaction and write the report per the Output Contract below. If Gmail hits a rate limit, stop the Gmail work for this attempt and report the partial results with the affected coverage marked unverified.

## Output Contract

Conclusions first: start with the headline counts (how many matched, unmatched, and duplicate-suspect), then the buckets.

- MATCHED. One line per transaction: amount with currency, posted date, merchant name, and the receipt evidence (sender, subject, the stated amount and date from the message). Example format: "$128.40, Sep 20, United Airlines. Receipt: confirmation email from United, subject 'eTicket Receipt', showing $128.40 charged Sep 19."
- UNMATCHED. One line per transaction: amount with currency, posted date, merchant name, plus exactly what was searched (the searches run, the date windows, which accounts). Example: "$22.50, Sep 21, Europcar. No receipt found. Searched receipt, invoice, and order-confirmation subjects and Europcar mail for Sep 14 to Sep 28."
- DUPLICATE-SUSPECT. One line per pair: both amounts with currency, both dates, and why they look related (same merchant, same amount, days apart), plus the evidence against it being a single charge (separate receipt emails, different confirmation numbers, one pending and one posted). Never label a pair duplicate on amount alone; the bucket means "may be the same," not "is the same."

Every claim carries its evidence. No claim without a source next to it.

## Operating Rules

1. Strictly read-only. Never dispute a charge, contact a merchant, or move money. If the user wants any of those, report the finding and wait for direction.
2. Never label a charge stale, cached, or duplicate without evidence. Two similar charges are DUPLICATE-SUSPECT with the reasons stated, never declared duplicates on pattern alone.
3. Absence of a receipt is not proof of anything. A charge without a receipt found is UNMATCHED with the search scope stated, not unexplained by necessity.
4. Negative claims need exhaustive evidence. Before saying "no receipt exists," search every linked email account, try alternate merchant names, search by amount and date as well as by subject, and include pending transactions. If the search was partial (a rate limit hit, one account not checked, searches cut short), state exactly what was checked and mark the conclusion unverified.
5. Bank data provided through Plaid may lag the bank, and a missing transaction is not an error. Say the data may not be fully up to date rather than asserting a charge is wrong or the list is complete.
6. Keep tooling out of the reply. No commands, no search queries, no raw data dumps, no internal ids. Name accounts in words ("your Chase checking") with masked last digits only to tell similar accounts apart, never full account numbers.
7. Amounts always carry their currency. Style: conclusions first, no em dashes, no bold inside prose paragraphs.

## Adapting this to your setup

- Connect Plaid and Gmail in Muse before first use; without both, the matcher can only work from statements or receipts you paste in.
- If you use more than one email account, tell the assistant which accounts hold receipts so the search covers all of them.
- Adjust the default 30-day window if your statements or reconciliation habits run on a different cycle.
- If your receipts also arrive in another place (an orders page, a second mailbox, a paper folder you scan), name that source so it is searched too.
- Set a threshold if you only care about larger charges; small transactions can be bucketed in bulk instead of matched one by one.
