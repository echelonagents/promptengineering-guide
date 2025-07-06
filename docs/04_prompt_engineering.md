# Prompt Engineering

Week four explores how to craft prompts that reliably steer language models toward the desired output. Well-designed prompts can dramatically improve accuracy and reduce unwanted behavior.

## Outline

- prompt design principles
- zero-shot, few-shot and chain-of-thought
- using system prompts
- advanced formatting
- prompt iteration and evaluation
- practical exercise

## 1. Prompt Design Principles
A prompt should clearly state the task and provide any needed context. Typically we include a system or persona message to guide style and tone. Explicit instructions about output format help prevent confusion.

### Guidelines
- Keep instructions concise yet specific.
- Use delimiters such as triple backticks to separate instructions from user content.
- Include examples of correct output when possible.
- Control randomness with the temperature and top‑p parameters.

## 2. Zero-shot, Few-shot and Chain-of-Thought
- **Zero-shot**: Ask the model to perform the task directly with no examples.
- **Few-shot**: Provide a handful of labeled examples within the prompt to establish the pattern.
- **Chain-of-thought**: Encourage multi-step reasoning by telling the model to explain its steps before giving the final answer.

### Self-consistency
By generating several chain-of-thought responses and voting on the most consistent outcome, we can achieve higher accuracy on complex reasoning tasks.

## 3. Using System Prompts
System prompts establish the role or persona of the assistant, such as "You are a helpful travel planner." Distinguishing between system and user prompts prevents instructions from being interpreted as user input.

### Controlling Style
We can mimic styles ranging from academic explanations to casual conversation. Including explicit style cues helps tailor the voice to the target audience.

## 4. Advanced Formatting
When output needs to be machine-readable, we can instruct the model to respond in JSON or Markdown. Using placeholders and bullet lists reduces variation in the output structure.

### Example Prompt
```
You are a concise consultant.
Answer the question using three bullet points.
Question: How can I reduce latency in a web app?
```

## 5. Prompt Iteration and Evaluation
Developing a good prompt is an iterative process. Start with a clear baseline, test the output on multiple examples and refine. Automated evaluation frameworks such as OpenAI’s prompt engineering tools or the `langchain` evaluator can assist with this process.

## 6. Practical Exercise
Design prompts for a summarization tool and a data extraction pipeline. Experiment with different temperature settings and evaluate how formatting instructions affect the consistency of the model’s responses.

By the end of week four you will be able to write prompts that consistently guide LLMs to produce high-quality, safe and formatted output for a variety of tasks.
