# International Planning Competition 2023 Learning Tracks

This is the website for the learning tracks of the [IPC
2023](https://ipc2023.github.io).

## Setup

The learning tracks will use a similar setup as in 2008, 2011 and 2014. The main
difference will be that participants don't have access to the selected PDDL
domains and they don't learn the domain knowledge themselves. Instead, they
submit a fully automated learning system. Then the organizers learn the domain
knowledge and evaluate the submitted planners on unseen test instances from the
same domain. The main motivation for this setup is to make it easier to run
learning systems from other authors, which increases reproducibility and helps
to turn learning algorithms into off-the-shelve tools.

Participants will submit two Singularity scripts:

* `./train DOMAIN TASK [TASK]...` (N tasks in ascending "difficulty")
* `./plan DOMAIN TASK DK PLAN` ("DK" path contains domain knowledge)

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

(preliminary plan, details might change)

### Single-core Track
 - single CPU core
 - Limits training: 72 hours, 90 GiB
 - Limits evaluation: 30 minutes, 8 GiB
 - Multiple plans can be returned, the one with the lowest cost is counted.
 - The score of a planner on a solved task is the ratio C\*/C where C is the
   cost of the cheapest discovered plan and C\* is the cost of a reference plan. The score on an unsolved task is 0. The score of a planner is the sum of its scores for all tasks.
 - If an invalid plan is returned, all tasks in the domain are counted as unsolved.
 - If that happens in more than one domain, the entry is disqualified.

**More tracks might be added**


## PDDL Fragment
TBA

## Registration
Comming soon

## Organizers
 - [Jendrik Seipp](https://jendrikseipp.com) (Linköping University)
 - [Javier Segovia-Aguas](https://jsego.github.io/) (Universitat Pompeu Fabra)

Contact: <mailto:jendrik.seipp@liu.se,javier/dot/segovia/at/upf/dot/edu>
