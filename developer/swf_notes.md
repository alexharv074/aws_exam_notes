# SWF

Ideal for:

- media processing
- webapp backends
- business process workflows
- analytics pipelines

> Maintaining your application's execution state (e.g. which steps have completed, which ones are running, etc.) is a perfect use case for SWF.

> Amazon SWF is useful for automating workflows that include long-running human tasks (e.g. approvals, reviews, investigations, etc.) Amazon SWF reliably tracks the status of processing steps that run up to several days or months.

Tasks:

- Can execute code etc

Workers:

- Programs that interact with SWF to get tasks, process tasks

Decider:

- Program that controls the coordination of tasks

Domains:

Scope that contains the workers and deciders
Parameters are in a JSON file

Workflow maximum 1 year in seconds

SWF v SQS

SWF is task-oriented API and SQS is message-oriented API.
SWF a task is assigned only once and never duplicated.
SWF keeps track of tasks ; SQS you keep track of messages yourself.

