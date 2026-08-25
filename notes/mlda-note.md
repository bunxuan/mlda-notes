# Symbol Emergence and MLDA: Literature Study and Preliminary Reproduction

**Wenxuan Xu**  
August 2026

---

## 1. Theoretical Motivation

Symbol emergence begins with a fundamental tension between **operational closure** and **arbitrariness**. Operational closure means that each agent’s internal states—its perceptions, concepts, and interpretations—are inherently private and inaccessible to others. Arbitrariness means that the mapping between a symbol and its meaning is not fixed by nature but must be constructed and negotiated through interaction. When these two conditions coexist, a deep problem arises: **How can a stable, shared symbol system emerge if agents cannot observe each other’s internal representations and if symbols have no intrinsic meaning?**

This tension makes symbol emergence a nontrivial phenomenon. Shared conventions must arise from interactions among agents who begin with different internal worlds and no guaranteed alignment mechanism. Stability, therefore, cannot be assumed; it must be explained. A symbol is “emergent” only when its use becomes reproducible, predictable, and resilient across interactions—not merely when a latent cluster appears in a model.

It is also important to note that in robotics and embodied AI, the term *symbol* is used more loosely than in linguistics or semiotics, often referring to any discrete internal variable. Thus, discussions of symbol emergence must distinguish between “discrete representational units” and “socially stabilized symbols,” as only the latter truly address the closure–arbitrariness problem.

### Principles Distilled from the Symbol Emergence Framework

The table below presents six conceptual claims that I have synthesized from the Symbol Emergence Systems literature. These are not stated as explicit principles by the authors; rather, they represent recurring theoretical themes that help clarify the gap between the symbolic–interaction perspective and current computational models such as MLDA. Each claim is paired with a corresponding computational question and an assessment of how existing models address (or fail to address) that question.


| No. | Theoretical Claim | Computational Question | Current Computational Status |
|-----|-------------------|------------------------|------------------------------|
| **1** | Symbolic patterns can emerge from interaction rather than being fully predefined. | What counts as “emergence,” and under what conditions does an emergent symbol become *stable* rather than accidental? | MLDA produces latent categories, but stability must be defined and tested; otherwise clusters may reflect random structure rather than genuine emergence. |
| **2** | Agents have private internal states (operational closure), yet communication can establish shared conventions despite arbitrary symbol–meaning mappings. | How can agents form shared symbol conventions without access to each other’s internal representations? | Single-agent multimodal models can form shared latent representations but cannot explain multi-agent interaction or common-ground formation. |
| **3** | Symbol systems are dynamic and should be understood as evolving rather than static snapshots. | How can a computational model adapt to new experience while preserving previously established meanings? | Batch MLDA yields only a static snapshot. Araki et al. (2012) extend MLDA to online concept and word learning, but do not explain how group-level symbol stability is maintained over time. |
| **4** | Symbolic meaning should be grounded in embodied/multimodal experience, not only defined through symbol-to-symbol relations. | How can we determine whether a latent symbol is truly grounded rather than merely reflecting feature correlations? | MLDA integrates multiple modalities into a shared latent concept, but multimodality alone does not guarantee grounding. Here, “grounding” is treated as a stronger requirement than simply combining modalities. |
| **5** | Agents should acquire and use symbols through embodied interaction rather than relying exclusively on predefined symbolic categories. | How can an agent become an active participant in a symbol system through real interaction? | MLDA does not model embodied multi-agent participation; full grounding requires perception, action, communication, and social interaction. |
| **6** | Individual interactions can produce collective conventions, which in turn reshape individuals (micro–macro loop). | How do individual interactions lead to group-level symbol conventions, and how do group conventions feed back into individual learning? | Single-agent latent models cannot express this loop; multi-agent or collective models are required. |

Rows **2**, **5**, and **6** independently point to the same bottleneck:

> **Current multimodal latent models (e.g., MLDA) explain how an individual forms grounded concepts,  
> but they do not explain how multiple agents negotiate, stabilize, and evolve shared symbol systems.**

This gap defines the core research direction:  
**bridging individual multimodal concept formation with collective symbol emergence.**


## 2. From Theory to Computational Models

### 2.1 Latent concepts as internal representations

