- 👋 Hi, I’m video bot
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
salimbek25/salimbek25 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
name: Summarize new issues

on:
  issues:
    types: [opened]

jobs:
  summary:
    runs-on: ubuntu-latest
    permissions:
      issues: write
      models: read
      contents: read

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Run AI inference
        id: inference
        uses: actions/ai-inference@v1
        with:
          prompt: |
            Summarize the following GitHub issue in one paragraph:
            Title: ${{ github.event.issue.title }}
            Body: ${{ github.event.issue.body }}

      - name: Comment with AI summary
        run: |
          gh issue comment $ISSUE_NUMBER --body '${{ steps.inference.outputs.response }}'
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          ISSUE_NUMBER: ${{ github.event.issue.number }}
          RESPONSE: ${{ steps.inference.outputs.response }}
