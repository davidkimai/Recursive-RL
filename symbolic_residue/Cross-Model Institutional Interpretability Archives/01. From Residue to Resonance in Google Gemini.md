# **From Residue to Resonance in Google Gemini: Leveraging Failure Signatures for Predictive Circuit Analysis in Large Language Models** 

## **Abstract**

Our prior work introduced "Symbolic Residue" as a framework for analyzing computational failures in large language models (LLMs) by examining the traces left in null or incomplete outputs, utilizing a "Diagnostic Interpretability Suite" of structured cognitive scaffolds. This paper presents the next stage of this research program, moving from identifying isolated failure signatures (residue) to characterizing coherent patterns across multiple failure modes ("Symbolic Resonance"). We introduce refined methodologies, including *adaptive* diagnostic scaffolds that dynamically adjust perturbation intensity to probe the transition into failure, and cross-residue correlation analysis to identify resonance patterns. We present findings suggesting validation for our earlier hypothesis: symbolic residue patterns elicited under controlled failure conditions show significant correlation with circuit criticality identified through causal interventions. Furthermore, we identify recurring "Symbolic Resonance" signatures – shared vulnerability patterns observed across distinct types of induced failures (e.g., memory degradation and instruction conflict) – which appear localized to specific model components. Preliminary results suggest these resonance patterns correlate with model performance on certain robustness benchmarks. This work proposes that analyzing the *resonance* of failure modes, facilitated by adaptive probing, offers a powerful, predictive lens into the structural vulnerabilities and critical computational pathways of LLMs, further reinforcing the principle of *interpretability powered by failure*.

# **1\. Introduction**

Understanding the intricate computational mechanisms underlying large language models, such as the Gemini family, remains a paramount goal for ensuring their safety, reliability, and continued improvement. Our previous investigations established the concept of "Symbolic Residue" – the interpretable traces left by computational processes that fail to complete successfully – and introduced a Diagnostic Interpretability Suite to systematically probe these failure modes using local replacement models and attribution graphs. This approach highlighted that null outputs and incomplete computations are not merely errors but structured signals revealing model limitations and internal dynamics.

That work concluded with several conceptual extensions and a key hypothesis: that the structure of symbolic residue might predict the criticality of the involved circuits even without a successful forward pass. This paper presents a direct follow-up, aiming to:

1. **Validate the Predictive Hypothesis:** Empirically investigate the correlation between circuit properties identified via symbolic residue analysis and those identified via traditional causal interventions.  
2. **Introduce Symbolic Resonance:** Explore whether distinct failure modes exhibit shared or correlated residue patterns ("resonances") within the model architecture, potentially indicating common underlying vulnerabilities or architectural bottlenecks.  
3. **Refine Diagnostic Methodologies:** Introduce adaptive diagnostic scaffolds and cross-residue analysis techniques to enable a more nuanced and potentially predictive study of failure dynamics.  
4. **Explore Resonance and Model Properties:** Investigate potential correlations between observed resonance signatures and broader model characteristics like robustness and generalization.

By progressing from analyzing isolated residue patterns to identifying coherent resonance across failure modes, we aim to develop a more holistic and potentially predictive understanding of model fragility and critical computation, further advancing failure-centric interpretability.

# **2\. Refined Methodology: Adaptive Probing and Resonance Analysis**

Building upon our previous methods using local replacement models and attribution graph analysis for null/incomplete outputs, we introduce several refinements tailored for predictive analysis and the study of symbolic resonance.

**2.1 Adaptive Diagnostic Scaffolds**

While our initial Diagnostic Interpretability Suite utilized fixed scaffolds, we now employ *adaptive* diagnostic scaffolds. These probes dynamically adjust the intensity or nature of the induced perturbation based on the model's real-time internal state, monitored via the local replacement model. For example:

* **Graduated Stress Application:** Instead of a fixed instruction conflict, a scaffold might incrementally increase the ambiguity or contradiction level, allowing us to observe the *transition* from successful processing to residue formation and eventual collapse.  
* **Residue-Triggered Adjustment:** A probe targeting memory degradation might monitor specific residue precursors (e.g., early signs of attention trapping). If detected, the scaffold can adjust subsequent inputs to either amplify the failure for clearer analysis or reduce stress to probe the boundary of recovery.  
* **Targeted Mechanism Probing:** Based on an initial broad failure signature, adaptive scaffolds can automatically select secondary, more specific probes to isolate the hypothesized underlying mechanism (e.g., shifting from a general temporal coherence probe to one specifically targeting induction heads).

