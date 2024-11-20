---
theme: seriph
background: /background.jpg
title: Learning Bayesian networks and discovering causality from data
class: text-center
drawings:
  persist: false
mdc: true
---

# Learning Bayesian networks and discovering causality from data

<div class="abs-bl m-6 gap-2 text-left">
  mgr inż. Kasper Seweryn
</div>

---

# A few facts about me

<v-clicks>

- I am a programmer with extensive experience with Linux, web technologies, Rust, IoT and Android
- Free and open source contributor
  - 376 issues reported, 159 Pull Requests closed, 6 still open
  - 2014 - 2015 - waff-query
  - 2019 - 2020 - one of maintainers of mineflayer
  - 2021 - 2023 - one of maintainers of funkwhale.audio
- Teaching
  - 2017 - 2019 - climbing instructor
  - 2023 - 2024 - Teaching at Bialystok University of Technology: Programming JavaScript applications, Introduction to WWW applications, Analysis and Testing of IT systems
- Graduate of Bialystok University of Technology
  - 2023 - Bachelor thesis „Design and implementation of a JavaScript front-end framework”
  - 2024 - Master thesis „Efficiency of WASM+Rust compared to JavaScript in the context of web applications”

</v-clicks>

<!--
- issues and PRs on Github ALONE
- waff-query - a DOM manipulation library which was on average faster than jQuery due to a custom parser rather than using regular expressions
- mineflayer - minecraft bot API which was also used to train neural networks learning to play the game - mostly hunting and fixing bugs
- funkwhale - A decentrailized music sharing / library management platform - a complete rewrite of the frontend focusing on performance and UX optimizations

- bachelor - the scope was to create a fast, reliable, reactive and user friendly framework
- master - the scope was to find differences in both technologies which impact the performance and asses which technology is better suited to performance critical web applications
-->

---
layout: center
---

# Bayesian Networks (BNs)

---
layout: image-right
image: /bn.png
backgroundSize: contain
---

# Bayesian Networks
What are Bayesian Networks?

- A BN is a directed acyclic graph (DAG) where nodes represent random variables, and edges represent conditional dependencies between them.
- BNs use Bayes' Theorem to calculate the likelihood of various outcomes by updating beliefs (probabilities) in the light of new evidence.
- They can efficiently compute the probability of certain events or outcomes, even in complex systems, by leveraging conditional independencies between variables.


<!--
They help us model causality

If there's an arrow from node A to node B, B is conditionally dependent on A.

They allow expert knowledge as input for prior probabilities

They deal greatly in cases when there is missing data or low amount of data
-->

---
layout: two-cols
---

# Learning BNs from data
Constraint-Based Search methods: The PC algorithm
<v-clicks>

1. Start with a complete undirected graph
2. Check each pair of vertices using conditional independence tests and remove an edge between them if found

</v-clicks>

<v-click>
<div class="abs-tr p-8">

$A \perp B$ <br>
$A \perp D | B, C$

</div>
</v-click>

<v-clicks at="+2">

3. Check for V-structures and orient the arrows accordingly

</v-clicks>

<v-click at="+2">

4. Orient all remaining edges avoiding cycles and new V-structures

</v-click>



::right::

<DagPc/>

<!--
Peter-Clark

V-structures: X-Z-Y, independent not conditioning on Z

The output is an equivalence class
-->
---
layout: two-cols
---
<script setup>
import { ref } from 'vue'
const score = ref(0)
</script>

# Learning BNs from data
Score + Search methods: Greedy Equivalence Search

<v-clicks>

1. Start with a random DAG
2. Evaluate it's score

</v-clicks>

<v-click>
<div class="abs-tr p-8">Score: {{ score.toFixed(4) }}</div>
</v-click>

<v-clicks>

3. Mutate it either by adding, removing or reversing an arc
4. Evaluate every possible mutation state
5. Choose a DAG with the highest score
6. Repeat from step 3 unitl no better DAG is found
7. Restart with a new random DAG to avoid local maxima

</v-clicks>

::right::

<DagBsl v-model="score" />

---

# Research direction

- Developing a hybrid approach for learning BNs combining both constraint-based and score-based approaches

<br>

<v-click>

<h3>Other potential research directions</h3>
<br>

</v-click>

<v-clicks>

- Parallelization of existing BN learning algorithms
- Developing new heuristics for GES based methods
- Exploring ways to drop faithfullness condition or to deal with casual effects cancelling each other out
- Developing a new algorithm for discovering causality from data from ground up
- Investigating methods for learning Bayesian networks when data is noisy or incomplete

</v-clicks>
---

# My future plans

<v-clicks>

- Family
- Continue teaching at Bialystok University of Technology
- Research

</v-clicks>

---
layout: center
---

# Thank You for Your Attention!
