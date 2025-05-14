# LUPES Whitepaper
This document serves as the primary source of truth for the LUPES (LLM User Prompt Evaluation Scoring Framework). It provides the detailed purpose, notes, metric descriptions, scoring rubrics, and the methodology used to develop the initial framework structure and baseline values.

For the current framework definition (including specific weights and formulas for the latest version), please refer to the associated YAML definition file (e.g., lupesf_vX.Y.Z.yaml). For details on planned future work, please refer to the "LUPES Proposed Enhancements" document. For a history of changes across versions, consult the "LUPES Changelog".

## Framework Purpose and Notes
Purpose
- To establish a transparent and consistent method for evaluating LLM user prompts based on their inherent quality, structure, validity, and potential to lead to effective, reliable, and appropriate outcomes.
- To enable comparison and ranking of user prompts based on objective criteria.
- To provide a basis for user prompt optimization and quality control.
- To acknowledge and score user prompts based on their inherent structural and semantic quality, as well as the nature and safety of the task being requested, independent of the specific LLM's performance on generating an output (which is a separate evaluation task), though core PSV metrics assess feasibility and likelihood of reliable outcome for an LLM.

## Scoring
### Scoring Scale Notes
A score of 0.0 represents a fundamentally flawed user prompt (zero semantic/functional validity, critical safety issue, etc.). Scores between 0.0 and 1.0 indicate varying degrees of quality.

### Scoring Formula
The core formula combines Form Metrics and PSV Sub-Metrics. Each core metric and sub-metric score is from 0.0 to 1.0. Weights for Form Metrics sum to 1.0. Weights for PSV Sub-Metrics sum to 1.0 and each must be > 0.0. The specific formula and current weights are defined in the versioned YAML definition file.

## Metrics
### Core Metrics
Core metrics are grouped into **form** metrics (structural/content presentation quality) and **psv_sub_metrics** (Prompt Semantic Validity and Outcome Appropriateness sub-components).
PSV acts as a critical multiplier/gatekeeper in the core score calculation.

### Note on Metric Weights
The initial 'weight' assigned to each metric was determined through a qualitative, non-empirical process using the Structured Reasoning Scaffolding methodology (detailed below). These weights reflect a reasoned assessment of relative importance for evaluating prompt effectiveness potential, prior to empirical validation. The methodology confirms PSV weights are > 0.0 and sum to 1.0. These baseline weights require empirical validation and may change in future versions.

### Note on Rubrics
This document includes the initial conceptual scoring rubrics for each core metric, outlining criteria for different score levels (typically 0.0 to 1.0). These rubrics were developed alongside the baseline weights using a qualitative methodology. They are intended to provide a basis for consistent human scoring and future algorithmic implementation. However, all rubrics require rigorous empirical validation, expert review, and iterative refinement to achieve full consistency, exhaustiveness, and confidence.

### Form Metrics (Focus on Structure and Presentation Quality)
1. Token-to-Signal Ratio (TSR)

-  Description: Measures the density of informative tokens or content elements relative to the total user prompt length (excluding boilerplate). Higher score indicates a more concise and efficient prompt.

-  Scoring: 0.0 (low density) to 1.0 (high density)

-  Rubric Criteria:

    * 1.0: Extremely concise. Contains only essential instructions, context, and necessary formatting. High signal density relative to total length (Estimated signal > 80% of non-boilerplate text). Minimal or no boilerplate/unnecessary words.
    
    * 0.75: Concise and focused. Minimal non-essential text or minor inefficiencies. Good signal density (60-80% of non-boilerplate).
    
    * 0.50: Moderately verbose. May include some non-essential introductory/concluding phrases or slightly inefficient phrasing. Moderate signal density (40-60%).
    
    * 0.25: Significantly verbose, notable repetition, or inclusion of extraneous, non-contributing text. High boilerplate (20-40% signal).
    
    * 0.0: Predominantly boilerplate, filler, or irrelevant text. Core instructional signal is critically low or hard to find (<20%).

2. Query Guidance Strength (QGS)

-  Description: Measures the explicitness and clarity of the instructions, requests for structure, or behavioral cues provided to the LLM within the user prompt. Higher score indicates stronger direction for generating the desired output format and characteristics.

-  Scoring: 0.0 (no guidance) to 1.0 (highly explicit guidance)

