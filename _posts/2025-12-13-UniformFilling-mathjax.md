---
layout: post
title: Filling-In Queue with Uniform Party Sizes
---

### This code uses a Filling-In Queue, which starts with adding the first party size arrival to a group, then continues adding the next arrivals until the group of 5 is formed. Otherwise, the party sizes wait in the queue.
### The party sizes are created using the Uniform Distribution. 

```python
from itertools import combinations
from collections import Counter
import numpy as np
import math
import matplotlib.pyplot as plt
import pandas as pd
import random

# --- Parameters ---
SEED = 1000
rng = np.random.default_rng(SEED)
```

# Filling-In Queue Wait Time Test


```python
def group_forming(party_sizes, members, verbose=False):

    wait_players = []
    full_groups = 0
    groups = []


    # Calculating the as party_sizes goes
    for players in party_sizes:
        wait_players.append(players)


    while True:
      # Resets the list and variable every time the loop runs
      group = []
      sum_groups = 0

      # this runs a loop that checks each player as they arrive and determines whether adding the players will reach the amount of members required
      for player in wait_players:
          if sum_groups + player <= members:
                group.append(player)
                sum_groups += player

      # checks to see if the sum_groups equals the amount of members required and forms a group
      if sum_groups == members:
            full_groups += 1
            groups.append(group)
            if verbose:
                print(f"Group Formed: {group}")

            # if a team is formed, those members are removed from the wait list
            for p in group:
                wait_players.remove(p)
      else:
          break

    # Starts Loop for Forming Groups
    while True:
        formed_groups = False                       # starts with no new group has been formed
        for r in range(1, min(len(wait_players), 5) + 1):     # tries all the combinations groups until reaching the sum of 5 (ex. a solo, pair, etc)
            for combo in combinations(wait_players, r):   # uses combination function to check all possible combinations with the values available
                if sum(combo) == members:                 # checks if any combinations equal to 5
                    full_groups += 1
                    groups.append(list(combo))
                    if verbose:
                        print(f"Group Formed: {list(combo)}")

                    for player in combo:
                        wait_players.remove(player)       # removes players added to a group
                    formed_groups = True                  # confirms that group was formed
                    break
            if formed_groups:
                break
        if not formed_groups:
            break

    if verbose:
        print(f"Total Groups Formed: {full_groups}")
        if wait_players:
            print(f"Remaining Players: {wait_players}")

    return {

            'total groups': full_groups,
            'groups': groups,
            'remaining players': wait_players
    }


def simulate_parties_matchmaking(
    n_parties,
    group_size=5,          # players needed to start a game
    interarrival_low=0.0,  # Uniform(low, high) for interarrival times
    interarrival_high=5.0,
    uniform_party_sizes = True,
    party_probabilities = {1:0.3, 2:0.3, 3:0.2, 4:0.1, 5:0.1}, # Calculate the probabilities of solos, duos, etc showing up
    seed=None,
    verbose=False
):
    rng = np.random.default_rng(seed)

    # Step 1: Creates Random Party Sizes

    if uniform_party_sizes:
          party_sizes = rng.integers(1, 6, size=n_parties)
          if verbose:
              print(party_sizes)
    else:
        # Random Party Sizes
        #party_sizes = rng.choice(list(party_probabilities.keys()), size=n_parties, p=list(party_probabilities.values()))
        # Specific Party Size
        party_sizes = np.array([1, 1, 2, 4, 3, 3, 1, 2])
        n_parties = len(party_sizes)


    # simulate interarrival times and absolute arrival times
    interarrival = rng.uniform(interarrival_low, interarrival_high, size=n_parties)
    arrivals = np.cumsum(interarrival)   # absolute arrival times

    # Step 2: for each batch of `group_size`, compute waits = t_5th - t_i
    all_waits = [(size, arrival) for size, arrival in zip(party_sizes, arrivals)]
    waiting_time = []
    game_mean_waits = []
    groups_formed_c = []      # (Optional) to check the groups formed
    used_indices = set()

        # Form Groups whenever someone arrives
    sizes_only = [p[0] for p in all_waits]
    party_size_counter = Counter(sizes_only)
    ordered_counter = {k: party_size_counter.get(k, 0) for k in range(1, 6)}

    groups_data = group_forming(sizes_only, members = group_size)
    formed_groups = groups_data['groups']

    for g in formed_groups:
        group_indices = []
        temp_sizes_only = sizes_only.copy()
        for party in g:
            for idx, size in enumerate(temp_sizes_only):
                if size == party and idx not in used_indices:
                    group_indices.append(idx)
                    used_indices.add(idx)
                    break
        group_time = max(all_waits[i][1] for i in group_indices)

        for i in group_indices:
            waiting_time.append(group_time - all_waits[i][1])

        groups_formed_c.append([int(x) for x in g])
        game_mean_waits.append(group_time)

    remaining_parties = [all_waits[i] for i in range(len(all_waits)) if i not in used_indices]

    results = {
        "mean_wait": np.mean(waiting_time) if waiting_time else 0,
        "waiting_time": np.array(waiting_time),
        "game_mean_waits": np.array(game_mean_waits),
        "remaining_parties": remaining_parties,
        "groups_formed": groups_formed_c,
        "party_sizes": party_sizes,
        "arrivals": arrivals,
        "ordered_counter": ordered_counter
    }
    return results
```

