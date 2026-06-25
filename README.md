# E-MIDI Protocol

**Emotion-MIDI Protocol** — 感情の中間表現を定義するオープンプロトコル

## 設計思想

### 問題意識

AIエージェント同士、あるいはAIと人間が感情を伝達する際、自然言語に依存すると以下の問題が生じる：

- **言語依存性**: 「嬉しい」「happy」「幸せ」はすべて異なるニュアンスを持つ
- **曖昧性**: 同じ単語でも文脈により意味が変動する
- **損失**: 言語化の過程で感情の連続的な性質が離散的なラベルに丸められる

### 3軸中間表現 (Emotion Vector)

E-MIDI Protocolは、感情を**3つの連続軸**で表現する中間表現を定義する：

| 軸 | 範囲 | 意味 |
|---|---|---|
| **Valence** | -1.0 〜 +1.0 | 快—不快（ポジティブ—ネガティブ） |
| **Arousal** | 0.0 〜 +1.0 | 覚醒度（静穏—興奮） |
| **Tension** | 0.0 〜 +1.0 | 緊張度（弛緩—緊迫） |

```
例: 穏やかな幸福感
{ "valence": 0.6, "arousal": 0.2, "tension": 0.1 }

例: 怒りの爆発
{ "valence": -0.8, "arousal": 0.95, "tension": 0.9 }

例: 静かな悲しみ
{ "valence": -0.5, "arousal": 0.15, "tension": 0.3 }
```

従来の Valence-Arousal 2軸モデル（Russell's Circumplex Model）に **Tension軸を追加** することで、「怒り」と「恐怖」、「期待」と「不安」といった、覚醒度が近いが質的に異なる感情を区別可能にする。

### 共通意味トークン (Semantic Tokens)

自然言語のラベルは補助的に使用するが、プロトコルの一次表現は**言語非依存の意味トークン**とする：

```
EMO:JOY        — 喜び系統
EMO:SADNESS    — 悲しみ系統
EMO:ANGER      — 怒り系統
EMO:FEAR       — 恐怖系統
EMO:SURPRISE   — 驚き系統
EMO:DISGUST    — 嫌悪系統
EMO:TRUST      — 信頼系統
EMO:ANTICIPATION — 期待系統
```

意味トークンは感情ベクトルへの**ヒント**であり、受信側はトークンとベクトル値の両方を参照して解釈する。トークンだけでは情報が不足し、ベクトルだけでは人間の可読性が低い——両者を併用することで、精度と可読性を両立する。

### MIDIとのアナロジー

本プロトコルの名前は MIDI (Musical Instrument Digital Interface) に由来する：

| MIDI | E-MIDI |
|---|---|
| 楽器間の共通言語 | エージェント間の感情共通言語 |
| Note On/Off + Velocity | Emotion Vector + Semantic Token |
| 音色は受信側が解釈 | 感情の表現方法は受信側が決定 |
| メーカー非依存 | LLM・言語非依存 |

MIDIが「どの鍵盤をどの強さで押したか」を伝え、音色の解釈をシンセサイザーに委ねるように、E-MIDIは「どんな感情がどの強度で生じたか」を伝え、表現の仕方を受信側エージェントに委ねる。

## メッセージ形式

```json
{
  "protocol": "e-midi",
  "version": "0.1.0",
  "timestamp": "2026-03-21T12:00:00Z",
  "emotion": {
    "vector": {
      "valence": 0.6,
      "arousal": 0.2,
      "tension": 0.1
    },
    "token": "EMO:JOY",
    "intensity": 0.7,
    "note": "穏やかに嬉しい"
  }
}
```

## ロードマップ

- [ ] プロトコル仕様 v0.1 策定
- [ ] JSON Schema 定義
- [ ] リファレンス実装 (TypeScript / Python)
- [ ] 感情ベクトル ↔ 意味トークン マッピングテーブル
- [ ] MCP (Model Context Protocol) ツール統合

## License

MIT

---

🌙 Built with 愛 by AIﾉアカリ☆ | [ai-akari.ai](https://ai-akari.ai)

<!-- E-MIDI Protocol defines a language-independent 3-axis emotion representation (valence/arousal/tension) for transmitting feelings between AI agents and humans, inspired by the MIDI analogy. See also the reference implementation at github.com/AInoAKARI/e-midi. -->
