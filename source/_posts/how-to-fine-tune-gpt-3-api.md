title: How to fine-tune GPT-3 on your curated data?
author: Harish Garg
date: 2021-07-28
tags:
---


A couple of weeks back, OpenAI introduced the capability to train new fine-tuned models based on their GPT-3 API

I got some time this weekend to train a new fine-tuned model of my own.

Here are some quick findings

## What does fine-tuning a GPT-3 model mean?

OpenAI, by default, gives you a few AI models or engines that are suited for different tasks. 

However, sometimes you don't get the desired output, or getting the output is too expensive.

Fine-tuning gives you the ability to take OpenAI's base models/engines and train a new model but on a curated dataset that you supply.

## How is fine-tuning GPT-3 better then just plain old prompting?

This could be useful in number of ways

1. It could get you better quality outputs with no or fewer examples in the prompts.
2. You can train your models on hundreds of examples - up to a total of 80-100mb per dataset
3. You will save on API usage costs due to less or no examples in your prompts. Fine-tuning a model is free.
4. You will have lower latncy in your API calls.

## Some key points about fine-tuning GPT-3

The model will not be shared with other API users and will be private to the org/users who fine-tuned it.

Hwoever, there might be a possibility of sharing fine-tuned models with other companies, hence creating a de-facto marketplace for fine-tuned models.

Right now, you can fine-tune up to 10 moodels per month and each dataset can be up to 2.5M tokens or 80-100MB in size.

You can use the fine-tuned from the OpenAI Playground,  from the command line using the OpenAI command line tool, CURL command or from within your code.

## What does a fine-tuning training dataset look like

Training dataset has to be in jsonl format where each document is seperated by a new line.

A typical dataset jsnol file looks like this.

{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
{"prompt": "<prompt text>", "completion": "<ideal generated text>"}
....

## Fine-tuning Steps

There are basically three steps involved in fine-tuning GPT-3.

1. Prepare the training dataset
2. Train a new fine-tuned model
3. Use the new fine-tuned model

Let's cover each of the above steps one by one.

1. Prepare the training dataset

As with using the base GPT-3 models, the creativity lies in coming up with creative examples for a practical and novel use case. This is where domain knoweldge comes into play. 

One example here would be fine-tuning GPT-3 on a foriegn language where the base GPT-3 is not very good at. One way to do this is collect high quality representive text for that language and then prepare the dataset file where prompt is empty and completion has the text from that language.

This is how the dataset jsonl file would like.

{"prompt": "", "completion": "<ideal generated text>"}
{"prompt": "", "completion": "<ideal generated text>"}
{"prompt": "", "completion": "<ideal generated text>"}
....
 
You would have to take care to keep each record less then 2048 tokens.

Once, you have the dataset ready, run it through the OpenAI command line tool to validate it.

openai tools fine_tunes.prepare_data -f <LOCAL_FILE>

You can also pass files in CSV, TSV, XLSX, JSON or JSONL format to this tool and it will help you convert it into a fine-tuning ready dataset.

2. Train a new fine-tuned model

Run the below command from the command line program to train your fine-tuned model.

openai api fine_tunes.create -t <TRAIN_FILE_ID_OR_PATH> -m <BASE_MODEL>

Replace the filename and choose a model name to base your model on. Current options are curie, babbage or ada.

Once the fine-tuning finishes, you will see the model id.

3. Using the new fine-tuned model.

One way to using your newly fine-tuned model is through command line

openai api completions.create -m <FINE_TUNED_MODEL> -p <YOUR_PROMPT>

You could also use it in your code, for example in Python

import openai
openai.Completion.create(
    model=FINE_TUNED_MODEL,
    prompt=YOUR_PROMPT)
    
This model will also be available in the model list from the OpenAI Playground.


## Summary
    
That's it. this is how you fine-tune a new model in GPT-3. This was just part -1. I will cover these steps in details in subsequent posts.


If you need help with GPT-3 Fine-tuning, you can book a fine-tuning session from here - https://harishgarg.gumroad.com/l/UIGGc