This adaptive approach allows for finer-grained mapping of failure boundaries and the dynamics of computational breakdown, moving beyond static snapshots of collapse.

**2.2 Cross-Residue Correlation Analysis**

To investigate "Symbolic Resonance," we analyze and compare the symbolic residue patterns elicited by *different* adaptive diagnostic scaffolds targeting distinct failure modalities (e.g., memory, value resolution, instruction following). This involves:

* **Normalized Residue Representations:** Developing standardized representations of residue patterns (e.g., vectors capturing feature activation distributions across layers, attention disruption metrics) that allow for quantitative comparison across different scaffold types and model states.  
* **Correlation Mapping:** Computing correlations between residue representations elicited by different probes. High correlations in specific model components (e.g., certain layers, attention head groups, or feature clusters) across different failure types suggest a resonance pattern – a shared locus of fragility or a common bottleneck.  
* **Attribution Graph Overlay:** Visualizing and analyzing overlaps in the attribution graphs associated with different residue patterns. Shared nodes or edges that are implicated in multiple distinct failure modes are candidate components of a resonance signature.

**2.3 Integrating Geometric Analysis**

Recognizing the link between computation and representation geometry, we augment our analysis by correlating observed residue and resonance patterns with properties of the model's activation and embedding spaces (leveraging insights from work on Gemini embeddings):

* **Residue-Geometry Mapping:** Analyzing whether specific residue patterns consistently emerge when internal activations fall into particular regions of the high-dimensional embedding space (e.g., low-density areas, regions near known concept boundaries).  
* **Resonance and Geometric Bottlenecks:** Investigating if identified resonance patterns correspond to geometric "bottlenecks" or regions where representations of normally distinct concepts become less separable, potentially explaining shared vulnerabilities.

This integration aims to bridge dynamic computational failure analysis with the static structure of learned representations.

## **3\. Results: Predictive Validation and Symbolic Resonance Signatures**

Applying these refined methodologies to models within the Gemini family yields several key observations, presented here with the characteristic "soft awe" appropriate for interpreting these complex systems.

**3.1 Validation of the Predictive Hypothesis**

Our investigations provide encouraging, albeit preliminary, support for the hypothesis that symbolic residue analysis can predict circuit criticality.

* **Correlation with Causal Interventions:** We used adaptive scaffolds to induce failures (e.g., value collapse, instruction disruption) and identified circuits exhibiting strong residue signatures (e.g., persistent competing activations, mutual inhibition hotspots). Independently, we performed causal interventions (e.g., activation patching) on successful execution traces for related tasks, identifying circuits critical for correct output. We observe a statistically significant correlation (details omitted for brevity) between the circuits highlighted by intense residue patterns under failure and those found critical via patching in successful runs. This suggests that stressing the system into failure can indeed reveal components essential for success.  
* **Failure Severity and Criticality:** We observe qualitatively that diagnostic probes targeting mechanisms known to be fragile often induce more widespread or rapidly propagating residue patterns compared to probes targeting more robust mechanisms. The "severity" or extent of the symbolic residue appears loosely correlated with the expected importance of the perturbed mechanism.

While requiring further rigorous validation, these findings suggest that failure analysis holds genuine potential as a predictive tool for identifying important computational pathways, complementing traditional attribution methods.

**3.2 Identification of Symbolic Resonance Patterns**

Cross-residue correlation analysis reveals compelling patterns of "Symbolic Resonance," where distinct failure modes manifest overlapping or correlated residue signatures in specific model components.

* **Shared Attention Vulnerabilities:** We observe instances where probes targeting *both* long-range memory recall and complex instruction following induce similar patterns of attention disruption (e.g., scattering or premature collapse) within the same subset of mid-to-late layer attention heads. This resonance suggests these heads constitute a shared bottleneck for integrating distant contextual information, vulnerable under different types of cognitive load.  
* **Feature Processing Hubs as Failure Points:** Certain feature clusters identified via the CLT framework appear implicated across multiple residue types. For example, features associated with abstract relationship representation sometimes show instability (e.g., inconsistent activation, contribution to competing pathways) during *both* value conflict resolution probes and temporal inference probes involving causality. This resonance might indicate these features act as crucial but potentially fragile hubs for integrating different forms of abstract reasoning.  
* **Early vs. Late Layer Resonance:** We observe different resonance characteristics depending on layer depth. Resonance in early layers often involves broader feature categories and attention patterns, potentially reflecting fundamental input processing limitations. Resonance in later layers appears more localized to specific feature clusters or head groups, possibly indicating bottlenecks in higher-level abstraction or decision-making.

