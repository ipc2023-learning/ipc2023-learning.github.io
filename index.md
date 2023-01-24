Welcome to the website for the learning tracks of the [IPC
2023](https://ipc2023.github.io). The IPC learning tracks are a competition
where algorithms learn domain-specific knowledge in an offline pre-processing
phase and then feed that knowledge to a planner to solve new tasks from the same
domain.

## Overview

The learning tracks use a similar setup as in
[2008](https://ipc08.icaps-conference.org/learning/),
[2011](http://www.plg.inf.uc3m.es/ipc2011-learning/FrontPage.html) and
[2014](https://www.cs.colostate.edu/~ipc2014/). The main novelty in 2023 is that
participants don't have access to the PDDL domains and they don't learn the
domain knowledge themselves. Instead, they submit a fully automated learning
system. Then the organizers learn the domain knowledge and evaluate the
submitted planners on unseen test instances from the same domain. The main
motivation for this setup is to make it easier to run learning systems from
other authors, which increases reproducibility and helps to turn learning
algorithms into off-the-shelve tools.

Participants will submit separate scripts for each submitted learner and planner:

* `./train DOMAIN TASK_DIR` ("TASK_DIR" contains N tasks from "DOMAIN" in ascending "difficulty")
* `./plan DOMAIN DK TASK PLAN` ("DK" path contains domain knowledge)

Details concerning the submissions will be provided via the mailing list below.

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


## Schedule

| Event/Deadline                   | Date               |
|:---------------------------------|:-------------------|
| Call for domains                 | July, 2022         |
| Domain submission deadline       | December 9, 2022   |
| Demo problems provided           | February, 2023     |
| Team registration                | February 22, 2023  |
| Feature stop (final submission)  | April 19, 2023     |
| Planner abstract submission      | May 24, 2023       |
| Contest run                      | May - June, 2023   |
| Results announced                | July 12, 2023      |
| Results analysis due             | September 20, 2023 |


## Environments

### Single-Core
 - 1 CPU core, no GPU
 - Limits training: 72 hours, 90 GiB
 - Limits evaluation: 30 minutes, 8 GiB

### Multi-Core
 - 1 full CPU (32 cores), 1 GPU
 - Limits training: 72 hours, 90 GiB
 - Limits evaluation: 30 minutes, 8 GiB

## Metrics

If an invalid plan is returned (or suboptimal plan for optimal metric), all
tasks in the domain are counted as unsolved. If that happens in more than one
domain, the entry is disqualified.

### Satisficing
 - Multiple plans can be returned, the one with the lowest cost is counted.
 - The score of a planner on a solved task is the ratio C\*/C where C is the
   cost of the cheapest discovered plan and C\* is the cost of a reference plan. The score on an unsolved task is 0. The score of a planner is the sum of its scores for all tasks.

### Agile
 - The cost of the discovered plan and the training time are ignored, only the time to find a plan is counted.
 - The score of a planner on a solved task is 1 if the task was solved within 1 second and 0 if the task was not solved within the resource limits. If the task was solved in T seconds (1 ≤ T ≤ 300) then its score is 1 - log(T)/log(300). The score of a planner is the sum of its scores for all tasks.

### Optimal
 - Plans must be optimal.
 - The score of a planner is the number of solved tasks.


## PDDL Fragment

Learners and planners must support the following subset of PDDL 3.1: STRIPS,
action costs, types, negative preconditions. Some of the training and testing
tasks may be hard to ground.


## Procedure

The competitors must submit the source code of their learner and planners, which
will be run by the organizers on the competition domains/problems, unknown to
the competitors until this time. This way no fine-tuning of the learners or
planners will be possible.

All competitors must submit an up to 8-page paper describing their learner and
planners (see [schedule](#schedule)). After the competition we ask the
participants to analyze the results of their learners and planners and submit an
extended version of their paper. An important requirement for IPC 2023
competitors is to give the organizers the right to post their paper and the
source code of their learners/planners on the official IPC 2023 web site, and
the source code of submitted planners must be released under a license allowing
free non-commercial use.

As in the classical tracks of IPC 2018, we will use the container technology
[Apptainer](http://apptainer.org/) (formerly known as Singularity) to promote
reproducibility and simplify program compilation. In contrast to IPC 2018, we
will host repositories of planners ourselves. The repositories will be hosted on
GitHub under the [ipc2023-learning](https://github.com/ipc2023-learning)
organization, and they will be kept private until the end of the competition
when we make them public.

When a competition team registers (see below), we create a private repository
(or multiple repositories if needed) and add competitors as users with write
access.
After the "feature stop" deadline (see [schedule](#schedule)), we allow competitors to
send only pull requests with bug fixes.
We will review every pull request with its accompanying description of the bug
fix to make sure that no big changes or parameter tuning are possible.
To help us with the debugging process, in contrast to previous years, planner
authors will be responsible for detecting if the run of their planner and our
analysis of the results was successful. We will provide more details on this
later.


## Registration

This year, we will allow to submit **multiple learners/planners** to multiple
tracks from a **single repository**. Thus, each team only needs one repository per
code base and different parameters for different tracks can be set by providing
multiple Apptainer files. More details to follow.

To register a team, the participants need to send an e-mail with a
subject containing "[Registration for Learning Tracks]" to
[jendrik.seipp@liu.se](jendrik.seipp@liu.se).
The email must contain:
1. names of participants,
2. email contacts,
3. GitHub usernames,
4. the number of repositories (code bases) the team needs (multiple
   learners/planners can be built from the same repository),
5. a (tentative) list of [environments](#environments) and [metrics](#metrics),
   where the team intends to submit their learners/planners.

Based on that, we will create private repositories under the
[ipc2023-learning](https://github.com/ipc2023-learning) organization and add
all participants as users with with write access and participants can commit
to the repository as they wish until the "feature stop" deadline.

All participants must subscribe to the **[Google
Group](https://groups.google.com/g/ipc2023-learning)**. We will announce further
details on the submission process there in due time.

To propose a domain for the competition, please contact the organizers (see below).


## Organizers
 - [Jendrik Seipp](https://jendrikseipp.com) (Linköping University)
 - [Javier Segovia-Aguas](https://jsego.github.io/) (Universitat Pompeu Fabra)

Public questions: <mailto:ipc2023-learning@googlegroups.com>

Contact organizers: <mailto:jendrik.seipp@liu.se,javier/dot/segovia/at/upf/dot/edu>

Acknowledgment: some of the text above has been adapted from the IPC 2023
classical track.
