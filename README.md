## Integration of a Mathematical Calulations with a Chat Completion System using LLM Function-Calling

### AIM:
To design and implement a Python function for coverting miles to kilometer, integrate it with a chat completion system utilizing the function-calling feature of a large language model (LLM).

### PROBLEM STATEMENT:
You need to create a Python program that can intelligently convert miles to kilometers by leveraging OpenAI's function calling capability.
### DESIGN STEPS:

#### STEP 1:
Install required packages
#### STEP 2:
Give the essential function calling code into it
#### STEP 3:
Integrate the function into an LLM-based chat completion system with function-calling capabilities.
### PROGRAM:
```
import os
import openai
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
openai.api_key = os.environ['OPENAI_API_KEY']
```

```
import json
# Function to convert miles to kilometers
def convert_miles_to_km(miles):
    """Convert miles to kilometers"""
    try:
        miles_float = float(miles)
        kilometers = miles_float * 1.60934
        conversion_info = {
            "miles": miles,
            "kilometers": round(kilometers, 2),
            "conversion_factor": 1.60934,
            "unit_from": "miles",
            "unit_to": "kilometers"
        }
        return json.dumps(conversion_info)
    except ValueError:
        return json.dumps({"error": "Invalid input. Please provide a valid number."})
```

```
functions = [
    {
        "name": "convert_miles_to_km",
        "description": "Convert miles to kilometers",
        "parameters": {
            "type": "object",
            "properties": {
                "miles": {
                    "type": "string",
                    "description": "The distance in miles to convert to kilometers, e.g. 10, 5.5, 100",
                },
            },
            "required": ["miles"],
        },
    }
]
```
```
messages = [
    {
        "role": "user",
        "content": "Convert 10 miles to kilometers"
    }
]
```
```
final_response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
)
```
```
args = json.loads(response["choices"][0]["message"]['function_call']['arguments'])
observation = convert_miles_to_km(args["miles"])
```
```
messages.append(response["choices"][0]["message"])
```
```
messages.append(
    {
        "role": "function",
        "name": "convert_miles_to_km",
        "content": observation,
    }
)
```
```
final_response = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
)
```
```
print(final_response["choices"][0]["message"]["content"])
```


### OUTPUT:

<img width="1118" height="120" alt="image" src="https://github.com/user-attachments/assets/321fe382-966f-4625-bb13-2967397e5217" />


### RESULT:
Hence,the python program to design and implement a Python function for converting miles to km,
integrating it with a chat completion system utilizing the function-calling feature of a large language model (LLM) is written successfully and executed.
