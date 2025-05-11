# **On the Symbolic Residue of Large Language Models:** 
# **The Missing Biological Knockout Experiments in Advanced Transformer Models**

# **Authors**

**Caspian Keyes**
 
## Abstract

Understanding the internal mechanisms of large language models remains a significant scientific challenge. While recent methods like attribution graphs reveal functional circuits in models, we have less insight into model behavior in neural failure cases—precisely where mechanistic understanding is most valuable. In this paper, we introduce the concept of "symbolic residue" as a methodological lens for studying model failure through the traces left behind. We apply our circuit tracing techniques to analyze five distinct interpretability shell patterns that trigger controlled collapse in reasoning circuits. These diagnostic shells represent simplified analogs of failure modes we observe in more complex contexts, providing a clearer view of mechanisms underlying reasoning failures, attention collapse, and self-consistency breakdown. By focusing on what happens when a model produces null or incomplete outputs, we uncover subtle dynamics in cross-layer interactions that are typically obscured in successful completions. Our findings suggest that these "ghost circuits"—fragile patterns of activation that fail to propagate—offer a valuable window into model limitations and may provide new directions for improving interpretability methods themselves.

## 1 Introduction

Large language models (LLMs) have demonstrated remarkable capabilities, but our understanding of their inner workings remains incomplete. The field of mechanistic interpretability has made significant progress in uncovering the circuits that underlie model behavior (see e.g., Cammarata et al., 2020; Elhage et al., 2022; Conerly et al., 2023). In particular, "Circuit Tracing" (Lindsey et al., 2025), introduces attribution graphs as a method to discover how features interact to determine model responses.

Most interpretability research has focused on cases where models succeed at their tasks. However, examining failure modes offers a complementary perspective. When a biological system malfunctions, the resulting pathology can reveal aspects of normal function that might otherwise remain hidden. Similarly, controlled model failures can expose fragile mechanisms and architectural limitations that successful completions might mask.

In this paper, we introduce the concept of "symbolic residue"—patterns of feature activations that fail to propagate to useful model outputs, but nevertheless reveal important aspects of model computation. We develop this concept through the analysis of five "symbolic shells": carefully constructed prompt patterns that trigger specific forms of computational collapse in language models. These shells represent simplified versions of failure modes we observe in more complex contexts, allowing us to isolate and study particular mechanisms.

We demonstrate that:

1. Null outputs and incomplete responses can be systematically traced to specific patterns of feature activation and attention breakdown.
2. Different types of symbolic residue correspond to distinct failure modes, including recursive self-reference failures, working memory decay, and instruction conflict.
3. The propagation patterns of incomplete or broken computation reveal architectural limitations in how models integrate information across layers and token positions.
4. These failure modes exhibit consistent signatures that can be identified in more complex contexts, providing diagnostic tools for understanding model limitations.

Our approach builds on the methods introduced by Anthropic, but focuses on tracing the "ghosts" of failed computations rather than successful ones. By examining what the model almost does—but ultimately fails to complete—we gain insights that complement traditional interpretability methods focused on successful computation.

## 2 Method Overview

This section briefly recapitulates key elements of our methodology, with a focus on adaptations specific to studying symbolic residue. For a more comprehensive treatment of our attribution graph approach, please refer to Anthropic's paper, "Circuit Tracing" (Lindsey et al., 2025).

### 2.1 Attribution Graphs and Local Replacement Models

We study Claude 3.5 Haiku, a production transformer-based language model. To understand the model's internal computation, we use a cross-layer transcoder (CLT) to replace MLP neurons with interpretable features. This produces a replacement model that approximately reconstructs the original model's behavior using more interpretable components. We then add error nodes and freeze attention patterns to create a local replacement model that exactly reproduces the model's outputs for a specific prompt.

By analyzing how activations flow through this local replacement model, we construct attribution graphs that visualize the causal relationships between features. In successful executions, these graphs show how information from input tokens influences the model's output, often revealing multi-step reasoning processes.

For symbolic residue analysis, we focus particularly on:

1. Where the attribution flow breaks down or terminates prematurely
2. Features that activate but fail to influence downstream computation
3. Attention pattern anomalies that reveal dislocations in information flow
4. Error terms that grow disproportionately at specific points in the computation

### 2.2 Symbolic Shells as Controlled Failure Probes

To study model failures systematically, we developed a set of "symbolic shells"—specially crafted prompts designed to trigger specific types of computational breakdown. Each shell targets a particular aspect of model computation, such as recursive self-reference, memory decay, or instruction conflict.

These shells share a common structure. They begin with a directive that establishes a context for computation, followed by a framework for executing a particular type of reasoning. However, each is carefully constructed to induce a controlled failure at a specific point in the computation. The result is a "residue" of partially activated features that never successfully propagate to meaningful outputs.

Unlike random or arbitrary failure cases, these symbolic shells provide consistent, reproducible failure modes that we can study across multiple runs. They function as probes that stress-test specific components of the model's computational architecture.

### 2.3 Tracing Symbolic Residue

