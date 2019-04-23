This page is about the code and data related to our paper
[Lightweight Multi-Language Syntax Transformation with Parser Parser Combinators](https://www.cs.cmu.edu/~rvantond/pdfs/ppc-pldi-2019.pdf).
<details>
  <summary>Click for Citation</summary>
  
```
@inproceedings{vanTonderPLDI2019,
  author =       {Rijnard {van~Tonder} and Claire {Le~Goues}},
  title =        {Lightweight Multi-Language Syntax Transformation with Parser Parser Combinators},
  booktitle =    {Programming Language Design and Implementation},
  series =       {PLDI '19},
  year =         {2019},
  doi =          {10.1145/3314221.3314589},
}
```

</details>

# Data

Check out [PullRequests.md](PullRequests.md) for a summary of our merged Pull Requests.

# Comby Tool

Check out [https://comby-tools.github.io/comby-ui](https://comby-tools.github.io/comby-ui/) if you're interested in trying Comby tool in the browser **right now**.

Check out [https://github.com/comby-tools/comby](https://github.com/comby-tools/comby) if you're interested in the core implementation and command line tool. 

# PLDI VM Artifact

If you're interested in producing results consistent with the tables in the paper, please see the VM instructions below.
Note: the artifact software is outdated, unmaintained, and less fully featured than the software above. To proceed, first [download the VM](https://zenodo.org/record/2642857#.XL4nkJNKjok).

## VM Getting Started Guide

- Please import the VM image with virtualbox. It is an Ubuntu-64 image. The following VM configuration is highly recommended:
  - 2 virtual CPUs, Execution Cap to 100%
  - 4096 MB RAM
  
- After booting up, log in with username/password: vagrant/vagrant

Note: this VM was run on a 2.2GHz MacBook Pro (2018). Experiment output *may differ* depending on CPU and speeds, 
due to timeout settings in our scripts. We have provided expected output. If expected outputs differ wildly, please note it during the evaluation process.

## Step-by-Step Instructions

<details>
  <summary>Click to Expand Instructions</summary>

### Artifact Overview

The paper evaluation is 2-part, broken down as follows:

(1) Large scale multi-language rewriting (§5.1)
  - Table 1: Rewrite patterns and matches when we ran across 1,200 repositories. 248 of 1,200 repositories contain matches (i.e., the aggregate of Proj column in Table 1).
  - Figure 6: Rewrite templates used in Table 1. 
  - Table 2: Rewrite patterns and matches for 50 projects (a subset of Table 1) for which we issued pull requests.
  
(2) Comparison with existing tools (§5.2)
  - Table 3: Tool comparison to 9 existing checker/rewrite tools.
  
In this artifact, we *do not* include the full 1,200 repositories and experiment for reproducing Table 1. 
We provide only the output for Table 1 of our original experiments for reference. There are two reasons:

- (a) With the VM configuration, we incur a 6x to 20x slow down compared to the 20 core server used in the original experiment. This means that running the experiment will take at minimum 5 hours, but is likely to take in excess of 10 hours on the VM.
- (b) It significantly increases (an already large) VM by roughly 70GB. If desired, the full data set is available for download [here](https://cmu.box.com/s/lwbspz39707ywb9pccoi54vvi7lexrtx).

Naturally, performance claims in Table 1 cannot be reproduced in the VM. Instead, we provide the full set of repositories and scripts for reproducing Table 2. 
Because Table 2 is a large, representative, and more detailed subset of Table 1, it supports in part the claims and results of Table 1. 
The results in Table 2 are those rewrites that are part of active projects for which we could validate changes (e.g., with CI builds or compilation).

We include the experiments for (2) in full, and because the evaluation in §5.2 focused on running our tool on a traditional developer machine, performance in the VM should be comparable to that of the paper.

In summary, the claims from the paper supported by this artifact are:

- Application and effectiveness of the DEL representation and parser-matching operationalization to multiple languages; evaluated on 12 languages. (§5.1, Table 1, Table 2)
- Simple declarative specifications for nontrivial transformations (§5.1, Figure 6)
- The transformation is effective in real settings, i.e., real, active, and large projects (§5.1, Table 2)
- Feature/expressiveness parity for a subset of language-specific transformation/syntactic checks in existing tools across multiple languages; evaluated on 9 languages (§5.2, Table 3)
- Lower implementation burden of rewrite patterns compared to existing tool implementation, as a function of LOC (§5.1, Table 2)
- Competitive running time compared to existing tools and meets demands of real-time development response times (§5.1, Table 2)
- Comprehensive rewrite capability compared to a dedicated language-specific tool for Clojure (§5.1, Table 2)

The claims from the paper not supported by this artifact are (due to VM restrictions):

- Fast performance at large scale (§5.1, Table 1, Time column).

### VM Directory structure

Note: Directory naming and scripts do not currently reflect the (new) tool name. Please ignore naming accordingly.

- `comby`: the `Comby` tool, containing the implementation (`src` and `lib`) and rewrite specifications (`catalogue`)
- `table-1`: reference output for our Table 1 experiments
- `table-2`: directory for reproducing results of Table 2
- `table-3`: directory for reproducing results of Table 3
- Everything else: dependencies needed for the above

### Running Experiments and Viewing Results

#### §5.1, Table 1

- Simply run `./table-1.sh` to view aggregate statistics for our large experiment. The individual data files are in the `table-1` directory.

#### §5.1, Table 2

- Simply run `./table-2.sh` for all results
- Expected runtime of VM on 2018 Macbook Pro with recommended configuration: ~30 mins
- A sample of the script can be run for respective languages if desired. Simply `cd ~/table-2/go` and run `./run.sh` for the `go` results, or `cd ~/table-2/python` and run `./run.sh` for the python results, etc. 
- Sample output is given `~/table-2/table-2.out`

Summary of output:

- For each pattern, the output displays the number of matches and lines of code, rounded to the nearest hundred lines of code. It is ordered as in Table 2.
- Pull requests: a list of open and merged pull requests and patches are available in the [PullRequests.md](https://github.com/squaresLab/pldi-artifact-2019/blob/master/PullRequests.md) file.

Notes on output:

- We made a small entry error for the Elm `node-test-runner` project. The number of matches and lines of code correspond to a different, inactive elm project (`json-to-elm`), whereas we did submit a pull request to `node-test-runner`. We have included both projects in the experiments to show the numbers:

```
- node-test-runner
  "number_of_matches": 2,
  "lines_of_code": 40051,
- json-to-elm
  "number_of_matches": 6,
  "lines_of_code": 2089,
```

We will correct the `node-test-runner` row in the camera ready so that matches is `2` and lines of code is `40.1`; apologies.

- There are small differences in the output compared to our experiments on the server: this is expected due to timeout values set when matching on files, and differing performance of VM versus server CPUs. We generally increased timeout values for the scripts in the VM, but it's not possible to know what timeout values correspond to the relative computational power of our server CPUs. This has the effect that more matches may be found (example: Prometheus has 6 matches in the VM results, compared to 2 mentioned in the paper). In other cases, lines of code may increase or decrease by a thousand lines of code or so (e.g., the `Go/go`, `OCaml/pyre-check`, `C/radare2` projects). There are no missing matches due to timeouts. Note that for this reason, output for these experiments may differ slightly on your machine based on the VM configuraton and base hardware.

See the next section for details on inspecting the templates.

#### §5.1, Figure 6

The templates in Figure 6 may all be viewed in subdirectories of the `~/comby/catalogue`. Each pattern contains a `match` and `rewrite` file corresponding to the Match Template and Rewrite Template columns in Figure 6 respectively. Optionally, the directory may contain a `match_rule` or `rewrite_rule` for matching rules. In order of Figure 6:

- `go/staticcheck/redundant-nil-check-loop-s1031`
- `go/staticcheck/replace-with-string-contains-s1003`
- `dart/dart_style/use-is-not-empty`
- `dart/dart_style/use-where-type`
- `dart/dart_style/use-is-not-empty`
- `julia/style/simple-map`
- `julia/performance-tips/tweaks/*`
- `julia/teresy/micro-clone-conditional-or`
- `javascript/teresy/micro-clone-conditional-{and,or}`
- `rust/clippy/style/redundant-field-names-1`
- `rust/clippy/style/redundant-pattern-*`
- `scala/scalafmt/rewrite-rules/redundant-parens-enumerator-guard-for`
- `scala/teresy/use-forall`
- `scala/scapegoat/filter-dot-size`
- `elm/elm-lint/simplify-property-access`
- `elm/elm-lint/simplify-piping/pipe-left`
- `ocaml/teresy/simplify-module-include`
- `ocaml/teresy/simplify-module-functor-include*`
- `c++/clang-tidy/readability/redundant/control-flow-for-continue`
- `clojure/kibit/equality/{nil-*,true,false}`
- `erlang/tidier/append-4.3`
- `python/teresy/micro-clone/*same-conditional-test`

Note: tests for equality specified implicitly in Figure 6 (e.g., `(:[1] || :[1])`) is implemented in the above checks as, for example, `(:[1] || :[2])` and an accompanying `match_rule` file containing `where :[1] = :[2]`. Later versions of our tool support the former notation natively, but this later version does not correspond with the version used during experiments.

Note: the Python template in Figure 6 should actually read `elif`, not `else if` (we will correct in the camera ready). Additionally, the alternatie `brace-same-conditional-test` Python pattern appears with braces to account for preprocessing.

#### §5.2, Table 2

- Simply run `./table-3.sh`. Please note: this command will only output the expected results the first time it is run, due to a setup oversight in the VM. Unfortunately we did not have time to correct this and repackage and reupload the VM to our server. Please direct the output to a file and/or create a snapshot VM before running the command if needed. Also ignore the `fatal: Not a git repository...` messages.
- Expected runtime of VM on 2018 Macbook Pro with recommended configuration: ~40 seconds
- A sample of the script can be run for respective languages if desired. Comment out the undesired parts in `~/table-3/run.sh`.
- Sample output is given `~/table-3/table-3.out`
  
Summary of output:

- Each language is separated by a header:

```
============================
============================
```

- Followed by the language and pattern name.
- Optionally followed by output of the tool we are comparing against. This output can be ignored.
- Followed by the time taken for the pattern of the tool we are comparing against (**Time (ms), Tool** column) in the format:

```
real    0m0.909s 
user    0m1.224s
sys     0m0.083s
```

where we care about `real` time.

- Followed by the statistics (number of matches and lines of code of the file) (**LOC and Matches** columns)
- Followed by the wall time of our tool's time (**Time (ms), Us** column)
- Followed by potential differences in output, and an explanation of why the diff is expected if not empty.
- Optionally followed by a separator `=========================` if there is another pattern, continued by the above.

After this, we print the implementation lines of code for each pattern, that starts with a header: `=========== LOC =========`
- Followed by the pattern name
- Followed by the tool name and the lines of code of its implementation. This is computed either from a local version of the tool if the check is completely contained in a file. Otherwise, it is counted from the lines of code in a reference implementation (only counting the relevant lines), for which we give a a web URL reference to the implementation (**Impl. (LOC) Tool** column)
- Followed by the aggregate lines of code for our match/rewrite templates (**Impl. (LOC), Us** column)

Notes:

- The parens-guard for scala runs twice to fixpoint on the same result of Scalafmt--unfortunately, we overlooked adding the combined time to our tool. The corrected camera ready version will accurately report the time as 260 ms, the typical combined runtime. This does not change the overall claim or results.
- Differences are expected for: erlang (erl-tidy adds unneeded resh variables, as noted in paper); calng-tidy (clang-tidy introduces extra whitespace (a newline)); clojure (static method transformation fails, as noted in paper).
- Line count calculation can be seen by consulting `count.sh` in the respective `lang-comparison` directory under `table-3`.
</details>
