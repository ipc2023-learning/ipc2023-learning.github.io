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

* `./learn DK DOMAIN TASK1 TASK2 ...`
* `./plan DK DOMAIN TASK PLAN`

We plan to pass between 50 and 100 tasks from "DOMAIN" in ascending
"difficulty". The learner will write its learned domain knowledge into the "DK" file.
For details concerning the submission, [see below](#submission).

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
| Team registration                | <strike>February 22, 2023</strike> April 19, 2023 |
| Feature stop (final submission)  | April <strike>19</strike> 27, 2023     |
| Planner abstract submission      | May 24, 2023       |
| Contest run                      | May - June, 2023   |
| Results announced                | July 12, 2023      |
| Results analysis due             | September 20, 2023 |


## Environments

### Single-Core
 - 1 CPU core (from an Intel Xeon Gold 6130 CPU), no GPU
 - Limits training per domain: 72 hours, 90 GiB
 - Limits evaluation per task: 30 minutes, 8 GiB

### Multi-Core
 - 1 full CPU (Intel Xeon Gold 6130 with 32 cores), 1 GPU (NVIDIA® T4, CUDA 11.7, see [GPU instructions](https://www.nsc.liu.se/support/systems/tetralith-GPU-user-guide/))
 - Limits training per domain: 72 hours, 90 GiB
 - Limits evaluation per task: 30 minutes, 8 GiB

<!-- For training and evaluation we limit disk space to 2 GiB. -->

When the learner exceeds the time limit, we send them the SIGTERM signal, which can be caught to write the domain knowledge file and then gracefully exit. After an additional 60 seconds, we send SIGKILL.


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

### Optimal (cancelled)
 - Plans must be optimal.
 - The score of a planner is the number of solved tasks.


## PDDL Fragment

Learners and planners must support the following subset of PDDL 3.1: STRIPS,
action costs, types, negative preconditions. Some of the training and testing
tasks may be hard to ground.

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
to the repository as they wish until the "feature stop" deadline (see [schedule](#schedule)).

After the feature stop deadline, we allow competitors to send only pull requests
with bug fixes. We will review every pull request with its accompanying
description of the bug fix to make sure that no big changes or parameter tuning
are possible.

All participants **must** subscribe to the **[Google
Group](https://groups.google.com/g/ipc2023-learning)**.

To propose a domain for the competition, please contact the organizers (see below).

## Submission

The competitors must submit the source code of their learner and planners, which
will be run by the organizers on the competition domains/problems, unknown to
the competitors until this time. This way no fine-tuning of the learners or
planners will be possible.

An important requirement for IPC 2023 competitors is to give the organizers the
right to post their paper and the source code of their learners/planners on the
official IPC 2023 web site, and the source code of all submissions must be
released under a license allowing free non-commercial use.

As in the classical tracks of IPC 2018, we will use the container technology
[Apptainer](http://apptainer.org/) (version 1.1.5, formerly known as
Singularity) to promote reproducibility and simplify program compilation. In
contrast to IPC 2018, we will host repositories of planners ourselves. The
repositories will be hosted on GitHub under the
[ipc2023-learning](https://github.com/ipc2023-learning) organization, and they
will be kept private until the end of the competition when we make them public.

We allow to submit multiple learners/planners to multiple tracks from a single
repository. In each repository, we only consider the branch `ipc2023-learning`.
Feel free to use other branches for development as you wish, but we will ignore
them. Any pair of files called `Apptainer.<shortname>.{learn|plan}` in the root
directory of this branch defines one entry. For the `<shortname>`, please use
the name and variant of your planner as a short identifier (a single word, up to
16 characters long, starting with a letter, using only letters, digits, and
underscores). If you build different versions of your planner from the same
repository, use a different `<shortname>` per version. A single entry can
participate in multiple tracks, see "Apptainer Images" for details.

### Details for learners

* Given the domain knowledge (DK) prefix "dk", your learner must write domain knowledge files "dk.1", "dk.2", etc., whenever "better" knowledge is found. We will use the last dk.x file for deciding the IPC ranking, but reserve the possibility to also analyze the quality of the domain knowledge over time in later analyses.
* To avoid having corrupted DK files due to your learner being terminated, you should write the new DK to a temporary file and rename the file to dk.x once it's written.
* Since you can write better DK files over time, there should be no need to be notified when the learning time budget is about to be exhausted. Still, after 72 hours, we will send the SIGTERM signal, which can be caught by the learner to gracefully exit. After an additional 60 seconds, we send SIGKILL.
* If your DK files are large, please compress them. We recommend xz for this purpose.
* Our cluster has strict storage limits, which is why we need to limit
  * the total disk usage to 10 GiB per team and domain, and
  * the number of files to 1000 per team and domain.
  * If you really need to use more disk space or generate more files temporarily, you must delete them afterwards, or compress them in a single (tar.xz or similar) file. Compressing them is preferable for debugging purposes.
* You may print a warning to stderr if your learner deems the training set to be inadequate (e.g., too few easy instances). If so, please also print how the training set would need to be changed (e.g., "We require at least 3 instances with state spaces that can be exhaustively explored.")

### Details for planners:

* A plan is valid if it is accepted by the plan validator [VAL](https://github.com/KCL-Planning/VAL).
* Given the plan prefix "plan", your planner must write plan files "plan.1", "plan.2", etc. Even if your planner is not an anytime planner and only finds a single plan, please still name that plan "plan.1".
* For the agile metric, we will only take the time to find "plan.1" into account.
* For the satisficing metric, we will only take the cheapest found plan into account.

### Apptainer images

We prepared a [demo submission](https://github.com/ipc2023-learning/baseline01)
that showcases how to set up the repository and Apptainer scripts.

Your Apptainer recipe files have to specify the following labels:

* `Name`: name of the submission
* `Description`: a short description of your submission
* `Authors`: a list of authors, including contact email addresses (`Firstname Lastname <firstname.lastname@email.example>`)
* `License`: the license under which you publish this code. It has to be permissive enough to allow us to publish the code after the competition.
* `Environments`: a comma-separated list of environments this submission should participate in. Use only the following terms: `single-core`, `multi-core`.
* You don't need to specify evaluation metrics: all submissions will be evaluated under both the [agile](#agile) and [satisficing](#satisficing) metric.
* Your Apptainer recipe must contain all of the following labels describing
supported PDDL features. Each label must be set to either `yes` or `no`, or if
the feature is supported only partially, then set it to `partially, ` followed
by the description of what is and isn't supported:

  * `SupportsDerivedPredicates`: specify whether your submission supports the `(:derived ...)` construct
  * `SupportsUniversallyQuantifiedPreconditions`: `(forall ...)` in actions' preconditions
  * `SupportsExistentiallyQuantifiedPreconditions`: `(exists ...)` in actions' preconditions
  * `SupportsUniversallyQuantifiedEffects`: `(forall ...)` in actions' effects
  * `SupportsNegativePreconditions`: `(not (...))` in actions' preconditions
  * `SupportsEqualityPreconditions`: `(= ?x obj1)`, for action parameter `?x` and constant `obj1`, in actions' preconditions
  * `SupportsInequalityPreconditions`: `(not (= ?x ?y))` or `(not (= ?x obj1))`, for action parameters `?x` and `?y`, and constant `obj1`, in actions' preconditions
  * `SupportsConditionalEffects`: `(when ...)` in actions' effects
  * `SupportsImplyPreconditions`: `(imply ...)` in actions' preconditions

Even though the labels will be the same in most cases for both
`Apptainer.<shortname>.{learn|plan}` files, we ask you to add this information
to both recipes.

To improve reproducibility, we require Apptainer images to be self-contained and
licensed appropriately.

* The recipe should copy the content of the repository into the container at the
start of the build. In particular, do not clone repositories in the recipe.

* If you use third-party libraries, either install them through standard package
managers (apt, yum, pip), or copy them into the repository. Please do not use
git submodules to include dependencies.

* If possible, use explicit versions. For example, do not use `ubuntu:latest` as
your base image but pick a specific version. If you install packages through pip,
pick specific versions of those packages.

* If your build depends on closed-source libraries that require a license,
please contact us.

* If you use a portfolio of existing learners/planners, it is up to you to get permission
from the authors of the portfolio components and give appropriate credit and
licensing. We recommend contacting the original authors.

In addition to reproducibility and licensing issues, we ask that you make your image as small as possible using the following tricks:

* Use a [multi-stage build](https://apptainer.org/docs/user/latest/definition_files.html#multi-stage-builds)
where one stage is used for compiling the planner and one for running the
planner. Copy the compiled planner from the first stage to the second, and only
copy/install the files that are required at runtime. The size of the compilation
stage then does not matter and the second stage can be limited to contain only
essential files.

* [Strip binaries](https://www.man7.org/linux/man-pages/man1/strip.1.html) after compilation

* Use small packages. For example, use python-minimal instead of python if possible.

### Extended abstracts

All competitors must submit an up to 8-page paper describing their learner and
planners (see [schedule](#schedule)). After the competition we ask the
participants to analyze the results of their learners and planners and submit an
extended version of their paper.

### Large files
GitHub repos have a file size limit of 100 MB. If you need files larger than
this, you must upload them to an long-term file preservation site such as
[Zenodo](https://zenodo.org/) and let your Apptainer script download them. The
maximum size for Apptainer images is 2 GiB.

## Bug-fixing Policy

To help us with the debugging process, planner authors will be responsible for
detecting if the run of their planner and our analysis of the results was
successful. After the feature stop deadline, we will run all planners on all
tasks and give the participants access to the results of their planners. For
each run, the data will contain the log files of the planner, measured time and
memory consumption, exit code, and our conclusion about what this means in terms
of solving the instance. We ask participants to check their results for any
errors. If an error was caused by a bug in the planner, please send a pull
request on GitHub with a detailed description of the bug and the fix. If the
error was on our side (e.g., malformed PDDL) let us know as soon as possible. We
will do at least two rounds of this starting after the "feature stop" deadline.

## Organizers
 - [Jendrik Seipp](https://jendrikseipp.com) (Linköping University)
 - [Javier Segovia-Aguas](https://jsego.github.io/) (Universitat Pompeu Fabra)

Public questions: <mailto:ipc2023-learning@googlegroups.com>

Contact organizers: <mailto:jendrik.seipp@liu.se,javier/dot/segovia/at/upf/dot/edu>

Acknowledgment: some of the text above has been adapted from the IPC 2023
classical track.
