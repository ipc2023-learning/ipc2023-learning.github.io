# International Planning Competition 2023 Classical Tracks

This is the website for the classical (sequential, deterministic) track of the
[IPC 2023](https://ipc2023.github.io).
This is the 10th IPC containing classical tracks making it the oldest part of
IPC.

## Calls
Comming soon

## Preliminary Schedule

| Event                                         | Date             |
|:----------------------------------------------|:-----------------|
| Call for domains / expression of interest     | July, 2022       |
| Domain submission deadline                    | December, 2022   |
| Demo problems provided                        | December, 2022   |
| Initial planner submission                    | January, 2023    |
| Feature stop (final planner submission)       | March, 2023      |
| Planner Abstract submission deadline          | May, 2023        |
| Contest run                                   | May - June, 2023 |
| Results announced                             | July, 2023       |
| Result analysis deadline                      | August, 2023     |


## Tracks

### Optimal Track
 - single CPU core
 - 8Gb memory limit
 - 30min time limit
 - Plans must be optimal
 - The score of a planner is the number of solved tasks
 - If a suboptimal or invalid plan is returned, all tasks in the domain are counted as unsolved.
 - If that happens in more than one domain, the entry is disqualified.

### Satisficing Track
 - single CPU core
 - 8Gb memory limit
 - 30min time limit
 - Multiple plans can be returned, the one with the lowest cost is counted.
 - The score of a planner on a solved task is the ratio C\*/C where C is the
   cost of the cheapest discovered plan and C\* is the cost of a reference plan. The score on an unsolved task is 0. The score of a planner is the sum of its scores for all tasks.
 - If an invalid plan is returned, all tasks in the domain are counted as unsolved.
 - If that happens in more than one domain, the entry is disqualified.

### Agile Track
 - single CPU core
 - 8Gb memory limit
 - 5min time limit
 - The cost of the discovered plan is ignored, only the CPU time to discover a plan is counted.
 - The score of a planner on a solved task is 1 if the task was solved within 1 second and 0 if the task was not solved within the resource limits. If the task was solved in T seconds (1 ≤ T ≤ 300) then its score is 1 - log(T)/log(300). The score of a planner is the sum of its scores for all tasks.
 - If an invalid plan is returned, all tasks in the domain are counted as unsolved.
 - If that happens in more than one domain, the entry is disqualified.

**More tracks comming soon**


## PDDL Fragment
TBA

## Registration
Comming soon

## Organizers
 - [Daniel Fišer](https://danfis.cz) (Saarland University)
 - [Florian Pommerening](http://ai.cs.unibas.ch/people/pommeren/index.html) (University of Basel)

Contact us: [ipc2023-classical@googlegroups.com](ipc2023-classical@googlegroups.com)