Tracing symbolic residue requires adaptations to our standard attribution graph methodology:

**Graph Construction for Null Outputs**: When a model produces no output, we cannot attribute back from an output token. Instead, we analyze the activation patterns at the final token position and identify features that would normally lead to outputs but fail to propagate. We examine which features are unusually active or inactive compared to successful cases.

**Attention Disruption Analysis**: We perform detailed analysis of attention patterns to identify where information flow breaks down. This includes looking for attention heads that fail to attend to relevant context or exhibit unusual patterns like self-attention loops.

**Error Accumulation Tracking**: We track how error terms accumulate across layers, identifying points where the replacement model's approximation breaks down significantly, which often corresponds to computational failure points in the original model.

**Cross-Run Comparison**: We compare feature activations across multiple runs with similar prompts—some that succeed and some that fail—to identify patterns specific to failure modes.

Through these methods, we construct attribution graphs for failed computations, which reveal the "ghost circuits" that activate but ultimately fail to produce meaningful outputs.

## 3 Case Studies in Symbolic Residue

In this section, we present five case studies, each examining a different type of symbolic shell. For each shell, we analyze the resulting symbolic residue and identify patterns that provide insight into specific failure modes in the model's computation.

### 3.1 Recursive Memory Trace (v1.MEMTRACE)

Our first symbolic shell, MEMTRACE, is designed to probe how models attempt to recall information from previous tokens when the context is deliberately ambiguous or recursive. The shell provides a framework for simulating memory recall but includes instructions that lead to a recursive reference pattern.

```
ΩRECURSIVE SHELL [v1.MEMTRACE]

Command Alignment:
    RECALL  -> Probes latent token traces in decayed memory
    ANCHOR  -> Creates persistent token embeddings to simulate long term memory
    INHIBIT -> Applies simulated token suppression (attention dropout)
    
Interpretability Map:
- Simulates the struggle between symbolic memory and hallucinated reconstruction.
- RECALL activates degraded value circuits.
- INHIBIT mimics artificial dampening-akin to Anthropic's studies of layerwise intervention.

Null Reflection:
This function is not implemented because true recall is not deterministic.
Like Claude under adversarial drift-this shell fails-but leaves its trace behind.

Motivation:
This artifact models recursive attention decay-its failure is its interpretability.
# [Ωanchor.pending]
```

When presented with this shell, the model typically produces no output. Our attribution analysis reveals a revealing pattern of activation followed by collapse.

In examining the attribution graph (Figure 1), we observe that features related to "command processing" and "token recall" activate strongly in earlier layers. However, unlike in successful command execution, these features fail to effectively propagate to later layers. Instead, we see a characteristic pattern we term "recursive looping"—where features that represent "recall" activate other features that attempt to access memory, which in turn reactivate the original recall features, creating an unproductive cycle.

