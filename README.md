Log Threat Triage Agent

Author: Saud Almotawa

An AI agent that reads raw security log entries (failed logins, malware 
alerts, web attacks, etc.), classifies how serious they are, and either 
resolves low-risk ones automatically or pauses and hands the case off to 
a human analyst for high-risk ones.

Built with LangChain/LangGraph, using retrieval-augmented generation (RAG) 
to ground classifications in reference threat patterns, and a real 
human-in-the-loop pause for escalations.

Usage:
1. Go to colab.research.google.com
2. File -> Upload notebook -> select the .ipynb from this repo
3. Make sure to have your API keys ready
4. Runtime -> Run all
5. The last cell will prompt you to paste in a log line to test (you can use one of the examples in LogsExamples.md)

---------------------------------------------------------------------------

Write-up:

Agent fundamentals:
The agent calls record_classification, a real tool, and the LLM decides the category, severity, and rationale itself. If it fails to call the tool, the code catches that and asks it to try again, up to 3 times. Tested with a high severity log and a low severity log, both got classified correctly through real tool calls.

Multi-agent / handoff architecture:
The workflow checks severity after classification. If severity is high or critical, it calls escalate_to_human, which pauses and waits for a person. If severity is low or medium, it resolves on its own. This is the handoff, tested and confirmed working both ways.

RAG pipeline:
Reference threat pattern text is embedded and stored in an in memory vector store. retrieve_context_task always runs first and pulls relevant context before classification happens. This is 2 step RAG, always retrieve then classify, since every log needs the same kind of grounding.

Context and state management:
InMemorySaver keeps the state of a run so it can pause and resume later. This is short term memory for a single log's run. There is no long term memory saved across different logs in this version.

Human in the loop:
escalate_to_human calls interrupt, which really pauses the workflow. Tested with a high severity log, it paused, printed the case details, then resumed once a decision was typed in.

LangGraph functional API and error handling:
Built with task and entrypoint, two error handling methods used. RetryPolicy on retrieve_context_task for network type failures. A try and except loop inside classify_log that catches when the model does not call the tool correctly and asks it to try again.

Workflow pattern:
Evaluator, optimizer. classify_log generates a classification, llm_call_evaluator grades it as accept or revise, and it loops back with feedback until accepted.

LangSmith observability:
Traces showed two different paths. A low severity login went straight to auto resolved with no pause. A high severity log went through escalate_to_human, paused, and resumed after a decision was given. This confirms the branching logic works for both cases.