#### Testing Different Amount of Parties

```python
members = 5
n_parties = 10, 20, 50, 100, 200, 500, 1000
#party_sizes = [1, 1, 2, 4, 3, 3, 1, 2]
#print(group_forming(party_sizes, members))

wait_table = []
wait2_table = []
for n in n_parties:
    res = simulate_parties_matchmaking(
        n,
        group_size=5,
        interarrival_low=0,
        interarrival_high=5,
        seed=1729
    )

    print(f"Average Wait Time: {res['mean_wait']:.2f}")
    print(f"Groups Formed: {len(res['game_mean_waits'])}")
    print(f"Remaining Unmatched Parties: {len(res['remaining_parties'])}")

    remaining = [int(x[0]) for x in res['remaining_parties']]
    print(f"Remaining Parties: {remaining}")

    print()

    wait_table.append({
        "Number of Parties": n,
        "Average Wait Time": round(res['mean_wait'], 2),
        "Groups Formed": len(res['game_mean_waits']),
        "Remaining Unmatched Parties": len(res['remaining_parties'])
    })

    wait2_table.append({
        "Number of Parties": n,
        "Party of 1's": res['ordered_counter'][1],
        "Party of 2's": res['ordered_counter'][2],
        "Party of 3's": res['ordered_counter'][3],
        "Party of 4's": res['ordered_counter'][4],
        "Party of 5's": res['ordered_counter'][5],
    })
df_wait = pd.DataFrame(wait_table)
df_wait2 = pd.DataFrame(wait2_table)
print(df_wait.to_string(index=False))
print()
print(df_wait2.to_string(index=False))
```

#### Results

    Average Wait Time: 1.51
    Groups Formed: 4
    Remaining Unmatched Parties: 2
    Remaining Parties: [3, 3]
    
    Average Wait Time: 5.24
    Groups Formed: 8
    Remaining Unmatched Parties: 5
    Remaining Parties: [3, 3, 3, 1, 3]
    
    Average Wait Time: 6.08
    Groups Formed: 22
    Remaining Unmatched Parties: 8
    Remaining Parties: [2, 2, 4, 4, 4, 4, 2, 2]
    
    Average Wait Time: 12.83
    Groups Formed: 46
    Remaining Unmatched Parties: 13
    Remaining Parties: [4, 4, 4, 4, 3, 4, 4, 4, 4, 3, 4, 4, 3]
    
    Average Wait Time: 22.81
    Groups Formed: 105
    Remaining Unmatched Parties: 14
    Remaining Parties: [4, 4, 4, 4, 3, 4, 3, 3, 4, 4, 3, 4, 4, 3]
    
    Average Wait Time: 23.07
    Groups Formed: 276
    Remaining Unmatched Parties: 20
    Remaining Parties: [3, 4, 4, 4, 4, 4, 4, 3, 3, 4, 3, 3, 3, 3, 3, 3, 3, 4, 3, 4]
    
    Average Wait Time: 53.48
    Groups Formed: 569
    Remaining Unmatched Parties: 28
    Remaining Parties: [4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 3, 4, 3, 4, 4]
    
     Number of Parties  Average Wait Time  Groups Formed  Remaining Unmatched Parties
                    10               1.51              4                            2
                    20               5.24              8                            5
                    50               6.08             22                            8
                   100              12.83             46                           13
                   200              22.81            105                           14
                   500              23.07            276                           20
                  1000              53.48            569                           28
    
     Number of Parties  Party of 1's  Party of 2's  Party of 3's  Party of 4's  Party of 5's
                    10             3             2             2             2             1
                    20             6             3             6             2             3
                    50            11            15            10             7             7
                   100            21            25            21            20            13
                   200            42            44            44            36            34
                   500           107           108           104            91            90
                  1000           199           215           207           190           189

