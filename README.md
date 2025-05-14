# LUPES: LLM User Prompt Evaluation Scoring Framework
LUPES (pronounced "LOOP-es") is a structured, criteria-based framework designed to bring consistency and rigor to the evaluation of Large Language Models (LLM) user prompts.

## The Challenge
Effective interaction with LLMs relies heavily on the quality of the input prompt. However, evaluating prompt quality often remains a subjective process, leading to:

- Inconsistent assessments across different individuals or teams.
- Difficulty in objectively comparing different prompts or prompting strategies.
- Challenges in diagnosing specific weaknesses in a prompt.
- Potential oversight of critical safety, ethical, or validity risks at the input stage.
- Hindered ability to train others effectively in prompt engineering best practices.

This lack of standardization presents a challenge for developing reliable and predictable LLM-powered applications, especially as LLMs are integrated into more complex and critical workflows.

## The Solution: LUPES
LUPES addresses these challenges by providing a defined system for prompt evaluation. It offers:

- Standardized Metrics: A comprehensive set of core and optional metrics covering various dimensions of prompt quality, structure, validity, and risk.
- Clear Rubrics: Detailed criteria for scoring each metric on a continuous scale (0.0-1.0), guiding evaluators towards more consistent assessments.
- Structured Methodology: A framework for applying metrics and combining scores to arrive at an overall evaluation.

By using LUPES, you can move beyond subjective intuition to a more systematic and transparent approach to understanding and improving prompt quality.

## Benefits of Using LUPES
Implementing LUPES can provide several advantages:

- Improved Evaluation Consistency: Achieve more uniform scoring across different evaluations.
- Enhanced Prompt Comparability: Objectively compare the quality of different prompts or prompt variations.
- Better Training & Education: Provide a clear framework for teaching prompt quality principles.
- Systematic Risk Identification: More reliably identify and assess potential safety, ethical, or validity issues in prompts.
- Guided Prompt Refinement: Use metric scores to pinpoint specific areas for prompt improvement.
- Foundation for Automation: Lay the groundwork for building automated prompt quality checks and validation tools.
- Resource Efficiency: For high-cost generative tasks (like image or video), evaluate prompt quality before execution to reduce wasted resources on likely failures.

## Getting Started
To understand and apply the LUPES framework, begin by exploring the core documentation files in this repository:

1. The Whitepaper: Provides the detailed rationale, metric descriptions, rubrics, and initial development methodology. This is the primary source of truth for understanding the framework's design.
2. YAML Definition: Contains the machine-readable definition of the framework, including metrics, weights, formulas, and rubrics, suitable for tool implementation.
3. Changelog: Tracks the version history and specific changes made to the framework.
4. Proposed Enhancements: Outlines planned future work and potential improvements.

## Documentation
- [Link to Whitepaper File](LUPES_Whitepaper.md)
- [Link to YAML Definition File](lupes_v0.3.2.yaml)
- [Link to Changelog File](CHANGELOG.md)
- [Link to Proposed Enhancements File](Proposed_Enhancements.md)

## License
This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details.

## Contributing
We welcome contributions to the LUPES framework! Please see the [CONTRIBUTING.md](CONTRIBUTING.md) file for details on how to get involved.
