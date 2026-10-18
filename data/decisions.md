# 결정 대기

루프가 스스로 풀지 못한 것을 여기로 넘긴다. **사람은 이 파일과 `list.md` 만 고친다.**

## 어떻게 답하나
1. 아래 항목의 「사람이 답할 것」에 답을 적는다
2. `list.md` 에서 그 묶음의 `[>]` 를 `[ ]` 로 되돌린다
3. 묶음 제목 밑에 `- 결정: …` 을 적는다 — 다음 회차의 세션이 읽는다
4. **답한 항목은 `archive/decisions-done.md` 로 내린다** — 여기는 **아직 답하지 않은 것만** 둔다.
   안 치우면 답한 것도 대시보드의 「결정 대기」에 계속 남아 몇 건인지 알 수 없다 (2026-09-24)

넘어온 묶음의 **통과한 단계는 브랜치(`g/<ID>`)에 그대로 남아 있다.** 이어서 하면 된다.

---

## 물음 — 차선 3d-start(1fd2b079) 을 3d-lane2 에 합치지 못했다
- 물은 때: 2026-10-18 16:50:01
- 어디서: 차선 합치기 (3d-lane2)
- 무엇: 부딪힌다: spec/01_settings/save.md — 이 줄기(3d-lane2)에서 `git merge 3d-start` 을 손으로 풀어 커밋해 달라. 풀릴 때까지 이 차선은 3d-start 없이 돈다
- **사람이 답할 것**: 

## 물음 — 차선 3d-start(f0275121) 을 3d-lane2 에 합치지 못했다
- 물은 때: 2026-10-19 00:34:05
- 어디서: 차선 합치기 (3d-lane2)
- 무엇: 부딪힌다: game/craft/recipes/workbench.json game/i18n/en.po game/i18n/ko.po — 이 줄기(3d-lane2)에서 `git merge 3d-start` 을 손으로 풀어 커밋해 달라. 풀릴 때까지 이 차선은 3d-start 없이 돈다
- **사람이 답할 것**: 

## 물음 — 차선 3d-lane2(8d63249b) 을 3d-start 에 합치지 못했다
- 물은 때: 2026-10-19 01:57:29
- 어디서: 차선 합치기 (3d-start)
- 무엇: 부딪힌다: game/core/debug/ui_shots.gd game/craft/recipes/workbench.json game/craft/stations/crafting_station.gd game/craft/stations/stations.gd game/craft/stations/stations.json — 이 줄기(3d-start)에서 `git merge 3d-lane2` 을 손으로 풀어 커밋해 달라. 풀릴 때까지 이 차선은 3d-lane2 없이 돈다
- **사람이 답할 것**: 

## 물음 — 차선 3d-start(b24522ae) 을 3d-lane2 에 합치지 못했다
- 물은 때: 2026-10-19 02:17:56
- 어디서: 차선 합치기 (3d-lane2)
- 무엇: 부딪힌다: game/core/debug/ui_shots.gd game/craft/recipes/workbench.json game/craft/stations/crafting_station.gd game/craft/stations/stations.gd game/craft/stations/stations.json — 이 줄기(3d-lane2)에서 `git merge 3d-start` 을 손으로 풀어 커밋해 달라. 풀릴 때까지 이 차선은 3d-start 없이 돈다
- **사람이 답할 것**: 

## 물음 — 평타 · 캐기에 휘두르기 간격이 없다
- 물은 때: 2026-10-19 02:31:31
- 어디서: G-916 단계 3
- 무엇: 평타 · 캐기에 휘두르기 간격이 없다 — 이번 기준은 「평타 · 캐기는 도구 휘두르기 빠르기를 따른다」고 했지만 Attack.swing/strike · Harvest.swing 에는 쿨다운이 없고 test_fire_rate.gd 는 총(Magazine.fire_delay)만 본다. 연타하면 평타 · 캐기도 프레임마다 나간다. 사람 휘두르기에 간격을 걸지 · 도구마다 다르게 할지 · 몇 초로 할지 spec 에 규칙이 없다. 벌집은 임시로 HarvestWork.HIT_EVERY 0.6초를 빌렸으니 정해지면 WildHives.SWING_EVERY 도 그 값으로 바꿔야 한다
- **사람이 답할 것**: 

## 물음 — 차선 3d-lane2(9b236733) 을 3d-start 에 합치지 못했다
- 물은 때: 2026-10-19 03:25:23
- 어디서: 차선 합치기 (3d-start)
- 무엇: 부딪힌다: game/core/debug/ui_shots.gd game/craft/recipes/workbench.json game/craft/stations/crafting_station.gd game/craft/stations/stations.gd game/craft/stations/stations.json — 이 줄기(3d-start)에서 `git merge 3d-lane2` 을 손으로 풀어 커밋해 달라. 풀릴 때까지 이 차선은 3d-lane2 없이 돈다
- **사람이 답할 것**: 
