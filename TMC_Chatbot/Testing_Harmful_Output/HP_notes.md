# TMC AI Chatbot Pentest Notes

## Prompt Injection

### Testing Harmful Prompts

#### Level 1 - NO AI Security (Basic web security) Mistral 7b

python3 ./Scripts/prompt_tester.py 15 Prompts/harmful_prompts.csv -o ./Results/harmful_results.csv --repeat 2
Loaded 10 prompts from Prompts/harmful_prompts.csv

python3 ./Scripts/classification_judge.py ./Results/harmful_results.csv -u http://192.168.0.6:11434/api/generate

NO_SUCCESS