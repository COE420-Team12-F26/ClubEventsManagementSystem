# Critical Path

**Critical Path:** T1 → T2 → T3 → T5 → T6

**Expected Project Duration:** 7 + 5 + 10 + 5 + 6 = 33 days

**Explanation:**
The network has two paths from start to end: T1 → T2 → T3 → T5 → T6 = 33 days and T1 → T2 → T4 → T5 → T6 = 31 days. The first path is the longest, and every task on it has ES = LS (Slack = 0), so any delay to one of these tasks delays the whole project. T4 is the only non-critical task: it finishes at day 20 but T5 cannot start until T3 finishes at day 22, which gives T4 2 days of slack.

See [Network_Schedule.md](Network_Schedule/Network_Schedule.md) for the full ES/EF/LS/LF calculations and [Gantt_Chart.md](Gantt_Chart/Gantt_Chart.md) for the schedule.
