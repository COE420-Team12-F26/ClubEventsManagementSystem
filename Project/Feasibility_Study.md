# Initial Feasibility Study

## A. Technical Feasibility

Our team has a reasonable spread of technical skills across web development basics, databases, and general programming (see `Team/Skills.md`), even though building a full web app with an approval workflow isn't something any of us has done end-to-end before. The features we're planning (user accounts/roles, forms, a database with a few related tables, basic status/approval logic) don't require anything exotic — they can be built using common, well-documented, free tools (e.g., a standard web framework, a relational database, and version control we're already using via GitHub). We consider the project technically feasible, with the main risk being the learning curve on the approval/budget logic specifically, which we've already flagged in the risk register (R3).

## B. Economic Feasibility

As a course project, we don't have a real budget — all tools we plan to use (frameworks, database, hosting for a demo if needed) have free tiers or are open-source, so there's no direct monetary cost. The main "cost" is our time, which is already accounted for through the course schedule and lab hours. We consider the project economically feasible since it doesn't require spending beyond what's freely available to students.

## C. Operational Feasibility

The problem we're solving is a real one — most clubs currently rely on scattered tools (WhatsApp, spreadsheets, email) to manage events and budgets, which causes confusion about what's actually approved and how much budget is left. Officers and advisors both stand to benefit from a single source of truth, so we believe there's a genuine incentive for them to use it, provided the interface stays simple. The main condition for adoption is that the system has to be noticeably easier than what clubs already do, otherwise people will just fall back to old habits — something we'll need to keep in mind as we design the UI.

## D. Schedule Feasibility

The course runs over a single semester with weekly lab checkpoints, which lines up reasonably well with our incremental plan (4 increments, see `Process_Model.md`). The core features — event creation, approval workflow, and budget requests — should be achievable in this timeframe since they form the first three increments. Some of the "nice to have" features (notifications, a polished dashboard) are placed in the last increment specifically so they can be trimmed if we run out of time without affecting the core functionality.

## E. Overall Feasibility

Overall, we believe the project is feasible within the scope and timeframe of this course. The main constraints are our limited hands-on experience with building this specific type of workflow (approval + budget tracking) and the fixed semester timeline, both of which we're addressing by keeping the core scope tight, building incrementally, and validating the riskier parts (like the budget logic) early rather than leaving them until the end.
