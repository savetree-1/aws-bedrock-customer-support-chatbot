# Customer Support Chatbot - Evaluation Observation

## Overview
This document summarizes the testing and evaluation of the Bedrock Flow application designed for the Customer Support Chatbot project. The evaluation ensures that the chatbot correctly handles Bug Reports, Platform Questions, and Other Requests according to the provided requirements.

## Automated Test Coverage
An automated test suite was developed using the `generate-eval-dataset.py` script to programmatically invoke the Bedrock Flow and validate its behavior.

The test suite (`starter/flow-tests.json`) provides comprehensive coverage across the three distinct routing paths:
1. **Bug Report Path:** Simulates a user reporting an application crash and validates that the system correctly collects the required information (description, steps to reproduce, environment) and invokes the `create_bug_report` tool.
2. **Platform Question Path:** Simulates a user asking a question covered by the FAQ (e.g., "How do I track my order?") and validates that the response is accurately drawn from the embedded FAQ.
3. **Other Request Path:** Simulates a user providing general feedback and validates that the system politely directs them to human customer support without attempting to create a bug report.

## Dataset Generation and Upload
Running `generate-eval-dataset.py` successfully produced the evaluation dataset in JSONL format (`starter/output_eval_dataset_agentcore_final.jsonl`). 

This generated dataset was successfully validated and uploaded to an S3 bucket (`s3://udacity-agentic-engineer-c1-eval-516219161816/output_eval_dataset_agentcore_final.jsonl`), ready for ingestion by Amazon Bedrock Evaluations.

## Bedrock Evaluation Results
An automated evaluation job was run in Amazon Bedrock using the LLM-as-a-judge method (`customer-support-agentcore-final`). The outputs of the Bedrock Flow were scored against the reference responses defined in the dataset.

The evaluation yielded the following metrics:
- **Correctness:** `1.00`
- **Completeness:** `0.81`
- **Following instructions:** `1.00`

### Interpretation of Results
- **Correctness (1.00):** The model achieved a perfect score for correctness, demonstrating that its responses were highly accurate and factually aligned with both the strict bug-reporting rules and the embedded FAQ content. The model successfully avoided hallucinating unsupported answers.
- **Following Instructions (1.00):** The model perfectly followed the complex, multi-stage instructions defined in the `system_prompt.txt`, particularly regarding the strict collection rules and avoiding premature tool execution.
- **Completeness (0.81):** While the model's answers were correct and compliant, a completeness score of 0.81 indicates that some responses could have been more thorough or provided slightly more detail from the reference material. This leaves room for minor improvement, potentially by adjusting the system prompt to explicitly request more comprehensive explanations where appropriate, but the score is more than satisfactory for a production-ready baseline.
