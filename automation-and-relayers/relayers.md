# Relayers

### Overview

Relayers are services that execute auto-voting actions on behalf of users who have enabled automation. They watch the blockchain, see who has opted in, submit votes, and claim rewards — all automatically. In return, relayers earn a share of the fee pool.

Anyone can become a relayer: apps, community members, developers. Registration is open — just call `registerRelayer()` on the RelayerRewardsPool contract.

### How Relayers Earn

Every user served pays 10% of their weekly rewards (max 100 B3TR per user) into a shared pool. At the end of the round, that pool gets split among all relayers based on how much work each one did.

#### Weighted Points

Work is measured in weighted points:

| Action                       | Points       | Why                |
| ---------------------------- | ------------ | ------------------ |
| Casting a vote for a user    | 3 points     | More gas-intensive |
| Claiming rewards for a user  | 1 point      | Less gas-intensive |
| **Full user (vote + claim)** | **4 points** |                    |

More points = bigger share of the pool.

**Example**: If the pool has 1,000 B3TR and a relayer completes 200 out of 800 total weighted points, they earn 250 B3TR.

#### All-or-Nothing Rule

Every user must be served. If even one user gets missed — no vote cast, no rewards claimed — nobody gets paid. The whole pool stays locked until every single user is taken care of.

**Safety net**: If registered relayers don't finish within 5 days after the round ends, anyone can step in and complete the remaining work. This ensures users always receive their rewards.

#### First-Come-First-Served

If another relayer handles a user before you, you get nothing for that user (and waste gas trying). Relayers compete to serve users as quickly as possible.

### Why Apps Should Be Relayers

If you're an app on VeBetterDAO, becoming a relayer is a strategic advantage:

* **Before**: You pay veDelegate to get votes directed your way
* **After**: Your users set you as a preference, you execute their votes (which go to your app), and you earn relayer fees on top

You go from **paying for votes** to **getting paid to handle them**.

> Important: Add your app to the user's preference list — don't replace their other choices.

### vs veDelegate

| Feature                   | veDelegate                        | VeBetterDAO Auto-Voting |
| ------------------------- | --------------------------------- | ----------------------- |
| X Allocation voting       | Yes                               | Yes                     |
| Governance voting         | Yes                               | No (manual only)        |
| Compounding (B3TR → VOT3) | Auto                              | Manual                  |
| Token custody             | Leaves wallet                     | Stays in wallet         |
| Centralization            | Single entity                     | Many relayers           |
| Cost to apps              | Apps may need to pay transactions | Apps earn fees          |

