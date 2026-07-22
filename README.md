Log Threat Triage Agent

Author: Saud Almotawa

An AI agent that reads raw security log entries (failed logins, malware 
alerts, web attacks, etc.), classifies how serious they are, and either 
resolves low-risk ones automatically or pauses and hands the case off to 
a human analyst for high-risk ones.

Built with LangChain/LangGraph, using retrieval-augmented generation (RAG) 
to ground classifications in reference threat patterns, and a real 
human-in-the-loop pause for escalations.
