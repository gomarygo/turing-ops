# turing-ops

튜링 사내 운영 자동화. Slack·Notion·Google 커넥터를 쓰는 봇들의 **스킬과 운영 문서**를 모아둔 곳이다.

코드를 배포하는 저장소가 아니다. 여기 있는 `SKILL.md`는 claude.ai Routine이 예약 시각에 새 세션을 띄워 읽고 그대로 수행하는 절차서다. 절차를 고치려면 이 저장소의 파일을 고치면 되고, 변경 이력이 남는다.

## 봇 목록

| 봇 | 스킬 | 하는 일 | 문서 |
| --- | --- | --- | --- |
| 📣 확성기봇 | `amplifier-collect`, `amplifier-weekly` | 📣 찍힌 슬랙 메시지를 모아 매주 금요일 「튜링 위클리」 발행 | [docs/amplifier-bot.md](docs/amplifier-bot.md) |

## 동작 구조

```
claude.ai Routine (예약)
  → 새 세션 생성 (이 저장소를 소스로 체크아웃)
  → .claude/skills/<봇>/SKILL.md 를 읽고 수행
  → Slack / Notion 커넥터로 실제 작업
```

## 새 Routine을 만들 때 반드시 확인할 것

Routine 설정에서 **두 가지가 별개**다. 하나만 하면 세션은 "성공"으로 끝나면서 아무 일도 하지 않는다.

1. **커넥터** — 필요한 커넥터(Slack, Notion 등)를 명시적으로 연결
2. **저장소** — 소스에 `gomarygo/turing-ops` 추가 (없으면 SKILL.md 자체를 못 읽는다)

세션에서 `create_trigger`로 Routine을 만들면 둘 다 비어 있는 채로 생성되므로, claude.ai의 Routine 편집 화면에서 직접 붙여야 한다.

## 원칙

- **사람에게 보이는 발행물은 사람이 보낸다.** 봇은 초안까지만 만들고, 전송 버튼은 사람이 누른다.
- **자동 실행 중에는 쓰기를 최소화한다.** 사람이 없는 시간에 도는 작업은 읽기와 내부 기록까지만 한다.
- **없는 숫자를 지어내지 않는다.** 결과가 0이면 0이라고 보고한다.
