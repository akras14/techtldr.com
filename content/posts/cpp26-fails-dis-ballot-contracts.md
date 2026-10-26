---
title: "C++26 failed its ISO DIS ballot: 12 of 20 voting members said yes, 9 of 22 no"
slug: "cpp26-fails-dis-ballot-contracts"
date: 2026-10-26T09:05:00+0000
summary: "The C++26 draft failed its ISO DIS ballot, reportedly the first C++ draft ever to do so. It needed two-thirds approval from voting members (got 12 of 20, 60%) and at most 25% No votes (got 9 of 22, 41%). The author expects all nine No votes to be about contracts, though the national-body comments won't be published until mid-January 2027."
source: "https://train-it.eu/blog/cpp26-contracts-the-ballot-result/"
source_title: "The result of the C++26 DIS ballot"
source_author: "Mateusz Pusz"
source_site: "Train IT"
source_date: "2026-10-25"
hn_url: "https://news.ycombinator.com/item?id=50239748"
---

**Bottom line:** C++26 failed its DIS ballot, which closed on 23 October 2026. As far as the author knows it is the first C++ draft to fail one. The draft missed both thresholds: it needed two-thirds Yes from voting (P-)members and got 12 of 20 (60%), and it needed at most 25% No votes from all bodies and got 9 of 22 (41%). Reflection, `std::execution`, library hardening and contracts are all in a draft ISO has not approved, and the length of the delay is unknown.

**Who voted how:** Denmark, Finland, Iran, Italy, Japan, Luxembourg, Romania, Spain and Sweden voted No. France and Ireland abstained with comments, and Poland and the US abstained. The author, a member of the Polish national body, explains that Poland's internal vote to reject didn't pass, and that the committee barely reached quorum because some listed members had left their jobs or gone inactive.

**Why:** The author expects all nine No votes to be about contracts (`pre`, `post`, `contract_assert`), based on the pre-ballot opposition on record. This is an inference: the national-body comments are due within 12 weeks of the vote, by mid-January 2027, and the official result isn't public.

**What happens next:** ISO's directives allow a revised enquiry draft (eight-week vote), a revised committee draft, a technical specification, or cancellation. No choice has been announced. WG21 meets in Búzios, Brazil, 16 to 21 November 2026. One camp (P4433R0) wants another ballot without changing contracts; another, including WG21 Direction Group member Daveed Vandevoorde, recommends removing contracts while keeping library hardening so the rest can ship. P4238R1 estimates the delay at months rather than years. The author loves contracts but can live with removing them, as long as the rest of C++26 isn't held back.
