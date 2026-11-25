# API keys, secrets and Google Drive access

LLMFA needs two kinds of access: permission to use your Google Drive, and (for the research stage) a few API keys. This page explains both and how to keep your keys safe.

## 1. Google Drive access

After the code starts running in Google Colab, it asks to connect to your Google Drive. The setup cell (Cell 1) triggers this, so watch for the pop-up as soon as you run it.

1. A pop-up asks whether to let the notebook access your Google Drive. Click **Connect to Google Drive**.
2. Choose your Google account.
3. Review the permissions and click **Allow**.

Without this, the notebook cannot read your FY reports or save your results. The notebook only uses the folder you set as `BASE_DIR` (default `MyDrive/LLMFA/Model_1`): it reads your reports from `FY_Reports/` and writes its outputs there. See [DYNAMIC_DATASET.md](DYNAMIC_DATASET.md).

If you decline, the run stops at the first cell. Run it again and click Allow.

## 2. Which keys you need

| Key | Used for | Needed when |
|-----|----------|-------------|
| `GEMINI_API_KEY` | Gemini writes the answers to the 6 live research questions | Research stage only (Round 2) |
| `TAVILY_API_KEY` | Tavily searches the live web for those answers | Research stage only (Round 2) |
| `HF_TOKEN` | Hugging Face token, used when downloading the Llama model | Optional |

Round 1 (the 22 questions, 7 charts and live stock charts) does not need the Gemini or Tavily keys. You can skip them and add them later when you reach the research stage.

Create the keys on each service's own website: Google AI Studio for Gemini, tavily.com for Tavily, and your Hugging Face account settings (Access Tokens) for `HF_TOKEN`. Each service decides its own free limits and prices, so check them there.

## 3. Keeping your keys secret (Colab Secrets)

Never paste a key into a notebook cell, a file, or a GitHub commit. Use Colab Secrets:

1. In Colab, click the **key icon** in the left sidebar (Secrets).
2. Click **Add new secret**.
3. Enter the name exactly: `GEMINI_API_KEY`, `TAVILY_API_KEY` or `HF_TOKEN`.
4. Paste the key as the value.
5. Turn on **Notebook access** for that secret.

Colab stores the secret in your Google account, outside the notebook, so it is not saved in the `.ipynb` file or shared with anyone who opens a copy.

The notebook looks for each key in this order:
1. Colab Secrets.
2. An environment variable of the same name (useful when running locally).
3. A hidden prompt. If neither of the first two is set, the notebook asks you to type the key. The input is hidden, and you can leave it blank to skip.

## 4. Good habits

- Do not commit keys. The repository's `.gitignore` blocks files such as `.env`, `*.key` and `secrets*`, but it cannot protect a key you paste into a cell.
- Keep one key per service and use only the access you need.
- If a key is ever exposed (for example pasted into a shared notebook or a screenshot), delete it on the service's website and create a new one.
- Check screenshots before sharing: they can show keys typed into the prompt.