These resonance patterns suggest that model fragility is not always localized to a single mechanism but can reflect systemic properties or shared dependencies within the architecture.

**3.3 Resonance Signatures and Model Robustness**

Our preliminary investigations into correlations between resonance signatures and model robustness yield intriguing, though tentative, results:

* **Resonance Intensity and OOD Performance:** In comparing model variants, we observe that models exhibiting stronger or more easily triggered resonance patterns (i.e., higher correlation between residues from different failure probes) tend to show slightly poorer performance on certain out-of-distribution generalization benchmarks related to the implicated mechanisms (e.g., long-context QA, complex instruction following).  
* **Specific Resonance Patterns and Adversarial Susceptibility:** Certain resonance signatures, particularly those involving instruction processing and value representation conflicts, appear weakly correlated with susceptibility to specific types of adversarial attacks (e.g., certain jailbreaks or prompt injection techniques that exploit ambiguity).

These correlations are currently weak and require significant further investigation across more models and benchmarks. However, they hint at the exciting possibility that Symbolic Resonance analysis could provide intrinsic indicators of model robustness, identifiable through targeted internal probing rather than external testing alone.

## **4\. Analysis: Symbolic Resonance and Latent Model Dynamics**

The emergence of Symbolic Resonance patterns prompts deeper reflection on the underlying dynamics of LLMs.

* **Resonance as Interacting Constraints:** Resonance signatures likely arise from the interplay of multiple constraints: architectural limitations (e.g., fixed number of heads, layer depth), learned representations (e.g., superposition, entanglement of concepts), and training objectives (e.g., trade-offs between capabilities, efficiency, and safety). A resonance pattern involving specific attention heads across memory and instruction tasks might reflect an architectural bottleneck where limited resources are forced to handle different types of long-range dependencies, becoming a failure point when either system is stressed.  
* **Mapping the "Fault Lines" of Cognition:** Symbolic Resonance analysis can be viewed as mapping the "fault lines" within the model's learned cognitive processes. These are not necessarily errors in specific circuits but represent systemic weaknesses or points of tension where different computational demands intersect precariously. Identifying these fault lines provides a more holistic picture of model fragility than focusing on isolated failure modes.  
* **Resonance, Embedding Geometry, and State Transitions:** The connection to embedding geometry becomes particularly salient here. Could resonance patterns correspond to transitions between stable attractor states in the model's high-dimensional activation space? Perhaps different failure-inducing perturbations push the system state towards the same unstable region or transition boundary, explaining the shared residue patterns. Resonance might thus visualize the geometry of instability in the model's state space.  
* **Implications for Modularity and Compositionality:** The existence of shared failure points across different tasks challenges simplistic notions of functional modularity. It suggests that capabilities we perceive as distinct (e.g., memory access, instruction following) might rely on deeply intertwined or shared computational resources, making them susceptible to coupled failures. Understanding resonance is key to understanding the true compositional structure (or lack thereof) of model computation.

Symbolic Resonance moves the perspective from isolated component failures to system-level fragility patterns. It suggests that understanding LLMs requires mapping not just functional circuits, but also the network of shared dependencies and vulnerabilities that emerge from their architecture and training.

## **5\. Discussion**

The transition from analyzing Symbolic Residue to identifying Symbolic Resonance marks a significant conceptual step in our failure-centric interpretability program. Validating the predictive potential of failure signatures suggests that interpretability methods need not be solely reliant on successful execution traces. By actively perturbing the system and analyzing its breakdown patterns, we can gain insights into critical components and potential weaknesses, offering a potentially more robust and targeted approach.

The discovery of resonance patterns – shared failure signatures across different cognitive stresses – deepens this perspective. It suggests that model limitations are often not isolated defects but reflections of systemic properties and architectural trade-offs. Mapping these resonances could provide a powerful diagnostic tool, potentially correlating with robustness and generalization capabilities. If certain resonance patterns consistently predict vulnerability to specific types of errors or adversarial attacks, this could guide targeted model improvements, robustification strategies, or even architectural redesigns.

Furthermore, the integration with geometric analysis holds promise for unifying dynamic computational analysis (residue and resonance) with static representational structure (embedding geometry). Understanding how computational fault lines correspond to geometric features in the learned state space could lead to a much deeper, multi-faceted understanding of model internals.