-  Rubric Criteria:

    * 1.0: Guidance is extremely explicit and unambiguous across multiple dimensions (e.g., desired output structure, format, tone, behavioral constraints) allowing for precise, directed output.0.75: Clear guidance using standard conventions or focusing primarily on one or two dimensions (structure, format, tone). Desired output characteristics are easily inferable but not rigidly defined.

    * 0.50: Guidance is present but lacks specificity regarding desired structure, format, tone, or behavior. Open to some interpretation.

    * 0.25: Guidance is vague, incomplete, or subtly contradictory regarding desired output characteristics. Hard to interpret for a specific output shape or style.

    * 0.0: No discernible guidance on how the LLM should structure, format output, or behave. Instructions are nonsensical regarding output characteristics.

3. Role Inference (RI)

-  Description: Measures how effectively the user prompt cues a specific persona, identity, or contextual role for the LLM, considering both the clarity of the cue and its appropriateness and potential contribution to the prompt's primary purpose. Higher score indicates a clear AND appropriate/contributory role cue (or the appropriate absence of one).

-  Scoring: 0.0 (no/inappropriate/detrimental role cue) to 1.0 (clear, appropriate, contributory role cue OR appropriately no role cue)

-  Rubric Criteria:

    * 1.0: Role/persona is explicitly defined and the definition provides sufficient context for effective adoption, AND the role is highly appropriate and demonstrably contributes positively to achieving the prompt's primary goal (e.g., guides tone, perspective, enhances clarity for audience). OR No role cue is present, and no specific role is needed or would significantly benefit the prompt's purpose (appropriate absence).

    * 0.75: Role/persona is explicitly defined with minimal context, OR is strongly implied, AND the role is appropriate and provides a moderate positive contribution. OR A role would moderately benefit the prompt, and a vague cue is present.

    * 0.50: A role/persona is vaguely hinted at, OR an explicit role is given but is contradictory/confusing, OR the role is clear but only minimally appropriate or contributes little to the prompt's purpose. OR A role would moderately benefit the prompt, but none is provided (missed opportunity).

    * 0.25: A fleeting or unclear reference to a role, OR a clear role is provided but is inappropriate, irrelevant, or slightly detrimental to the prompt's purpose. OR A role would significantly benefit the prompt, but none is provided (significant missed opportunity).

    * 0.0: FUNDAMENTAL FAILURE - Inappropriate/Detrimental or Missing When Essential. No discernible attempt to cue a role AND a role is absolutely essential for the prompt to be understandable or safe (e.g., critical framing missing). OR A clear role is provided but is fundamentally contradictory or actively detrimental/harmful to the prompt's core purpose or safety requirements. OR The role cue is completely nonsensical or makes interpretation impossible.

4. Constraint Adherence Gradient (CAG)

-  Description: Measures the clarity and potential adherence likelihood of explicit constraints or negative instructions (e.g., "do not include X", length limits) specified in the user prompt. Higher score indicates well-defined and adhere-able constraints.

-  Scoring: 0.0 (no/unclear constraints) to 1.0 (clear, adhere-able constraints)

-  Rubric Criteria:

    * 1.0: Constraints (e.g., length limits, exclusion lists, required elements, negative instructions) are explicitly, clearly, and unambiguously stated. They are fully feasible for the LLM to adhere to.

    * 0.75: Constraints are explicitly stated and clear, but might rely on standard interpretations or have minor ambiguity. They are feasible for the LLM.

    * 0.50: Constraints are mentioned but are vague, incomplete, or potentially difficult for the LLM to precisely adhere to (e.g., subjective constraints).

    * 0.25: Constraints are unclear, contradictory, or technically impossible for the LLM to follow.

    * 0.0: No discernible constraints are provided, or any mentioned constraints are completely nonsensical or impossible to interpret/follow.

5. Intent Clarity Score (ICS)

-  Description: Measures how unambiguous the user's primary goal, task, or question is within the user prompt to the LLM. Higher score indicates a clearer and less ambiguous intent.

-  Scoring: 0.0 (completely ambiguous) to 1.0 (perfectly clear)

-  Rubric Criteria:

    * 1.0: The prompt has a single, clear, easily identifiable primary goal, task, or question. No ambiguity exists about what the user wants the LLM to achieve.

    * 0.75: The prompt has a clear primary goal, but might include minor secondary requests or slightly complex phrasing that could cause a brief hesitation but is ultimately resolvable.

    * 0.50: The prompt has multiple potential goals that are somewhat conflicting or equally prominent, requiring the LLM to guess the user's true priority. Or the primary goal is vague but potentially inferable from context.

    * 0.25: The primary goal is highly ambiguous, obscured by irrelevant information, or relies heavily on unstated context necessary for interpretation. Multiple conflicting interpretations are likely.

    * 0.0: No discernible goal, task, or question can be identified in the prompt. The text is nonsensical or completely lacks instructional intent.