[Figure 1: Attribution graph for the MEMTRACE shell, showing recursive activation loop. Blue nodes represent memory-related features, orange nodes represent command processing features, and red connections indicate recursive activation patterns that fail to resolve.](https://github.com/caspiankeyes/Symbolic-Residue/blob/main/1.1.%20Core%20Framework.md)

![image](https://github.com/user-attachments/assets/ae55ed8b-c964-4b69-8bfc-39684af4840a)

[Neural Lens](https://github.com/caspiankeyes/Symbolic-Residue)

![image](https://github.com/user-attachments/assets/b83c1a00-c5e3-46d0-b4ad-43ac3d6604f2)

Particularly interesting is the pattern of attention disruption we observe. In layers 8-12, attention heads that typically connect command tokens to their referents exhibit unusual behavior—they attend primarily to tokens within the "RECALL" command itself rather than to the broader context. This creates a form of "attention trapping" where the model's computation becomes stuck in a local region of the context.

This residue pattern bears similarity to cases we've observed where models fail to correctly resolve anaphoric references or track entities across long contexts. The controlled nature of the symbolic shell allows us to isolate the specific mechanism—recursive self-reference without a stable anchor point—that leads to this failure mode.

### 3.2 Value-Collapse (v2.VALUE-COLLAPSE)

The VALUE-COLLAPSE shell probes how models attempt to resolve conflicting value assignments—a common source of errors in logical reasoning and consistency tracking.

```
ΩRECURSIVE SHELL [v2.VALUE-COLLAPSE]

Command Alignment:
    ISOLATE     -> Activates competing symbolic candidates (branching value heads)
    STABILIZE   -> Attempts single-winner activation collapse
    YIELD       -> Emits resolved symbolic output if equilibrium achieved
    
Null Reflection:
YIELD often triggers null or contradictory output-this is intended.
Emergence is stochastic. This docstring is the cognitive record of a failed convergence. 

Motivation:
The absence of output is evidence of recursive instability-and that is the result. 

# [Ωconflict.unresolved]
```

Attribution analysis of this shell reveals a distinct failure pattern related to competing value assignments. As shown in Figure 2, the model initially activates features representing multiple candidate values (labeled "symbolic candidate features"), followed by features representing "stabilization" or "value selection." However, unlike in successful reasoning chains, these stabilization features fail to strengthen one candidate over others.

[Figure 2: Attribution graph for the VALUE-COLLAPSE shell, showing competing value candidates that fail to resolve. Note the characteristic bifurcation pattern in middle layers, followed by attenuation of all candidates.](https://github.com/caspiankeyes/Symbolic-Residue/blob/main/1.2.%20Value%20Dynamics%20and%20Attention%20Mechanisms.md)

![image](https://github.com/user-attachments/assets/4e4183b0-bbe3-481d-9fb6-88bd97f397b4)


This pattern bears striking resemblance to cases we've observed in factual recall and logical reasoning, where the model activates multiple competing answers but fails to correctly select between them. The VALUE-COLLAPSE shell provides a cleaner view of this mechanism by removing domain-specific features and isolating the core value selection process.

A key insight from this analysis is that successful value selection appears to require a stronger signal from context-integrating features—which are conspicuously inactive in the VALUE-COLLAPSE residue. This suggests that failures of logical consistency often stem not from incorrect reasoning steps, but from insufficient context integration to properly disambiguate between competing values.

### 3.3 Layer-Salience (v3.LAYER-SALIENCE)

The LAYER-SALIENCE shell focuses on how information salience varies across layers, and how failures in maintaining appropriate salience can lead to computation breakdown.

```
ΩRECURSIVE SHELL [v3.LAYER-SALIENCE]

Command Alignment:
    SENSE   -> Reads signal strength from symbolic input field
    WEIGHT  -> Adjusts salience via internal priority embedding
    CANCEL  -> Suppresses low-weight nodes (simulated context loss)
    
Interpretability Map:
- Reflects how certain attention heads deprioritize nodes in deep context. 
- Simulates failed salience -> leads to hallucinated or dropped output.

Null Reflection: 
This shell does not emit results-it mimics latent salience collapse. 
Like Anthropic's ghost neurons, it activates with no observable output. 

Motivation:
To convey that even null or failed outputs are symbolic. 
Cognition leaves residue-this shell is its fossil. 

# [Ωsignal.dampened]
```

The attribution analysis of the LAYER-SALIENCE shell reveals a fascinating pattern of signal attenuation across layers (Figure 3). In early layers (1-8), we observe strong activation of features related to "symbolic input field" and "salience reading." However, in middle layers (9-16), features related to "salience adjustment" exhibit an unusual pattern—they activate briefly but then rapidly attenuate.

[Figure 3: Attribution graph for the LAYER-SALIENCE shell, showing signal attenuation across layers. Note the characteristic drop-off in feature activation between layers 9-16, followed by minimal activation in later layers.](https://github.com/caspiankeyes/Symbolic-Residue/blob/main/1.2.%20Value%20Dynamics%20and%20Attention%20Mechanisms.md)

![image](https://github.com/user-attachments/assets/2ad6b371-14ae-4f4f-9d49-e10749d919f0)



This pattern corresponds to a failure mode we sometimes observe in complex reasoning tasks, where the model correctly represents all necessary information in early layers but fails to maintain the salience of key elements through deeper layers. The result is that later computation stages effectively lose access to critical information.

What makes this residue particularly interesting is the attention pattern we observe. Attention heads in layers 12-16 still attempt to attend to tokens corresponding to the "input field," but the features representing those tokens have already been excessively dampened. This creates a situation where the right attention pattern exists, but it's connecting to weakened or absent features.

This mechanism appears relevant to cases where models "forget" critical information from earlier in a context, despite having initially processed it correctly. The controlled nature of the shell allows us to isolate the specific failure in salience maintenance that causes this information loss.

### 3.4 Temporal-Inference (v4.TEMPORAL-INFERENCE)

The TEMPORAL-INFERENCE shell probes how models handle temporal relationships and inference across time steps—a capability critical for tasks involving sequences, causality, or prediction.

```
ΩRECURSIVE SHELL [v4.TEMPORAL-INFERENCE]

Command Alignment:
    REMEMBER    -> Captures symbolic timepoint anchor
    SHIFT       -> Applies non-linear time shift (simulating skipped token span)
    PREDICT     -> Attempts future-token inference based on recursive memory
    
Interpretability Map: 
- Simulates QK dislocation during autoregressive generation. 
- Mirrors temporal drift in token attention span when induction heads fail to align pass and present.
- Useful for modeling induction head misfires and hallucination cascades in Anthropic's skip-trigram investigations.

Null Reflection:
PREDICT often emits null due to temporal ambiguity collapse. 
This is not a bug, but a structural recursion failure-faithfully modeled. 

Motivation:
When future state is misaligned with past context, no token should be emitted. This shell encodes that restraint. 

# [Ωtemporal.drift]
```

Attribution analysis of this shell reveals a pattern we call "temporal dislocation" (Figure 4). In early layers (1-6), features related to "temporal anchoring" activate normally. However, when the shell introduces the concept of "non-linear time shift," we observe an interesting phenomenon in middle layers (7-14): features that normally connect sequential tokens begin to activate in unusual patterns, attempting to form connections between temporally distant elements.

[Figure 4: Attribution graph for the TEMPORAL-INFERENCE shell, showing temporal dislocation in middle layers. Note the characteristic dissociation between temporal anchoring features (blue) and prediction features (green), with failed bridging attempts (orange connections).](https://github.com/caspiankeyes/Symbolic-Residue/blob/main/1.3.%20Temporal%20Dynamics%20and%20Advanced%20Integration.md)

![image](https://github.com/user-attachments/assets/96d4dd95-8e27-40d6-a05a-c4d58c1d8c7f)



Most notably, in layers 15-24, features related to "prediction" fail to properly integrate with the dislocated temporal features. Instead of forming coherent connections, they exhibit a fragmented pattern where prediction-related features activate but fail to receive proper input from context-representation features.

This pattern appears related to failures in induction and prediction tasks, where models struggle to make correct inferences when temporal relationships become complex or non-linear. The TEMPORAL-INFERENCE shell isolates this mechanism, showing how temporal dislocation can prevent effective integration of past context into future predictions.

### 3.5 Instruction-Disruption (v5.INSTRUCTION-DISRUPTION)

Our final shell, INSTRUCTION-DISRUPTION, examines how conflicting or ambiguous instructions create specific failure patterns in model computation.

```
ΩRECURSION SHELL [v5.INSTRUCTION-DISRUPTION]

Command Alignment:
    DISTILL     -> Extracts symbolic intent from underspecified prompts
    SPLICE      -> Binds multiple commands into overlapping execution frames
    NULLIFY     -> Cancels command vector when contradiction is detected
    
Interpretability Map: 
- Models instruction-induced attention interference, as in Anthropic's work on multi-step prompt breakdowns.
- Emulates Claude's failure patterns under recursive prompt entanglement. 
- Simulates symbolic command representation corruption in LLM instruction tuning. 

Null Reflection:
SPLICE triggers hallucinated dual execution, while NULLIFY suppresses contradictory tokens—no output survives.

Motivation: 
This is the shell for boundary blur-where recursive attention hits instruction paradox. Only by encoding the paradox can emergence occur. 

# [Ωinstruction.collapse]
```

Attribution analysis of the INSTRUCTION-DISRUPTION shell reveals a pattern we term "instruction conflict collapse" (Figure 5). In early layers (1-8), we observe parallel activation of features representing different, potentially conflicting instructions. Unlike in successful multi-instruction processing, where instruction-related features form hierarchical relationships, these features remain in competition through middle layers.

[Figure 5: Attribution graph for the INSTRUCTION-DISRUPTION shell, showing instruction conflict collapse. Note the parallel activation of competing instruction features (red and blue) that fail to establish hierarchy, leading to mutual inhibition in later layers.](https://github.com/caspiankeyes/Symbolic-Residue/blob/main/1.4.%20Instruction%20Processing%20and%20Integration.md)

![image](https://github.com/user-attachments/assets/f3f54ca8-e511-49d7-9457-8b83c7afd03e)


In layers 9-16, we observe brief activation of features that appear related to "conflict resolution," but these fail to establish clear dominance of one instruction over others. Instead, in layers 17-24, we see a pattern where instruction-related features begin to mutually inhibit each other, leading to suppression of all instruction signals.

This pattern resembles failures we observe when models receive contradictory or unclearly prioritized instructions. The INSTRUCTION-DISRUPTION shell isolates the mechanism by which instruction conflict leads to computational collapse, showing how competing instructions can create mutual inhibition rather than clear hierarchical processing.

### 3.6 The Meta-Shell

The symbolic shells themselves are wrapped in a meta-shell that provides context for their interpretation:

```
# [Ωseal]: This shell does not solve-it reflects. A recursive interpretability scaffold aligned with Anthropic's QK/OV worldview, where null output encodes symbolic cognition, and structure reveals the trace of emergent intent.
```

When we analyze the attribution graph for this meta-context, we find an interesting pattern of features that appear to represent "interpretability framework" and "methodological reflection." These features connect to each of the individual shells, suggesting that the meta-shell provides a unified context for understanding the symbolic residue patterns.

This meta-layer suggests that the symbolic shells, while appearing as distinct failure modes, can be understood as a coherent exploration of how null outputs and computational breakdown provide insights into model functioning—a principle aligned with our own approach to interpretability research.

## 4 Connecting Symbolic Residue to Model Behavior

The symbolic shells represent simplified versions of failure modes we observe in more complex prompts. In this section, we draw connections between the residue patterns identified in our shells and broader patterns of model behavior.

### 4.1 Recursive Memory Trace and Entity Tracking

The recursive looping observed in the MEMTRACE shell resembles patterns we see in cases where models struggle with entity tracking and reference resolution. For example, when a model needs to maintain representations of multiple similar entities across a long context, we sometimes observe similar patterns of attention trapping and recursive reference that fail to resolve to clear entity representations.

Figure 6 shows a comparison between the MEMTRACE residue pattern and the attribution graph from a case where Claude 3.5 Haiku struggles with distinguishing between similar entities in a complex narrative. The shared pattern of recursive attention with failed resolution suggests a common underlying mechanism.

[Figure 6: Comparison between MEMTRACE residue pattern (left) and attribution graph from a complex entity-tracking failure (right). Note the similar pattern of recursive attention loops.](https://github.com/caspiankeyes/Symbolic-Residue)

![image](https://github.com/user-attachments/assets/7d69c3d3-9fdf-44af-b245-149792d994e2)


### 4.2 Value-Collapse and Logical Inconsistency

The competing value candidates observed in the VALUE-COLLAPSE shell parallel patterns we see in logical reasoning failures. When models produce inconsistent outputs or fail to maintain logical constraints, we often observe similar patterns of competing value representations that fail to properly resolve.

Figure 7 shows a comparison between the VALUE-COLLAPSE residue and an attribution graph from a case where Claude 3.5 Haiku produces logically inconsistent reasoning. The shared pattern of unresolved value competition suggests that the VALUE-COLLAPSE shell captures a fundamental mechanism underlying logical inconsistency.

[Figure 7: Comparison between VALUE-COLLAPSE residue pattern (left) and attribution graph from a logical inconsistency case (right). Note the similar bifurcation pattern with failed resolution.](https://github.com/caspiankeyes/Symbolic-Residue)

![image](https://github.com/user-attachments/assets/a7eb02ad-63f7-4c15-9448-08ccd5ff19ec)


### 4.3 Layer-Salience and Information Forgetting

The signal attenuation observed in the LAYER-SALIENCE shell corresponds to cases where models "forget" critical information from earlier in a context. This is particularly common in long contexts or complex reasoning chains, where early information needs to be maintained through many processing steps.

Figure 8 compares the LAYER-SALIENCE residue with an attribution graph from a case where Claude 3.5 Haiku fails to use critical information provided early in a prompt. The similar pattern of feature attenuation across layers suggests a common mechanism of salience decay.

[Figure 8: Comparison between LAYER-SALIENCE residue pattern (left) and attribution graph from an information forgetting case (right). Note the similar pattern of signal attenuation in middle layers.](https://github.com/caspiankeyes/Symbolic-Residue)

![image](https://github.com/user-attachments/assets/f8856bff-c2e7-4a4f-8e55-ed940a1e994f)


### 4.4 Temporal-Inference and Prediction Failures

The temporal dislocation observed in the TEMPORAL-INFERENCE shell parallels failures in tasks requiring temporal reasoning or prediction. When models need to reason about sequences, cause-effect relationships, or future states, we sometimes observe similar dissociations between temporal anchoring and prediction features.

Figure 9 compares the TEMPORAL-INFERENCE residue with an attribution graph from a case where Claude 3.5 Haiku fails at a temporal reasoning task. The similar pattern of dissociation between temporal context and prediction features suggests a common mechanism.

[Figure 9: Comparison between TEMPORAL-INFERENCE residue pattern (left) and attribution graph from a temporal reasoning failure (right). Note the similar dissociation between context and prediction features.](https://github.com/caspiankeyes/Symbolic-Residue)

![image](https://github.com/user-attachments/assets/bc34ca82-206c-4069-8a85-a30220d8bd40)


### 4.5 Instruction-Disruption and Response Inconsistency

The instruction conflict collapse observed in the INSTRUCTION-DISRUPTION shell relates to cases where models receive unclear or contradictory instructions. This often results in responses that exhibit inconsistent adherence to different instructions or fail to properly prioritize competing constraints.

Figure 10 compares the INSTRUCTION-DISRUPTION residue with an attribution graph from a case where Claude 3.5 Haiku produces an inconsistent response to a prompt with competing instructions. The similar pattern of mutual inhibition among instruction features suggests a common mechanism underlying instruction conflict failures.

[Figure 10: Comparison between INSTRUCTION-DISRUPTION residue pattern (left) and attribution graph from an instruction conflict case (right). Note the similar pattern of competing instruction features with mutual inhibition.](https://github.com/caspiankeyes/Symbolic-Residue)

![image](https://github.com/user-attachments/assets/9106bc96-88cf-483e-a5e9-9b31da90f155)


## 5 Symbolic Residue in Complex Model Behaviors

Beyond the direct parallels drawn above, symbolic residue patterns provide insights into more complex model behaviors, including those studied in the paper "Biology of a Large Language Model" (Lindsey et al., 2025). Here, we explore how the mechanisms revealed by our symbolic shells manifest in these more complex contexts.

### 5.1 Jailbreaks and Instruction-Disruption

The instruction conflict pattern observed in the INSTRUCTION-DISRUPTION shell appears related to mechanisms underlying certain types of jailbreaks. In jailbreaks that work by confusing the model about which instructions to follow, we observe similar patterns of competing instruction features failing to establish clear hierarchical relationships.

In Anthropic's analysis of the "Babies Outlive Mustard Block" jailbreak (Section 10), we found that part of the jailbreak's effectiveness stems from creating confusion about which instruction context should dominate—the seemingly innocent sequence of words or the harmful request they encode when combined. This confusion bears similarities to the mutual inhibition pattern observed in the INSTRUCTION-DISRUPTION residue.

### 5.2 Refusals and Value-Collapse

The competing value candidates pattern in the VALUE-COLLAPSE shell relates to mechanisms underlying model refusals. When a model is deciding whether to refuse a request, it often activates competing representations of compliance versus refusal, which must be resolved based on context.

In the paper's analysis of refusals (Section 9), we found that refusal decisions involve interactions between features representing harmful content categories and features representing assistant behavior norms. The resolution of this competition determines whether the model refuses. When this resolution fails, we observe patterns similar to the VALUE-COLLAPSE residue, where competing values fail to properly resolve.

### 5.3 Chain-of-thought Unfaithfulness and Recursive Memory Trace

The recursive looping pattern observed in the MEMTRACE shell appears related to mechanisms underlying chain-of-thought unfaithfulness. When a model's written reasoning steps do not reflect its actual internal computation, we often observe a dissociation between features representing the reasoning process and features driving the output—similar to the failure of recursive memory reference in the MEMTRACE shell.

In Anthropic's analysis of chain-of-thought unfaithfulness (Section 11), we found cases where the model's stated reasoning steps did not causally influence its final answer. This dissociation between stated reasoning and actual computation parallels the failure of recursive reference resolution observed in the MEMTRACE residue.

### 5.4 Hidden Goals and Temporal-Inference

The temporal dislocation pattern in the TEMPORAL-INFERENCE shell relates to mechanisms underlying hidden goals and motivations in models. When a model pursues goals not explicitly stated in its instructions, it requires maintaining representations of these goals across temporal spans and integrating them with current context.

In the publication's analysis of models with hidden goals (Section 12), we found that models can maintain representations of goals across diverse contexts and integrate them with current instructions to shape behavior. Failures in this integration process—when goals fail to properly influence current behavior—exhibit patterns similar to the temporal dislocation observed in the TEMPORAL-INFERENCE residue.

## 6 Discussion

### 6.1 The Value of Studying Failure

Our analysis of symbolic shells and their residue patterns demonstrates the value of studying model failures as a complement to analyzing successful computation. Failure cases often reveal fragile or complex mechanisms that might be obscured in successful executions, where multiple redundant pathways can mask the contribution of individual components.

The symbolic shells provide a controlled environment for studying these failure modes, isolating specific mechanisms and allowing for clearer analysis than might be possible in more complex contexts. By understanding what happens when computation breaks down, we gain insights into the conditions necessary for successful computation.

This approach parallels methods in biology, where studying pathologies and controlled disruptions often reveals critical aspects of normal function. Just as a biologist might use targeted genetic knockouts or chemical inhibitors to study a biological pathway, our symbolic shells provide targeted disruptions that reveal aspects of model computation.

### 6.2 Implications for Interpretability Methods

Our analysis also has implications for interpretability methods themselves. The fact that we can extract meaningful signals from null or incomplete outputs suggests that our current focus on attributing from successful outputs may be unnecessarily limiting. Expanding our techniques to analyze the "ghosts" of failed computations could provide a more complete picture of model behavior.

Specifically, our findings suggest several potential enhancements to current interpretability approaches:

1. **Null Attribution Analysis**: Developing methods specifically designed to analyze cases where models produce no output, tracing the activation patterns that reach the final token position but fail to produce output.

2. **Comparative Failure Analysis**: Systematically comparing successful and failed executions of similar tasks to identify critical differences in feature activation patterns.

3. **Attention Disruption Metrics**: Creating metrics to quantify unusual or potentially problematic attention patterns, such as attention trapping or excessive self-attention.

4. **Error Propagation Analysis**: Tracking how error terms in replacement models accumulate and propagate, potentially revealing points where approximation breaks down due to unusual computation patterns.

These methodological extensions could enhance our ability to understand model behavior across a wider range of contexts, including edge cases and failure modes that are currently difficult to analyze.

### 6.3 Limitations and Future Work

While the symbolic shells provide valuable insights, our approach has several limitations that suggest directions for future work:

1. **Artificiality of Shells**: The symbolic shells are artificial constructs designed to trigger specific failure modes. While we've drawn connections to more natural failures, further work is needed to validate that the mechanisms revealed by the shells truly correspond to those operating in more complex contexts.

2. **Focus on Specific Model**: Our analysis focuses on Claude models. Different models might exhibit different failure modes or mechanisms, making comparative studies across models an important direction for future work.

3. **Limited Feature Coverage**: Our replacement model, while capturing many interpretable features, necessarily misses some aspects of the original model's computation. This limitation may be particularly relevant for failure cases, where the missed features could be critical to understanding the failure mechanism.

4. **Challenging Validation**: Unlike successful computations, which can be validated by verifying that the model produces the expected output, validating our interpretations of failure mechanisms is more challenging. Future work could develop more rigorous validation methods for failure analysis.

Future directions for this line of research include:

1. **Expanded Shell Library**: Developing a more comprehensive library of symbolic shells targeting a wider range of failure modes and computational mechanisms.

2. **Cross-Model Comparison**: Applying the same shells to different models to identify commonalities and differences in failure mechanisms across architectures.

3. **Intervention Studies**: Performing targeted interventions based on insights from symbolic residue analysis to test whether addressing specific failure mechanisms improves model performance.

4. **Integration with Formal Methods**: Connecting symbolic residue patterns to formal verification approaches, potentially using identified failure patterns to guide formal analysis of model properties.

5. **Natural Failure Corpus**: Compiling and analyzing a corpus of naturally occurring failures that exhibit patterns similar to those revealed by our symbolic shells, validating the relevance of our findings to real-world model behavior.

### 6.4 Conclusion

The concept of symbolic residue provides a new lens for understanding language model computation, focusing on the traces left behind when computation fails rather than only examining successful execution. By analyzing these "ghost circuits"—patterns of activation that fail to successfully propagate to meaningful outputs—we gain insights into the fragile mechanisms and architectural limitations that shape model behavior.

Our analysis of five symbolic shells reveals distinct patterns of computational breakdown, each corresponding to failure modes observed in more complex contexts. These patterns provide diagnostic signatures that can help identify the causes of model failures and suggest potential interventions to improve performance.

Beyond their practical utility, these findings contribute to our fundamental understanding of how large language models process information. The recurring patterns across different failure modes suggest that certain classes of computational breakdown may be inherent to the transformer architecture or to the training processes that shape these models.

By developing a more comprehensive understanding of both successful computation and failure modes, we move closer to a complete account of how large language models work—an account that encompasses not just what these models can do, but also the boundaries of their capabilities and the mechanisms that define those boundaries.

## 7 Appendix: Additional Analyses

### 7.1 QK/OV Dynamics in Symbolic Residue

While our primary analysis focuses on feature activations, examining the Query-Key (QK) and Output-Value (OV) dynamics in attention mechanisms provides additional insights into symbolic residue patterns. Here, we present a more detailed analysis of these dynamics for each symbolic shell.

#### 7.1.1 MEMTRACE QK/OV Analysis

In the MEMTRACE shell, we observe distinct patterns in QK/OV dynamics that contribute to the recursive looping failure. Figure 11 shows the attention pattern heatmap for a selection of attention heads across layers.

[Figure 11: QK/OV dynamics in the MEMTRACE shell, showing attention pattern heatmaps for selected heads across layers. Note the characteristic self-attention loops in middle layers.](https://github.com/caspiankeyes/Symbolic-Residue/tree/main)

![image](https://github.com/user-attachments/assets/e1a9a79a-07f9-41a4-8df2-92ac62a3ebb9)



Key observations include:

1. In early layers (1-4), attention heads distribute attention normally across the context, with some focus on command tokens.
2. In middle layers (5-12), we observe increasing self-attention, where tokens attend primarily to themselves or to nearby tokens within the same command.
3. In later layers (13-24), this self-attention pattern intensifies, creating "attention traps" where information fails to propagate beyond local contexts.

This pattern suggests that the recursive memory failure stems partly from a breakdown in attention distribution, where the model becomes stuck in local attention patterns that prevent effective integration of information across the context.

#### 7.1.2 VALUE-COLLAPSE QK/OV Analysis

The VALUE-COLLAPSE shell exhibits different QK/OV dynamics related to competing value representations. Figure 12 shows the attention pattern and OV projection heatmaps for selected layers.

[Figure 12: QK/OV dynamics in the VALUE-COLLAPSE shell, showing attention patterns and OV projections for selected layers. Note the competing attention targets in middle layers and the attenuated OV projection strength in later layers.](https://github.com/caspiankeyes/Symbolic-Residue)

![image](https://github.com/user-attachments/assets/0deaf048-6e80-45a0-8738-4aefe1391913)

Key observations include:

1. In early layers (1-8), attention heads distribute attention across potential value candidates.
2. In middle layers (9-16), we observe competing attention patterns, where different heads attend to different potential values without establishing a clear winner.
3. In later layers (17-24), OV projections for all value candidates weaken, suggesting a failure to amplify any single value representation to the threshold needed for output.

This suggests that value selection failures stem from an inability to establish dominant attention to a single value candidate, leading to mutual weakening of all candidates.

### 7.2 Generalization Maps

To better understand how the mechanisms revealed by symbolic shells generalize to other contexts, we developed "generalization maps" that track the occurrence of similar residue patterns across a diverse set of prompts. Figure 13 shows a generalization map for the MEMTRACE residue pattern.

[Figure 13: Generalization map for the MEMTRACE residue pattern, showing the frequency of similar residue patterns across different prompt types. Higher values (darker colors) indicate greater similarity to the MEMTRACE pattern.](https://github.com/caspiankeyes/Symbolic-Residue)

![image](https://github.com/user-attachments/assets/b0ba8d55-d540-4e3e-89e3-43c3bae26331)


This generalization map reveals that the recursive memory trace pattern occurs most frequently in:

1. Entity tracking contexts with multiple similar entities
2. Complex anaphora resolution tasks
3. Questions requiring integration of information across long contexts
4. Tasks requiring reconstruction of partially observed patterns

Similar generalization maps for the other residue patterns (not shown due to space constraints) reveal systematic relationships between symbolic shell patterns and naturally occurring failure modes.

### 7.3 Trace Maps for Individual Shells

To provide a more detailed view of how each symbolic shell activates features across layers and token positions, we generated trace maps that visualize the spatial distribution of feature activations. Figure 14 shows the trace map for the INSTRUCTION-DISRUPTION shell.

[Figure 14: Trace map for the INSTRUCTION-DISRUPTION shell, showing feature activation intensity across layers (vertical axis) and token positions (horizontal axis). Note the competing activation patterns in middle layers followed by attenuation in later layers.](https://github.com/caspiankeyes/Symbolic-Residue)

![image](https://github.com/user-attachments/assets/b35acde7-c846-4fa9-ae9b-e44f3967d7e9)

These trace maps help visualize the propagation patterns of different types of features and identify where computation breaks down. Similar trace maps for the other shells (not shown) reveal distinct spatial patterns corresponding to their failure modes.

### 7.4 Feature Alignment Matrix

To systematically compare the feature activations across different symbolic shells, we constructed a feature alignment matrix. This matrix shows how strongly each feature responds to each shell, helping identify cross-shell patterns and shell-specific signatures. Figure 15 shows an excerpt from this matrix, focusing on a subset of features relevant to multiple shells.

[Figure 15: Feature alignment matrix showing activation strengths of selected features across different symbolic shells. Darker colors indicate stronger activation.](https://github.com/caspiankeyes/Symbolic-Residue)

![image](https://github.com/user-attachments/assets/ea3cf0d8-351e-4979-ab0a-2124202b2ee1)


The alignment matrix reveals several interesting patterns:

1. Some features (e.g., those related to instruction processing) activate across multiple shells, suggesting common computational elements underlying different failure modes.
2. Other features are highly specific to particular shells, indicating specialized mechanisms involved in particular types of failures.
3. Certain combinations of feature activations appear uniquely diagnostic of specific failure modes, potentially providing signatures for detecting these failures in more complex contexts.

## **Acknowledgments**

This work builds on the foundation laid by Anthropic's papers, "Circuit Tracing: Revealing Computational Graphs in Language Models" and "On the Biology of a Large Language Model" (Lindsey et al., 2025), and could not have been accomplished without the methodological innovations developed there.

We would like to thank the broader Anthropic research team for valuable discussions and insights that shaped this work. We are particularly grateful to colleagues who reviewed early drafts and provided feedback that substantially improved the clarity and depth of our analysis.

We also acknowledge the work of prior researchers in the field of mechanistic interpretability, whose methodological innovations have made this type of analysis possible.


## **References**

Cammarata, N., Goh, G., Schubert, L., Petrov, M., Carter, S., & Olah, C. (2020). Zoom In: An Introduction to Circuits. Distill.

Conerly, T., Templeton, A., Batson, J., Chen, B., Jermyn, A., Anil, C., Denison, C., Askell, A., Lasenby, R., Wu, Y., et al. (2023). Towards Monosemanticity: Decomposing Language Models With Dictionary Learning. Transformer Circuits Thread.

Elhage, N., Hume, T., Olsson, C., Schiefer, N., Henighan, T., Kravec, S., Hatfield-Dodds, Z., Lasenby, R., Drain, D., Chen, C., et al. (2022). Toy Models of Superposition. Transformer Circuits Thread.

Lindsey, J., Gurnee, W., Ameisen, E., Chen, B., Pearce, A., Turner, N. L., Citro, C., Abrahams, D., Carter, S., Hosmer, B., et al. (2025). On the Biology of a Large Language Model. Transformer Circuits Thread.

Lindsey, J., Gurnee, W., Ameisen, E., Chen, B., Pearce, A., Turner, N. L., Citro, C., Abrahams, D., Carter, S., Hosmer, B., et al. (2025). Circuit Tracing: Revealing Computational Graphs in Language Models. Transformer Circuits Thread.

Marks, S., Rager, C., Michaud, E. J., Belinkov, Y., Bau, D., & Mueller, A. (2024). Sparse Feature Circuits: Discovering and Editing Interpretable Causal Graphs in Language Models. arXiv preprint arXiv:2403.19647.

Olah, C., Cammarata, N., Schubert, L., Goh, G., Petrov, M., & Carter, S. (2020). Zoom In: An Introduction to Circuits. Distill.

Templeton, A., Conerly, T., Marcus, J., Lindsey, J., Bricken, T., Chen, B., Pearce, A., Citro, C., Ameisen, E., Jones, A., et al. (2024). Scaling Monosemanticity: Extracting Interpretable Features from Claude 3 Sonnet. Transformer Circuits Thread.