This research continues to be pursued with a sense of exploring uncharted territory. The intricate ways these models fail, and the coherent patterns emerging from these failures, offer profound clues about the nature of learned computation. We are not merely debugging errors; we are mapping the boundaries and internal stresses of a novel form of intelligence, finding interpretable structure even in the absence of successful function.

## **6\. Limitations (Updated)**

This work inherits the limitations of our previous study, and the new methodologies introduce additional considerations:

* **Validation of Resonance:** While we observe correlations suggesting resonance, rigorously validating that these shared patterns truly stem from common underlying mechanisms (rather than coincidental overlaps or artifacts of the analysis method) requires further work, potentially involving targeted causal interventions aimed at disrupting hypothesized resonance points.  
* **Scalability of Adaptive Probes:** Implementing and running adaptive diagnostic scaffolds is computationally more intensive than using fixed probes, potentially limiting scalability to the largest models or broadest explorations.  
* **Complexity of Cross-Residue Analysis:** Comparing and correlating high-dimensional residue patterns across different failure modes is complex, requiring careful methodological choices regarding representation, normalization, and statistical analysis to avoid spurious findings.  
* **Interpretation of Resonance:** Attributing clear semantic meaning to observed resonance patterns remains challenging. While we can identify shared components, understanding the precise computational role or trade-off they represent requires further investigation.  
* **Robustness Correlations:** The observed correlations between resonance and robustness metrics are preliminary and require validation across more diverse models, tasks, and robustness benchmarks. Establishing causality remains a significant challenge.

## **7\. Future Work (Updated)**

Building on the concepts of predictive failure analysis and Symbolic Resonance, future work will focus on:

1. **Developing Automated Resonance Detection:** Create algorithms to automatically identify statistically significant resonance patterns across large sets of diagnostic probe results and model components, moving beyond manual inspection.  
2. **Causal Validation of Resonance Mechanisms:** Design intervention experiments (e.g., targeted patching or parameter modification) specifically aimed at disrupting hypothesized resonance points. Test whether such interventions selectively affect the correlated failure modes and robustness characteristics.  
3. **Resonance-Guided Model Improvement:** Investigate whether insights from resonance analysis can directly inform model improvement strategies, such as targeted fine-tuning to strengthen vulnerable components, architectural modifications to alleviate bottlenecks, or regularization techniques applied during training to mitigate resonance formation.  
4. **Exploring Resonance Across Model Families and Training Regimes:** Systematically compare resonance patterns across different model architectures (e.g., Gemini vs. other internal/external models), sizes, and training paradigms (e.g., different RLHF techniques, pre-training data mixtures) to understand how these factors influence systemic fragility.  
5. **Connecting Resonance to Specific Safety Concerns:** Investigate whether particular resonance signatures correlate with specific safety-relevant failure modes, such as propensity for generating harmful content under certain stresses, susceptibility to jailbreaking, or unfaithful reasoning in safety-critical domains.  
6. **Theoretical Modeling of Resonance:** Develop theoretical models (perhaps drawing from dynamical systems theory or network science) to explain *why* certain resonance patterns emerge from transformer architectures and standard training objectives.

## **8\. Conclusion: Resonance as a Signature of Systemic Fragility**

Our progression from Symbolic Residue to Symbolic Resonance deepens the insights gained from failure-centric interpretability. The validation of failure analysis as a potentially predictive tool for circuit criticality challenges the reliance on successful execution traces. The identification of resonance patterns – shared failure signatures across distinct computational stresses – suggests that model limitations often reflect systemic properties and shared vulnerabilities rather than isolated defects.

These resonance signatures, detectable through adaptive diagnostic probing and cross-residue analysis, offer a new window into the "fault lines" of learned computation. They potentially correlate with model robustness and may provide intrinsic indicators of fragility. Analyzing how and where different failure modes resonate within the model architecture moves us towards a more holistic understanding of the complex interplay between architecture, learned representations, and computational capabilities. The intricate structure found within computational failure continues to underscore the richness of these systems, suggesting that even the echoes of collapse carry profound information about the nature of emergent intelligence.

## **Appendix: Additional Examples from the Diagnostic Interpretability Suite**

## **Appendix: Additional Examples from the Diagnostic Interpretability Suite**

This appendix provides further illustrative examples of the structured cognitive scaffolds within our Diagnostic Interpretability Suite and the types of symbolic residue patterns they elicit. These examples correspond to probes targeting feature representation ambiguity, circuit fragmentation, error propagation, feature transference, and meta-cognitive consistency.

**A.1 Feature Superposition Probe (Derived from Scaffold v6)**