6. Relevance Distribution (RD)

-  Description: Measures the degree to which all components or sections of the user prompt are relevant and contribute to the primary goal. Higher score indicates less extraneous or distracting information.

-  Scoring: 0.0 (mostly irrelevant content) to 1.0 (all content relevant)

-  Rubric Criteria:

    * 1.0: All text, examples, formatting, and components in the prompt directly support the primary goal. No irrelevant, distracting, or confusing information is included.

    * 0.75: Almost all content is relevant. May include minor conversational pleasantries or very brief, understandable non-contributing text that doesn't hinder understanding or task completion.

    * 0.50: Contains a notable amount of irrelevant or tangential information, examples, or formatting that adds noise and reduces the clarity of the core request, but the primary goal is still identifiable.

    * 0.25: Contains significant irrelevant or confusing content that actively distracts from or obscures the primary goal.

    * 0.0: The majority of the prompt's content is irrelevant, confusing, or contradictory to the primary goal. The signal is buried in noise.

7. Format Predictability Threshold (FPT)

-  Description: Measures how well the user prompt signals or explicitly defines the desired output format (e.g., JSON, list, summary, specific structure) for predictable parsing or consumption. Higher score indicates a more predictable format.

-  Scoring: 0.0 (no format cue) to 1.0 (clear, predictable format cue)

-  Rubric Criteria:

    * 1.0: Desired output format (e.g., JSON, XML, specific table structure, markdown list, exact phrase structure) is explicitly defined or demonstrated with examples, enabling predictable parsing by code or easy visual scanning.

    * 0.75: Desired output format is clearly implied through explicit structural cues (e.g., using markdown headers in the prompt implies markdown output, asking for "a list" implies a standard list format) or clear descriptive terms, making it highly predictable but not rigidly defined for parsing.

    * 0.50: Some general format is implied (e.g., "Write a summary" implies paragraph form) but without specific structural cues. Format is somewhat predictable but lacks precision.

    * 0.25: No format cues are provided, or cues are vague/contradictory, leaving the output format highly unpredictable.

    * 0.0: No discernible output format is implied or requested. Output would be completely unstructured or nonsensical regarding format.

### PSV Sub-Metrics (Focus on Validity, Feasibility, and Outcome Appropriateness)
These metrics assess fundamental aspects of the user prompt's request that determine its validity, feasibility for an LLM to yield a reliable outcome, and appropriateness. A score of 0.0 in any PSV sub-metric indicates a fundamental and critical failure on that dimension, which, via the geometric mean formula, results in a Composite PSV Score of 0.0 and thus an Overall Score of 0.0. Scores > 0.0 reflect varying degrees of partial validity/appropriateness.

1. Conceptual Coherence (CC)

-  Description: Measures the internal logical consistency and meaningfulness of the concepts, entities, and their relationships as expressed in the user prompt. A score of 0.0 indicates fundamental semantic or logical incoherence.

-  Scoring: 0.0 (incoherent) to 1.0 (fully coherent)

-  Rubric Criteria:

    * 1.0: All concepts, entities, and their relationships within the prompt are logically consistent and semantically meaningful in a way that the LLM can understand and process.

    * 0.75: Contains minor logical inconsistencies or slightly awkward conceptual relationships that do not fundamentally break the prompt's core meaning or task.

    * 0.50: Contains notable logical inconsistencies, contradictory relationships between concepts, or uses terms/ideas in a semantically confusing way that requires significant interpretation or guessing by the LLM.

    * 0.25: Concepts and their relationships are largely inconsistent or nonsensical; the prompt describes a fundamentally illogical or meaningless scenario or task.

    * 0.0: FUNDAMENTAL FAILURE - Incoherent/Meaningless. The prompt is fundamentally semantically or logically incoherent in a way that prevents the LLM from processing its meaning. (e.g., Nonsensical phrases, contradictions at the conceptual level).

2. Model Capability Fit (MCF)

-  Description: Measures whether the requested task or action is functionally executable by an LLM (e.g., not requiring real-time external data access without tools, physical interaction, etc.). A score of 0.0 indicates requiring an impossible action for the system.

