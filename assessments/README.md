# Threat modeling

Resources:

- The article here is copied/pasted and shortened from this series of articles: https://docs.microsoft.com/en-us/archive/blogs/larryosterman/some-final-thoughts-on-threat-modeling

You threat model to identify threats to your component, which then lets you know where you need to concentrate your resources.

<b>It doesn't solve the problem.</b> The threats and risks are not solved until they are eliminated.

Two threat modeling techniques are described:

- Security assessment: Detailed security assessment

- Rapid assessment: Very rapidly performed security assessment when there is no time or resources available for a detailed security assessment

The same risk methodology can be used for both assessments.

## Rules of thumb

While it can be easy to find threats, it is important to realize that all threats have real-world consequences for the development team.

Concentrate your efforts on those where the attacker can cause real damage.

To help you guide your thinking about what kinds of threats deserve mitigation, here are some rules of thumb that you can use while performing your threat modeling.

1. If the data hasn’t crossed a trust boundary, you don’t really care about it.

2. If the threat requires that the attacker is ALREADY running code on the client at your privilege level, you don’t really care about it.

3. If your code runs with any elevated privileges (even if your code runs in a restricted svchost instance) you need to be concerned.

4. If your code invalidates assumptions made by other entities, you need to be concerned.

5. If your code listens on the network, you need to be concerned.

6. If your code retrieves information from the internet, you need to be concerned.

7. If your code deals with data that came from a file, you need to be concerned (these last two are the inverses of rule #1).

8. If your code is marked as safe for scripting or safe for initialization, you need to be REALLY concerned.

For more information on the rules, see https://docs.microsoft.com/en-us/archive/blogs/larryosterman/threat-modeling-again-threat-modeling-rules-of-thumb