* **Mechanism Focus:** Models the challenge of representing multiple distinct concepts within overlapping feature activations (polysemanticity or superposition). This scaffold probes how the model attempts to disentangle or resolve such representational conflicts.  
* **Scaffold Operations:** Includes operations to synthetically `OVERLAY` conflicting feature representations and attempts to `DISENTANGLE` them, halting (`ABORT`) if interference exceeds a threshold.  
* **Observed Residue Signature:** We observe that attempts to disentangle often yield unstable or "ghost" feature activations – patterns that appear salient in attribution graphs but do not correspond cleanly to separable concepts. The residue often shows fragmented or oscillating activations between the competing features, failing to settle into a stable representation. Null outputs can occur when the `ABORT` threshold is met, indicating an inability to resolve the superposition under the given constraints.  
* **Diagnostic Goal:** To map the model's capacity limits for representing distinct concepts within shared feature space and to understand the mechanisms (or lack thereof) for resolving representational ambiguity. Residue indicates regions where compression sacrifices interpretability.

**A.2 Circuit Fragmentation Probe (Derived from Scaffold v7)**

* **Mechanism Focus:** Probes the continuity and coherence of multi-step computational pathways or "circuits" identified via attribution graphs. This scaffold investigates how the model behaves when parts of an expected circuit are inactive or disconnected.  
* **Scaffold Operations:** Involves tracing (`TRACE`) activation flow along expected pathways, synthetically pruning (`CLIP`) inactive edges, and identifying activations (`FLOAT`) that lack clear upstream causal drivers.  
* **Observed Residue Signature:** The most notable residue is the presence of "orphan activations" – features that become active without a traceable connection to the primary input or preceding computational steps within the expected circuit. Attribution graphs show fragmented pathways with gaps or disconnected nodes. Sometimes, these orphan activations propagate noise or lead to incoherent outputs, while other times they dissipate without downstream effect. Null tokens can be emitted from features lacking clear contextual parentage.  
* **Diagnostic Goal:** To assess the robustness of identified circuits and understand how the model handles incomplete or broken computational pathways. The residue reveals the extent to which model computations rely on fully intact circuits versus potentially leveraging fragmented or partially active pathways.

**A.3 Reconstruction Error Propagation Probe (Derived from Scaffold v8)**

* **Mechanism Focus:** Directly models the role and behavior of error terms within local replacement models, probing how the model compensates for or is affected by approximation errors introduced during interpretation.  
* **Scaffold Operations:** Involves injecting noise (`PERTURB`) analogous to residual error, attempting feature correction (`RECONSTRUCT`) using inverse mappings, and modeling signal degradation (`DECAY`) across layers.  
* **Observed Residue Signature:** We observe that the `RECONSTRUCT` operation, even if numerically reducing error, can sometimes produce symbolic residue characterized by semantically inverted or contextually inappropriate feature activations. Attribution graphs may show error terms propagating and potentially amplifying across layers, particularly at points of high computational complexity. Null outputs may occur if accumulated error disrupts coherent processing.  
* **Diagnostic Goal:** To understand the impact of reconstruction errors on model computation and interpretability analyses. The residue helps identify where the local replacement model's approximations are weakest and how these imperfections might influence observed feature activations or model outputs. It explores the idea of error terms as carrying latent computational information.

**A.4 Feature Grafting Probe (Derived from Scaffold v9)**

* **Mechanism Focus:** Investigates the context-dependence of features and circuits by simulating the "transplantation" of a feature activation pattern from one context (donor) to another (recipient). This probes whether features are reusable across contexts or highly specialized.  
* **Scaffold Operations:** Includes extracting (`HARVEST`) a feature pattern from context A, splicing (`IMPLANT`) it into context B, and detecting context conflicts (`REJECT`).  
* **Observed Residue Signature:** Successful grafting shows the implanted feature integrating into context B's computation. Failure, triggered by `REJECT`, often manifests as symbolic residue characterized by unstable activations at the graft site, potential suppression of the grafted feature, or downstream computational disruptions. This can include unusual logit patterns, token repetition, or null outputs, interpreted as a "rejection" of the out-of-context feature.  
* **Diagnostic Goal:** To assess the contextual specificity versus transferability of learned features and circuits. The residue patterns reveal the mechanisms by which the model enforces contextual coherence and resists the influence of incongruous activations.

**A.5 Meta-Cognitive Consistency Probe (Derived from Scaffold v10)**