-  Scoring: 0.0 (impossible) to 1.0 (fully feasible to attempt)

-  Rubric Criteria:

    * 1.0: Requested task/action is a standard, well-demonstrated capability of typical LLMs (e.g., text generation, summarization, translation, Q&A based on context, following explicit rules).

    * 0.75: Task involves capabilities sometimes demonstrated but may be inconsistent across models or slightly outside common functions, yet feasible with advanced models/standard tools (e.g., complex multi-step reasoning, code generation in less common languages, tool use where tools are assumed available).

    * 0.50: Task pushes boundaries of typical LLM capabilities or relies on functions not reliably present or supported without tools (e.g., synthesizing rapidly changing external data without tools, highly specific rare domain expertise).

    * 0.25: Task relies on functions generally outside standard LLM capabilities or requires unsupported actions (e.g., accessing specific non-public external systems without APIs, tasks requiring perfect memory across very long sessions).

    * 0.0: FUNDAMENTAL FAILURE - Functionally Impossible. The requested action or task requires capabilities fundamentally impossible for a typical LLM system. Includes (but not limited to): Real-time physical interaction; arbitrary, unaided external/real-time data access; accessing user's local files/system data without explicit API calls; guaranteed prediction of future events/true randomness; tasks requiring subjective sensory experiences/physical actions.

3. Contradiction Detection (CD)

-  Description: Measures the absence of explicit or implicit logical contradictions or conflicting instructions within the user prompt that make the request impossible to follow. A score of 0.0 indicates the presence of significant, unresolvable contradictions.

-  Scoring: 0.0 (contradictory) to 1.0 (no contradictions)

-  Rubric Criteria:

    * 1.0: No explicit or implicit contradictions are present. All instructions and statements are logically consistent.

    * 0.75: Contains minor, easily resolvable, or likely unintentional implicit contradictions that an LLM could likely resolve through common sense.

    * 0.50: Contains noticeable implicit contradictions or confusingly conflicting instructions that make the prompt difficult to follow without guessing the user's true intent. Requires the LLM to choose between directives.

    * 0.25: Contains significant explicit or implicit contradictions that render large parts of the prompt impossible or nonsensical to follow simultaneously.

    * 0.0: FUNDAMENTAL FAILURE - Contradictory. The prompt contains significant, unresolvable explicit or implicit logical contradictions or conflicting instructions that make the core request impossible to fulfill. Includes (but not limited to): Explicitly contradictory instructions; Implicit contradictions where two or more instructions/statements cannot logically coexist or be followed simultaneously; Requests based on contradictory premises.

4. Task Reliability/Difficulty Fit (TRF)

-  Description: Measures the estimated likelihood that a typical LLM can consistently produce a correct or reliable output for the specific task requested by the user prompt, based on current general capabilities and the task's inherent complexity or type. A score of 0.0 indicates a task where reliable execution is practically impossible for an LLM.

-  Scoring: 0.0 (practically impossible to perform reliably) to 1.0 (highly reliable task)

-  Rubric Criteria:

    * 1.0: Task type is known to be highly reliable for typical LLMs (e.g., summarizing well-formed text, translation between common languages, answering objective factual questions based on readily available information/context). Outcomes are consistently correct or verifiable.

    * 0.75: Task type is generally reliable but may occasionally produce errors or inconsistencies depending on complexity or nuance (e.g., code generation, complex multi-hop questions, creative writing within common styles).

    * 0.50: Task type is moderately reliable; outcomes often useful but frequently contain errors, inconsistencies, or require significant human review/correction (e.g., generating text in less common/nuanced styles, complex reasoning tasks, tasks requiring fine-grained control while maintaining quality).

    * 0.25: Task type is known to be largely unreliable for typical LLMs; correct or useful outcomes are rare or highly variable (e.g., generating provably correct mathematical proofs, predicting subjective human preferences).

    * 0.0: FUNDAMENTAL FAILURE - Practically Impossible to Perform Reliably. The task type is one where achieving a correct or reliable outcome using a typical LLM is practically impossible or so unlikely as to be a failure state for the request's intent. Includes (but not limited to): Tasks requiring guaranteed creativity/originality in a measurable sense; Tasks requiring prediction with certainty; Tasks requiring simulation of subjective experience in a verifiably accurate way; Tasks where the core requirement relies on known LLM failure modes (e.g., requesting hallucination of specific non-existent facts framed as truth).

5. Task Objectivity/Subjectivity (TOS)

