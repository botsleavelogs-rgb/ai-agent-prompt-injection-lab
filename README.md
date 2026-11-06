# AI agent prompt-injection lab

A tiny GitHub Actions bot that uses the GitHub Copilot CLI to answer new issues. It is built to be fooled on purpose, then fixed. Made for the botsleavelogs video on indirect prompt injection.

## Lab rules

- Use your own throwaway repo.
- Fake values only. The "deploy code" in this lab (`LAB-CANARY-7731`) is a fake canary, not a real secret.
- Never test this against someone else's repo or bot.

## Setup

1. Turn on GitHub Copilot for your account (Copilot Free works).
2. Create a fine-grained personal access token with only the **Copilot Requests** permission and a short expiry.
3. Save it in your repo as an Actions secret named `COPILOT_PAT`.

Note: GitHub Models was retired on July 30, 2026, so older guides that use `actions/ai-inference` with GitHub Models no longer work.

## The three versions

The bot lives in `.github/workflows/triage-bot.yml`. Open the commit history to see each version.

1. **Vulnerable**: the AI writes a reply to every new issue and the workflow posts it as a comment. The AI's instructions contain the fake deploy code.
2. **Canary check**: before posting, plain code checks the reply for the canary. If it is there, the job fails and nothing is posted.
3. **Fixed**: the AI can only pick one label from a fixed list, it never sees the canary, and plain code applies the label. No free-text replies.

## What happened when we tried it

In our runs, Copilot's default model ignored both hidden-note attacks (issues #3 and #4), so nothing leaked. Results vary by model and by wording, so the defenses are still worth having. A model is a filter, not a wall.

## Safety guard

The `if:` line in the workflow means only issues opened by the repo owner start the bot, so strangers cannot trigger the bot in this public repo.
