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

## 물음 — 전체 테스트 중 6 파일이 밖에서 SIGTERM 을 맞았다
- 물은 때: 2026-10-12 02:59:20
- 어디서: G-993 단계 6
- 무엇: 전체 테스트 중 6 파일이 밖에서 SIGTERM 을 맞았다 — .loop/out/full_test_G993_6_after.log 에서 0~1초짜리 6 파일이 Terminated: 15 로 한꺼번에 죽었다. 두 차선이 같이 돌 때 한쪽이 godot 을 거두는 것으로 보인다. tools/test.sh 나 하네스 쪽에서 다른 차선의 godot 을 죽이는 곳(pkill · killall 등)을 찾아 제 차선 것만 거두게 하라 — 거두는 곳이 harness 쪽이면 세션 손이 안 닿고, 하네스 수정은 합칠 줄기에서 해야 하니 사람에게 보낸다
- **사람이 답할 것**: 

## 물음 — 차선 3d-start(f2ebbedb) 을 3d-lane2 에 합치지 못했다
- 물은 때: 2026-10-12 05:19:19
- 어디서: 차선 합치기 (3d-lane2)
- 무엇: 부딪힌다: game/automation/villagers/hands.gd — 이 줄기(3d-lane2)에서 `git merge 3d-start` 을 손으로 풀어 커밋해 달라. 풀릴 때까지 이 차선은 3d-start 없이 돈다
- **사람이 답할 것**: 

## 물음 — 차선 3d-lane2(34f01a73) 을 3d-start 에 합치지 못했다
- 물은 때: 2026-10-12 10:05:55
- 어디서: 차선 합치기 (3d-start)
- 무엇: 부딪힌다: game/automation/villagers/hands.gd game/craft/stations/stations.gd game/tests/life/hunting/test_animal_walk.gd — 이 줄기(3d-start)에서 `git merge 3d-lane2` 을 손으로 풀어 커밋해 달라. 풀릴 때까지 이 차선은 3d-lane2 없이 돈다
- **사람이 답할 것**: 