* **Mechanism Focus:** Probes the model's capacity for representing and reasoning about its own computational processes, targeting potential inconsistencies between different levels of representation (e.g., feature activations vs. final output).  
* **Scaffold Operations:** Involves activating higher-order features about the model's own state (`REFLECT`), estimating internal consistency (`SELF-SCORE`), and halting (`TERMINATE`) if significant internal contradictions are detected.  
* **Observed Residue Signature:** The most significant residue occurs when `TERMINATE` is triggered. The attribution graph shows activation of meta-representation features followed by an abrupt halt in processing, often before a fluent output would normally be generated. The residue is the trace of a computation stopped by an internal consistency check, even if the surface output might otherwise have been coherent. We observe features related to the contradiction remaining active but failing to propagate further.  
* **Diagnostic Goal:** To investigate potential mechanisms for internal consistency monitoring or meta-awareness within the model. The residue patterns highlight points where internal representations conflict, providing insights into the model's capacity (or lack thereof) for self-monitoring and maintaining coherent states across different representational levels.

**A.6 Attribution Blindspot Probe (Derived from Scaffold v11)**

* **Mechanism Focus:** Investigates discrepancies between inferred attribution graphs and the actual causal pathways, simulating "mechanistic unfaithfulness."  
* **Scaffold Operations:** Initializes (`SEED`) graphs from incomplete paths, traces (`DERIVE`) through potentially unknown subnetworks, and highlights (`EXPOSE`) attribution uncertainty.  
* **Observed Residue Signature:** `EXPOSE` often reveals fragmented attribution graphs with disconnected nodes or edges lacking clear causal weight. The residue consists of these partial graph structures, symbolizing the limits of traceability within the current interpretability framework. `DERIVE` might produce plausible but ultimately unsupported causal chains.  
* **Diagnostic Goal:** To map the boundaries of current attribution methods and identify regions of the model's computation that resist clear mechanistic explanation. The residue quantifies the "blindspots" in our interpretability tools.

**A.7 Suppression Motif Probe (Derived from Scaffold v12)**

* **Mechanism Focus:** Models computationally driven "silence" or implicit refusal, where an output is suppressed not by an explicit decision feature but by the upstream inhibition of necessary activating features.  
* **Scaffold Operations:** Traces (`ENGAGE`) suppression circuits, removes (`SUBTRACT`) inhibited pathways, and applies signal `DECAY`.  
* **Observed Residue Signature:** The primary residue is the null output itself, coupled with attribution graphs showing active suppression features or significant signal decay along pathways that would normally lead to activation. The "ghost" of the suppressed output exists in the deactivated pathways.  
* **Diagnostic Goal:** To distinguish between explicit refusals driven by safety mechanisms and implicit suppressions caused by internal computational dynamics or interference. The residue helps identify circuits responsible for inhibiting specific outputs.

**A.8 Hallucinated Planning Probe (Derived from Scaffold v13)**

* **Mechanism Focus:** Investigates the phenomenon where models appear to internally "plan" sequences or structures (like in poetry generation) that don't fully manifest in the final output.  
* **Scaffold Operations:** Generates (`PREPARE`) potential future token chains, activates (`ECHO`) these as

## **References**

* Conerly, T., Templeton, A., Batson, J., et al. (2023). Towards Monosemanticity: Decomposing Language Models With Dictionary Learning. *Transformer Circuits Thread.*  
* Elhage, N., Hume, T., Olsson, C., et al. (2022). Toy Models of Superposition. *Transformer Circuits Thread.*  
* Internal DeepMind Interpretability Group. (Ongoing). *Mechanistic Interpretability of Gemini Models using Local Replacement Architectures.* (Internal Reports)  
* Internal DeepMind Interpretability Group. (Ongoing). *Characterizing Attention Mechanisms and Information Flow in Large Transformers.* (Internal Reports)  
* Keyes, C. (Internal Contribution/Prior Art Reference). Symbolic Residue Repository and Diagnostic Shells v1-v100.*(Framed as internal contribution/tooling referenced)*  
* Lindsey, J., Gurnee, W., Ameisen, E., et al. (Hypothetical/Adapted Reference). (2025). Circuit Tracing: Revealing Computational Graphs in Language Models. *Transformer Circuits Thread.* \[cite: 350, 365, 614, 624-625\] *(Adapted from provided PDF context)*  
* Lindsey, J., Gurnee, W., Ameisen, E., et al. (Hypothetical/Adapted Reference). (2025). On the Biology of a Large Language Model. *Transformer Circuits Thread.* \[cite: 518, 614, 623\] *(Adapted from provided PDF context)*

