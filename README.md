# Ultrashort: Output Token Compression Skill

A Claude Code skill that reduces AI response length by ~80% by stripping filler without losing technical accuracy.

Output tokens cost 4–8× more than input tokens. Most AI responses are 60–90% preamble, restatement, transitions, and trailing summaries. This skill removes all of it.

## Usage

Add `SKILL.md` to your Claude Code skills folder. Then say `ultrashort`, `compress`, or `/ultrashort` to activate.

## Benchmark

Tested across 240 API calls: 6 models × 5 question types × 8 styles.

**Output token reduction vs baseline:**

| Style      | Avg reduction |
|------------|--------------|
| **ultrashort** | **−80.0%** |
| minimal    | −70.6%       |
| caveman    | −68.3%       |
| structured | −65.1%       |
| bullets    | −64.4%       |
| telegraph  | −53.6%       |

**Per model:**

| Model          | Best style  | Reduction |
|----------------|-------------|-----------|
| llama-3.3-70b  | caveman     | −94.3%    |
| gemma4-26b     | ultrashort  | −92.7%    |
| gpt-oss-120b   | ultrashort  | −85.9%    |
| qwen3-8b       | ultrashort  | −80.3%    |
| gpt-oss-20b    | minimal     | −76.9%    |
| qwen3.6-35b    | ultrashort  | −57.3%    |

**Per question type:**

| Question    | Best style  | Reduction |
|-------------|-------------|-----------|
| debug_task  | ultrashort  | −87%      |
| design_q    | ultrashort  | −83%      |
| sql_explain | ultrashort  | −80%      |
| jwt_auth    | caveman     | −67%      |
| simple_code | minimal     | −67%      |

## Example

**Baseline - jwt_auth, qwen3-8b (3078 tokens):**
> "JWT (JSON Web Token) is a compact, URL-safe means of representing claims to be transferred between two parties. It consists of three parts: a Header, a Payload, and a Signature. When a user logs in, the server creates a token signed with a secret key..."

**Ultrashort mode (431 tokens, −86%):**
> Server signs a token on login; client sends it with each request; server verifies signature. No DB lookup needed.
> ```python
> import jwt
> token = jwt.encode({"user_id": 1}, "secret", algorithm="HS256")
> decoded = jwt.decode(token, "secret", algorithms=["HS256"])
> ```

## Notes

- Thinking models (e.g. qwen3.6-35b) are harder to compress: internal reasoning tokens are not reduced by style instructions; expect ~57% savings
- `caveman` outperforms `minimal` on llama-3.3-70b specifically (−94.3%); consider using caveman if that is your primary model
- For code-heavy sessions, use `/ultrashort micro` level: code block only, no prose

## Files

- [`SKILL.md`](skills/ultrashort/SKILL.md): the skill instruction file for Claude Code
- [`output_style_test.ipynb`](output_style_test.ipynb): full benchmark notebook (OpenRouter, requires `.env` with `OPENROUTER_API_KEY`)
- [`output_style_results.csv`](output_style_results.csv): raw results
