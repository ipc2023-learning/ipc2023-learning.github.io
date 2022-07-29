This is the website for the learning tracks of the [IPC
2023](https://ipc2023.github.io).

**Below is our preliminary plan. Details might change and feedback is welcome!**

## Setup

The learning tracks use a similar setup as in 2008, 2011 and 2014. The main
difference is that participants don't have access to the selected PDDL
domains and they don't learn the domain knowledge themselves. Instead, they
submit a fully automated learning system. Then the organizers learn the domain
knowledge and evaluate the submitted planners on unseen test instances from the
same domain. The main motivation for this setup is to make it easier to run
learning systems from other authors, which increases reproducibility and helps
to turn learning algorithms into off-the-shelve tools.

Participants will submit two Singularity scripts:

* `./train DOMAIN TASK [TASK]...` (N tasks in ascending "difficulty")
* `./plan DOMAIN TASK DK PLAN` ("DK" path contains domain knowledge)

We call a domain *polynomial* if all its tasks can be solved suboptimally in
polynomial time. The competition will include both polynomial and non-polynomial
domains.

Example domain knowledge includes but is not limited to:
* a general policy ([example](https://arxiv.org/abs/2101.00692))
* a C++ program ([example](https://aair-lab.github.io/genplan22/papers/segovia_aguas_GenPlan22.pdf))
* a domain-general heuristic or partial policy, possibly encoded
  in a neural network ([example](https://jair.org/index.php/jair/article/view/11633/))
* a planner configuration or a sequential/parallel portfolio that
  works well for the given domain ([example](https://rlplab.com/papers/seipp-et-al-aaai2015.pdf))
* a dynamic algorithm configuration policy ([example](https://prl-theworkshop.github.io/prl2022-icaps/papers/PRL2022_paper_9.pdf))
* a set of macro actions for the domain ([example](https://www.jair.org/index.php/jair/article/view/10426/))

For example PDDL tasks, see https://github.com/aibasel/downward-benchmarks.

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

### Single-Core Track
 - 1 CPU core, no GPU
 - Limits training: 72 hours, 90 GiB
 - Limits evaluation: 30 minutes, 8 GiB

### Multi-Core Track
 - 1 full CPU (32 cores), 1 GPU
 - Limits training: 72 hours, 90 GiB
 - Limits evaluation: 30 minutes, 8 GiB

## Metrics

If an invalid plan is returned (or suboptimal plan for optimal metric), all
tasks in the domain are counted as unsolved. If that happens in more than one
domain, the entry is disqualified.

### Optimal
 - Plans must be optimal.
 - The score of a planner is the number of solved tasks.

### Satisficing
 - Multiple plans can be returned, the one with the lowest cost is counted.
 - The score of a planner on a solved task is the ratio C\*/C where C is the
   cost of the cheapest discovered plan and C\* is the cost of a reference plan. The score on an unsolved task is 0. The score of a planner is the sum of its scores for all tasks.

### Agile
 - The cost of the discovered plan and the training time are ignored, only the time to find a plan is counted.
 - The score of a planner on a solved task is 1 if the task was solved within 1 second and 0 if the task was not solved within the resource limits. If the task was solved in T seconds (1 ≤ T ≤ 300) then its score is 1 - log(T)/log(300). The score of a planner is the sum of its scores for all tasks.


## PDDL Fragment

Learners and planners must support the following subset of PDDL 3.1: STRIPS, action costs, types, negative
preconditions.

## Registration
Coming soon

## Organizers
 - [Jendrik Seipp](https://jendrikseipp.com) (Linköping University)
 - [Javier Segovia-Aguas](https://jsego.github.io/) (Universitat Pompeu Fabra)

Contact: <mailto:jendrik.seipp@liu.se,javier/dot/segovia/at/upf/dot/edu>
