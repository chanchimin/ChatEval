ChatEval: Towards Better LLM-based Evaluators through Multi-Agent Debate
---

<p align="center">
  <a href="https://arxiv.org/abs/2308.07201">Paper</a> •
  <a href="#-simple-video-demo">Demo</a> •
  <a href="#-getting-started">Getting Started</a> •
  <a href="#citation">Citation</a>    
</p>

**ChatEval** is designed to simplify the process of human evaluation on generated text. Understanding its workflow and functionalities is crucial for optimal usage. First check out our video demo to know how it works!

## 🎥 Simple Video Demo

We'd like to extend our heartfelt gratitude to [FastChat](https://github.com/lm-sys/FastChat) for their outstanding framework. Our demo was built upon the foundation they provided.



https://github.com/chanchimin/ChatEval/assets/75533759/35834dfd-5472-482a-905f-44b92708c90b




## 🚀 Getting Started

### Installation

Our code is basically built upon [AgentVerse](https://github.com/OpenBMB/AgentVerse), we recommend that users refer to their repository for package installation before running our code.


### Prepare Dataset

We provide the faireval benchmark examples in `agentverse/tasks/llm_eval/data/faireval/preprocessed_data/test.json`.
 
Your custom data should look like

```json
[
    {
        "question_id": 1,
        "question": "What are the primary factors that influence consumer behavior?",
        "response": {
            "gpt35": "There are several primary factors that influence consumer behavior, including:\n\n1. Personal factors - These include individual characteristics such as age, gender, income, education, personality, and lifestyle.\n\n2. Psychological factors - These include the consumer's perception of the product or service, motives, attitudes, beliefs, and values.\n\n3. Social factors - These include the consumer's family, friends, social class, culture, and reference groups.\n\n4. Situational factors - These include the physical environment, time, and mood of the consumer.\n\n5. Marketing mix factors - These include product, price, promotion, and distribution, and how they affect the consumer's decision-making process.",
            "vicuna": "There are several primary factors that can influence consumer behavior, including:\n\n1. Personal factors: These include individual characteristics such as age, gender, education, income, personality, and values.\n2. Psychological factors: These include emotional, cognitive, and perceptual factors that can influence how a consumer perceives and processes information about products and services.\n3. Social factors: These include cultural, social, and environmental factors that can affect how consumers view and use products and services.\n4. Economic factors: These include factors such as the price of a product or service, the availability of alternative products or services, and the consumer's purchasing power.\n5. Marketing factors: These include the way a product or service is marketed, including advertising, packaging, and distribution.\n6. Product and service factors: These include the quality, features, and benefits of a product or service, as well as its design, performance, and warranty.\n\nUnderstanding these factors can help businesses and organizations develop effective marketing strategies and tailor their products and services to meet the needs and preferences of their target audience."
        }
    }
]
```


### Configure Custom Debater Agent

You should modify your custom designed agent in `agentverse/tasks` folders.
Take `agentverse/tasks/llm_eval/multi_role/only_static_assign/faireval/two_turns_sequential/two_different_role/calc_score_comparison/gpt_35_0301/config.yaml` as an example, your agent configuration in config.yaml should look like:

```yaml
# config.yaml
agents:
  -
    agent_type: llm_eval_multi  # this should be one of the types in ./agentverse/agents/
    name: Critic  # name your agent
    final_prompt_to_use: |-   # this is used to control agents' behaviour in the last round.
      Please first provide a comprehensive explanation of your evaluation, avoiding any potential bias and ensuring that the order in which the responses were presented does not affect your judgment.
      Then, output two lines indicating the scores for Assistant 1 and 2, respectively.

      Remember that you are not required to output the same value as other referees !
      Output with the following format strictly:
      Evaluation evidence: [your explanation here]
      The score of Assistant 1: [score only]
      The score of Assistant 2: [score only]
    role_description: |-    # give your agent its personality
      You are now ... You like ... You 
    memory:
      memory_type: chat_history
    memory_manipulator:
      memory_manipulator_type: basic
    prompt_template: your prompt template  # substitute it with your custom prompt template
    llm:
      model: "gpt-3.5-turbo-0301"
      llm_type: gpt-3.5-turbo-0301
      temperature: 0
      max_tokens: 512
```

### Run the scripts

Now, you are good to run the experiments.
Try out this first.
```shell
bash scripts/llm_eval/multi_role/only_static_assign/faireval/two_turns_sequential/two_different_role/calc_score_comparison/gpt_35_0301.sh
```

## Citation
If you find this repo helpful, feel free to cite us.
```bibtex
@misc{chan2023chateval,
      title={ChatEval: Towards Better LLM-based Evaluators through Multi-Agent Debate}, 
      author={Chi-Min Chan and Weize Chen and Yusheng Su and Jianxuan Yu and Wei Xue and Shanghang Zhang and Jie Fu and Zhiyuan Liu},
      year={2023},
      eprint={2308.07201},
      archivePrefix={arXiv},
      primaryClass={cs.CL}
}
```
