# OpenAI portal: what Vittorio still has to provide

Layout the portal reads (per the "Upload and submit" and "Package your plugin" pages, checked 09/10/2026): one ZIP whose root holds `plugin.json` (Agent Plugins format), `mcp.json` (one remote server, `type: streamable-http`), `skills/<name>/SKILL.md` and `assets/`. No `.app.json`, no hooks. The ZIP is `dist/zei231-skills-openai.zip`, built from `openai/zei231-registro/`. A `.codex-plugin/plugin.json` is not needed: the root `plugin.json` carries the `extensions.com.openai` block.

The path is "With MCP": the MCP server must be in the first ZIP. It cannot be added later to a skills-only plugin.

| # | Item | State |
|---|---|---|
| 1 | OpenAI platform account, organisation owner or Apps Management Write | Vittorio |
| 2 | Individual or business verification (developer identity) | Vittorio |
| 3 | `assets/logo.png` and `assets/icon.png` | done (512 px, from icon.svg) |
| 4 | Domain token: `https://zei.services/.well-known/openai-apps-challenge` returned 307 to /accedi on 09/10/2026. Allow the path in the proxy | repo change, needs approval |
| 5 | Demo video URL (`review.demo_recording_url`), reviewer-accessible | missing (TK) |
| 6 | Reviewer account in the secure dashboard form: no MFA, no code, no magic link. Not in the ZIP | Vittorio |
| 7 | Run the 8 test cases once in ChatGPT before submit | Vittorio |
| 8 | Live pages: privacy, terms, support URLs return 200 | check before submit |
| 9 | Choose the country list (empty = no restriction) | decision |
| 10 | `shortDescription` limit is 30 characters: "Modello 231 dalla chat" (22) | done |
| 11 | Press: write to press@openai.com before any press release | later |

Open points: the doc does not say whether the ZIP root is the plugin folder or its parent; the ZIP uses the plugin folder content at the root, as in the examples. If the portal rejects it, zip the folder `zei231-registro/` itself.
