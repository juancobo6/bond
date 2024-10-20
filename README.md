# Bond

**Bond** is a Python tool that leverages `ollama` and `langchain` to obtain structured outputs from a locally run Large Language Model (LLM). By combining these technologies, Bond allows users to extract specific, structured information from textual input based on a provided format.

## Features

- **Structured Output**: Bond ensures that responses from the LLM follow a structured format (JSON) defined by the user.
- **Customizable Prompts**: Users can define the prompt, format, and task to tailor how the LLM processes the input text.
- **Seamless Integration**: It leverages `ollama` to run a local instance of an LLM and `langchain` for easy model interaction.

## Getting Started

### Prerequisites

To use Bond, ensure you have the following installed:

- Python 3.8+
- [Ollama](https://ollama.com/) installed locally.
- The required Python libraries:
  ```bash
  pip install langchain_ollama
  ```

### Installation

Clone the Bond repository:

```bash
git clone https://github.com/yourusername/bond.git
cd bond
```

### Usage

1. **Define the idea**: Write the textual idea you want to extract structured information from.
2. **Create a format**: Define the structure for the output (in JSON) that the LLM should adhere to.
3. **Run the model**: Use the `Bond` class to send your prompt to the LLM and retrieve the structured output.

#### Example

Here’s an example of how to use Bond to extract key points from a business plan:

```python
from bond import Bond
from prompt import Prompt

# Input text (the "idea")
idea = """The business plan for a company offering a software platform to deploy mathematical models 
would focus on providing a seamless, user-friendly solution that allows organizations to easily integrate, 
test, and run complex mathematical models in various applications. The platform would target industries such 
as finance, engineering, healthcare, and logistics, where predictive analytics, optimization, and simulation 
are crucial. It would offer a cloud-based service with scalable resources, enabling users to deploy models 
without requiring extensive coding or infrastructure expertise. Revenue would come from subscription-based 
pricing, tiered by usage and features, catering to both small businesses and large enterprises. The platform 
would support a wide range of mathematical models, including machine learning algorithms, statistical analysis, 
and operational research, offering flexibility through integrations with popular programming languages like 
Python, R, and MATLAB. The marketing strategy would focus on demonstrating the platform’s ability to streamline 
the deployment process, reduce development time, and improve decision-making by offering real-time results. 
Partnerships with academic institutions, research organizations, and consulting firms would help drive adoption, 
while customer support, continuous updates, and data security would be key to maintaining trust and ensuring 
long-term client relationships."""

# Define the output format
format = {
    "point1": {
        "title": "***point1_title***",
        "description": "***point1_description***"
    },
    "point2": {
        "title": "***point2_title***",
        "description": "***point2_description***"
    },
    "point3": {
        "title": "***point3_title***",
        "description": "***point3_description***"
    },
    "point4": {
        "title": "***point4_title***",
        "description": "***point4_description***"
    },
    "point5": {
        "title": "***point5_title***",
        "description": "***point5_description***"
    }
}

# Create the prompt object
prompt = Prompt(idea, format)

# Initialize the Bond class with the selected model
bond = Bond("llama3.2:1b")

# Get the structured output
output = bond.ask(prompt)

# Display the result
print(output)
```

In this example:
- The `idea` contains a business plan.
- The `format` specifies the expected structure of the output.
- The `Bond` object interacts with the `llama3.2:1b` model, running locally via Ollama, to generate structured JSON based on the given `idea` and `format`.

### Output

The output will be a JSON object that follows the structure you defined in the `format` variable. Example:

```json
{
    "point1": {
        "title": "Target Industries",
        "description": "Finance, engineering, healthcare, and logistics."
    },
    "point2": {
        "title": "Cloud-based Service",
        "description": "Offers scalable resources for deploying models without extensive coding."
    },
    "point3": {
        "title": "Revenue Model",
        "description": "Subscription-based pricing, tiered by usage and features."
    },
    "point4": {
        "title": "Supported Models",
        "description": "Supports machine learning, statistical analysis, and operational research models."
    },
    "point5": {
        "title": "Marketing Strategy",
        "description": "Focuses on demonstrating the platform’s ability to streamline deployment and improve decision-making."
    }
}
```

## Classes

### `Prompt`
Represents the prompt that will be sent to the LLM.

- **Attributes**:
  - `idea`: The main idea or text to be processed.
  - `task`: The task the model should perform (default: `"Extract the main points of the following idea."`).
  - `format`: The structure for the expected output.
  - `indications`: Instructions for the model on how to respond (default: instructs the model to respond in JSON format).

- **Methods**:
  - `get()`: Returns the full prompt as a string, ready to be passed to the model.

### `Bond`
Handles the interaction with the LLM using the `langchain_ollama` library.

- **Attributes**:
  - `model`: The name of the LLM model (default: `"llama3.2:1b"`).
  - `config`: Configuration for the model (currently unused).
  - `structured`: If `True`, forces the model to return a structured response (default: `False`).

- **Methods**:
  - `ask(prompt)`: Sends the provided prompt to the model and returns the structured output as a Python dictionary.

