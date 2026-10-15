# Company law — status, assets, transfer

Type: Law · what a company's status allows, how status changes, and what keeps a company sellable; pulled through rulesindex at a status change, an asset purchase or expiry, a sale or transfer, and at repo spin-up (/repospinup, /repotransfer). Update: one rule = one line; replace the line and delete the superseded one in the same edit. The reasoning and the incident behind each rule: `rsot/knowledge/governance-law-rationale.md` §company-law, same rule IDs, open when a rule's judgment is in play.

## 1 · Status gates what you may do

| Status | Allowed |
|---|---|
| PLANNING | Planning docs and structure only — **zero build hours** until the portfolio gate (cost to proof · time to revenue · run cost · kill date + metric) is approved; unset gate fields are flagged, never assumed. |
| KICKED-OFF / LIVE | Build under the org rules (LIN-19). |
| FOR-SALE | Feature freeze; transfer procedure (/repotransfer) only. |
| RETIRED / SOLD | Read-only (global card §5). |

- **MGR-44** PLANNING → KICKED-OFF only on the portfolio gate approved by Raze (recorded in the HQ repolaneindex row and the repo card in the same pass), KICKED-OFF → LIVE on the first main path proven for real, and any → FOR-SALE / RETIRED only on Raze's call with the HQ repolaneindex row, repo card and Linear project state updated together; a SOLD company's repolaneindex row keeps status sold and is never deleted.
- **MGR-47** A legal-entity line carries zero build until its company number, incorporation date and wrapped company are recorded in the repolaneindex row.

## 2 · Transferability

- **MGR-34** Every repo holds its own sellable kit: documentation, ledger, P&L, PIC.
- **MGR-46** A domain's RDAP-verified expiry date is recorded in the repolaneindex row at purchase, the RDAP date beats a dictated date, and renewal is a hard date.
- **MGR-48** Each website's content repository sits on the website's own account and transfers with it, never on a shared machine account, and from the second website on each gets its own GitHub organization, never a second personal account.
- **MGR-49** No country of origin or location is ever named on any surface — copy, profile, gig, proposal, seat card, rule or memory; every surface says remote/global.

