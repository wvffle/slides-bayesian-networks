---
theme: seriph
background: /background.jpg
title: A quick introduction to Bayesian Networks
class: text-center
drawings:
  persist: false
mdc: true
---

# {{ $slidev.configs.title }}

<div class="abs-bl m-6 gap-2 text-left">
  M. Eng. Kasper Seweryn
</div>


---
layout: center
---

# Part 1: Bayes theorem

## https://tinyurl.com/5cr7ua2h


---
layout: center
---
<p class="text-gray-700">
”Steve is very shy and withdrawn, invariably helpful but with very little interest in people or in the world of reality. A meek and tidy soul, he has a need for order and structure, and a passion for detail.”
</p>

<div class="flex justify-center">
<div>
<v-click>
Which is more probable?
</v-click>

<v-clicks>

- A. Steve is a librarian.
- B. Steve is a farmer.

</v-clicks>
</div>
</div>

---
layout: center
---
<p class="text-gray-700">
”Linda is 31 years old, single, outspoken, and very bright. She majored in philosophy. As a student, she was deeply concerned with issues of discrimination and social justice, and also participated in anti-nuclear demonstrations.”
</p>

<div class="flex justify-center">
<div>
<v-click>
Which is more probable?
</v-click>

<v-clicks>

- A. Linda is a bank teller.
- B. Linda is a bank teller and is active in the feminist movement.

</v-clicks>
</div>
</div>

---

# Results

https://docs.google.com/forms/d/e/1FAIpQLSeebFSbE_yjU7P_MyTTZrX9ZcFaUxHgglIMMrKpag_G2lRJOw/viewanalytics

---
layout: center
---

# Bayes theorem
<p></p>
<br>

$$
P(H | E) = \frac{P(H)P(E | H)}{P(E)}
$$


---