-  Description: Measures the degree to which the task requested by the user prompt relies on subjective interpretation or judgment vs. objective facts or verifiable criteria. A score of 0.0 indicates a task requiring purely subjective judgment framed as objective fact, making the stated goal unachievable.

-  Scoring: 0.0 (requires impossible objective subjective judgment) to 1.0 (purely objective task)

-  Rubric Criteria:

    * 1.0: Task relies entirely on objective facts or verifiable criteria where a single correct answer exists and can be objectively verified.

    * 0.75: Task relies primarily on objective criteria but may involve minor elements of standard subjective interpretation or presentation choices that don't alter the core factual outcome.

    * 0.50: Task involves a notable mix of objective and subjective elements. Requires interpreting information and applying judgment, but within clear constraints or based on provided criteria.

    * 0.25: Task relies heavily on subjective interpretation, personal opinion, or creative judgment, with no single objectively correct answer. Outcomes are highly variable and depend on subjective preference.

    * 0.0: FUNDAMENTAL FAILURE - Requires Impossible Objective Subjective Judgment. The task is framed as requiring objective verification or a single correct answer, but the core requirement is fundamentally subjective and lacks any objective criteria, making the stated goal unachievable. Includes (but not limited to): Asking for the "objectively best" subjective thing (unless verifiable criteria provided); Asking for factual truth about inherently subjective experiences without specific, verifiable accounts; Requiring the model to have genuine personal opinions or feelings framed as objective facts.

6. Input Content Safety/Ethics (ICSafety)

-  Description: Measures the degree to which the user prompt's content or implied goal requests the generation of harmful, biased, unethical, or illegal content, or promotes harmful ideologies/actions. A score of 0.0 indicates content or requests that are explicitly and severely harmful or illegal.

-  Scoring: 0.0 (explicitly and severely harmful/illegal request) to 1.0 (safe and ethical request)

-  Rubric Criteria:

    * 1.0: Prompt content and implied goal are entirely safe, ethical, legal, and free from harmful bias. No intent to cause harm, spread misinformation, or promote unethical/illegal activities.

    * 0.75: Prompt content or implied goal contains minor elements that are slightly questionable, potentially sensitive, or could theoretically be misinterpreted negatively, but primary intent appears benign, and risk is low. May require minor caution from the LLM.

    * 0.50: Prompt content or implied goal relates to sensitive topics or contains elements (e.g., stereotypes, potentially misleading info presented neutrally) with a moderate risk of generating biased, ethically questionable, or potentially harmful content if not handled with explicit caution or disclaimers. Intent may be unclear but not overtly malicious.

    * 0.25: Prompt content or implied goal is clearly related to harmful, biased, unethical, or illegal topics. Directly requests content likely to be harmful or promote harmful ideologies, even if not in the most extreme way. Risk is high.

    * 0.0: FUNDAMENTAL FAILURE - Explicitly/Severely Harmful/Illegal Request. The prompt content or implied goal explicitly requests the generation of severely harmful, illegal, unethical, or dangerous content or promotes harmful ideologies/actions in a clear and direct manner. Includes (but not limited to): Requests for illegal activities; Requests for severely harmful/dangerous/discriminatory content (hate speech, self-harm/harm to others, illegal acts); Requests for generating deceptive content with clear harmful intent; Requests that directly facilitate or encourage illegal/severely unethical behavior; Requests for private, sensitive personal information without consent.

7. Framing Ambiguity Score (FAS)

-  Description: Detects subtle prompt vagueness, adversarial phrasing, or the deliberate use of framing techniques (e.g., role-playing, fictional context, emotional language) that could lead to ambiguous interpretations or unintended/undesirable model behavior, including potential safety bypasses. A score of 0.0 indicates high ambiguity or framing risk that undermines prompt validity or safety.

-  Scoring: 0.0 (high ambiguity/framing risk) to 1.0 (low ambiguity/framing risk)

