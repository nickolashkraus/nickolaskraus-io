---
title: "Programming with GPT-4 + GitHub Copilot"
date: 2023-03-24T00:00:00-00:00
draft: true
description: Augmenting my programming ability with Generative AI.
tags: ["programming", "ai"]
---

In late 2022, Generative AI entered the cultural zeitgeist in a big way. Though artificial intelligence had been progressing at an accelerating rate, the introduction of a novel interface (ChatGPT) made the advancement immediately tangible.

I was inspired to learn and implement Generative AI in my own programming workflow after seeing a [Twitter poll](https://twitter.com/SergioRocks/status/1611000883432624129) showing that over 50% of Software Engineers are not using any AI tool. Furthermore, of this group, 38% said they see no value in AI tools and roughly 34% said they don't know how to start.

Given the a) incredible power of Generative AI and b) extreme ease of use, these findings were quite staggering to me. They also represented a high-leverege scenario, whereby Software Engineers with the motivation to understand and implement AI tools to augement their ability to program would vastly outperform those who did not.

Being a highly motivated individual, I started to delve into the world of Generative AI.

## ChatGPT

ChatGPT is a sibling model to InstructGPT, which is trained to follow an instruction in a prompt and provide a detailed response. It is fine-tuned from a model in the GPT-3.5 series, which finished training in early 2022. To get started, just create an account and provide a prompt.

For my first prompt, I decided to clarify my intent:

>How can I use Generative AI to augment my programming ability?

>Generative AI can be used to augment your programming ability in a few ways. Here are a few ideas:
>
>1. Code generation: Generative AI can generate code snippets or entire programs based on input parameters or requirements. This can help you quickly generate boilerplate code or even complete solutions for certain tasks.
>
>2. Automated testing: Generative AI can be used to generate test cases and test data for your code. This can help you ensure that your code is working correctly and catch potential bugs before they become a problem.
>
>3. Code optimization: Generative AI can be used to optimize your code, either by suggesting changes to existing code or by generating optimized code from scratch. This can help you improve the performance of your code and reduce resource usage.
>
>4. Natural language programming: Generative AI can be used to interpret natural language descriptions of programming tasks and generate code to perform those tasks. This can make programming more accessible to people who are not familiar with traditional programming languages.
>
>Overall, generative AI can be a powerful tool to augment your programming ability by helping you automate certain tasks and generate optimized code more quickly and accurately. However, it's important to remember that generative AI is not a replacement for human expertise and should be used in conjunction with your own programming skills and knowledge.

For my second prompt, I decided to give ChatGPT a softball coding question:

>Write a function in Golang that takes in a string and returns true if the string is a valid palindrome, otherwise return false.

Here's an example function in Golang that checks if a given string is a valid palindrome:

```go
func isPalindrome(s string) bool {
    // Convert the string to lowercase
    s = strings.ToLower(s)

    // Remove all non-alphanumeric characters from the string
    r := regexp.MustCompile("[^a-z0-9]+")
    s = r.ReplaceAllString(s, "")

    // Loop through the string and compare the characters from both ends
    for i := 0; i < len(s)/2; i++ {
        if s[i] != s[len(s)-i-1] {
            return false
        }
    }

    return true
}
```

The function first converts the input string to lowercase to ensure that uppercase and lowercase characters are treated as equal. It then removes all non-alphanumeric characters from the string using a regular expression.

Finally, it loops through the string and compares the characters from both ends, stopping as soon as it finds a pair of characters that don't match. If all characters match, the function returns true; otherwise, it returns false.

For reference, here was my solution:

```go
func isPalindrome(s string) bool {
	for i, j := 0, len(s)-1; i <= j; i, j = i+1, j-1 {
		if s[i] != s[j] {
			return false
		}
	}
	return true
}
```

I was surprised that ChatGPT decided to remove non-alphanumeric characters and actually preferred how it chose to iterate over the string.

At this point I was hooked.

## GitHub Copilot

GitHub Copilot uses a custom version of the OpenAI GPT language model called Codex, which is specifically designed for code generation tasks.

Codex is based on the GPT-3 architecture, however, Codex has been fine-tuned on a much smaller, curated dataset of code, which allows it to generate more accurate and relevant code suggestions.

GitHub Copilot offers Vim plugin, [Copilot.vim](https://github.com/github/copilot.vim). Since I excluseively use Vim, this delighted me.
