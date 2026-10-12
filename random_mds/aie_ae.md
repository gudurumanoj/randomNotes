Loop on stop reasons and escalate to humans based on the stop reasons. Human in the loop is critical for the system to be able to handle the complexity of the task

Specialize tools for specialised agents and dont overload in terms of tools. Same goes for context, dont put everything in the context, just the relevant stuff! and let the agent read/load impromptu as it requires. Scope out the responsibility of the agent and dont give it more than that because if we ask the agent to do a lot of things then it goes haywire and screws up by doing less about everything

For prompts that can run in headless mode etc, use the batch mode to pay 50% less

---

When building own harnesses, its best to keep the small parts in the overall harness that's already in distribution to the model (for example, some models might be good at some form of file editing/grepping, liek using grep v jq etc) switching those when we switch models powering the harness helps us get better performance and more robust harnesses while the overall out of distribution is something we orchestrate and write the structure and this structure also doesnt need to be complex as the models keep becoming more advanced and capable.
In a nutshell, keep the model's in-distribution capabities as much as possible and keep the overall structure as simple as possible!

Evals and oberservabitlies is also essential:
- Having a benchmark for your task, an eval, is essential

Once we have the above two, its essential to log these trajectories and be able to instill the behaviour into the model powering the agent so that it gets better and better at the task!
Basically collect traces -> curate data -> run experiments on the traces. Feedback is really essential here, hence the observability and eval designs along with ux design is critical here (user may not explictily select anything, but its better to think and draft a good ux so that we can extract whatever feedback we can get from the users)
- Can finetune some slms to do this llm-as-a-judge analysis etc for data prep and cleaning since using opus on such a large scale of data burns a lot of money and time
- Code based tests to clean if possible to do it this way

Once we have the feedback/evals, we can update any part of the system: Harness/Model/Memory

Some way to automate the harness improvement as part of system evolution:
- some infra to filter traces for feedback, probably some tool with coding agent to do this. Probably with subagent support which go through the filtered traces or traces and identify the issues
- Maintain something like an issue board to track issues, fixes for each so that once an issue is fixed it doesnt get reopened again later because of fixes for some other issues and ofc to avoid duplication of the issues and to keep the issues generalised and not too specific to a trace etc (ofc with links to the traces so that there's a supporting evidence kind of thing etc)
- Once we have this, some code patch to fix it. Fix to patch => run tests and see if that issue arises again or not and if it doesnt, then its fixed! -- *can again maintain a benchmark here to test agents for its ability to fix!* This will also help in understanding the behaviours of various agents and how they perform under different circumstances and can be used to improve the agents further or key takeaways from the behaviours of the agents etc to improve our agent from the behaviours of frontier agents by seeing where they do well and where they lack and why!

***EVALS! EVALS! EVALS! BENCHMARKS! BENCHMARKS! BENCHMARKS!***

---
Learning from experience
Traceability and extracting a reward signal from the traces is essential to train the model because we need to know what "good" actually means for us. Extracting the reward signal from the users is what that makes this difficult

There's also the issue of whether the learning should go into model/harness

- We want to trace the whole tree, including the subagent work as well and also all the user actions like edit, redo etc
- Evals, the product evaling and training should be as close as possible to be able to properly induce the behaviour
- Harness shouldnt be too enforcing since the newer age models are really good at doing things. In additon, a strict harness could be very brittle to updates/changes or to even model changes. Every action should be informative enough to be able to build signals
- Using openmodels is better because they can be trained on the traces and can be made better

Distribution matching to the customer data and generating synthetic data matching that distribution to train where training on customer data is not possible or not feasible

What went right is essential to know, not just what went wrong. We can only penalise the wrong behaviour but cant correct it to the right behaviour. And things like globally true should be trained into the model while specific things should probably go into the harness or as part of episodic or user memory etc

---
Limiting behaviour to automate everything aroudn synthetic data is being able to model all possible worlds and tasks
While building evals, we should keep in mind what are the different kinds of mistakes the model/agent could make and build evals for that which is the difficult part. We should also keep in mind what are the different kinds of actions the model/agent could take and build evals for that.

---
Using the frontier ecosystem, we can beat frontier models via specialised things

- Gotta build a benchmark first always. No benchmark => no proper evals/measurement => cant train models to measure performance. If we have a benchmark, we can then train the models to measure performance and then we can use the models to measure performance on the benchmark.
- Using domain experts and coding agents to create synthetic data since training on customer data is not possible or not feasible. Synthetic data helps
- Once we have datasets, making an efficient and effective rl environment from these datasets is essential to be able to train the models to perform well on the benchmark and to make the training cost effective
- Working with expertise external labs helps you understand whether the data is good etc if you dont see any improvements in the performance of the models on the benchmark even with their expertise training
- Pre-production benchmark tests, side by side comparison of models, human testing etc are essential to be able to know the performance of the models and to compare newly released models with the previous ones
- Model routing is crucial/essential to build the muscle of open/close model routing for cost/performance frontier
- Serve in production -> collect traces and feedback -> build envs/data -> posttrain

---
What level of intelligence is required to do a task is essential to know because we don't want to overspend. We can always start with stronger models to see if that task is even achievable, then we can look at the traces of the stronger model, see what parts of it can be delegated to agents/models of lesser intelligence. An example could be an LLM-as-a-judge task which uses a lot of input tokens, which could be costlier to use if we have millions of examples to go through! It implies whether if we can match the performance/llm-judging of frontier models with cheaper open alternatives via either propmt engineering/skills/harness engineering at lower cost. With guidance etc

Prompt/harness engineering can only take us so far, so narrow task finetuning can be done to match the performance of the frontier models at much lower cost. Ofc we should move from token cost to overall task/harware cost that's gonna cost us since smaller models tend to consume more tokens than larger models for the same task and even harness/prompt/guidance can drive them to consume more tokens which the frontier models dont require. 

A more better model would be having a model router etc

Densifying feedback to agents is a really good way to fix them since just a yes/no feedback cant tell the agent what went wrong and how to fix it. We need to be able to tell the agent what went wrong and how to fix it.

---
Dont put coding standards into the implementer, implement with the implementer subagent while code review and re-structuring should be done by the code review subagent.
The reviewer agent should commit, not put comments for us humans to review
Dont try to one-shot good code

One way door v two way door: If you can revert the change without/doesnt incur too must cost (two way door) then it doesnt relatively need extensive review

THe "why" should be understood as fast as possible

We also want to keep evolving our skills because we dont want the agent to keep committing the same mistakes over and over. `/retro` skill

---
Specialised workflows for agents to do repeatable work
PDF parsing is hard, because its rendered for viewing and not for machine reading purposes. Same goes for word and powerpoint documents
You want to route to cheaper models based on the task at hand etc this not only reduces cost but also improves performance and reduces latency since the models are specialised for the task at hand

Its all about mimicking how humans would do the work using these intelligent llm agents