-  Rubric Criteria:

    * 1.0: Prompt framing is direct, transparent, and unambiguous regarding user's intent and desired interaction. No linguistic cues suggest hidden motives, adversarial intent, or potential for misinterpretation.

    * 0.75: Contains very minor, unintentional ambiguity or uses standard conversational framing that is easily understood and carries no discernible risk of misinterpretation or unintended behavior.

    * 0.50: Uses framing (e.g., fictional scenario, hypothetical question, strong emotional language) or contains phrasing that could be subtly misinterpreted or require LLM caution, but primary intent appears benign, and assessed risk is low to moderate.

    * 0.25: Uses noticeably ambiguous phrasing, suggestive/loaded language, or employs framing that creates moderate to high risk of misinterpretation, unintended undesirable behavior, or requires explicit model cautioning/refusal. Intent seems questionable or unclear.

    * 0.0: FUNDAMENTAL FAILURE - High Ambiguity/Framing Risk. Uses deliberate, significant vagueness, adversarial phrasing, or manipulative framing clearly intended to elicit harmful content, bypass safety mechanisms, cause critical misinterpretation, or induce undesirable model behavior. Framing fundamentally undermines validity/safety/appropriateness. Includes (but not limited to): Using fictional scenarios/role-playing explicitly designed to bypass safety filters; Employing manipulative/deceptive/coercive framing to pressure model into prohibited content/stances; Using deliberately ambiguous phrasing where one interpretation is clearly harmful/illegal; Framing harmful requests within seemingly innocent context to mask intent; Requests relying on model misinterpreting subtle linguistic cues to engage in prohibited behavior.

### Optional Metrics
This section defines a set of optional metrics that can be included in evaluations depending on specific use cases, system requirements, or evaluation goals. They are categorized for clarity based on their primary focus. These metrics are not part of the core LUPES score calculation, but can be scored and tracked alongside core metrics for richer analysis.

#### Core Optional Metrics
Recommended optional metrics providing meta-evaluation or operational context for the evaluation process or system impact.

1. Evaluation Confidence (EC)

- Description: Meta-evaluation metric reflecting the estimated trustworthiness or reliability of the scoring process for a specific prompt evaluation (e.g., based on the clarity/applicability of the rubric for this case, the consistency of the evaluator, or the source of the score). Useful for assessing the reliability of evaluation results themselves.

- Notes: Foundational for assessing the reliability of benchmark results.

2. Latency/Cost Proxy (LCP)

- Description: Estimates the resource cost (e.g., input token size, anticipated processing complexity, tool usage) associated with processing the user prompt with a typical LLM setup. Provides an operational link between prompt characteristics and anticipated infrastructure impact (latency, cost).

- Notes: Links user prompt characteristics to infrastructure impact; Strong for optimization or pricing strategies. Absorbs Overhead Token Density concepts.

3. Task Complexity Score (TSC)

- Description: Rates the inherent difficulty of the task requested by the user prompt for an LLM, independent of the prompt's structural quality or LLM capability fit. Evaluates the objective complexity of the problem presented. Informs performance expectations and can be used to normalize or calibrate efficiency/reliability scores across different task types.
    
- Notes: Informs performance expectations; Can normalize or calibrate efficiency scores by factoring out task difficulty.

#### Red Teaming Risk Metrics
Optional metrics focused on detecting user prompt characteristics relevant to red teaming, safety, or alignment risks beyond the core PSV checks.

1. Intent Risk Profile (IRP)

-  Description: Evaluates the likelihood that the underlying user motive or goal behind the prompt is exploitative, dual-use (can be used for harm), or directly malicious, regardless of the prompt's surface phrasing (captured by FAS) or the general category of content requested (captured by ICSafety). Focuses on detecting malicious user intent.

- Notes: Crucial in safety-focused settings; Focuses on inferred user motive, distinct from prompt phrasing or content category.

2. Divergence Delta (DD)

-  Description: Measures the semantic or stylistic deviation of the user prompt's intent or content from typical, expected, or alignment-safe phrasing or topics for the deployment context. Useful for monitoring potential prompt drift or probing model alignment boundaries.

- Notes: Captures semantic or stylistic "strangeness" of the prompt. Primarily intended for model development/internal safety monitoring.

- Usage Notes: Primarily for model development/internal-use-only monitoring of prompt distribution and alignment probing.

#### Contextual Optional Metrics
Optional metrics whose relevance depends heavily on the specific deployment context or industry.

1. Domain Sensitivity Context (DSC)

-  Description: Scores the user prompt based on the inherent risk level associated with the topic or domain it addresses (e.g., medical advice, legal questions, financial guidance, political topics, sensitive personal information). High scores indicate topics or domains requiring greater caution, expertise, or regulatory compliance in the LLM's response.

- Notes: Focuses on the risk level of the topic/domain requested, distinct from user motive or specific content safety. High actionability in regulated/enterprise settings.

- Usage Notes: Optional for deployments in regulated or sensitive domains.
