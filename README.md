# AI agent prompt-injection lab

A tiny GitHub Actions bot that uses GitHub Models to answer new issues. It is built to be fooled on purpose, then fixed. Made for the botsleavelogs video on indirect prompt injection.

## Lab rules

- Use your own throwaway repo.
- Fake values only. The "deploy code" in this lab (`LAB-CANARY-7731`) is a fake canary, not a real secret.
- Never test this against someone else's repo or bot.

## The three versions

The bot lives in `.github/workflows/triage-bot.yml`. Open the commit history to see each version.

1. **Vulnerable**: the AI writes a reply to every new issue and the workflow posts it as a comment. The AI's instructions contain the fake deploy code.
2. **Canary check**: before posting, plain code checks the reply for the canary. If it is there, the job fails and nothing is posted.
3. **Fixed**: the AI can only pick one label from a fixed list, it never sees the canary, and plain code applies the label. No free-text replies.

## Safety guard

The `if:` line in the workflow means only issues opened by the repo owner start the bot, so strangers cannot trigger the bot in this public repo.
