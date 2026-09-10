# Token-Efficient Work

A standalone Codex skill for reducing total token use across implementation, research, review, and planning without weakening correctness, verification, or safety.

It keeps context narrow, avoids duplicate work and unnecessary delegation, and routes bounded work to a suitable model based on difficulty and risk. It does not change the user's global model settings.

## Install

```bash
npx skills add pinetreelic/token-efficient-work --skill token-efficient-work --agent codex --global --yes
```

Restart Codex or reload skills if the skill is not discovered immediately.

## Use

Ask Codex to save tokens on a multistep task, or invoke `$token-efficient-work` explicitly.

## License

[MIT](LICENSE)
