---
{"dg-publish":true,"permalink":"/information-heap/rasa-training/","noteIcon":"","created":"2025-09-20T02:41:00.694-05:00"}
---

Date: [[-Daily Activity Log-/2025 09-September 20\|2025 09-September 20]]



Based on the log messages, the Rasa model is successfully loaded, but it's failing to correctly identify the user's intent. The repeated error "Could not determine the intent to group data. Please be more specific" along with the debug logs showing `parse_data_intent={'name': None, 'confidence': 0.0}` for all inputs confirms that the model is **not trained to recognize the `group_data` intent** from the provided text examples.

---

### Analysis of the Problem

The core issue isn't that the model isn't loaded; it's that the **NLU model hasn't learned the patterns** associated with the `group_data` intent. Rasa's NLU pipeline works by training on examples you provide in a `nlu.yml` file. When you give it new text, it compares it to its training data to predict the most likely intent and entities.

The logs show that for all the user inputs—from the long wheat experiment description to the shorter "Group my players by team..." phrase—the model's confidence in an intent is `0.0`. This is a strong indicator that the model has not been trained on any examples that look like these inputs.

### Solution Steps

To resolve this, you need to provide Rasa with good training examples that map to the `group_data` intent and its associated entities.

#### 1. Add Training Examples to `nlu.yml`

You must add a variety of example phrases to your `nlu.yml` file under the `intent: group_data`. These examples should cover different ways a user might express the need to group data.

YAML

```
# data/nlu.yml

version: "3.1"
nlu:
- intent: group_data
  examples: |
    - Group my players by team and then by position.
    - I want to group the genetic varieties by type of wheat.
    - Organize the plots by variety.
    - Show me the data grouped by city and then by state.
    - Can you please create groups based on quarter and player?
    - I need to see the results grouped by experiment and then by trial.
    - Group all the time series data by plot replicant.
    - I want to look at the data by variety and plot.
    - Group the data by [group level 1] and then by [group level 2].
    - Group the [data points] by [group level 1].
```

> **Note:** The last two examples with bracketed text are generic templates that help the model learn the structure of the intent.

---

#### 2. Define Entities for Grouping

Your code expects specific entities like `group_level_1`, `group_level_2`, `entity_level`, and `data_points`. You must define these entities in your `domain.yml` file and label them in your `nlu.yml` examples.

YAML

```
# domain.yml

version: "3.1"
intents:
  - group_data

entities:
  - group_level_1
  - group_level_2
  - data_points
```

Then, you must add examples with these entities in `nlu.yml`:

YAML

```
# data/nlu.yml

- intent: group_data
  examples: |
    - Group my players by [team](group_level_1) and then by [position](group_level_2). The data includes [points per game and rebounds](data_points).
    - I want to group the [genetic varieties](data_points) by [type of wheat](group_level_1).
    - Can you create groups based on [quarter](group_level_1) and [player](group_level_2)?
    - I need to see the results grouped by [experiment](group_level_1) and then by [trial](group_level_2).
    - Group all the [time series data](data_points) by [plot replicant](group_level_1).
```

> **Tip:** You may need to create a custom Rasa component or an advanced entity extraction approach to handle all the "data points" you mentioned. Simple entity labeling might not be sufficient for complex phrases.

---

#### 3. Retrain the Rasa Model

After updating your `nlu.yml` file, you must retrain the model. From your project's root directory (the same place you ran `poetry run python -m app.app`), execute the Rasa training command:

Bash

```
poetry run rasa train
```

This command will train a new model and save it as a `.tar.gz` file in your `models/` directory. Your `load_rasa_model` function will automatically find and use this new, better-trained model the next time you run your Flask application.

**In short, your app is running fine. The problem is with the trained model itself—it hasn't been taught how to understand the user's request. You need to train it with relevant data.**

---

You're correct that training a complex Rasa model and packaging it can be a significant undertaking. Routing to an AI API or hosting your own Rasa server are both valid and common solutions for this problem.

---

### 1. Option: Routing to an AI API (e.g., GPT-4o, Claude 3)

Using a powerful, general-purpose AI API is a very flexible approach. Instead of training a Rasa model, you would send the user's natural language request directly to the API, and the API would return a structured response.

#### **How it Works**