If symbolic meaning is grounded in experience and shaped through interaction, then internal concepts cannot be treated as predefined labels. They must instead be modeled as latent structures that agents infer from their own multimodal observations. Because agents cannot directly access each other's internal states, a computational account of shared concepts must specify a mechanism by which alignment can arise from observable experience and interaction. In this report, statistical regularities are one modeling choice for studying that problem, not a conclusion that follows from operational closure alone. The computational question becomes how to represent such latent concepts so that they can be inferred, updated, and used for prediction.

### 2.2 Why probabilistic generative models?

Probabilistic generative models provide a natural way to express how hidden concepts give rise to observable data. They specify a forward process—\(z \rightarrow w\)—that captures uncertainty, variation, and multimodal structure. This modeling direction aligns with the theoretical requirement: concepts are unobserved, observations are available, and inference must proceed from evidence back to latent structure. Generative models therefore allow us to ask a meaningful computational question: *if a concept existed, what observations would it generate?*

### 2.3 Bayesian inference: recovering latent concepts

Given observations \(w\), the agent must infer the posterior distribution over latent concepts \(z\). Bayes’ theorem provides the mechanism:



\[
P(z \mid w) \propto P(w \mid z) P(z).
\]



This formulation connects likelihood (how well a concept explains an observation) with prior expectations, producing an updated belief after seeing data. The sequence emphasized in the probability chapter—conditional probability → marginalization → Bayes → posterior → prediction—maps directly onto the computational workflow required for concept formation.

### 2.4 Why probabilistic inference is essential for multimodal concepts

Multimodal concept formation requires a single latent variable to explain observations across different sensory channels. Probabilistic inference offers a unified way to combine heterogeneous evidence: visual cues can update the latent concept, which can then predict auditory or haptic features. This cross-modal inference is central to symbol grounding and multimodal categorization, and it provides the computational basis for emergent communication.

### 2.5 From Probabilistic Modeling to MLDA

MLDA instantiates these ideas in a concrete generative model: a shared latent variable connects visual, auditory, and haptic observations, and posterior inference estimates that latent structure. The original study and the LightMLDA implementation must be distinguished: LightMLDA uses collapsed Gibbs sampling and also provides a Metropolis-Hastings option, while the original paper's inference procedure must be reported from the paper itself. In this sense, MLDA operationalizes the theoretical progression:

**Symbol Emergence Theory → Latent Concepts → Generative Modeling → Bayesian Inference → Multimodal Concept Formation → MLDA.**

This establishes the bridge from theory to computation that the rest of the report builds upon. In particular, multimodal categorization treats heterogeneous perceptual signals as observations generated through a shared latent representation, enabling cross-modal inference. This is the computational role of MLDA in the present report; it should not by itself be equated with socially emergent symbols.


## 3. Reproduction and Analysis

### 3.1 Reproduction Objective

The goal of this study is to develop a detailed, mechanistic understanding of MLDA and to verify the behavior of its open-source LightMLDA implementation. The experiments reported here use the dataset bundled with the repository; they do not use the original paper's dataset or preprocessing pipeline.

This requires clarifying three elements:

- **Original model**  
  MLDA as described in the Symbol Emergence Systems literature: a probabilistic generative model grounded in conditional probability, joint distributions, marginalization, and Bayesian inference.

- **Original experiment**  
  Multimodal categorization of robot-perceived objects using visual, auditory, and tactile features. The original paper's inference procedure must be kept separate from the sampler used by LightMLDA.

