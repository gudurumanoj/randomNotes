## RL Envs
The reward stops being just a function and becomes the environment the agent acts in, this is essential because the models now dont just answer in one turn but take actions in complex environments

The environment will keep track of the "state"

Environment is usually presetn in a sandbox and it shouldn't contain (and usually wouldn't contain) anything related to task verification because we dont want it to hack the problem but truly learn to do the task

The model weights will get updated, but its the harness powered by the model that'll interact with the environment and take actions and then get feedback from the environment. There's nothing much like "training the harness"

THe best place to always start with an agent RL environment training is to first perform eval (even in mercor blog, they do this to identify and mitigate issues) of the harness with some model and use the reward as the proxy for eval score -- it can help surface any bugs that are present, get us some baselines etc and all

We can also use distillation here instead of doing rl!! We can get both the student and teacher to perform the tasks and do reverse kl instead of reward/rollouts' grpo based updates. Even on-policy self distillation can also be done here!
Just to add, use a good model to generate traces via the env and do sft as well lol

env is typically implemented as "gym" with two major functions: *reset* which resets the environment to the initial state and *step* which updates the state with the action the llm gave, with *state* to get the current state

*hf's trl+openenv toolkit* for doing agentic rl here, defining the env and hooking it to the `grpotrainer/asyncgrpotrainer` thats all!

To do grpo, we sample *k* rollouts and each rollout acts in the environment and gets a reward, then we compute the grpo update with the *k* rollouts

Can maintain some metrics to identify reward hacking, like some specific number of tool calls, specific set of tool calls etc required types and seeing spikes/anamolies there can help detect reward hacking!

