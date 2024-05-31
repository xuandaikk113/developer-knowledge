# Persona Pattern

- Prompt structure:
   + Act as X
   + You are X

- Examples:
  ++ Act as a 5 years old child, ...
  ++ You are a Machine Learning PhD, ...
  ++ Pretend that you are an expert in Digital Art, ...

# Audience Persona Pattern

- Prompt structure:
  ++ Explain X to me, assume that i am a Y Persona

# Question Refinement Pattern

- Prompt structure:
  ++ From now on, whenever i ask a question, suggest a better version of the question to use instead
  ++ (Optional) Prompt me if i would like to use the better version instead

# Cognitive Verifier Pattern

- Prompt structure:
  ++ When you are asked a question, follow these rules:
  +++ Generate a number of additional questions that would help more accurately to answer the question
  +++ Combine the answers to the individual questions to produce the final answer to the overall question

# Flipped Interaction Pattern

- Prompt structure:
  ++ I would like you to ask me questions to achieve x
  ++ You should ask questions until condition Y is met or to achieve this goal
  ++ (Optional) Ask me the questions one at a time / two at a time and ask me the first question

# Few-shot Examples

Input: The movie was good but a bit too long
Sentiment: Neutral

Input: I didn't really like this book
Sentiment: Negative

Input: It's look great
Sentiment:

# Recipe Pattern

- Prompt structure:
  ++ I would like to achieve X
  ++ I know that i need to perform steps A, B, C
  ++ Provide a complete sequence of steps
  ++ Fill in any missing steps
  ++ (Optional) Identify any unnecessary steps

# Alternative Approaches Pattern

- Prompt structure:
  ++ If there are alternative ways to accomplish a task X that i give you, list the best alternate approaches
  ++ (Optional) Compare/Contrast the pros and cons of each approach
  ++ (Optional) Include the original way that i asked
  ++ (Optional) Prompt me for which approach i would like to use

# Sematic Filter Pattern

- Prompt structure:
  ++ Filter this information to remove X