- **Implementation used in reproduction**  
  The publicly available LightMLDA implementation (https://github.com/naka-tomo/LightMLDA), which provides a clean and minimal version of MLDA suitable for inspecting the generative process, posterior inference, and category formation.

The objective is to demonstrate how the probabilistic model maps to the actual implementation and to identify the limits of this repository-level verification. This report does not reproduce the entire Symbol Emergence Systems framework or claim a numerical reproduction of the original paper; it focuses on multimodal concept formation as a computationally tractable entry point.


### 3.2 Reproduction Scope

The study is organized into two experimental levels, with the original paper used as a reference point:

1. **Implementation verification:** a six-object hand-worked example for tracing one Gibbs update.
2. **Repository verification:** the multimodal dataset bundled with LightMLDA, used to test the executable and compare modality conditions.

The original published experiment is described separately as a literature reference, but is not reproduced here because its dataset, preprocessing, and evaluation protocol were not used. The repository dataset is therefore treated as an implementation-verification dataset, not as a reproduction of the published experiment.

### 3.3 Experimental Setups

#### 3.3.1 Original Literature Experiment (Reference Only)

The journal publication is dated 2011 in bibliographic databases; some publisher pages identify the online publication as 2012.

| Item | Setting | Source |
|------|---------|--------|
| Modalities | Visual / Auditory / Tactile (3 modalities) | Nakamura et al. (2011), Sec. 3 |
| Visual feature | SIFT descriptors (128-dim) → vector-quantized to a 500-word codebook | Nakamura et al. (2011), Sec. 3.1 |
| Auditory feature | 13-dim MFCC → vector-quantized to a 50-word codebook | Nakamura et al. (2011), Sec. 3.2 |
| Haptic feature | 2-dim (pressure sensor sum + angle change) → vector-quantized to a 5-word codebook | Nakamura et al. (2011), Sec. 3.3 |
| Objects | 50 toys initially collected; narrowed to 40 objects on which 8 human volunteers reached category consensus | Nakamura et al. (2011), Sec. 4.1 |
| Categories | 8 | Nakamura et al. (2011), Sec. 4.1 |
| Inference | Variational EM, as reported in the paper; do not substitute the LightMLDA sampler | Nakamura et al. (2011), Sec. 2.3 |
| Evaluation | Leave-one-out cross-validation on the 40 objects for category recognition; a separate cross-modal prediction task was evaluated independently | Nakamura et al. (2011), Sec. 4.2 |
| Reported result | 100% category recognition accuracy (40/40 objects correctly classified under leave-one-out) | Nakamura et al. (2011), Sec. 4.2 |

**Note on divergence from LightMLDA:** The LightMLDA implementation used for repository verification uses collapsed Gibbs sampling, with a separate Metropolis-Hastings option. Its configuration uses 10 latent categories, and its bundled dataset has 50 objects. The correspondence between that dataset and the published experiment is not established here.

#### 3.3.2 Minimal Hand-Worked Example

| Item | Setting |
|------|---------|
| Objects | 6 |
| Modalities | 1 |
| Vocabulary dimension | 3 |
| Topics | 2 |
| Sampling | Gibbs Sampling |
| Iterations | 100 |
| Purpose | Sanity check, not performance evaluation |

This separation ensures clarity between “understanding the model” and “reproducing the paper.”

#### 3.3.3 LightMLDA Repository Dataset

The repository distributes visual, auditory, and tactile histograms together with category labels. I use this as a repository-level implementation verification dataset. Its precise correspondence to the original published experiment is not established in this report.

| Item | Setting |
|------|---------|
| Objects | 50 |
| Modalities | 3 |
| Vocabulary dimension | 500 50 15 |
| Topics | 10 |
| Sampling | Gibbs Sampling |
| Iterations | 100 |
| Purpose | Repository-level implementation verification |


### 3.4 Implementation Verification

This section checks the correspondence between the implementation and the probabilistic model. It does not by itself establish equivalence to the original paper's full experimental pipeline.

#### Pipeline

```text
Input histogram
       ↓
   SetData()
       ↓
wordID_l  →  observation w
       ↓
 RandomZ() → initial latent topic z
       ↓
  Sampling()
       ↓
N_dz / N_mwz / N_mz  → count tables
       ↓
CalcTheta() / CalcPhi()
       ↓
θ (document-topic) / φ (topic-word)
```

#### Code–Equation Mapping

| Implementation Variable | Mathematical Meaning |
|-------------------------|-----------------------|
| `wordID_l` | observation \( w \) |
| `z_l` | latent topic \( z \) |
| `m_N_dz` | \( N_{d,z} \) |
| `m_N_mwz` | \( N_{m,w,z} \) |
| `m_theta_dz` | \( \theta_{d,z} \) |
| `m_phi_mzw` | \( \phi_{m,z,w} \) |

#### Bayesian Consistency

The implementation must satisfy:

- \( P(z \mid w) = \frac{P(z,w)}{P(w)} \)
- \( P(z,w) = P(w \mid z) P(z) \)
- \( P(w) = \sum_z P(w \mid z) P(z) \)
- \( P(z \mid w) \propto P(w \mid z) P(z) \)

These checks verify the correspondence between the implementation's probability calculations and the probabilistic formulation used in the MLDA model. They do not establish equivalence between the complete LightMLDA implementation and the original paper's full experimental procedure.

---

### 3.5 Toy Experiment: Hand-Worked Gibbs Sampling

#### Purpose and setup

Before reproducing the original experiment, I constructed a small toy dataset to verify my understanding of the LightMLDA implementation.

The dataset contained 6 objects, 1 modality, 3 visual words, and 2 latent categories. The visual histogram for the six objects was:

| Object | w₀ | w₁ | w₂ |
|--------|----|----|----|
| 0      | 5  | 3  | 0  |
| 1      | 6  | 4  | 1  |
| 2      | 4  | 5  | 0  |
| 3      | 7  | 3  | 1  |
| 4      | 5  | 4  | 0  |
| 5      | 6  | 5  | 1  |

I traced how the histogram was converted into token-level observations in `SetData()`. For example, the first object, represented as `(5, 3, 0)`, was expanded into eight observations:

\[
[w_0, w_0, w_0, w_0, w_0, w_1, w_1, w_1].
\]





To make the global statistics complete, assume five additional documents:

| doc | topic0 | topic1 |
|-----|--------|--------|
| d0  | 4      | 4      |
| d1  | 6      | 5      |
| d2  | 5      | 4      |
| d3  | 6      | 5      |
| d4  | 5      | 4      |
| d5  | 6      | 6      |

These counts form the initial 
**N_dz** table.


#### Remove the token from all count tables

Before computing the new topic probability, the token must be removed from the statistics:

- `N_dz[d0][0]` becomes **3**
- global `N_z[0]` decreases by 1
- `N_mwz[w0][0]` decreases by 1

After removal:

- d0: topic0 = 3, topic1 = 4  
- global counts for w0:  
  - topic0: 17  
  - topic1: 15  
- global topic totals:  
  - topic0: 31  
  - topic1: 28  

This “remove first” step ensures the probability is computed **as if this token had no topic yet**.

#### Compute the unnormalized topic scores

MLDA uses:



\[
P(z=k) \propto (N_{d,k}+\alpha)\,
\frac{N_{w,k}+\beta}{N_k + V\beta}
\]

The first term reflects the compatibility between the current document and topic $k$, while the second term represents the probability of observing $w_i$ under topic $k$. The hyperparameter $\alpha$ is the Dirichlet prior for the document-topic distribution, and $\beta$ is the corresponding prior for the topic-word distribution. The term $V\beta$ appears because the vocabulary contains $V$ possible observations, each contributing a prior mass of $\beta$. Thus, the denominator normalizes the topic-specific word distribution.



With α = 25, β = 0.1, V = 3:

#### Topic 0

- document preference: \(3 + 25 = 28\)
- word–topic association: \(17 + 0.1 = 17.1\)
- normalization: \(31 + 0.3 = 31.3\)

Score:



\[
P_0 = 28 \times \frac{17.1}{31.3} \approx 15.30
\]



#### Topic 1

- document preference: \(4 + 25 = 29\)
- word–topic association: \(15 + 0.1 = 15.1\)
- normalization: \(28 + 0.3 = 28.3\)

Score:



\[
P_1 = 29 \times \frac{15.1}{28.3} \approx 15.48
\]



#### Normalize to obtain probabilities



\[
P(z=0)=\frac{15.30}{15.30+15.48}\approx 0.497
\]




\[
P(z=1)=\frac{15.48}{15.30+15.48}\approx 0.503
\]



Thus the token is almost equally likely to belong to either topic.

#### Draw a random sample

If the random number is:

- **r = 0.3** → falls in topic0 → assign topic0  
- **r = 0.8** → falls in topic1 → assign topic1  

This stochastic choice is what allows Gibbs sampling to explore the posterior distribution.

#### Add the token back into the count tables

Finally, the chosen topic is added back:

- `N_dz[d0][topic]++`
- `N_mwz[w0][topic]++`
- `N_z[topic]++`

If topic0 was chosen, d0 returns to:

- topic0 = 4  
- topic1 = 4  

If topic1 was chosen, d0 becomes:

- topic0 = 3  
- topic1 = 5  

#### What this update really means

Compressed into conceptual form:

1. Take a token \(w_i\) out of the model.  
2. Compare how much the document “prefers” each topic.  
3. Compare how much the word \(w_i\) “fits” each topic.  
4. Combine these two factors into a probability.  
5. Sample a new topic according to that probability.  
6. Insert the token back with its new topic.

#### From Sampling Updates to Latent Concept Distributions

The Gibbs update described above explains how individual tokens gradually settle into stable topic assignments. After many iterations, these assignments accumulate into three count tables:

- **N_dz** — how often document *d* uses topic *z*  
- **N_mwz** — how often word *w* in modality *m* is assigned to topic *z*  
- **N_mz** — total observations in modality *m* assigned to topic *z*

These tables are the raw statistical structure produced by sampling.  
MLDA then converts this structure into two probability distributions that define the latent concepts:  
**θ (document–topic distribution)** and **ϕ (topic–word distribution)**.

---

#### θ: What topics characterize each object?



\[
\theta_{d,z} = \frac{N_{d,z} + \alpha}{N_d + K\alpha}
\]



θ answers: **For object *d*, what is the probability of each latent topic?**

The following is an independent illustrative example, not a set of converged counts from the six-object toy dataset above. It is included only to demonstrate how the formula is evaluated at a larger count scale. If sampling yields:

- d0: topic0 = 1400, topic1 = 200  
- \(N_{d0} = 1600\), \(K = 2\), \(\alpha = 25\)

then:



\[
\theta_{0,0} \approx 0.864,\quad
\theta_{0,1} \approx 0.136
\]



This distribution expresses the latent identity of the object.

---

#### ϕ: What features characterize each topic?



\[
\phi_{m,z,w} = \frac{N_{m,w,z} + \beta}{N_{m,z} + V\beta}
\]



Here, $V$ is the vocabulary size for modality $m$ and, in this illustrative example, $V=3$ and $\beta=0.1$. ϕ answers: **For topic *z*, what words does it tend to generate?**

If topic0 has:

- w0 → 1000  
- w1 → 500  
- w2 → 100  

then:



\[
\phi_{0,0,w0} \approx 0.625,\quad
\phi_{0,0,w1} \approx 0.3125,\quad
\phi_{0,0,w2} \approx 0.0625
\]



This distribution describes the semantic profile of topic0.

---

#### Putting it together

Gibbs sampling provides the mechanism:

- remove a token  
- evaluate topic fit  
- sample a new topic  
- update counts  

Repeated many times, this yields stable count tables.  
From these tables:

- **θ = P(z|d)** tells us *what each object is*  
- **ϕ = P(w|z)** tells us *what each topic means*

Together, they transform sampling dynamics into interpretable latent concepts.


This is one complete Gibbs sampling step. Repeating this for all tokens over many iterations gradually reveals the latent category structure of the data.



#### Output Files

- `Nmwz000.txt`  
- `Ndz.txt`  
- `theta.txt`  
- `phi.txt`

These are checked against the expected probabilistic structure.

**This experiment is used purely as a sanity check and not as a performance evaluation.**

---

### 3.6 Relation to the Original Experiment

The original experiment is not reproduced in this study. The table in Section 3.3.1 records the published setup as a reference, whereas the experiments below use the LightMLDA repository dataset and implementation.

#### Procedure

The repository-level experiments verify that the supplied executable can process multimodal histograms and produce latent category assignments. They should not be interpreted as a numerical reproduction of the published results.

---


### 3.7 Preliminary Results and Verification

| Modality condition | Inference | Weights | Accuracy |
|---|---|---|---|
| All three modalities (Vision + Audio + Tactile) | Gibbs | 280 / 340 / 160 | 0.70 |
| Vision only | Gibbs | 200 / - / - | 0.46 |
| Audio only | Gibbs | - / 200 / - | 0.64 |
| Tactile only | Gibbs | - / - / 200 | 0.42 |

**Observation.** In the explicit Gibbs runs, the audio-only condition achieved higher accuracy than the vision-only and tactile-only conditions. Audio alone (0.64) is also relatively close to the multimodal Gibbs result (0.70), although the multimodal run uses a different weight policy. This is consistent with the repository's default design, which assigns audio the highest modality weight (vision=280, audio=340, tactile=160), but it is not evidence that audio is generally the most informative modality.

**Caveats.** This comparison is based on one repository dataset and one executable run for each condition; each run used 100 trials and retained the best accuracy. The random seed is not explicitly controlled or reported, and the bundled category semantics are not documented. The result should therefore be confirmed across multiple independently seeded runs before making claims about modality ranking or stability. No claim is made about why audio is more discriminative for these particular objects.

These results are preliminary repository-level observations rather than a controlled modality comparison. Although the table uses Gibbs for all four conditions, the weight policies differ, so these values should not be used to rank the modalities. The published experiment and the LightMLDA repository run also differ in dataset, category setup, preprocessing, evaluation protocol, and inference implementation; a direct comparison with a headline accuracy from the paper would therefore compare different experiments rather than measure reproduction fidelity.

#### What can be reported

1. **Protocol requirement.** The current measurements do not provide a clean multimodal-versus-single-modality comparison. A fair comparison requires rerunning all four conditions with the same sampler, weights, trial count, and seed policy.


#### What this does and does not demonstrate

This study does not replicate the original paper's headline accuracy because the experimental conditions differ. It demonstrates that the LightMLDA executable can process the bundled multimodal data and produce category assignments under the tested conditions. It does not establish social symbol emergence, cross-dataset generalization, or stability across random seeds.
### 3.8 Analysis and Discussion

#### What the model demonstrates

- MLDA can form shared latent concepts from multimodal observations.  
- Cross-modal inference emerges naturally from the shared latent variable.  
- Bayesian inference provides a principled mechanism for concept formation.

#### What the model does not demonstrate

- MLDA alone does not explain how multiple agents negotiate shared symbols.  
- It does not model long-term social stabilization of meaning.  
- It does not address symbol evolution or adaptation.

### 3.9 Limitations and Open Questions

The current reproduction focuses on a probabilistic multimodal concept model rather than a full multi-agent symbol emergence system.  
Therefore, social interaction, collective convention formation, and temporal adaptation remain outside the scope of the current study.
#### Open Questions and Future Directions

The toy experiment clarifies how MLDA forms latent topics through Gibbs sampling and how θ and ϕ translate sampling statistics into probabilistic concepts. However, several theoretical questions remain open and point toward future research directions.

**1. How do symbols emerge, and what counts as stability?**  
MLDA can produce persistent assignments within a given run, but whether those assignments are stable across random initializations, datasets, or interactions has not been systematically evaluated. One operational definition worth testing is **recovery speed**: after a controlled perturbation of the observations or assignments, how quickly does the system return to its previous categorization structure? This could be connected to the shuffle-ratio ablation used in my world-model analysis, by measuring recovery as the fraction of observations or transitions that are shuffled increases. A symbol may need to recover reliably while remaining flexible enough to incorporate genuinely new experience.

**2. How can agents establish common ground?**  
Different random initializations are expected to cause latent-category label permutations and may also affect the learned assignments; this has not yet been systematically evaluated in the current study. Real agents differ not only in data but also in embodiment, sensors, and learning mechanisms. This suggests that common ground cannot be a passive byproduct of “using the same method”; it likely requires an explicit negotiation process.

**3. How do individual interactions scale into social conventions?**  
Symbol Emergence Systems emphasizes the micro–macro loop: individual learning shapes collective conventions, which in turn reshape individuals. A computational model that captures this loop must allow symbols to be both self-organized and socially stabilized. Whether such conventions must first emerge spontaneously before being defined is still unclear.

**4. How can symbol systems adapt without collapsing?**  
A symbol system must incorporate new experience without losing previously established meanings. Humans learn new words by extending existing conceptual structures rather than replacing them. Designing computational models that support such incremental, non-destructive adaptation remains a challenge.

**5. Are shared symbols truly grounded in embodied experience?**  
Robotic multimodal grounding and human embodied grounding may behave similarly at the functional level, yet it is unclear whether they constitute the same phenomenon. Understanding the relationship between artificial multimodal concepts and human embodied meaning is an important open question.

These questions highlight the gap between current probabilistic models and the broader theoretical landscape of symbol emergence. They also suggest that MLDA, while useful for studying latent concept formation, represents only one component of a larger system required for shared, adaptive, and socially meaningful symbols.

## References

1. Taniguchi, T. (Ed.). (2026). Symbol Emergence Systems: An Interdisciplinary Discussion about Cognition, Language and Society. Springer. https://doi.org/10.1007/978-981-95-1327-7
2. Nakamura, T., Araki, T., Nagai, T., & Iwahashi, N. (2011). *Grounding of Word Meanings in Latent Dirichlet Allocation-Based Multimodal Concepts*. Advanced Robotics, 25(17), 2189–2206. Published online in 2012.
3. naka-tomo. *LightMLDA*. GitHub repository. https://github.com/naka-tomo/LightMLDA
4. Araki, T., Nakamura, T., Nagai, T., Nagasaka, S., Taniguchi, T., & Iwahashi, N. (2012). *Online learning of concepts and words using multimodal LDA and hierarchical Pitman-Yor language model*. IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS), 1623–1630.