1. Craft a good prompt: The key is to design a prompt that instructs the AI to extract the same information your Rasa model was designed to find. You can use a prompt like this:
    
    "You are an expert at parsing user requests for data visualization. Given the following user input, identify the grouping levels and data points and return a JSON object. If you cannot determine the grouping levels, return an error.
    
    User input: [User's text]
    
    Expected JSON format: {"group": "...", "subgroup": "...", "data_points": ["..."]}"
    
2. **Make an API call:** Your Python app would use a library like `requests` or an official SDK (e.g., `openai`) to send the prompt to the AI API.1
    
3. **Process the JSON response:** The API will return a JSON string, which you can parse into a Python dictionary.2 You can then use this dictionary to generate your XML.
    

#### **Pros**

- **No training required:** You avoid the complexity of training, retraining, and managing Rasa models.
    
- **Flexibility:** The API can handle a wider range of inputs and variations without being explicitly trained on them.
    
- **Rapid prototyping:** This is the fastest way to get a working NLP solution.
    

#### **Cons**

- **Cost:** API calls are typically priced per token, which can become expensive with high usage.
    
- **Internet dependency:** Your software must have an internet connection to function.
    
- **Latency:** The response time can be slightly slower than a locally hosted solution.
    

---

### 2. Option: Hosting a Trained Rasa Model on a Server

This is the most common production-level solution for this problem. Instead of including the Rasa model with every local installation, you deploy a single, central Rasa server.

#### **How it Works**

1. **Set up a server:** Deploy your trained Rasa model to a server or cloud service (e.g., AWS, GCP, Azure).3 This involves running the Rasa server using a command like `rasa run --enable-api`.4
    
2. **Expose an API endpoint:** The Rasa server will expose an endpoint (e.g., `/webhooks/rest/webhook`) that accepts HTTP POST requests with user messages.5
    
3. **Call the endpoint:** Your local software installations would send a request to this endpoint with the user's text. The server processes the request and returns the NLU prediction.
    

#### **Pros**

- **Cost-effective at scale:** After the initial server cost, the marginal cost per request is negligible, making it much cheaper than a commercial AI API for high usage.
    
- **Performance:** A dedicated server can be optimized for low latency and high throughput.6
    
- **Control and privacy:** You have full control over your data and model, which is crucial for sensitive or proprietary information.7
    

#### **Cons**

- **Infrastructure management:** You are responsible for setting up, securing, and maintaining the server.
    
- **Internet dependency:** Similar to the API option, this requires an internet connection.
    

### **Recommendation**

For initial development and prototyping, **using an AI API is the easiest and fastest way to test your idea**. This will prove the concept without the hassle of training and hosting.

For a production application, **hosting a Rasa model on a server is the recommended long-term solution**. It offers a better balance of cost, performance, and control for a dedicated application.

---

Yes, you can constrain the responses of an AI API to fit a certain format.1 Most modern large language models (LLMs) from providers like OpenAI, Google, and others offer **native JSON modes** or other structured output features that guarantee a valid JSON response.2

---

### 1. Using a Response Schema or JSON Mode3

This is the most reliable method for ensuring a specific output format. You define a **JSON schema** (a blueprint for a JSON object) that specifies the fields, data types, and required properties.4 The API will then attempt to produce a response that strictly adheres to this schema.5

- **How it works:** When you make an API call, you include a parameter like `response_format` and set it to `{"type": "json_object"}`. 6For more advanced control, you can define a full JSON Schema that dictates the exact structure, and the model will be constrained to follow it. This method prevents the model from generating extra text or invalid JSON.
    
- **Benefits:** It guarantees a valid, machine-readable output, making it easy to parse the response in your code without extra validation or error handling.7
    

---

### 2. Prompt Engineering

Even without a native JSON mode, you can use prompt engineering to guide the model's output.8 This involves giving the model very clear and explicit instructions within the prompt itself.

- **How it works:** You can include instructions like, "You must only respond with a JSON object. Do not include any other text, explanations, or markdown. The JSON object should have the following keys: `group`, `subgroup`, and `data_points`."
    
- **Benefits:** It works with a wider range of models and APIs, even those without native structured output features.
    
- **Drawbacks:** This method is not foolproof. The model may occasionally include extra text or generate malformed JSON, requiring your code to have robust error handling and parsing logic.9
    

---

### 3. Combining Approaches

For the best results, **combine a JSON-focused prompt with a native JSON mode**. The prompt provides the human-readable instructions and context, while the native mode provides a strict, technical constraint that forces the model to obey the format. This dual approach minimizes errors and ensures high-quality, predictable outputs for your application.

This video on unlocking amazing AI videos explains how JSON prompting can be used to achieve highly specific and structured outputs for AI-generated content.