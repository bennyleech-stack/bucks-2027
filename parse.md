# Reply format: BUCKS VOTE v1

Every reply sent from the vote page is plain text in exactly this shape. Paste any number of them in one
block (straight from WhatsApp is fine) and ask Claude to tally them using the rules below.

```
BUCKS VOTE v1 - <Name>
Status: <In|Probably|Can't make it|->
1st: <Gold Coast|Melbourne|Adelaide|->
2nd: <Gold Coast|Melbourne|Adelaide|->
No: <comma-separated destinations, or ->
Budget: <Up to $600|$600-800|$800-1,000|$1,000+|->
Arrive: <Saturday morning|Friday night|Saturday arvo or later|->

My weekend (<1st choice destination, or ->):
- Saturday daytime: <pick>
- Saturday afternoon: <pick>
- Saturday night: <pick>
- Sunday daytime: <pick>
- Sunday night: <pick>
- Monday: <pick>

Must-haves: <comma-separated list, or ->
Notes: <free text, may run over several lines>
END
```

## Rules

- **Splitting**: a reply starts at a line containing `BUCKS VOTE v1 - ` and ends at a line that is exactly `END`.
  Anything outside those pairs (chat chatter, timestamps) is ignored. WhatsApp prefixes such as
  `[18/11/26, 9:41 pm] Dave:` in front of a line should be stripped before parsing.
- **Name**: everything after `BUCKS VOTE v1 - ` on the first line. De-duplicate case-insensitively;
  **the latest reply for a name wins**.
- **`-`** always means "not answered" / "none".
- **Plain text only**: no emoji. Hyphens are plain `-` (budget is `$600-800`, not an en dash). Apostrophes are `'`.
- **Field order** is fixed as shown. The blank lines are there for readability; ignore them.
- **My weekend**: the destination in brackets is the sender's 1st choice. If they have no 1st choice it is `-`
  and every slot is `-`.
- **Picks**: either one of that destination's preset options (text exactly as listed in `index.html`), or
  `Other: <their text>`, or just `Other` if they chose Other and typed nothing.
- **Must-haves** options: Winery day, Topless poker, Afternoon strippers at the accommodation, Strip club late night,
  Race day, Boat day, Beach club, Live bands, Watching sport at a pub, Adventure activity (skydive, jet ski).
  The last one contains a comma inside brackets, so split the list on commas that are **not inside brackets**.
- **Notes**: optional. The line is omitted when empty. It may span several lines; everything from `Notes:` up
  to `END` belongs to it. A notes line that would read just `END` is sent as `END.` so it can't end the reply early.

## Tallying (what tally.html does, and what to ask Claude for)

1. **Headcount** by Status (In / Probably / Can't make it / no answer).
2. **Destination score**: +2 for each 1st, +1 for each 2nd, -1 for each No. Rank by score, then by number of
   1sts, then 2nds. Show raw 1st / 2nd / No counts.
3. **Budget** and **Arrive** breakdowns (counts per option).
4. **Weekend plan**: for the winning destination, using only replies whose `My weekend (...)` is that
   destination, the most-picked option per slot with its vote count (note ties and "Other" ideas).
5. **Must-haves** ranked by count.
6. **Notes**: every note with the sender's name.

Optional: exclude votes from people marked `Can't make it` (tally.html has a checkbox for this).

## Example

```
BUCKS VOTE v1 - Dave O'Brien
Status: In
1st: Melbourne
2nd: Gold Coast
No: Adelaide
Budget: $600-800
Arrive: Friday night

My weekend (Melbourne):
- Saturday daytime: Super Saturday at Flemington
- Saturday afternoon: Stay at the races
- Saturday night: Laneway bars, Cherry Bar bands, strip club late
- Sunday daytime: Other: Golf at Albert Park
- Sunday night: Quiet one
- Monday: Recovery brunch, fly home

Must-haves: Race day, Live bands
Notes: Can't do anything before 10am Saturday.
Also keen on a pub to watch the footy.
END
```
