+++
title = 'Job Offer from Longest Common Subsequence'
date = 2025-09-11T00:00:00-00:00
lastmod = 2025-09-11T00:00:00-00:00
draft = false
description = '''
How I got a job offer from knowing the Longest Common Subsequence algorithm and
why knowing stuff is still important in the age of AI.
'''
tags = ['technology', 'career', 'algorithms']
+++

Back in January, I spent (literally) 10 hours understanding and implementing
[Longest Common Subsequence][Longest Common Subsequence]. Like many problems
that can be solved using dynamic programming, 99% of the effort goes into
understanding the algorithm. The code is quite simple:

```python
def longest_common_subsequence(a: str, b: str) -> int:
    """
    Given two strings, `a` and `b`, compute the length of their Longest Common
    Subsequence (LCS).
    """
    m, n = len(a) + 1, len(b) + 1
    dp: list[list[int]] = [[0 for j in range(n)] for i in range(m)]

    for i in range(1, m):
        for j in range(1, n):
            if a[i - 1] == b[j - 1]:
                dp[i][j] = 1 + dp[i - 1][j - 1]
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

    return dp[i][j]
```

Fast-forward 8 months. While interviewing at a company[^1], I got a technical
take home test through [CoderPad][CoderPad]. The gist of the problem was to
find a subsequence of string `a` containing string `b`. Now, CoderPad boasts on
their website:

>AI broke technical hiring.  
>We fixed it.

And they are not wrong. I had access to chat-based LLMs for the assessment,
albeit weaker ones[^2]. The presumed goal of allowing a candidate to use an LLM
during a coding interview is to remove the unfair advantage they provide.
Instead of going on faith that a candidate will not use AI when asked politely,
all candidates are given access to AI. This is where my thesis, namely, knowing
stuff is still important in the age of AI, garners some supporting evidence.

If you were to dump the problem into the chat and say, "How would you solve
this?" you would get back a rather straight-forward, unimaginative, contrived
solution[^3]. Without going into the details of the actual problem, an optimal
solution is to use the Longest Common Subsequence algorithm, which is a novel,
more creative way to solve the problem[^4].

Though a specific example, I believe this scenario applies more generally to
most problems where the solution is inspired, rather than rote. In addition, it
evidences a theory around AI that I have been mulling for the past couple
months:

>Having knowledge at your fingertips is still, and will continue to be,
>extremely important.

There seems to be a pernicious, Mephistophelian bargain developing in the tech
industry where even very smart people are slowly allowing LLMs to do more of
their thinking and problem solving[^5]. This has the side effect of slowly
eroding their skills and deep understanding of the problem space. It also
causes their solutions to devolve toward the mean solution. Short-term
productivity traded for long-term understanding.

Ditto to the fact that people are not going to wait around for you to input
their question into an LLM before responding. In the normal course of dialog,
even discrete, under-the-table prompting kind of just kills the vibe of the
conversation.

I've personally wrestled with this new way of working, mourning the apparent
death of the inspired for the fast and good-enough. From this, I've developed
a simple framework, which I apply to my use of LLMs:

```
                             +-------------------------+
                             | Do I know how to do it? |
                             +-------------------------+
                                 /                 \
                             +-----+             +----+
                             | Yes |             | No |
                             +-----+             +----+
                                |                   |
             +--------------------+   +---------------------------------+
             | Have an LLM do it. |   | Learn how to do it with an LLM. |
             +--------------------+   +---------------------------------+
```

Striking a balance between generation and inspiration, I am able to be wildly
productive, while still being able to, you know, code and problem solve. As one
might expect, I still am very bullish on deeply understanding things, not only
for its utility, but that it generally *feels* good to acquire knowledge.

All in all, I'm actually quite worried about the future of society. Why, when
even very smart people can be duped into trading their skill for productivity
would the general public not as well? I feel like this is a textbook
application of Matthew 13:12, where those that are able to navigate the siren
song will come out wildly ahead, while those who do not will wallow in slop.
Maybe all the Sci-Fi cliches will come to pass and the human race will realize
that to be human is to think and if all of our thinking is outsourced to
machines we become less human. It follows then that we will choose to destroy
technology in a fit of humanistic frenzy, since AI will be viewed as a source
of existential competition. It does seem rather in keeping with our inherent
drive for survival.

Anyway, closing the loop on the titular thread of this post, I progressed
through the technical take home test in the interview process with a *Move
forward with high confidence*, which I believed was crucial in receiving an
offer. Had I not known LCS, I probably wouldn't have moved forward, thus
solidifying my belief that knowing stuff is still important in the age of AI.

[^1]: I won't dox them, but from my LinkedIn, you can probably guess what
company.
[^2]: GPT-4o mini, Claude Haiku 3.5, Gemini 2.5 Flash, Llama 4 Maverick
[^3]: When running the prompt through ChatGPT 5 and Claude Sonnet 4, ostensibly
more powerful models as compared to the ones offered by CoderPad, ChatGPT
provides a sensible, yet unimaginative solution, whereas Claude churns on the
problem hardcore and never produces a solution.
[^4]: Indeed, when I review the solution with a software engineer at the
company, which was part of the interview process, he relayed to me that my
solution stood out due to its uniqueness.
[^5]: There have been two notorious studies, which, though limited, have
intimated that the use of AI is detrimental to cognitive function: one out of
[MIT][Your Brain on ChatGPT] and one out of [METR][Measuring the Impact of Early-2025 AI].

[Longest Common Subsequence]: https://en.wikipedia.org/wiki/Longest_common_subsequence
[CoderPad]: https://coderpad.io
[Your Brain on ChatGPT]: https://arxiv.org/abs/2506.08872
[Measuring the Impact of Early-2025 AI]: https://metr.org/blog/2025-07-10-early-2025-ai-experienced-os-dev-study/