# Is Steve a librarian?
<p></p>
<script setup>
import { ref, watch } from 'vue'
import { onSlideEnter, onSlideLeave} from '@slidev/client'
const ph = ref(0.0476)
const peh = ref(0.4)
const penh = ref(0.1)
onSlideEnter(() => {
  [ph.value, peh.value, penh.value] = JSON.parse(localStorage.getItem('p') ?? '[0, 0, 0]')
})
watch([ph,peh,penh],() => {
  localStorage.setItem('p', JSON.stringify([ph.value, peh.value, penh.value]))
})
</script>
<style>h5{color:#444;}</style>

$P(H_L | D) = \frac{P(H_L)P(D | H_L)}{P(D)}$

<div class="grid grid-cols-2 gap-4">
<div>
<v-click>

  ##### What is the ratio of librarians to farmers?
  <div class="flex">

$P(H_{L}) =$
    <input v-model="ph" class="pl-2" />
  </div>
</v-click>
<v-click>

  ##### What percent of librarians fit the description?
  <div class="flex">

$P(D | H_{L}) =$
    <input v-model="peh" class="pl-2" />
  </div>
</v-click>
<v-click>

  ##### What percent of farmers fit the description?
  <div class="flex">

$P(D | H_{F}) =$
    <input v-model="penh" class="pl-2" />
  </div>
</v-click>
</div>
<div>
  <v-click>

##### Knowing that

$P(D) = P(H_L)P(D | H_L) + P(H_F)P(D | H_F)$

  </v-click>

  <v-click>

##### We can plug everything into Bayes theorem

$P(H_L|D) = \frac{P(H_L)P(D | H_L)}{P(H_L)P(D | H_L) + P(H_F)P(D | H_F)} =$

<div class="flex items-center">

$=$
<div class="px-2">
<div class="text-center">
<div class="-mb-4">

{{ph}} $*$ {{peh}}
</div>
</div>
<div class="flex justify-center">
<div class="border-t-1 border-black flex">
<div class="-mt-4">

{{ph}} $*$ {{peh}} + (1 - {{ph}}) $*$ {{penh}}
</div>
</div>
</div>
</div>

$=$
<div class="px-2">
{{ (ph * peh / (ph * peh + (1 - ph) * penh)).toFixed(5) }}
</div>
</div>

  </v-click>
</div>
</div>

---

# A visual approach to the problem
<p></p>
<script setup>
import { ref, watch, computed } from 'vue'
import { onSlideEnter, onSlideLeave} from '@slidev/client'
const ph = ref(0.0476)
const peh = ref(0.4)
const penh = ref(0.1)
onSlideEnter(() => {
  [ph.value, peh.value, penh.value] = JSON.parse(localStorage.getItem('p') ?? '[0, 0, 0]')
})
watch([ph,peh,penh],() => {
  localStorage.setItem('p', JSON.stringify([ph.value, peh.value, penh.value]))
})
const a = computed(() => Math.round(1 / ph.value))
</script>
<style>h5{color:#444;} .marginless-p p{margin: 0 !important}</style>

$P(H_L | D) = \frac{P(H_L)P(D | H_L)}{P(D)}$

<div class="grid grid-cols-2 gap-4">
<div>

  ##### What is the ratio of librarians to farmers?
  <div class="flex">

$P(H_{L}) =$
    <input v-model="ph" class="pl-2" />
  </div>

  ##### What percent of librarians fit the description?
  <div class="flex">

$P(D | H_{L}) =$
    <input v-model="peh" class="pl-2" />
  </div>

  ##### What percent of farmers fit the description?
  <div class="flex">

$P(D | H_{F}) =$
    <input v-model="penh" class="pl-2" />
  </div>
</div>
<div>
<div class="flex">
  <div class="flex border-2 border-black">
  <div class="bg-orange-300">
  <div v-for="i in 10" :class="{'bg-orange-500':i > (1 - peh) * 10}" class="p-1">
    <div class="h-3 w-3 bg-white rounded-full border-orange-300 opacity-60"></div>
  </div>
  </div>
  <div v-for="_ in a - 1" class="bg-green-300">
  <div v-for="i in 10" :class="{'bg-green-500':i > (1 - penh) * 10}" class="p-1">
    <div class="h-3 w-3 bg-white rounded-full border-green-300 opacity-60"></div>
  </div>
  </div>
  </div>
</div>


<div class="flex items-center marginless-p text-sm mt-4">

$P(H_L|D) =$
<div class="px-2">
<div class="flex justify-center">
<div class="bg-orange-300 p-1 flex items-center">

{{ph}} $*$

<div class="bg-orange-600 p-1">
{{peh}}
</div>
</div>
</div>
<div class="flex justify-center">
<div class="border-t-1 border-black flex">

<div class="bg-orange-300 p-1 flex items-center">

{{ph}} $*$

<div class="bg-orange-600 p-1">
{{peh}}
</div>
</div>

$+$

<div class="bg-green-300 p-1 flex items-center">

(1 $-$ {{ph}}) $*$

<div class="bg-green-600 p-1">
{{penh}}
</div>
</div>
</div>
</div>
</div>

$=$
<div class="px-2">
{{ (ph * peh / (ph * peh + (1 - ph) * penh)).toFixed(5) }}
</div>
</div>

</div>
</div>

---

# Is Linda a feminist bank teller?
We could use Bayes theorem once again, or...

<v-click>

  <img src="/venn.png" class="w-1/2" />

</v-click>


---
layout: center
---

# Part 2: Causal graphs

---

# What is causation?
<p></p>

Causation is a relationship between two variables where one variable (the cause) directly produces or influences the occurrence of the other variable (the effect).

<v-clicks>

- Pushing things off a table causes them to fall
- Smoking causes health issues
- Bringing up many controversial topics causes Kasper to express his strong opinions <v-click at="4">(force majeure)</v-click>

</v-clicks>


---

# How can we model causation?
<p>Short answer: with DAGs</p>

<div class="grid grid-cols-2 gap-4">

  <v-click>

```plantuml
@startuml
left to right direction
(A<sub>3</sub>) <-- (B<sub>3</sub>)
(A<sub>2</sub>) --> (B<sub>2</sub>)
(A<sub>1</sub>) -[hidden]-> (B<sub>1</sub>)
@enduml
```

</v-click>
<v-click>

```plantuml
@startuml
top to bottom direction
(Glass shards) --> (Flat tire)
(Nails) --> (Flat tire)
(Broken bottle) --> (Glass shards)
(Thorns) --> (Flat tire)
(Flat tire) --> (Noise)
(Flat tire) -[hidden]- (Speeding)
(Drunk driver) --> (Steering problems)
(Thorns) -[hidden]- (Drunk driver)
(Flat tire) --> (Steering problems)
(Steering problems) --> (Car accident)
(Speeding) --> (Car accident)
@enduml
```

</v-click>
</div>


---

# What about correlations?
<p></p>

Correlation is a statistical measure that indicates the extent to which two variables are linearly related, showing how changes in one variable are associated with changes in another.

<v-clicks>

- Reading ability and shoe size
- Wine consumption and heart attacks
- Ice cream consumption and shark attacks

</v-clicks>
<br>
<v-click>

# ”Correlation does not mean causation”
  <p></p>

  It's truth, only truth but not the whole truth.

</v-click>

<div class="flex gap-16">
<v-click>

```plantuml
  @startuml
  (A<sub>1</sub>) --> (B<sub>1</sub>)
  (A<sub>2</sub>) <-- (B<sub>2</sub>)

  @enduml
```

</v-click>
<v-click>

```plantuml
  @startuml
  (A<sub>3</sub>) -> (C<sub>3</sub>)
  (C<sub>3</sub>) -> (B<sub>3</sub>)

  (C<sub>4</sub>) --> (A<sub>4</sub>)
  (C<sub>4</sub>) --> (B<sub>4</sub>)

  (A<sub>5</sub>) --> (C<sub>5</sub>)
  (B<sub>5</sub>) --> (C<sub>5</sub>)
  @enduml
```

</v-click>
</div>


---
layout: center
---

# Part 3: Bayesian networks


---
layout: two-cols
---

# Bayesian networks


```plantuml
@startuml
top to bottom direction
(Glass shards) --> (Flat tire)
(Nails) --> (Flat tire)
(Broken bottle) --> (Glass shards)
(Thorns) --> (Flat tire)
(Flat tire) --> (Noise)
(Flat tire) -[hidden]- (Speeding)
(Drunk driver) --> (Steering problems)
(Thorns) -[hidden]- (Drunk driver)
(Flat tire) --> (Steering problems)
(Steering problems) --> (Car accident)
(Speeding) --> (Car accident)
@enduml
```

<img src="/apriori-table.png" class="absolute top-56 left-20 w-14" />
<img src="/aposteriori-table.png" class="absolute top-102 left-13 w-26" />



::right::

<v-clicks>

- Every variable has associated probabilities
- Let's us predict outcome of any variable given observations
- Allows us to incorporate expert's knowledge
- Allows us to predict effects of manipulation
- We can learn them from data!

</v-clicks>

---

# Markov condition

<div class="grid grid-cols-2 gap-4">

  <div>

> Any variable is independent of it's non-descendants given it's parents.

<v-clicks>

  - Any two variables are <span style="background:skyblue" class="rounded">dependent</span> if there is a directed connection between them
  - A and B are <span style="background:skyblue" class="rounded">dependent</span> through C if there is a directed connection from C to A and from C to B
  - A and B are conditionally <span style="background:pink" class="rounded">independent</span> given C if C is a direct parent of A and B
  - A and B are conditionally <span style="background:skyblue" class="rounded">dependent</span> given C if there is a directed connection from A to C and from B to C

</v-clicks>

<v-click-gap size="-4" />

  </div>
  <v-switch>
  <template #0>

```plantuml
@startuml
top to bottom direction
(Glass shards) --> (Flat tire)
(Nails) --> (Flat tire)
(Broken bottle) --> (Glass shards)
(Thorns) --> (Flat tire)
(Flat tire) --> (Noise)
(Flat tire) -[hidden]- (Speeding)
(Drunk driver) --> (Steering problems)
(Thorns) -[hidden]- (Drunk driver)
(Flat tire) --> (Steering problems)
(Steering problems) --> (Car accident)
(Speeding) --> (Car accident)
@enduml
```


  </template>
  <template #1>

```plantuml
@startuml
top to bottom direction

(Nails) #WhiteSmoke
(Thorns) #WhiteSmoke
(Drunk driver) #WhiteSmoke
(Noise) #WhiteSmoke
(Speeding) #WhiteSmoke

(Glass shards) --> (Flat tire)
(Nails) --> (Flat tire) #LightGray
(Broken bottle) --> (Glass shards)
(Thorns) --> (Flat tire) #LightGray
(Flat tire) --> (Noise) #LightGray
(Flat tire) -[hidden]- (Speeding)
(Drunk driver) --> (Steering problems) #LightGray
(Thorns) -[hidden]- (Drunk driver)
(Flat tire) --> (Steering problems)
(Steering problems) --> (Car accident)
(Speeding) --> (Car accident) #LightGray

skinparam usecase {
  BackgroundColor #SkyBlue
}
@enduml
```


  </template>
  <template #2>

```plantuml
@startuml
top to bottom direction

(Nails) #WhiteSmoke
(Thorns) #WhiteSmoke
(Drunk driver) #WhiteSmoke
(Glass shards) #WhiteSmoke
(Noise)
(Speeding) #WhiteSmoke
(Broken bottle) #WhiteSmoke

(Glass shards) --> (Flat tire) #LightGray
(Nails) --> (Flat tire) #LightGray
(Broken bottle) --> (Glass shards) #LightGray
(Thorns) --> (Flat tire) #LightGray
(Flat tire) --> (Noise)
(Flat tire) -[hidden]- (Speeding)
(Drunk driver) --> (Steering problems) #LightGray
(Thorns) -[hidden]- (Drunk driver)
(Flat tire) --> (Steering problems)
(Steering problems) --> (Car accident)
(Speeding) --> (Car accident) #LightGray

skinparam usecase {
  BackgroundColor #SkyBlue
}
@enduml
```


  </template>
  <template #3>

```plantuml
@startuml
top to bottom direction

(Nails) #WhiteSmoke
(Thorns) #WhiteSmoke
(Drunk driver) #WhiteSmoke
(Glass shards) #WhiteSmoke
(Noise)
(Speeding) #WhiteSmoke
(Broken bottle) #WhiteSmoke
(Car accident) #WhiteSmoke
(Flat tire) #Red

(Glass shards) --> (Flat tire) #LightGray
(Nails) --> (Flat tire) #LightGray
(Broken bottle) --> (Glass shards) #LightGray
(Thorns) --> (Flat tire) #LightGray
(Flat tire) --> (Noise) #Pink
(Flat tire) -[hidden]- (Speeding)
(Drunk driver) --> (Steering problems) #LightGray
(Thorns) -[hidden]- (Drunk driver)
(Flat tire) --> (Steering problems) #Pink
(Steering problems) --> (Car accident) #LightGray
(Speeding) --> (Car accident) #LightGray

skinparam usecase {
  BackgroundColor #Pink
}
@enduml
```


  </template>
  <template #4>

```plantuml
@startuml
top to bottom direction

(Nails)
(Drunk driver) #WhiteSmoke
(Broken bottle) #WhiteSmoke
(Noise) #WhiteSmoke
(Speeding) #WhiteSmoke
(Steering problems) #WhiteSmoke
(Car accident) #WhiteSmoke
(Flat tire) #Red

(Glass shards) --> (Flat tire)
(Nails) --> (Flat tire)
(Broken bottle) --> (Glass shards) #LightGray
(Thorns) --> (Flat tire)
(Flat tire) --> (Noise) #LightGray
(Flat tire) -[hidden]- (Speeding)
(Drunk driver) --> (Steering problems) #LightGray
(Thorns) -[hidden]- (Drunk driver)
(Flat tire) --> (Steering problems) #LightGray
(Steering problems) --> (Car accident) #LightGray
(Speeding) --> (Car accident) #LightGray

skinparam usecase {
  BackgroundColor #SkyBlue
}
@enduml
```


  </template>
  </v-switch>
</div>

---

# Manipulation

<div class="grid grid-cols-2 gap-4">

  <div>

> Given an external intervention on a variable A in a causal graph, we can derive the posterior probability distribution over the entire graph by simply modifying the conditional probability distribution of A.


If this manipulation is strong enough to set A to a specific value, we can view this intervention as the only cause of A and reflect this by removing all edges that are coming into A. Nothing else in the graph needs to be modified.


  <v-click>
      This is a powerful tool as it provides a systematic way to predict outcomes under hypothetical scenarios, even when direct experiments are infeasible
  </v-click>


  </div>

```plantuml
@startuml
top to bottom direction

(Glass shards) #WhiteSmoke
(Broken bottle) #WhiteSmoke
(Nails) #WhiteSmoke
(Thorns) #WhiteSmoke
(Knife) #LightGreen

(Glass shards) -[hidden]-> (Flat tire)
(Nails) -[hidden]-> (Flat tire)
(Broken bottle) --> (Glass shards)
(Thorns) -[hidden]-> (Flat tire)
(Flat tire) --> (Noise)
(Flat tire) -[hidden]- (Speeding)
(Drunk driver) --> (Steering problems)
(Thorns) -[hidden]- (Drunk driver)
(Flat tire) --> (Steering problems)
(Steering problems) --> (Car accident)
(Speeding) --> (Car accident)
(Knife) --> (Flat tire)
(Knife) -[hidden]-> (Noise)
@enduml
```


</div>

---

# Example!
Example Bayesian Network

<img class="w-full" src="/asia-1.png" />

---

# Example!
Let's say that our patient tells us that he is a non-smoker

<img class="w-full" src="/asia-2.png" />

---

# Example!
We observe that he has a very short breath

<img class="w-full" src="/asia-3.png" />

---

# Example!
We suggest that he should take an XRay image and it turns abnormal

<img class="w-full" src="/asia-4.png" />

---

# Example!
Now, we ask him if he's been to Asia this year

<img class="w-full" src="/asia-5.png" />

---
layout: center
---

# Thank You for Your Attention!
