# 할 일 목록

사람과 Claude 가 대화로 채운다. 루프가 위에서부터 **순서대로** 처리하고 체크한다.
루프 세션은 이 파일을 읽지 않는다 — 루프가 지금 할 단계만 차선마다 제 `current.md` 로 뽑아 준다.

**이 목록은 하나다** — 코드 브랜치가 아니라 `plan` 브랜치(본 저장소의 `plan/` 폴더)에 있다.
루프가 몇 개 돌든 모두 여기를 읽고 쓴다. 체크 · 결정 대기 · 차선 표시가 그 자리에서 바로 적히고 커밋된다.

## 형식

- `## [ ] ID 제목` — 묶음. 한 묶음 = 코드 브랜치 하나. 단계가 전부 통과해야 루프를 시작한 줄기(`3d-start` · `3d-lane2` …)에 합친다
- `- [ ] 1. 단계 제목` — 단계. 한 단계 = 루프 한 회차
- `  - 기준: ...` — 수용 기준 (QA 채점표). 단계 밑에 들여 쓴다
- `  - ...` — 기준이 아닌 참고
- 묶음 제목 바로 밑 `- spec: ...` · `- 결정: ...` — 묶음 전체에 붙는 정보
- 표시: `[ ]` 할 일 · `[x]` 끝남 · `[>]` 결정 대기로 넘어감 (`decisions.md`)
- 묶음 머리 밑 `- 차선: 3d-lane2` — 그 묶음을 맡은 차선(루프를 띄운 줄기 이름). 적혀 있으면 그 차선만 한다.
  없으면 처음 집는 차선이 적는다. **같은 파일을 고치는 묶음은 같은 차선에 둔다** — 다르면 줄기를 합칠 때 부딪힌다
  결정 대기 `[>]` 를 풀 때 차선 줄은 그대로 두면 그 차선이 이어 한다. 다른 차선에 맡기려면 이 줄을 고치거나 지운다
  (그 차선이 꺼져 있으면 아무도 안 집는다)
- 결정이 나면 `[>]` 를 `[ ]` 로 되돌리고 `- 결정: ...` 을 적는다. 브랜치에 남은 작업에서 이어간다

## [x] 끝난 묶음 — 단계와 기준은 archive/list-3d-done.md
- G-101 프로젝트 기반 · G-104 시험 섬 · G-102 메인 화면 · 캐릭터 · 월드 · G-103 3인칭 캐릭터 · 카메라 · 핫바 · ESC 설 · G-105 자원 · 도구 · 채집 · G-106 제작 · G-107 농사 · G-108 하루 20분 · 버프 음식 · G-109 섬에 들어가기 · G-110 전투 뼈대 · G-111 등반 · G-112 약초 · G-113 개척 섬 · G-114 막힌 진행을 푼다 · G-115 HUD · 조작 · G-117 진행이 또 끊겼다 · G-119 설치물 부수기 · G-120 보관 상자 · G-118 총

## [x] G-121 제작 창 — 게임 안에서 만들 수 있게
- spec: spec/05_craft/crafting-stations.md, recipes.md
- **레시피 22종이 다 있는데 게임에서 만들 길이 없다** (사람이 확인 2026-09-22).
  `station_window.gd` 가 제작대 이름만 띄운다 — 「레시피 · 타이머 · 출력 버퍼는 다음 단계에」라고
  적혀 있는데 **그 단계가 없었다**. `start_craft` · `collect` 를 부르는 곳이 테스트뿐이다
- [x] 1. 레시피를 골라 만든다
  - 기준: 제작대를 열면 **그 제작대의 레시피 목록**이 뜬다. 재료와 나오는 것이 보인다 (테스트)
  - 기준: 재료가 모자란 레시피는 고를 수 없다고 보인다 (테스트)
  - 기준: 고르면 재료가 빠지고 타이머가 돈다. 이미 돌고 있으면 새로 시작하지 않는다 (테스트)
  - 기준: 글자는 전부 번역 키다 (ko · en 둘 다)
- [x] 2. 남은 시간과 출력 버퍼
  - 기준: 만드는 중에 **남은 시간**이 보인다 (막대든 숫자든)
  - 기준: 끝나면 결과물이 버퍼 칸에 보인다. 눌러서 가방으로 가져온다 (테스트)
  - 기준: 버퍼가 가득 차면 다음 제작이 시작되지 않는다고 보인다 (테스트)
  - 기준: 창을 닫았다 다시 열어도 타이머와 버퍼가 그대로다 (테스트)

## [x] G-122 갈고리총 — 벽에만 걸린다 · 탄이 보인다
- spec: spec/04_life/climbing.md, spec/12_ui/hud.md
- 사람이 플레이해 보고 짚었다 (2026-09-22) — **허공에 쏴도 걸린다**.
  `climbing.gd:163` 이 아무것도 안 맞으면 최대 거리 지점을 그냥 걸어 버리고, 맞아도 기울기를 안 본다
- 결정: **갈고리 탄은 총알과 다른 것이다** (2026-09-22). 둘 다 제작대에서 만든다
- [x] 1. 벽에만 걸린다
  - 기준: 허공에 쏘면 걸리지 않는다 — 줄이 헛나가고 매달리지 않는다 (테스트)
  - 기준: 걸어 오를 수 있는 비탈 · 평지에도 걸리지 않는다. 절벽 벽면에만 걸린다 (테스트)
  - 기준: 헛나가도 탄은 준다 (허공에 마구 쏘는 것이 공짜가 아니게)
- [x] 2. 갈고리 탄과 탄약 표시
  - 기준: 「갈고리 탄」을 제작대에서 만든다. **총알로는 갈고리총이 채워지지 않는다** (테스트)
  - 기준: 갈고리총을 들면 남은 탄/탄창이 총과 같은 자리에 보인다. 다른 것을 들면 사라진다
  - 기준: R 로 재장전하고, 인벤토리의 갈고리 탄이 준다. 없으면 재장전되지 않는다 (테스트)

## [x] G-123 저장에서 빠진 것 — 부순 것 · 데스 상자
- spec: spec/01_settings/save.md, spec/08_combat/damage-death.md
- 결정: **밭 · 횃불은 뺀다** (사람 결정 2026-09-22) — 농사는 사람이 다시 설계 중이고(farming.md:7),
  횃불은 놓는 길 자체가 없다(lighting.md 미정). 저장은 이미 섬별로 된다 (world/frontier_islands/island_state.gd)
- [x] 1. 부순 것이 되살아나지 않는다
  - 기준: 부순 제작대 · 상자 · 선착장이 나갔다 들어와도 되살아나지 않는다 (테스트)
  - 기준: `game/settings/save/world_data.gd:6` 의 낡은 주석을 사실대로 고친다 —
    설치물 · 상자(안의 것까지) · 선착장 · 밝힌 지도는 이미 담긴다
- [x] 2. 데스 상자
  - 기준: 데스 상자가 남은 시간과 함께 저장되고, 불러온 뒤 이어서 줄어든다 (테스트)
  - 기준: 30분이 다 된 데스 상자는 불러올 때 상자와 안의 것이 같이 사라진다 (테스트)

## [x] G-124 총 네 계통 — 탄을 들고 다닌다 · 계통마다 네 단
- spec: spec/08_combat/tools-as-weapons.md, spec/05_craft/recipes.md
- 결정: **가방에 드는 것은 탄이고, 탄창은 총마다 따로 붙는다** (사람 결정 2026-09-22).
  권총 · 산탄총 · 돌격소총 · 저격총 네 계통, 계통마다 네 단 — 표는 spec 에 있다
- 이름은 나중에 바꿀 수 있다 (번역 키) — 지금은 표대로 박는다
- [x] 1. 탄창을 총에 붙인다
  - 기준: 총 A 로 쏜 뒤 총 B 를 들면 B 의 남은 탄이 그대로다 (테스트)
  - 기준: 총을 내려놓았다 다시 들어도 남은 탄이 그대로다 (테스트)
  - 기준: 들어가는 탄 수가 총마다 다르고, 값은 한 곳에 있다 (테스트)
- [x] 2. 탄 네 종 — 제 탄으로만 채워진다
  - 기준: 권총탄 · 산탄 · 돌격소총탄 · 저격탄을 제작대에서 만든다 (테스트)
  - 기준: 총은 제 계통의 탄으로만 재장전된다. 다른 탄으로는 채워지지 않는다 (테스트)
  - 기준: 갈고리총은 갈고리 탄만 쓴다 — 총알로는 채워지지 않는다 (테스트)
  - 기준: 글자는 전부 번역 키다 (ko · en 둘 다)
- [x] 3. 열여섯 자루와 사다리
  - 기준: 계통마다 네 단이 제작 사다리에 있다 — 윗단일수록 재료가 비싸다 (테스트)
  - 기준: 단이 오를수록 재장전에 손이 덜 간다 — 들어가는 탄 수 · 재장전 시간이 단마다 다르다 (테스트)
  - 기준: HUD 의 남은 탄 표시가 **든 총에 맞게** 바뀐다
  - 기준: 총알 레시피의 재료 · 개수가 spec 표와 같다 (사람 결정 2026-09-22) —
    단계 2 가 지어낸 값(권총탄 30 · 산탄 16 …)을 표대로 고친다 (테스트)
  - 3단 재료는 임시다 — 개척 섬 금속이 정해지면 바꾼다 (spec 미정)

## [x] G-125 선착장 — 기둥을 겨눈다 · 개척 섬은 부서진 채로 시작한다
- spec: spec/03_world/frontier-islands.md, spec/06_build/building.md
- 결정: 기둥을 겨눠 부순다 · 개척 섬 선착장은 부서진 채로 시작하고 **연구대 해금 아이템**으로 연다
  (사람 결정 2026-09-22 — 처음부터 배로 오가는 것을 막는다)
- [x] 1. 기둥을 겨눈다
  - 기준: 내가 지은 선착장은 물가 쪽 기둥을 겨눠 부순다. 부수면 선착장이 그대로 돌아온다 (테스트)
  - 기준: 널판에는 몸통이 없다 — 배에서 내린 캐릭터가 끼이지 않는다 (테스트)
  - 기준: 맵에 박힌 선착장은 부술 수 없다 (테스트)
- [x] 2. 개척 섬은 부서진 채로 시작한다
  - 기준: 새 월드에서 개척 섬 선착장은 기둥만 서 있다 (테스트)
  - 기준: 잠긴 섬으로는 배가 가지 않는다 — 내 섬 선착장에서 타도 안 간다 (테스트)
  - 기준: 글자는 전부 번역 키다 (ko · en 둘 다)
  - 기준: (앞 단계 QA) 커밋 안 된 .uid 하나 — game/core/debug/ui_shots.gd.uid 가 추적되지 않은 채 남아 있다 (이번 단계가 아니라 앞 커밋 12f4894 이 빠뜨렸다). 같은 폴더의 다른 .gd.uid 는 전부 추적되고 있어 이것만 빠졌다 — Godot 이 다시 만들면 uid 가 달라진다. git add game/core/debug/ui_shots.gd.uid 로 다음 커밋에 넣어라
- [x] 3. 연구대 열쇠로 연다
  - 기준: 해금 아이템은 **연구대에서만** 만들어진다 — 다른 제작대에서는 나오지 않는다 (테스트)
  - 기준: 그것을 선착장에 넣으면 널판이 깔리고 배가 간다 (테스트)
  - 기준: 열린 것이 저장본에 남는다 — 나갔다 들어와도 열려 있다 (테스트)
  - 해금 아이템의 재료는 임시다 (spec 미정)
  - 기준: (앞 단계 QA) 잠긴 섬으로 타려 해도 아무 말이 없다 — 내 섬 선착장은 성해 보이는데(널판이 깔려 있다) 우클릭하면 GameRoot.sail_to 가 조용히 return 만 한다 (game_root.gd:164). 소리도 글자도 없어 「고장났나」로 읽힌다. 다음 단계(G-125.3 연구대 열쇠로 연다)에서 같이 갚아라: sail_to 가 island_open 에 걸려 돌아설 때 HUD 에 한 줄 띄우고(번역 키 ko · en 둘 다 — 예 DOCK_ISLAND_LOCKED), tests/world/frontier_islands/test_locked_island.gd 에 「잠긴 섬으로 우클릭하면 그 글자가 뜬다 · 열린 섬으로 타면 안 뜬다」를 견주는 단언을 더해라

## [x] G-900 루프가 찾은 것 — 스스로 갚는다
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. test_enter_game.gd:109 test_the_other_world_did_not_get_t… (G-102 단계 3)
  - 기준: test_enter_game.gd:109 test_the_other_world_did_not_get_the_items 는 늘 참인 테스트다 — WorldData.to_dict() 는 {name, seed, created_at} 리터럴을 돌려주므로 'inventory' in w.to_dict() 는 어떤 코드에서도 false 다. 기준 3 은 다른 테스트가 제대로 막고 있어 통과지만, 이 테스트는 아무것도 지키지 않는다. 인벤토리가 캐릭터에만 붙는다는 것을 정말 보려면 '월드 A 에서 넣고 저장한 뒤 다른 캐릭터로 같은 월드 A 에 들어가면 인벤토리가 비어 있다'를 확인하게 고쳐라
- [x] 2. 「비우면 무작위」를 UI 를 거쳐 확인하는 테스트가 없다 (G-102 단계 3)
  - 기준: 「비우면 무작위」를 UI 를 거쳐 확인하는 테스트가 없다. test_empty_seed_is_random 은 WorldCreate.seed_from('') 를 직접 부르고, test_empty_seed_still_saves_the_world 는 이름과 오류 표시만 볼 뿐 저장된 world_seed 를 보지 않는다. _on_create_pressed 가 w.world_seed 넣는 줄을 빠뜨려도 두 테스트 다 통과한다. test_empty_seed_still_saves_the_world 에 '시드 칸을 비우고 만든 월드 둘을 저장본에서 읽으면 world_seed 가 서로 다르다'를 더해라
- [x] 3. Player._ready() 가 조건 없이 Pointer.set_captured(true) 를 한다 (G-103 단계 5)
  - 기준: (기준은 통과했다 — 다음 단계에서 터질 것들이다) Player._ready() 가 조건 없이 Pointer.set_captured(true) 를 한다. 설정 창이 떠 있는 동안 플레이어가 씬에 들어오면 커서를 도로 뺏는다. 지금은 GameRoot 안에 플레이어가 없어 드러나지 않지만, 섬·플레이어를 GameRoot 에 붙이는 단계에서 「창이 떴는데 커서가 사라진다」로 나온다. 붙일 때 GameRoot 가 창 상태를 보고 커서를 정하게 하고, 그 경우를 보는 테스트를 같이 짜라.
- [x] 4. GameRoot.set_menu_open(false) 이 씬 안 모든 Player 의 controls_… (G-103 단계 5)
  - 기준: GameRoot.set_menu_open(false) 이 씬 안 모든 Player 의 controls_enabled 를 무조건 true 로 되돌린다. debug_tools.gd:65 도 같은 값을 쓰므로, 디버그 비행 카메라를 켠 채 ESC 를 열었다 닫으면 꺼 뒀던 조작이 되살아난다. 지금은 디버그 도구가 시험 섬에만 있어 부딪히지 않는다 — 두 곳이 한 씬에 모이는 단계에서 「누가 껐나」를 세는 방식(이유별 잠금)으로 바꾸고 테스트를 붙여라.
- [x] 5. 클릭 차단은 창이 실제로 삼키는 것을 확인하지 못했다 (G-103 단계 5)
  - 기준: 클릭 차단은 창이 실제로 삼키는 것을 확인하지 못했다. headless 에는 커서가 없어 Godot GUI 판정이 안 돌기 때문에, 지금 테스트는 「좌클릭의 유일한 효과인 커서 재잡기가 안 일어난다」로 대신 본다. G-105 에서 좌클릭이 평타가 되면 이 확인이 비어 버린다 — 평타를 넣을 때 「창이 떠 있으면 평타가 안 나간다」를 직접 보는 테스트를 같이 짜라.
- [x] 6. 막 색 Color(0,0,0,0.55) 가 pause_menu.tscn 안에 있어 「임시」 표시를 달… (G-103 단계 5)
  - 기준: 막 색 Color(0,0,0,0.55) 가 pause_menu.tscn 안에 있어 「임시」 표시를 달 수 없다. report.json 에만 적혀 있어 다음 사람이 보기 어렵다 — pause_menu.gd 머리말에 「막 색은 임시, 디자인은 사람이 나중에」 한 줄을 남겨라.
- [x] 7. bag.tscn 안의 수치 두 개에 「임시」 표시가 없다 (G-105 단계 2)
  - 기준: 막는 것은 아니다. bag.tscn 안의 수치 두 개에 「임시」 표시가 없다 — Dim 의 알파 0.55 와 Window CanvasLayer 의 layer = 5 다. report.json 의 temporary 에는 적혀 있지만 .tscn 을 여는 사람은 그걸 못 본다. bag.gd 머리글 주석에 덮개 색과 레이어 값이 임시라는 줄을 한 줄 보태라 (SLOT_SIZE 는 이미 잘 적혀 있다).
- [x] 8. 「임시」 표시를 붙인다 ① — 밭 · 작물 · 섬 설계 값
  - 기준: 절벽 기울기 45° (Farmland.MAX_SLOPE_DEG, game/life/farming/farmland.gd:28-30) 가 report.json 에는 임시 수치로 적혀 있는데 코드 주석에는 「임시」가 없다. REACH · 흙판 두께 · 설치물 한 칸 주석은 전부 「임시 — … 미정이다」 꼴로 적혀 있으니(stations.gd:24 · harvest.gd:31 과 같은 투) 같은 꼴로 고쳐라: spec/04_life/farming.md 에 절벽 수치가 없다는 것과, 걸어 오르지 못하는 기울기(IslandShape.is_step_walkable)에 맞춰 둔 값이라는 것을 주석에 적으면 된다. 값 자체는 바꿀 것 없다.
  - 기준: 「물은 하루치다 — 준 다음 날 다시 마른다」가 임시라고 코드에 적혀 있지 않다. spec/04_life/farming.md 미정 줄이 「물 마름 규칙 미정」이라고 적고 있고 report.json 도 임시로 꼽았는데, crops.gd:133~134 의 next_day() 주석은 그냥 규칙처럼 적혀 있다. 같은 파일의 count(31줄) · 크기와 색(37줄)처럼 「임시다 — 물 마름 규칙 미정 (spec/04_life/farming.md)」 한 줄을 next_day() 주석에 넣어라. 코드 동작은 고치지 않는다.
  - 기준: 막는 것은 아니다 — 유황 섬 설계 값 game/world/frontier_islands/sulfur_island_blueprint.tres 가 「회색 초안」이라는 표시가 파일 쪽 어디에도 없다. report.json 에만 적혀 있는데 report 는 다음 회차에 지워진다. islands.json 의 _temporary 줄에 「sulfur 설계 값은 전부 임시 — 섬 모양은 사람이 같이 빚는다 (list.md G-113)」를 한 줄 더 적어라. 다른 임시 수치(Dock.REACH 7m · PLANK · dock_angle_deg · 제작대가 만들던 것을 안 담는다)는 코드와 islands.json 에 「임시」로 적혀 있어 문제없다.
  - 기준: (막는 것 아님) 임시 표시가 분화구 값에만 붙어 있다. island_blueprint.gd 의 crater_* 에는 「임시」가 적혀 있지만 화산 몸통(mountain_height 170 · mountain_radius 200 · mountain_profile 1.0 · ridge_strength 0.35)과 둘레 절벽(cliff_step 32 · cliff_coverage 0.9 · path_angle_deg 20)은 어디에도 임시라고 적혀 있지 않다. spec/03_world/frontier-islands.md 의 「분화구 크기 · 깊이는 임시」를 「설계 값은 모두 임시 — 사람이 같이 다듬는다」로 넓혀라.
  - 기준: bag.gd 의 임시 표시가 SLOT_SIZE 설명에 붙었다 — current.md 는 「bag.gd 머리글 주석에 한 줄 보태라」고 했는데, 덮개 알파 · 레이어 세 줄이 bag.gd:19~22 의 `## 칸 크기. 임시 …` 와 `const SLOT_SIZE` 사이에 들어가 버려 GDScript 가 이것을 SLOT_SIZE 의 doc 주석으로 읽는다. 칸 크기 설명과 창 모양 설명이 한 덩어리가 되어 어긋난다. 세 줄을 위쪽 머리글 블록(bag.gd:17 의 `## 모양은 아직 …` 다음, 빈 줄 앞)으로 옮겨라. 내용 자체는 맞다 — layer = 5 가 bag.tscn 이 아니라 core/game_root.tscn:22~23 의 Window CanvasLayer 에 있다는 report 의 정정도 확인했다.
- [x] 9. 「임시」 표시를 붙인다 ② — 상자 · 제작 창 · 갈고리총 · spec 미정
  - 기준: Chests.REACH 에 「임시」 표시가 없다 — report.json 은 놓는 거리(4m)를 임시로 꼽았는데 chests.gd:19 주석은 「제작대를 놓는 거리와 같은 값으로 뒀다」까지만 적혀 있다. 값이 미정이라는 말이 코드에 없으면 다음 사람이 정해진 값으로 읽는다. 「임시 — 도달 거리 값은 미정이다」 한 마디를 붙여라. stations.gd:26 의 REACH 도 같은 처지다(Harvest.REACH 와 같게 뒀다는 말만 있다)
  - 기준: 막대 크기와 소수 자릿수에 「임시」 표시가 없다 — station_window.tscn:37 의 ProgressBar custom_minimum_size = Vector2(400, 16) 과 station_window.gd:183 의 "%.1f" (남은 초를 소수 첫째 자리까지) 가 사람이 나중에 정할 값인데 코드에는 「임시」가 한 줄도 없다. report.json 의 temporary 에만 적혀 있어 .tscn 을 여는 사람은 못 본다 — G-900 6·7번과 똑같은 자리다. station_window.gd 머리글의 ROW_SIZE·BUFFER_SLOT_SIZE 주석과 같은 투로 「남은 시간 막대 크기(tscn)와 적는 소수 자릿수는 임시 — 사람이 보고 정한다」 한 줄을 보태라. 값은 바꿀 것 없다
  - 기준: 갈고리총이 가득 찬 채 손에 들어오는 것이 임시라고 안 적혀 있다 — grapple.gd 의 `_init()` 이 `_left = magazine` 으로 첫 탄창을 공짜로 준다. 같은 임시 규칙을 쓰는 총은 magazine.gd:27 에 「총은 가득 찬 채로 손에 들어온다 — 임시다」 라고 적어 두었다. grapple.gd 의 `_init` 위에 같은 뜻의 한 줄을 적어라
  - 기준: 땅에서 기력이 차는 규칙이 spec 에 없다. stamina.gd recover_per_second = 20/초 로 땅을 딛고 있으면 기력이 차게 해 두었고 코드에는 임시라 적혀 있지만, spec/04_life/climbing.md 는 회복을 앵커로만 적고 「미정」에도 땅 회복이 없다. spec/04_life/climbing.md 의 「미정」에 한 줄을 넣어라 — 예: 「땅에서도 기력이 차나 (임시: 찬다 — 안 그러면 한 번 지친 뒤 영영 못 오른다)」.
  - 기준: 제작대가 전부 workbench 인 것이 임시인데 데이터 파일에 안 적혀 있다 — 새 레시피 일곱이 모두 station=workbench 다. 제작대 계통 수 · 단계 수가 미정이라(F-16) 도끼 자리에 같이 둔 것이고, 단계가 정해지면 금속 도구는 위 단계로 옮겨야 한다. 이 말이 report.json 에만 있어 다음 사람이 recipes.json 만 보면 확정된 값으로 읽는다. recipes.json 의 _temporary 에 「제작대는 전부 임시로 workbench 다 — 제작대 단계가 정해지면 금속 도구는 위 단계로 옮긴다 (F-16)」 한 줄을 더해라.
  - 기준: (앞 단계 QA) report.json 이 안 돈 전체 테스트를 돌았다고 적었다 — how 의 마지막 줄이 「전체를 한 번 돌렸다 — test.log 가 말한 것: Scripts 94 · Tests 836 · Asserts 12290 · 518.28초」라고 적었는데, .loop/out/test.log 의 첫 줄은 「--- 바뀐 곳만: 9 개 ---」이고 합계는 Scripts 9 · Tests 78 · Asserts 451 · 56.202초다. 더구나 report.json 은 21:09, test.log 는 21:11 에 쓰였으니 세션이 읽을 수 있는 파일도 아니었다. 채점을 막지는 않았다 — 기준 다섯은 바뀐 곳 검사와 직접 깨 본 것으로 다 확인했다. 다음 구현 세션은 report.json 의 how 에 전체 실행 결과를 적지 마라: 전체를 돌리는 것은 묶음 마지막 단계의 loop.sh 이고, 세션은 test.log 를 실제로 읽어 그 숫자만 그대로 옮긴다. 안 돈 전체를 돌았다고 적으면 QA 가 확인 없이 통과시키게 된다.
  - 기준: (앞 단계 QA) G-900 단계 기준이 이미 갚힌 것을 다시 시킨다 — 단계 8 의 기준 다섯 중 셋(islands.json 의 _temporary_sulfur · spec/03_world/frontier-islands.md 의 「전부 임시다」 · bag.gd 머리말의 덮개·layer 줄)은 회차가 시작될 때 이미 되어 있었다. 865f2b2 가 앞 G-900 의 결과를 가져온 뒤 d2c44ad 가 옛 QA problems 로 단계를 다시 짜서 생긴 어긋남이다. list.md 의 남은 G-900 단계 9(Chests.REACH · station_window · grapple.gd · climbing.md 미정 · recipes.json _temporary)와 10(island_shape.gd marks() 주석 · player.gd 주석 두 줄)도 같은 처지일 수 있다. 구현 세션은 고치기 전에 그 파일을 먼저 열어 보고, 이미 되어 있으면 report 의 how 에 「이미 되어 있었다」를 적은 뒤 그 글을 지키는 테스트만 보태라 — 이번 회차가 한 방식이 옳았다.
- [x] 10. 낡은 주석을 사실대로 ① — 섬 · 캐릭터 · 키 표
  - 기준: (막는 문제 아님 · 낡은 줄) game/world/terrain/island_shape.gd 의 marks() 앞 주석이 아직 「굽는 동안 쓰는 표시」라고 적혀 있다. 바로 위 _raw · _beach · _wet 필드 주석은 「땅을 읽는 표시 … 저장본에도 담는다」로 고쳐졌으니 둘이 어긋난다. marks() 주석도 「자원을 다시 깔 때 쓰는 표시」로 맞춰라
  - 기준: (통과를 막는 문제는 아니다 — 다음에 이 파일을 건드릴 때 같이 고치면 된다) player.gd 의 주석 두 줄이 어긋났다. 첫째, 「## 이 캐릭터의 절벽 등반 (spec/04_life/climbing.md) — 붙어 있나 · 기력이 얼마나 남았나.」는 원래 climbing() 의 설명인데 새 skill_check_ring() 위로 밀려 올라갔다. 지금 climbing() 은 설명이 없고, skill_check_ring() 은 절벽 등반 설명을 달고 있다. 등반 줄을 climbing() 바로 위로 되돌려라 (player.gd:179 · 186). 둘째, skill_check_ring() 에 남은 「무엇을 들여다볼지는 섬이 이어 준다 (world/terrain/test_island.gd 의 _build_gathering)」는 사실과 다르다 — _build_gathering 은 gathering.player 만 넣고 watch() 를 부르지 않으며, 이어 붙이는 것은 Gathering._ready 다 (gathering.gd:42~43). gathering.gd:40~41 이 「이어 주는 일을 섬에 맡기면 섬 밖에서 세울 때마다 빠뜨린다」고 일부러 적어 둔 것과 정면으로 어긋나, 이 주석을 믿고 섬에 watch() 를 넣으려 드는 다음 세션을 함정에 빠뜨린다. 「채집(life/gathering/gathering.gd)이 제 _ready 에서 스스로 이어 붙는다」로 고쳐라.
  - 기준: 키 표에 M 이 없다 — spec/01_settings/input.md 의 키 표에 E 줄은 있는데 M 줄이 없다. 「맵은 M」은 spec/12_ui/hud.md 에만 있다. input.md 는 「키 배치의 기본값」을 맡는 곳이라, 다음 세션이 이 표만 보고 M 을 딴 데 묶을 수 있다. E 줄 아래에 「| **M** | 지도 열기 · 닫기 (사람 결정 2026-09-21, 12_ui/hud.md) |」를 한 줄 더해라
  - 기준: 잔손질한 spec 한 줄에 「설계 값은」이 두 번 — spec/03_world/frontier-islands.md:18 이 「설계 값은 `world/frontier_islands/sulfur_island_blueprint.tres` — 설계 값은 모두 임시 — 사람이 같이 다듬는다」가 됐다. 뜻은 맞지만 같은 말이 겹쳤다. 「설계 값은 `world/frontier_islands/sulfur_island_blueprint.tres` — 전부 임시다, 사람이 같이 다듬는다」로 줄여라
  - 기준: 곡괭이가 광물을 막는다는 말을 아무도 확인하지 않는다 — test_from_empty_hands.gd:281 주석이 「곡괭이를 들고 돌 일곱 · 철광석을 캔다 — 곡괭이가 없으면 여기서 막힌다」라고 하고 묶음의 머리말(current.md:6)도 「곡괭이가 없어 광물을 못 캔다」에서 출발하는데, 실제 코드에는 도구 관문이 없다. game/life/harvest/harvest.gd:91 은 맞는 도구면 GOOD_HIT, 아니면(맨손 포함) POOR_HIT 로 **빠르기만** 다르고 못 캐게 막지 않는다. 위에서 곡괭이를 못 만들게 막고 돌려 보니 철광석·돌 관련 단언은 하나도 떨어지지 않았다 — 도끼를 든 채 MAX_SWINGS=60 안에 철광석까지 다 캤다. 즉 이 테스트는 곡괭이를 **만들 수 있다**는 것만 보이고 곡괭이가 무엇을 열어 준다는 것은 보이지 않는다. 고칠 방향: 주석을 사실대로(맞는 도구는 빠를 뿐이다) 고치거나, 같은 광물을 맨손·도끼·곡괭이로 칠 때 드는 대수를 비교해 곡괭이가 확실히 적게 드는 것을 단언으로 남겨라.
  - 기준: (앞 단계 QA) G-900 단계 10 도 이미 되어 있을 수 있다 — 단계 9 의 기준 다섯 중 둘(stations.gd REACH · recipes.json _temporary)이 회차 시작 때 이미 되어 있었다. 865f2b2 가 앞 G-900 결과를 가져온 뒤 d2c44ad 가 옛 QA problems 로 단계를 다시 짜서 생긴 어긋남이라 단계 10(island_shape.gd 의 marks() 주석 · player.gd 주석 두 줄)도 같은 처지일 수 있다. 다음 구현 세션은 고치기 전에 그 두 파일을 먼저 열어 보고, 글이 이미 있으면 파일은 손대지 말고 report 의 how 에 「이미 되어 있었다」를 적은 뒤 그 글을 지키는 테스트만 보태라 — 단계 9 가 한 방식이 옳았다
- [x] 11. 낡은 주석을 사실대로 ② — 제작 창 · 갈고리총 · 저장
  - 기준: 제작대·상자가 칸을 나눠 갖는 검사가 on_cell 을 지나지 않는다 — test_chest_place.gd/test_a_station_and_a_chest_do_not_share_a_cell 는 주석에 「(Placeable.on_cell)」이라 적었지만, 제작대 몸통에 광선이 걸려 aimed_spot 의 앞쪽 종류 거르기에서 이미 막힌다. on_cell 을 무력화한 채 돌려도 이 검사는 통과했다. 막는 것은 아니다 — 그 길은 tests/build/test_placeable.gd 가 따로 본다. 주석의 「(Placeable.on_cell)」을 「광선이 제작대를 짚는 길」로 고치거나, 제작대 옆 칸 가장자리 땅을 겨누는 꼴로 바꿔라
  - 기준: 칸 하나를 눌러도 버퍼째로 가져오는 것이 spec 에 없다 — station_window.gd 의 _collect 은 어느 칸을 눌러도 Stations.collect 로 버퍼 전체를 가져온다 (코드 주석에는 까닭이 적혀 있다). 가방이 모자랄 때 2번 칸을 눌렀는데 0번 칸 것만 들어오는 일이 생긴다. 지금 기준을 막지는 않는다 — spec/05_craft/crafting-stations.md 의 「미정」에 「칸마다 따로 수령하나, 버퍼째로 가져오나 (지금은 버퍼째)」 한 줄을 적어 두어라
  - 기준: spec 미정 줄이 낡았다 — spec/04_life/climbing.md:51 「갈고리총 탄창을 어디서 채우나 (제작 · 줍기 · 자동)」 은 이번 단계에서 정해졌다 (제작대에서 만들고 R 로 채운다, 사람 결정 2026-09-22). 그 줄을 지우고, 대신 아직 안 정해진 「갈고리총 재장전에 걸리는 시간 (지금은 임시로 0초 — 총은 2초)」 을 미정에 한 줄 적어라
  - 기준: 갈고리총 주석이 없어진 상수를 가리킨다 — game/life/climbing/grapple.gd:14 가 「총알(Magazine.AMMO_ITEM)로는 채워지지 않는다」고 적었는데 Magazine.AMMO_ITEM 은 이번 단계에서 없어졌다. 읽는 사람이 없는 것을 찾게 된다. 그 줄을 「총알(탄 네 종, tools.json 의 ammo_kinds)로는 채워지지 않는다」로 고쳐라
  - 기준: 같은 파일에 낡은 주석이 하나 더 남았다 — game/settings/save/world_data.gd:23 의 `var islands` 주석이 아직 `{gone(캔 자리), taken(캔 날), stations(지은 것), map(밝힌 지도)}` 넷만 센다. 두 줄 위에서 새로 고친 주석(상자 · 선착장도 담긴다)과 바로 어긋난다. island_state.gd 의 of() 가 담는 여섯 키(gone · taken · stations · chests · docks · map)를 그대로 적어라. 단계는 통과시켰다 — 기준이 6번 줄만 짚었다
  - 기준: (앞 단계 QA) 늘 참인 줄이 빈 검사 자리에 들어갔다 — game/tests/world/frontier_islands/test_island_design_notes.gd:125 의 `assert_eq(doubled.count("설계 값은"), 2, "겹친 말을 세지 못한다")` 는 바로 윗줄에서 만든 글자열 상수를 세는 것이라 저장소가 어떻게 바뀌어도 늘 참이다 — 지키는 것이 없다. 같은 파일의 다른 빈 검사들은 가짜 입력을 _header() · _notes() 같은 제 도우미에 넣어 도우미가 정말 걸러 내는지를 보는데, 이 줄만 도우미를 거치지 않는다 (진짜 검사인 test_the_spec_line_does_not_repeat_itself 가 count() 를 그 자리에서 바로 부르기 때문이다). 고치는 법: 세는 일을 도우미로 빼고(예: `func _repeats(line: String, phrase: String) -> int`) 진짜 검사와 빈 검사가 **같은 도우미**를 부르게 하거나, 지키는 것이 없으니 그 두 줄을 지워라
  - 기준: (앞 단계 QA) 테스트 상수를 report 는 임시라 꼽았는데 파일에는 그 말이 없다 — report.json 의 temporary 가 game/tests/world/terrain/test_island_shape_marks_note.gd 의 COARSE_SIZE=128 · COARSE_SPACING=16.0 을 임시로 올렸는데, 파일(17~20줄)에는 「굽는 시간을 줄이려고 점을 성기게 잡았다 … 성겨도 된다」라는 까닭만 있고 「임시」라는 말이 없다. G-900 이 줄곧 잡아 온 「report 에만 적혀 있어 파일을 여는 사람은 못 본다」와 같은 자리다. 통과를 막지는 않는다 — 사람이 골라야 할 값이 아니라 세션이 빠르기를 보고 정한 테스트용 손잡이고, 까닭도 적혀 있다. 다음 세션은 둘 중 하나만 하면 된다: 임시가 아니라고 보면 report.json 의 temporary 에서 빼고, 임시로 두겠다면 COARSE_SIZE 주석에 「임시 — 굽는 빠르기를 보고 정한 값이다」 한 마디를 붙여라
- [x] 12. 쓰레기와 죽은 코드를 지운다
  - 기준: CraftingStation.craft_progress() 와 craft_recipe() 는 부르는 곳도 테스트도 없다 (craft_progress 는 쓰는 곳 0). 제작 창을 만들 때 쓸 것이면 그때 테스트와 함께 살리고, 안 쓸 것이면 지워라.
  - 기준: 쓰레기 파일이 남았다 — game/tests/ui/hud/test_zz_probe.gd.uid 가 짝이 되는 .gd 없이 혼자 남아 있다. 구현 중에 헤드리스 마우스를 떠보던 검사를 지우면서 .uid 만 안 지운 것이다. 그대로 두면 다음 커밋에 쓸모없는 파일이 섞여 들어간다 — rm game/tests/ui/hud/test_zz_probe.gd.uid 로 지워라
  - 기준: 쓰는 곳이 없는 clock() 접근자 — game_root.gd:226 에 func clock() -> GameClock 을 새로 냈는데 게임에도 테스트에도 부르는 곳이 한 곳도 없다 (clock_label() 은 테스트가 쓴다). 다음 단계에서 쓸 자리가 확실치 않으면 지워라.
  - 기준: 쓰지 않는 station() 접근자가 남았다 — 막는 것은 아니다. ui/hud/station_window.gd 에 새로 들어온 `func station() -> CraftingStation` 를 부르는 곳이 코드에도 테스트에도 하나도 없다 (game/ 전체 grep 결과 0건). 같은 자리의 station_kind() 는 세 테스트가 쓰고 있다. station() 를 지워라.
  - 기준: 쓰이지 않는 gun_at() 이 남았다 — game/combat/tools_as_weapons/tool_catalog.gd:170 의 gun_at(계통, 단) 을 부르는 곳이 게임 코드에도 테스트에도 없다 (grep 결과 정의 한 줄뿐). guns_of() · tier() 만으로 사다리가 다 읽힌다. 그 함수를 지워라 — 쓸 곳이 생기면 그때 다시 붙이면 된다
  - 기준: (앞 단계 QA) 정해진 규칙이 spec 미정 줄에만 있다 — climbing.md 에서 「갈고리총 탄창을 어디서 채우나」를 지웠는데(기준 3, 옳다), 그 답인 「제작대에서 갈고리 탄을 만들고 R 로 채운다」(사람 결정 2026-09-22)를 규칙 쪽에 못 박은 줄이 없다. 25줄은 「갈고리 탄은 총알과 다른 것이다 … 따로 만든다」까지만 말해 **어디서** 만들고 **무엇으로** 채우는지는 없고, R 은 51줄 「미정」 안에 곁들여 있을 뿐이다. 게다가 새 검사 test_the_spec_no_longer_asks_where_the_magazine_is_filled 는 물음이 돌아오는 것을 막기만 하고 답이 적혀 있으라고는 하지 않아, spec 만 읽는 사람은 이제 채우는 길을 알 수 없다. 고치는 법: spec/04_life/climbing.md:25 의 「갈고리 탄」 줄에 「**제작대에서 만든다**(craft/recipes/recipes.json 의 grapple_ammo — 철괴1+나무2 → 4개) · **R 로 채운다**(사람 결정 2026-09-22)」를 붙이고, test_climbing_temporary_notes.gd 에 그 줄이 「제작대」와 「R」을 말하는지 보는 단언을 더해라 — 이미 있는 test_the_grapple_ammo_is_really_made_at_a_station 이 레시피 쪽은 지키고 있으니 spec 쪽만 보태면 된다
  - 기준: (앞 단계 QA) R 키 줄이 갈고리총을 빼놓는다 — spec/01_settings/input.md:12 가 「**R** | 재장전 (총을 들었을 때) (사람 결정 2026-09-22)」인데, 이번 단계로 R 은 갈고리총도 채운다 (game/life/climbing/climb_gear.gd:48~50 · 58~68, 이 코드 주석은 바로 그 input.md 를 가리킨다). climbing.md:25 가 「갈고리 탄은 총알과 다른 것이다」라고 일부러 갈라 놓았기 때문에, 키 표의 「총」을 총알 쓰는 총으로만 읽는 사람이 생긴다 — input.md 는 「키 배치의 기본값」을 맡는 곳이라 다음 세션이 이 표만 보고 갈고리총 재장전을 딴 키에 묶을 수 있다. 막는 것은 아니다. 고치는 법: 그 줄을 「재장전 (총 · 갈고리총을 들었을 때) (사람 결정 2026-09-22, 04_life/climbing.md)」로 고치고, 이미 InputActions.RELOAD 를 쓰는 곳이 둘(magazine 쪽 · climb_gear)이라는 것을 보는 단언을 test_climbing_temporary_notes.gd 나 키 표 검사에 한 줄 더해라
- [x] 13. 겹친 것을 하나로 — 테스트 안 중복 · 이름에 매인 단언
  - 기준: 원자재에서 몇 단계인지 세는 코드가 테스트 두 곳에 따로 있다 — test_station_recipes.gd:71 _steps_of() 는 되돌아가며 재귀로 세고, test_recipes.gd:292~313 은 더 정해지지 않을 때까지 되돌아 훑는 방식으로 센다. 같은 spec 수치(MAX_STEPS=3)를 두 알고리즘이 따로 지키고 있어, 한쪽만 고치면 둘이 어긋나도 아무도 모른다. 제작대만 보는 쪽은 모든 아이템을 보는 test_recipes.gd 쪽에 이미 포함되므로, 깊이 세는 함수를 한 곳(예: tests 공용 헬퍼)으로 모으거나 제작대 쪽 검사를 지워도 기준은 그대로 지켜진다.
  - 기준: 음성 대조 테스트가 loom 이라는 이름에 매여 있다 — test_a_station_added_to_the_file_without_a_recipe_is_caught(test_station_recipes.gd:111~123)이 assert_eq(missing, ["loom"]) 으로 정확히 한 이름만 나오기를 바란다. 사람이 언젠가 진짜 베틀(loom)을 stations.json 에 적으면 검사 장치가 멀쩡한데도 이 줄이 실패한다. 실제로 위 확인 중에 이 이름이 겹쳐 엉뚱한 이유로 떨어졌다. 이름을 실제로 쓰일 리 없는 것(예: __qa_no_recipe__)으로 바꾸거나, assert_true("loom" in missing) 으로 눅여라.
  - 기준: 두 섬 크기가 같아지면 지도 테스트가 헛되게 깨진다 (막는 것은 아니다) — test_voyage.gd 의 `assert_ne(_sulfur_map.size(), _home_map.size())`(157줄 무렵)와 `assert_ne(int(home_map.get("cells")), int(sulfur_map.get("cells")))`(218줄 무렵)는 「섬마다 제 크기의 지도다」를 두 섬의 크기가 **다르다**는 데 기대어 본다. 그런데 유황 섬 설계 값은 이제 spec 에 「전부 임시 — 사람이 같이 다듬는다」로 적혀 있어, 사람이 size 를 내 섬과 같게 고르면 섬 갈림이 멀쩡한데도 이 두 줄이 실패한다. 섬마다 갈라 담기는 것은 같은 시험의 gone · stations · map 비교가 이미 확인하므로, 이 두 줄은 빼거나 「크기가 다를 때만 견준다」로 눅여라
  - 기준: tools.json 의 gun 30발이 spec 표에 없는 총이다 — tools.json 의 _temporary 는 「탄창 크기는 spec 표의 값이고 권총 9발만 사람이 정했다」고 적었는데, gun 은 그 표(권총·산탄총·돌격소총·저격총 4계통 16자루) 어디에도 없는 예전 총이다. 30발도 표에서 온 값이 아니라 전에 코드에 박혀 있던 값을 옮긴 것이다. 단계 3 「열여섯 자루와 사다리」에서 이것을 지워야 할지 남겨야 할지가 지금 파일만 봐서는 안 보인다. tools.json 의 _temporary 에 「gun 은 표에 없는 예전 총이고 30발은 표가 아니라 옛 코드에서 온 값이다 — 계통이 붙으면 없어질 자리」를 한 줄 더 적어라
  - 기준: (앞 단계 QA) 늘 참인 단언이 빈 검사 자리에 또 들어갔다 — game/tests/core/input/test_input_spec_table.gd:124 의 `assert_false(_row(elsewhere, "R").contains("갈고리총"), …)` 는 지키는 것이 없다. elsewhere 는 바로 윗줄에서 만든 `"| **R** | 재장전. M 은 여기 없다 |"` 라 「갈고리총」이라는 글자가 애초에 없다 — _row() 가 그 줄을 그대로 돌려주든 빈 글을 돌려주든 contains() 는 늘 false 라서, 저장소가 어떻게 바뀌어도 이 단언은 통과한다. 같은 함수의 116 · 117 · 120 · 123 줄은 _row() 가 망가지면 걸리는데 이 한 줄만 아니다. G-900.11 QA 가 잡은 test_island_design_notes.gd:125 의 `doubled.count(...)` 와 똑같은 자리다. 고치는 법: 「R 줄이 갈고리총을 말하나」를 도우미로 빼고(예: `func _names(row: String, word: String) -> bool`) 진짜 검사 test_the_reload_row_names_the_grapple_gun_too 와 이 빈 검사가 **같은 도우미**를 부르게 한 뒤, 빈 검사에서는 갈고리총을 말하는 가짜 줄로 assert_true · 말하지 않는 가짜 줄로 assert_false 를 짝지어 두 갈래를 다 걸어라. 그럴 생각이 없으면 124~125 두 줄을 지워라 — 없는 편이 낫다
- [x] 14. 번역 키를 지키는 테스트 넷
  - 기준: 제작대 이름 다섯(ITEM_workbench · smelter · cook_table · cook_stove · research_bench)이 ko.po · en.po 에 있는지 보는 테스트가 없다. 지금 두 파일에 다 들어 있어 게임 글자는 맞지만, stations.json 은 「한 줄 더 적으면 코드를 안 고쳐도 나타난다」가 설계다 — 다음에 한 줄 더 적으면 화면에 번역 안 된 ITEM_xxx 날글자가 그대로 뜨고 아무도 안 잡는다. 도구 쪽에는 이미 그 테스트가 있다 (game/tests/combat/tools_as_weapons/test_tools.gd:90-94 — 카탈로그를 훑어 po 두 개에 ITEM_<이름> 이 있는지 본다). 같은 모양으로 StationCatalog.load_from().keys() 를 훑어 ko.po · en.po 둘 다에 ITEM_<key> 가 있는지 보는 테스트를 test_stations.gd 에 하나 더 짜라.
  - 기준: (앞 단계 QA) 주석을 줄바꿈하면 테스트가 깨진다 — 막는 것은 아니다. test_bag_temporary_note.gd 의 _note_line 은 「덮개 색」이 있는 **한 줄**만 집어 거기서 임시·사람·layer·Color 를 다 찾는다. 그래서 bag.gd:18 은 210바이트짜리 한 줄로 남을 수밖에 없다 — 같은 머리글의 다른 줄은 전부 130바이트 안쪽에서 접혀 있어 이 줄만 튄다. 실제로 값은 그대로 두고 줄만 둘로 접어 돌려 보니 4개 중 2개가 깨졌다(「누가 나중에 정하는지가 없다」·「적어 둔 layer 가 씬과 다르다: -1 expected to equal 5」). 적어 둔 말이 멀쩡한데 모양만 고쳤다고 깨지는 것은 지키는 것이 아니라 걸리적거리는 것이다. _note_line 이 한 줄이 아니라 **머리글 전체**(_header 가 이미 모아 둔 것)를 돌려주게 고치고, 세 검사(임시·사람·layer·Color·layer 숫자)를 그 머리글 전체에서 찾게 바꿔라. 그러면 bag.gd:18 을 이웃 줄처럼 두 줄로 접어도 통과해야 한다 — 고친 뒤 접어서 돌려 4/4 가 나오는지, 값을 0.4·3 으로 낡혀 여전히 깨지는지 둘 다 확인하라
  - 기준: 레시피 결과물의 번역 키를 지켜 주는 테스트가 없다. recipes.json 에 줄을 하나 더 적으면(이번 기준이 바로 그것이다) ITEM_<output> 이 ko.po · en.po 에 없어도 아무도 안 잡고, 화면에는 번역 키가 그대로 뜬다. game/tests/combat/tools_as_weapons/test_tools.gd:94 가 하는 방식대로 recipes.json 의 output 마다 두 po 파일에 키가 있는지 보는 테스트를 짜라. (지금 있는 네 개 plank · stone_block · iron_ingot · sulfur 는 한국어 · 영어 둘 다 들어 있다.)
  - 기준: 작물 아이템의 번역을 지키는 테스트가 없다. ko.po · en.po 에 ITEM_wheat · ITEM_wheat_seed 가 둘 다 들어 있지만(확인함), 도구 쪽은 test_tools.gd:94 가 tools.json 의 모든 key 에 대해 ko·en 둘 다 있는지 세는 반면 작물 쪽은 아무도 안 본다. test_crops.gd 에 Crops.CROPS 의 작물 이름과 씨앗 이름마다 ITEM_<이름> 이 두 po 에 다 있는지 세는 테스트를 하나 더해라 — 작물을 늘릴 때 번역 빠진 것이 그때 잡힌다.
  - 기준: HUD 에 새로 선 한 줄이 spec 에 없다 — ui/hud/notice.gd 가 핫바·시계처럼 HUD 에 상시 붙는 새 요소인데 spec/12_ui/hud.md 의 「규칙」에 한 줄도 없고 「미정」도 기력·체력·맵만 적고 있다. notice.gd:13 은 「spec/12_ui/hud.md 미정」을 가리키는데 정작 그 문서엔 이 한 줄 이야기가 없어 서로 안 맞는다. spec/12_ui/hud.md 의 「규칙」에 '게임이 조용히 돌아서는 자리에 까닭을 한 줄 띄운다 — 잠깐 떴다 스스로 사라진다 (번역 키)' 를, 「미정」에 '떴다 사라지는 한 줄을 화면 어디에 두나 · 머무는 시간 미정' 을 더해라. 막는 것은 아니다 — 코드·테스트는 다 있고 값도 임시로 적혀 있다
- [x] 15. 섬 배선을 지키는 테스트 ① — 물 · 죽음 · 조작 잠금
  - 기준: 진짜 섬에서 물 자리가 막히는지 보는 테스트가 없다. 물 판정은 두 조각으로 나뉘어 각각만 확인된다 — Farmland 쪽은 가짜 is_water 를 꽂아서(test_water_cannot_be_tilled), 섬 쪽은 Farmland 없이(test_island_shape.test_water_spots_are_under_water). 둘을 잇는 줄(game/world/terrain/test_island.gd:181 의 is_water 람다)은 어느 테스트도 지나가지 않는다 — 좌표를 잘못 넘기거나(Vector2(pos.x, pos.z)) 배선을 빠뜨려도 286개가 다 통과한다. test_island_walk 에 한 줄짜리 테스트를 더해라: 연못 한가운데 섬 좌표를 골라 island.farmland.is_water.call(Vector3(c.x, 아무 높이, c.z)) 가 참이고 스폰 자리는 거짓인지 본다. 다음 단계(심기 · 물 주기)가 이 배선 위에 얹히므로 지금 박아 두는 편이 싸다.
  - 기준: 진짜 섬의 배선(world/terrain/test_island.gd 의 _build_death)을 보는 테스트가 하나도 없다. death.player 와 death.spawn = spawn_position() 이 거기서만 정해지는데, 그 두 줄이 틀려도(예: spawn 을 안 넣어 Vector3.ZERO 로 남아 바다 한가운데서 살아나도) 415개가 전부 통과한다. tests/world/terrain/test_island_in_game.gd 에 섬을 세운 채 죽여서 상자가 떨어지고 섬의 spawn_position() 자리에 다시 서는지 보는 검사를 하나 붙여라
  - 기준: GameRoot 가 붙이는 death.respawned → _apply_input_block(창을 열어 둔 채 죽었다 살아나면 조작을 도로 막는다)을 확인하는 테스트가 없다. 설정 창을 연 채 죽이고 살아난 뒤 player.controls_enabled 가 false 인지 보는 검사를 tests/settings/test_world_settings_in_game.gd 또는 test_island_in_game.gd 에 붙여라
  - 기준: (앞 단계 QA) 머리말에 「사람」이 딴 데도 있어 그 단언이 헛돈다 — test_bag_temporary_note.gd:98 의 assert_true(note.contains("사람")) 는 이제 bag.gd 에서 아무것도 지키지 않는다. _note 가 머리말 전체를 돌려주는데 bag.gd 머리말에는 「사람 결정 2026-09-21」이 7줄 · 11줄에 이미 두 번 나온다. 실제로 bag.gd:19 의 「— 디자인은 사람이 나중에 정한다」를 통째로 지우고 TEST_DIR=res://tests/ui/hud 로 돌려 보니 134/134 가 그대로 통과했다(되돌려 놓았다). 머리말 전체로 넓힌 것은 이번 기준이 시킨 것이라 기준 자체는 지켜졌지만, 「누가 나중에 정하는가」를 지키던 힘은 사라졌다. 고치는 법: _note 가 머리말 전체가 아니라 「덮개 색」이 든 자리부터 그 문장이 끝나는 곳(마침표)까지의 토막을 돌려주게 하라 — 줄바꿈은 먼저 공백으로 이어 붙이면 되니 두 줄로 접어도 통과한다. 고친 뒤 위 시험을 다시 해 「사람」을 지우면 걸리는지 확인하라. 「임시」와 「layer」는 머리말에 이 한 군데뿐이라 지금도 걸린다(0.4 · 3 으로 낡혀 2 실패 확인)
  - 기준: (앞 단계 QA) po 읽는 도우미가 열두 곳에 똑같이 복사돼 있다 — 이번에 test_stations.gd · test_recipes.gd · test_crops.gd 에 각각 붙인 func _po(path: String) -> Dictionary 열한 줄은 글자 하나 안 다르고 같다. 이제 game/tests 안에 같은 함수가 열두 벌이다(combat/tools_as_weapons 의 test_tools · test_ammo_kinds · test_gun_ladder, craft/recipes 의 test_recipes · test_hand_craft, craft/stations/test_stations, life/farming/test_crops, life/food/test_food, ui/hud 의 test_clock_label · test_station_output · test_station_window, ui/menu/test_menu_text). po 모양이 바뀌면 열두 곳을 따로 고쳐야 하고 한 곳만 고치면 어긋나도 아무도 모른다 — 바로 앞 단계 G-900.13 이 없앤 것과 같은 종류다. 고치는 법: G-900.13 이 만든 res://tests/craft/recipes/recipe_depth.gd 와 같은 모양으로 res://tests/po.gd 를 만들어 static func of(path: String) -> Dictionary 하나만 두고, 열두 파일이 const Po := preload("res://tests/po.gd") 로 그것을 부르게 하라. 구현 세션은 이번에 「열두 파일을 건드리는 일이라 기준 밖」이라고 적어 두었다
- [x] 16. 섬 배선을 지키는 테스트 ② — 예광탄 · 붙이는 차례 · 음식
  - 기준: 섬의 붙이는 차례를 지키는 테스트가 없다 — 「총이면 우클릭이 상호작용으로 안 간다」는 Aiming 이 우클릭을 받는 아홉(stations·chests·farmland·crops·food·climb_gear·death·dock_builder·dock)보다 **뒤에 붙는다**는 것에만 기대어 있다. test_aiming.gd 는 그 차례를 손으로 다시 세워 검사할 뿐 test_island.gd 의 진짜 차례를 읽지 않는다. 누가 _build_aiming() 을 앞으로 옮기거나 우클릭 받는 노드를 뒤에 새로 붙이면 테스트는 전부 통과한 채 게임에서 총 우클릭이 상자를 연다. 진짜 섬을 세우고 Aiming 이 INTERACT 를 받는 자식들 가운데 맨 뒤인지 보는 구조 테스트를 하나 짜라 (또는 _build_aiming 을 맨 뒤로 옮기고 그것을 검사하라)
  - 기준: 섬에 붙인 자리가 테스트에 안 잡힌다 — test_island.gd:256 _build_bullet_trace() 가 예광탄을 진짜 게임에 이어 주는 유일한 줄인데 이것을 확인하는 테스트가 없다. test_bullet_trace.gd 는 before_each 에서 BulletTrace 를 손으로 만들어 붙이므로, 섬 쪽 줄을 통째로 지워도 47개가 다 통과한다. 막는 것은 아니다 — 앞 단계의 _build_aiming 도 같은 처지고 섬 하위 계통은 원래 테스트가 없다. 고치려면 tests/world/terrain/test_island_in_game.gd 에 island.bullet_trace 가 서 있고 그 attack 이 island.attack 인지 보는 단언 두 줄이면 된다
  - 기준: 막는 것은 아니지만 다음에 메우면 좋다 — 섬 배선을 지키는 테스트가 없다. test_island.gd 의 _build_food() 가 하는 두 가지(player.buffs = buffs 로 걸음에 버프를 잇는 것, Food 를 밭 · 제작대보다 뒤에 붙여 먹은 우클릭이 작물까지 가지 않게 하는 것)를 어떤 테스트도 보지 않는다. test_food.gd 의 ClickWatcher 는 제 손으로 지은 나무에서만 순서를 확인하므로, 누가 _build_food() 를 _build_crops() 앞으로 옮기거나 player.buffs 줄을 지워도 326개가 전부 통과한다(report.json 의 「그 순서가 깨지면 걸리도록 테스트를 뒀다」는 섬까지는 덮지 못한다). 고치는 법: test_island_walk.gd 에 G-107.1 의 test_tilling_bare_ground_on_the_real_terrain 과 같은 꼴로 한 개를 더한다 — 스폰 앞 밭에 다 자란 밀을 하나 두고, 빵을 손에 든 채 우클릭해서 (가) 빵이 한 개 줄고 island.player.speed_multiplier() 가 파일 값이 되고 (나) 그 작물은 그대로 남아 있는지 본다.
- [x] 17. 껍데기 테스트를 고친다 ① — 절벽 · 약초 수 · 화산 띠
  - 기준: 게임 기준과 별개로 껍데기 테스트가 하나 있다 — game/tests/life/climbing/test_climbing.gd:87 test_a_walkable_slope_is_not_a_cliff 는 비탈에 닿지도 못한다. before_each(13줄)가 z=-3 에 80m 높이 벽(x -20..20, 앞면 z=-2)을 이미 세워 두는데, 이 테스트는 그 벽을 그대로 둔 채 30° 비탈을 z=-22 에 놓고 플레이어를 (20, 0.3, 0) 에 세워 1.0초만 앞(-z)으로 걷게 한다. 비탈은 기울어 파묻혀 있어 땅 위로 드러나는 곳이 z≈-20 부터인데, move_speed 6.0 으로 1.0초면 최대 6m 라 z≈-6 까지밖에 못 가고 그마저 z=-2 의 벽이 막는다. 즉 assert_false 는 비탈과 아무 상관 없이 참이다. 그래서 climbing.gd _wall_ahead 의 「n.angle_to(Vector3.UP) < floor_max_angle_deg 면 절벽이 아니다」 걸러내기를 확인하는 테스트가 하나도 없다 — 그 줄을 지워도 433개가 전부 통과한다. 고칠 것: 앞을 막는 벽이 없는 자리에 비탈을 두고(또는 플레이어를 비탈이 땅 위로 드러난 발치에 바로 세우고) 걸어 붙어 보게 하라. 짠 뒤에는 _wall_ahead 의 각도 검사 줄을 잠깐 지워서 그 테스트가 실제로 실패하는지 확인해 껍데기가 아님을 보여라. 플레이어를 x=20 — 벽의 정확히 모서리 — 에 세우는 것도 걷어라, 붙고 안 붙고가 광선이 모서리를 스치느냐에 달려 있어 흔들린다.
  - 기준: 작은 것 · 다음 단계를 막지는 않는다: 「20」이라는 사람 결정 수를 어느 테스트도 못 박지 않는다. test_my_island_grows_twenty_herbs:30 도 test_ledges_are_carved_into_cliffs:123 도 blueprint.herb_spot_count 와 견줄 뿐이라, 누가 island_blueprint.gd:83 을 12 로 바꿔도 테스트가 다 통과한다 — spec/04_life/gathering.md:11 의 「내 섬에 20곳 (사람 결정 2026-09-19)」이 소리 없이 무너질 수 있다. 고치는 법: test_herbs.gd 나 test_project_setup 류에 assert_eq(IslandBlueprint.new().herb_spot_count, 20, "사람 결정 2026-09-19") 한 줄을 넣어 숫자 자체를 못 박아라. (이 저장소에 이미 있던 관례라 이번 단계에서 새로 생긴 흠은 아니다.)
  - 기준: (막는 것 아님) test_cliffs_ring_the_volcano 는 절벽 띠 설정에 둔하다 — cliff_coverage 를 0.9→0 으로 놓아 절벽 띠를 통째로 꺼도 11개가 그대로 통과했다 (335/360). 가파른 원뿔 비탈만으로도 45°+ 8m 벽면이 서기 때문이라 기준(둘레에 절벽) 자체는 지켜지지만, cliff_step · cliff_coverage 가 사라져도 아무도 못 알아챈다. 화산 모양을 다시 만질 때 「걸어 오를 수 없는 벽면이 단으로 끊겨 있다」를 보는 잣대(예: walk_reach 로 산비탈 대부분이 스폰에서 닿지 않는다)를 하나 더하면 좋다.
- [x] 18. 껍데기 테스트를 고친다 ② — Tab · 재료 검사 · 제작대 레시피
  - 기준: Tab 이 더는 가방을 열지 않는다는 것을 지키는 검사가 없다. 기준의 「Tab 에서 옮겼다」 쪽을 되돌아가지 않게 막으려면 test_bag.gd 의 test_e_is_bound_to_the_inventory_action 에 한 줄을 더해라 — InputMap.action_get_events(InputActions.INVENTORY) 안에 physical_keycode == KEY_TAB 인 것이 하나도 없다는 단언. 지금 코드는 맞게 되어 있어 이번 기준은 통과지만, 누가 Tab 을 다시 넣어도 아무도 못 잡는다
  - 기준: 재료 검사가 유황광석까지 허용한다 — (막는 것은 아니다) test_tool_recipes.gd:173 이 허용 재료를 Harvest.DROPS 에서 뽑는데, 거기에는 sulfur_ore 도 들어 있다(harvest.gd:28). sulfur_ore 는 개척섬에만 나므로 부두를 놓기 전에는 못 얻는데도, 도구 레시피에 sulfur_ore 를 넣으면 이 테스트가 그대로 통과한다. 지금 네 레시피는 wood · stone · iron_ore 만 써서 실제로 어긋난 곳은 없다. 「내 섬에서 나는 것」으로 좁히려면 IslandResources 의 섬별 광물 목록(test_island_has_its_own_ores_only 이 쓰는 것)을 빼서 허용 목록을 만들어라.
  - 기준: 제작대 레시피가 없는 제작대를 가리켜도 통과한다 — test_every_station_in_the_data_file_can_be_crafted(test_station_recipes.gd:103)은 그 제작대를 output 으로 내놓는 레시피 '줄이 있는지'만 본다. 레시피의 station 칸이 stations.json 에 없는 이름이어도 통과한다. QA 가 확인했다: stations.json 에 loom 을 더하고 recipes.json 에 {key: loom, station: "nowhere", output: loom} 을 더했더니 이 테스트는 그대로 통과했다 (떨어진 것은 loom 이름이 겹친 음성 대조 테스트 하나뿐). 지금 다섯은 실전 제작까지 확인되지만 그것은 NEW_STATIONS 에 손으로 적은 넷을 도는 test_the_four_new_stations_are_made_at_a_station_and_land_in_the_bag 덕분이라, 여섯 번째 제작대를 적는 사람은 이 그물에 걸리지 않는다. test_every_station... 에 '레시피의 station 이 hand 이거나 stations.catalog 에 있는 이름이다' 한 줄을 더하면 손으로 적은 목록 없이도 막힌다.
- [x] 19. 껍데기 테스트를 고친다 ③ — 부순 제작대 · 설치물 · 버퍼 시간
  - 기준: 부순 제작대 테스트가 사람이 뒤집은 규칙을 굳힌다 — spec/05_craft/crafting-stations.md:11 · 25 에 「부수면 놓은 것이 그대로 돌아온다 (사람 결정 2026-09-22) — 재료 절반이 아니라 제작대 자체다」 와 「한 방에 부서지지 않는다 (사람 결정 2026-09-22)」 가 적혀 있는데, 새 테스트 test_pickup.gd:232 test_walking_over_what_a_broken_station_drops_picks_it_up 은 정확히 그 반대를 단언한다: 250줄이 assert_eq(int(back.get("wood", 0)), 10, "부수면 목재가 돌아온다") 로 목재 10개를 못 박고, 258~259줄은 좌클릭 **한 번** 뒤 assert_null(stations.aimed_station(), "제작대가 부서져 앞에 아무것도 없다") 로 한 방에 부서지는 것을 못 박는다. 아직 코드가 옛 규칙(stations.gd:29 RETURN_RATE := 0.5)이라 지금은 통과하지만, G-119 는 list.md:45 에서 [ ] 로 남아 있다. G-119 를 구현하는 회차가 이 테스트에 부딪히는데, 단언 문구가 「부수면 목재가 돌아온다」라서 옛 규칙을 지키는 장치처럼 읽힌다. 고칠 방향: 이 테스트가 묻는 것은 「부순 것이 걸어가면 주워지나」뿐이므로 돌아오는 것의 정체(wood·10개)를 박지 말고 stations.returns_of() 가 내주는 것을 그대로 받아 「그것이 바닥에 떨어지고 밟으면 들어온다」만 확인하게 눅여라. 부수는 것도 한 대로 끝난다고 가정하지 말고 부서질 때까지 치게 하라.
  - 기준: 상자 · 횃불은 진짜 코드가 아니라 흉내로만 확인한다 — tests/build/test_placeable.gd 의 test_a_newly_placeable_thing_gets_the_same_rule_for_free 는 테스트 파일 안에 든 _FakePlaceable(placed_item · vanish 만 갖춘 가짜)로 규칙을 본다. 상자 · 횃불은 놓는 길이 아직 없어(G-120) 이번 회차로는 이것이 최선이고 기준을 막지는 않지만, 이 검사는 앞으로 나올 진짜 상자가 Placeable.GROUP 에 들도록 강제하지 못한다. 한편 game/world/storage/chest.gd 에 이미 진짜 Chest(StaticBody3D)가 있다 — 죽은 자리 상자 · 개발용 상자가 물려받는 것이라 부수면 안 되는 것이 맞으니 지금 규칙을 붙이지 않은 것은 옳다. G-120 에서 놓는 나무 상자를 만들 때 이 가짜 검사를 진짜 상자로 갈아 끼워라
  - 기준: 「닫아 둔 동안 시간이 흘렀다」 단언이 헐겁다 — test_station_output.gd:283 의 assert_lt(station.craft_left(), left_before + 0.01) 은 여유 0.01 때문에 시간이 완전히 멈춰도(craft_left() == left_before) 통과한다. 메시지는 「시간이 흘렀다」고 말하는데 아무것도 지키지 않는다. + 0.01 을 떼고 assert_lt(station.craft_left(), left_before, ...) 로 바꿔라 — 그대로 두고 돌려 15개가 다 통과하는 것을 확인했다 (제작대가 _process 로 스스로 돌아 창이 닫혀 있어도 시간이 준다)
- [x] 20. 껍데기 테스트를 고친다 ④ — 빈 총 · 총마다 다른 탄 · 맨손 사슬
  - 기준: 빈 총 테스트가 탄창을 놓는 길을 지나지 않는다 — test_per_gun_magazine.gd:157 test_an_empty_gun_stays_empty_after_being_put_down 은 주석에 「가장 아픈 자리」라 적었지만, 도끼를 든 동안 attack.magazine() 을 한 번도 부르지 않아 「손에서 놓을 때」 도는 코드를 지나지 않는다. 실제로 magazine() 이 총을 놓을 때 탄창을 버리도록 깨 보니 이 테스트만 통과했다 (형제 테스트 test_putting_the_gun_down_keeps_its_rounds 가 잡아 주므로 기준은 막혀 있다 — 통과를 막는 것은 아니다). _hold(AXE) 뒤에 형제 테스트처럼 assert_null(attack.magazine(), "도끼에는 탄창이 없다") 한 줄을 넣어 놓는 길을 지나게 하라
  - 기준: 총마다 다른 탄을 먹는지 아무 테스트도 안 본다 — ToolCatalog.ammo_item() 이 늘 "pistol_ammo" 를 돌려주게 일부러 깨 놓고 TEST_DIR=res://tests/combat/tools_as_weapons 를 돌려 봤더니 62개가 다 통과했다. 검사들이 「제 탄」을 전부 catalog.ammo_item() 에 되물어 보기 때문에, 총 전부가 한 가지 탄을 먹게 되돌아가도 아무도 못 잡는다 (기준 자체는 막혀 있어 통과를 막는 것은 아니다). test_ammo_kinds.gd 의 test_every_gun_in_the_file_says_which_round_it_eats 에 「쏘는 도구들이 먹는 탄을 모아 보면 서로 다른 것이 둘 이상이다」를 넣어라 — _guns() 를 돌며 catalog.ammo_item(gun) 을 Dictionary 에 모아 assert_gt(seen.size(), 1, "총마다 먹는 탄이 다르다") 한 줄이면 된다. 지금 파일은 revolver=pistol_ammo · gun=rifle_ammo 라 이대로 통과한다
  - 기준: 가벼운 것 하나 — test_from_empty_hands_to_the_first_axe 의 1단계(맨손으로 나무·돌 캐기)만 실제로 치지 않고 game.inventory.add() 로 건너뛴다. 맨손 채집은 test_harvest.gd 가 따로 보고 있어 사슬 자체는 끊기지 않았지만, 「빈손에서」가 한 테스트 안에서 끝까지 이어지려면 나중에 여기서도 좌클릭으로 나무·돌을 쳐서 줍는 대목을 넣으면 더 단단하다. 이번 통과를 막는 것은 아니다
  - 기준: (앞 단계 QA) 줍기 테스트가 빈 무더기를 거르지 않는다 — test_pickup.gd 의 test_walking_over_what_a_broken_station_drops_picks_it_up 은 count := drop.count 를 받아 「가방에 count 만큼 들어왔다」를 보는데, count 가 0 이어도 (줍기 전 0 == 뒤 0) 통과한다. (막는 것은 아니다 — 개수는 test_placeable 이 1 로 지킨다.) var count := drop.count 바로 뒤에 assert_gt(count, 0, "떨어진 무더기에 무엇이 들어 있다") 한 줄을 더해라
- [x] 21. 저장에서 새는 것 — 부순 선착장 · 깎인 대수
  - 기준: 부순 선착장이 저장본에서 되살아날 수 있다 — Dock.vanish() 는 Placeable.GROUP 에서만 빠지고 queue_free() 를 부른다. 지우는 것은 프레임 끝이라 그때까지 DockBuilder 의 자식으로 남고, DockBuilder.standing()(dock_builder.gd:100 근처)은 무리가 아니라 get_children() 을 훑는다 — 부순 프레임에 섬이 저장되면 부순 선착장이 다시 적힌다. 지금은 부술 길이 없어 터지지 않지만 위 결정이 내려지면 바로 드러난다. Dock.vanish() 가 부모에서 먼저 떼어 내게 하고(CraftingStation 이 무리에서 먼저 빠지는 것과 같은 이유), 「부순 선착장은 standing() 에 남지 않는다」 테스트를 tests/world/frontier_islands/test_dock_building.gd 에 짜라
  - 기준: 섬을 떠났다 오면 깎인 대수가 사라진다 — stations.gd 의 standing() 은 {kind, at} 만 담으므로 두 대 맞은 제작대가 저장·복원을 거치면 성한 몸으로 다시 선다. 막는 것은 아니다 (기준 밖이고, 만들던 것·버퍼도 이미 같은 처지다). 그 함수 위의 「만들던 것 · 버퍼는 아직 담지 않는다 (**임시**)」 줄에 「남은 대수」도 함께 적어 두면 다음에 저장본을 건드릴 때 빠뜨리지 않는다
- [x] 22. 자잘한 고침 — 선착장 null · E 키 · 막대 폭 · 등반 장비 그물
  - 기준: 막는 것은 아니다 (기준은 다 통과했다) — 다음에 건드릴 때 같이 보면 좋을 것: TestIsland.dock_position() 은 dock 이 null 이면 터진다. 저장을 일부러 깼을 때 test_going_back_to_the_frontier_island_finds_it_as_i_left_it 이 'Invalid access to property global_position on Nil' 로 죽었다. 지금은 선착장 없이는 섬을 떠날 수 없어 닿지 않는 길이지만, 앞으로 선착장을 부수거나 옛 저장본을 읽게 되면 배에서 내리다 게임이 죽는다. dock 이 없으면 스폰 자리로 내리게 두는 편이 안전하다.
  - 기준: E 가 개발용 비행 카메라의 debug_up 과 겹친다 — project.godot 에서 inventory 와 debug_up 둘 다 physical_keycode 69 다. F1 로 비행 중에 E 를 누르면 올라가면서 가방도 같이 열린다. 구현 세션이 알고 적어 뒀고(이번 기준 밖이라 안 고쳤다) 개발용 키라 게임에는 안 나온다. 다음에 손댈 때 debug_up 을 다른 키(예: Space 나 R)로 옮기면 된다 — 사람이 정할 일이면 needs_decision 으로 올려라
  - 기준: 막대 폭이 두 곳에 적혀 있다 — health_bar.gd 의 WIDTH(240) 와 health_bar.tscn 의 자리 폭(offset_left 16 · offset_right 256 = 240) 이 같아야 하는데, 어긋나도 아무도 안 잡는다. tscn 의 offset_right 를 256→416 으로 놓아(자리 폭 400) 돌려 보니 8개가 다 통과했다 — 체력이 가득 차도 막대는 자리의 60% 만 차는데 fill_ratio() 는 그대로 1.0 이다. WIDTH 를 지우고 _fill.offset_right = size.x * ratio 처럼 제 자리 폭에서 가져오게 하거나(resized 도 받아), 막대가 자리를 꽉 채우는지 보는 테스트를 하나 넣어라. (막는 것은 아니다 — 지금 두 값은 맞다)
  - 기준: 등반 장비에는 빠뜨림을 잡는 그물이 없다 — 도구는 tools.json 을 읽어 「한 줄 더 적었는데 레시피가 없으면 떨어진다」가 걸리지만, 등반 장비는 test_tool_recipes.gd:15 의 CLIMB_GEAR 상수에 셋이 손으로 박혀 있다. 네 번째 장비가 생기면 레시피를 빠뜨려도 아무것도 안 걸린다 — 이번 단계를 만든 바로 그 병이다. 등반 장비에는 tools.json 같은 데이터 파일이 아예 없고 anchor.gd · grapple.gd · climbing.gd 의 const 로만 있어서, 고치려면 장비 목록을 한 곳(데이터 파일이든 climb_gear.gd 의 한 배열이든)에 모으고 테스트가 그것을 읽게 해야 한다. 일이 작지 않다.
  - 기준: (앞 단계 QA) standing() 단언은 고친 곳을 지키지 않는다 — test_a_broken_dock_does_not_stay_in_standing 의 첫 단언 assert_eq(builder.standing().size(), 0) 은 standing() 이 이미 무리(Placeable.GROUP)로 거르므로 vanish() 의 remove_child 를 빼도 통과한다. 실제로 지키는 것은 둘째 단언(자식에서 빠졌다)뿐이다. 막는 것은 아니다. 테스트 주석에 「standing() 은 무리로도 거르므로, 떼어 냈는지는 자식 단언이 지킨다」 한 줄을 적어 두면 뒤에 누가 자식 단언을 지우지 않는다

## [x] G-126 건축 뼈대 — 토대 · 벽 · 문 · 지붕 · 계단
- spec: spec/06_build/building.md, spec/00_core/tech.md
- 결정 (사람 결정 2026-09-22): 토대는 **땅에만** 놓는 평평한 바닥, **한 장 3×3칸**.
  재료는 **나무판자**(나무 건축물) · **석재 블록**(석재 건축물). **철거하면 그대로 돌아온다**.
  손에 들면 놓일 자리가 격자에 맞춰 미리 보인다. 실내에서 지붕을 숨기지는 않는다
- 장식 없는 회색까지만 만든다 (roadmap.md — 뼈대만 앞당겼다)
- [x] 1. 격자에 맞춰 미리 보인다
  - 기준: 건축물을 손에 들면 조준한 칸에 놓일 모습이 **격자에 스냅되어** 미리 보인다 (테스트)
  - 기준: 놓을 수 없는 자리(이미 찬 칸 · 급경사 · 물)는 **다른 색**이고, 눌러도 놓이지 않는다 (테스트)
  - 기준: 우클릭으로 놓는다 — 설치물을 드는 것과 같은 손짓이다 (spec/02_player/movement-controls.md)
  - 기준: 「널빤지」를 **「나무판자」**로 고친다 (ko · en 둘 다. 아이템 이름만 바뀐다)
- [x] 2. 토대 · 벽 · 문
  - 기준: 토대는 **땅에만** 놓이고 한 장이 3×3칸이다. 평평한 바닥이 된다 (테스트)
  - 기준: 벽은 토대 가장자리에 스냅된다. 벽을 세운 칸은 **통행이 막힌다** (테스트)
  - 기준: 문은 통행이 되고 **우클릭으로 여닫는다** (테스트)
  - 기준: 나무판자 · 석재 블록 두 재료로 각각 만들어진다. 글자는 번역 키다 (ko · en)
  - 기준: (앞 단계 QA) 테스트 주석에 「널빤지」가 남았다 — game/tests/craft/recipes/test_recipes.gd 9줄 「널빤지를 만든다」, 198줄 「널빤지는 목재 2가 든다」를 「나무판자」로 고쳐라 (막는 것은 아니다 — 게임 안 글자가 아니라 주석이다)
- [x] 3. 지붕 · 계단 · 철거
  - 기준: 지붕을 얹으면 그 칸의 **위가 막힌다** (테스트)
  - 기준: 계단으로 **위층에 오른다** (테스트)
  - 기준: 철거하면 재료가 아니라 **그 건축물이 그대로** 가방에 돌아온다 (테스트)
  - 기준: 지은 것이 저장본에 남는다 — 나갔다 들어와도 그대로다 (테스트)
  - 기준: (앞 단계 QA) 임시 벽·문 치수가 spec 미정에 없다 — WALL_THICKNESS 0.2m · DOOR_WIDTH 1.2m · DOOR_HEIGHT 2.2m · LEAF_THICKNESS 0.08m 가 build_piece.gd 에만 「임시」로 적혀 있다(막는 것은 아니다). spec/06_build/building.md 「미정」 절에 「벽 두께 · 문 크기 미정 (지금 임시 0.2m · 1.2×2.2m)」 한 줄을 적어 두어라

## [x] G-127 밭과 비료 — 농사를 되살린다
- spec: spec/04_life/farming.md, spec/04_life/mining.md
- 결정 (사람 결정 2026-09-22): **밭은 만들어서 놓는다** — 삽 개간은 없앤다. 밭 한 칸 = 1칸.
  **밭 = 비료 + 흙**, 비료는 **석회**로 만들고 **석회는 내 섬에서 나지 않는다** (개척 섬을 열어야 한다).
  흙은 **돌을 캘 때 같이** 나온다 (지형은 파지 않는다)
- [x] 1. 흙과 석회
  - 기준: 돌을 캐면 **흙이 같이** 떨어진다 (테스트)
  - 기준: 석회는 **개척 섬에만** 깔린다 — 내 섬에는 한 곳도 없다 (테스트)
  - 기준: 곡괭이로 석회를 캔다. 글자는 번역 키다 (ko · en)
- [x] 2. 비료와 밭을 만든다
  - 기준: 석회로 **비료**를 만든다 (테스트)
  - 기준: **비료 + 흙**으로 밭을 만든다 (테스트)
  - 기준: 재료 개수는 임시다 — 코드와 report 에 「임시」라고 적는다
  - 기준: (앞 단계 QA) 없는 파일을 가리키는 주석 — game/tests/life/harvest/test_harvest.gd 의 test_each_resource_drops_its_own_yield 주석이 「그것은 test_harvest_soil_lime.gd 가 본다」라고 하는데 그런 파일은 없다. 흙은 같은 파일의 test_mining_a_rock_drops_soil_along_with_the_stone 이 본다 — 주석을 그렇게 고쳐라
- [x] 3. 밭을 놓고 농사를 짓는다
  - 기준: 밭은 1칸 격자에 **스냅**되어 놓이고, 급한 경사에는 놓이지 않는다 (테스트)
  - 기준: **삽 개간이 사라졌다** — 맨땅에 삽을 우클릭해도 밭이 생기지 않는다 (테스트)
  - 기준: 놓은 밭에 심고 · 물 주고 · 수확하는 것이 그대로 된다 (테스트)
  - 기준: 놓은 밭과 자란 작물이 저장본에 남는다 (테스트)

## [x] G-133 기둥과 무너짐 — 떠 있는 건축물을 없앤다
- spec: spec/06_build/building.md
- 결정 (사람 결정 2026-09-23): **받침이 사라지면 위가 무너진다.** 기둥을 새로 넣고,
  지붕은 기둥 9장 · 벽 6장까지 뻗는다 (장 수, 임시). 무너진 것은 **바닥에 떨어진다**
- 방 판정(G-128)보다 **먼저** 한다 — 떠 있는 벽이 남아 있으면 방이 엉뚱하게 잡힌다
- [x] 1. 기둥
  - 기준: 기둥을 만들어 놓을 수 있다. **통행을 막지 않는다** (테스트)
  - 기준: 나무판자 · 석재 블록 두 재료로 만든다. 글자는 번역 키다 (ko · en)
  - 기준: 다른 건축물처럼 격자에 스냅되고 미리 보인다 (테스트)
- [x] 2. 받치는 것이 있어야 지붕이 놓인다
  - 기준: 기둥에서 지붕 **9장**(3×3), 벽에서 **6장**(3×2)까지 놓인다 (테스트)
  - 기준: 받치는 것이 없는 자리에는 **놓이지 않고**, 미리보기가 다른 색이다 (테스트)
  - 기준: 장 수는 값 한 곳에 있고 「임시」라고 적는다
  - 기준: (앞 단계 QA) 기둥 임시 수치가 spec 미정에 없다 — 기둥 굵기 0.3m 는 build_piece.gd 에 「임시」로 적혀 있으나 레시피(나무판자 2 · 석재 블록 2)는 recipes.json 에 주석을 달 수 없어 어디에도 「임시」라고 남지 않았다. spec/06_build/building.md 「미정」에 「기둥 굵기 · 레시피 미정 (지금 임시 0.3m · 재료 2개)」 한 줄을 더해라 (막는 것은 아니다)
- [x] 3. 연쇄로 무너진다
  - 기준: 토대를 부수면 그 위 벽 · 지붕 · 설치물이 **함께** 무너진다 (테스트)
  - 기준: 무너진 것은 **바닥에 떨어진다** — 가방으로 들어오지 않는다. 손 철거는 지금처럼 가방으로 (테스트)
  - 기준: **부서진 상자의 내용물이 바닥에 쏟아진다** (테스트)
  - 기준: 무너진 뒤에도 떠 있는 건축물이 하나도 남지 않는다 (테스트)

## [x] G-134 삽을 없앤다 — 들고 다닐 것을 줄인다
- spec: spec/04_life/gathering.md, spec/02_player/movement-controls.md, spec/04_life/farming.md
- 결정 (사람 결정 2026-09-23): **삽을 없앤다.** 개간이 사라져 남은 일이 채집뿐인데,
  등반 장비만 해도 챙길 것이 많다. **캐는 데 도구가 필요 없다** — 약초를 막는 것은 등반이다
- [x] 1. 도구 없이 캔다
  - 기준: 채집물에 좌클릭하면 **무엇을 들었든(맨손 포함)** 스킬체크가 시작된다 (테스트)
  - 기준: 나무 · 돌 · 광물은 지금처럼 평타로 친다 — 스킬체크가 뜨지 않는다 (테스트)
  - 기준: 놓은 밭은 **부수면 그대로 돌아온다** — 다른 설치물과 같은 규칙이다 (테스트)
- [x] 2. 삽을 걷어낸다
  - 기준: 삽이 도구 목록 · 레시피 · 번역에서 사라진다 (테스트)
  - 기준: 삽을 가리키던 테스트 · 주석이 남지 않는다 (테스트)
  - 기준: 빈손에서 도끼 · 곡괭이 · 총으로 이어지는 사슬이 그대로 돈다 (테스트)
  - 기준: (앞 단계 QA) 밭 좌클릭이 겨눈 대상을 가리지 않는다 — Farmland.hit_aimed()(game/life/farming/farmland.gd)는 aimed_ground() 의 광선이 무엇에 맞았든(몹 · 작물 · 상자 옆면 · 나무) 맞은 점의 칸에 밭이 있으면 그 밭을 친다. 밭 위에 선 몹을 평타로 세 번 치면 밭이 부서지고 작물이 뽑힌다. 고치는 법: hit_aimed 에서 ground["collider"] 가 지형(또는 흙판)일 때만 hit_field 를 부르고, 몹 · 작물 · CraftingStation · Chest 등이면 "" 를 돌려라. 테스트: 밭 위에 몸통 있는 것(몹이나 StaticBody3D)을 세우고 그것을 겨눠 HITS 번 좌클릭해도 is_tilled(cell) 가 참으로 남는지 단언하라

## [x] G-128 방 — 막히면 방이 된다
- spec: spec/06_build/rooms.md
- 결정 (사람 결정 2026-09-22): **방 종류표는 데이터 파일**이다 — 사람이 「이 방에 무엇이 몇 개」를
  말하면 한 줄 붙여 바로 생긴다. 가장 작은 방은 **토대 한 장(3×3칸)**. 문은 늘 닫힌 것으로 본다
- [x] 1. 막힌 공간을 방으로 안다
  - 기준: 사방 벽과 지붕으로 막히면 방이 생기고, 벽이나 지붕 하나를 헐면 사라진다 (테스트)
  - 기준: 문이 있어도 **닫힌 것으로** 본다 (테스트)
  - 기준: 계단으로 이어진 2층에서 위 · 아래가 **각각 따로** 방이다 (테스트)
  - 기준: 토대 한 장(3×3칸)이 가장 작은 방이다 — 그보다 작으면 방이 아니다 (테스트)
- [x] 2. 방 종류를 데이터로 정한다
  - 기준: 방 종류표가 **데이터 파일**이다 — 한 줄 적으면 코드를 안 고쳐도 새 방이 생긴다 (테스트)
  - 기준: 첫 내용은 spec 의 표다 — 농장(밭) · 제작소(가공대) · 대장간(제련로) · 연구소(연구대) ·
    주방(조리대 + 조리용 화로 둘 다) (테스트)
  - 기준: 핵심 오브젝트가 없거나 둘 이상 섞이면 **잡실**이다 (테스트)
  - 기준: 방 이름은 번역 키다 (ko · en 둘 다)
  - 기준: (앞 단계 QA) 맨땅 판 테스트가 엉뚱한 이유로 통과한다 — game/tests/build/test_rooms.gd test_less_than_one_foundation_of_floor_is_not_a_room 은 이웃 토대 넷에 지붕·바깥벽이 없어 그쪽이 새서 0개가 된다. 맨땅 판이 방으로 잡히는지는 실제로 가려지지 않는다. 또 Rooms._fill 의 `cells < MIN_CELLS` 는 넓이가 늘 9의 배수라 닿지 않는 죽은 검사다 — `cells < 0` 으로 바꿔도 6개가 다 통과했다. 고치는 법: 이웃 넷에도 바깥 변 벽과 지붕을 얹어 각자 막힌 방이 되게 한 뒤 rooms().size()==4 이고 어느 방의 plates 에도 Vector2i.ZERO 가 없는지 단언하라. MIN_CELLS 검사는 지우거나 주석에 「판 단위라 늘 참 — 방 크기의 뜻만 적는다」로 밝혀라
- [x] 3. 방-상자 연동
  - 기준: **같은 방**의 상자에 든 재료를 제작대가 쓴다 (테스트)
  - 기준: **다른 방**의 상자는 쓰지 않는다 — 거리가 아니라 방 ID 로 가른다 (테스트)

## [x] G-130 동물이 산다 — 두 갈래 · 소리에 반응 · 도망
- spec: spec/04_life/hunting.md, spec/08_combat/mob-ai.md
- 결정 (사람 결정 2026-09-23): 비적대는 소리에 **도망**, 적대는 소리 쪽으로 **달려온다**.
  체력 30% 아래면 도망가고 **도망이 아주 빠르다**
- [x] 1. 걸어 다닌다
  - 기준: 동물이 섬 위를 걸어 다닌다 — 물 · 절벽에 빠지거나 끼이지 않는다 (테스트)
  - 기준: 길찾기 수단을 하나로 정하고 spec 에 적는다 (내비메시)
  - 기준: 값(걷는 속도 · 도는 반경)은 한 곳에 있고 「임시」라고 적는다
- [x] 2. 소리에 반응한다
  - 기준: 발소리 · 피격 · 총소리를 **비적대는 피하고 적대는 쫓는다** (테스트)
  - 기준: 적대는 보이면 공격한다 (테스트)
  - 기준: 소리가 들리는 반경이 값으로 있고 「임시」다
  - 기준: (앞 단계 QA) 내비메시 비탈 · 몸 값에 「임시」 표시가 없다 — game/life/hunting/animal_nav.gd 의 MAX_SLOPE 40° · MAX_CLIMB 0.5 · AGENT_RADIUS 0.5 는 동물 목록이 미정이라 사실상 임시인데 주석에 「임시」가 없고 spec/04_life/hunting.md 「수치」에도 없다. 주석에 「임시」를 달고 수치 줄에 한 줄 더하라 (막는 것은 아니다)
- [x] 3. 도망간다
  - 기준: 체력 30% 아래면 도망간다 — 적대도 마찬가지다 (테스트)
  - 기준: 도망 속도가 평소보다 **뚜렷이 빠르다** (테스트)
  - 기준: (앞 단계 QA) 사람 몸이 걸을 때만 돈다 — game/player/movement_controls/player.gd 에서 _physics_process 끝의 `_turn_body(delta)` 가 새 함수 _footsteps 본문 맨 끝으로 딸려 들어갔다(181행). _footsteps 는 땅에 없거나 0.5 m/s 보다 느리면 일찍 return 하므로, 점프 중 · 물가 · 벽 앞에서 막혀 멈췄을 때 · 방향을 튼 직후 멈췄을 때 몸이 보는 쪽으로 돌지 않는다 (HEAD 에서는 늘 돌았다). `_turn_body(delta)` 를 _footsteps 에서 빼 _physics_process 의 `_footsteps(delta)` 다음 줄로 되돌려라. 테스트: 공중(점프 직후)에서 _facing 을 바꾸고 몇 프레임 뒤 몸(_body.basis)이 그쪽으로 돌았는지 보는 단언을 더하라
  - 기준: (앞 단계 QA) 가려진 사람도 보는 것이 테스트에 안 걸린다 — animal.gd _sees 의 광선 검사를 `return true` 로 바꿔도 tests/life/hunting 21개가 다 통과했다(QA 가 깨 보고 되돌렸다). test_hostile_does_not_bite_what_it_cannot_see 는 SIGHT 밖 거리만 본다. SIGHT 안(예: 8m)이지만 사이에 벽(StaticBody3D 상자)을 세운 사람을 적대가 CHASE 하지 않고 bit 도 없는지 보는 테스트를 더하라

## [x] G-135 웅크리기 · 배고픔 · 목마름
- spec: spec/02_player/hunger-thirst.md, spec/02_player/movement-controls.md, spec/12_ui/hud.md
- 결정 (사람 결정 2026-09-23): **달리기 키를 없앤다.** 기본이 달리기 속도고, 배고픔 · 목마름이
  바닥이면 느려진다 — 누르고 있는 달리기 키는 새끼손가락이 아프다. **굶어 죽지는 않는다**
- [x] 1. 웅크리기
  - 기준: **Ctrl 로 켜고 끈다**(토글). 웅크리면 느려진다 (테스트)
  - 기준: 웅크리면 **발소리가 나지 않아** 동물이 듣지 못한다 — 적대도 마찬가지다 (테스트)
  - 기준: **총소리는 웅크려도 난다** (테스트)
- [x] 2. 배고픔과 목마름
  - 기준: 둘 다 차 있으면 **달리기 속도**, 하나가 바닥이면 **걷기**, 둘 다면 거기서 **-30%** (테스트)
  - 기준: 바닥이어도 **죽지 않는다** (테스트)
  - 기준: 음식을 먹으면 배고픔이, 물을 마시면 목마름이 찬다 — 물은 연못 · 개울에서 마신다 (테스트)
  - 기준: **하루 세 끼 정도**로 준다 (하루 40분). 값은 한 곳에 있고 「임시」라고 적는다
  - 기준: 버프 음식은 **기본 속도 위에** 얹힌다 — 둘이 섞이지 않는다 (테스트)
  - 기준: (앞 단계 QA) test_crouch.gd 머리 주석이 없는 파일을 가리킨다 — game/tests/player/movement_controls/test_crouch.gd 3행이 발소리 · 총소리 테스트가 'tests/life/hunting/test_crouch_sound.gd' 에 있다고 적었는데, 그런 파일은 없다. 실제 테스트는 tests/life/hunting/test_animal_sound.gd 의 「웅크리기 (G-135.1)」 절에 있다. 주석의 경로를 그 파일로 고쳐라
- [x] 3. 동그란 게이지
  - 기준: 왼쪽 아래 **체력과 한자리**에 배고픔 · 목마름이 **동그란 게이지**로 보인다 (테스트)
  - 기준: **남은 양에 따라 색이 바뀐다.** 숫자는 없다 (테스트)
  - 기준: 글자가 있으면 번역 키다 (ko · en 둘 다)

## [x] G-131 부위별 사냥 — 머리 · 몸통 · 다리
- spec: spec/04_life/hunting.md
- 결정 (사람 결정 2026-09-23): 배율은 머리 1.3 · 몸통 1.0 · 다리 0.5(임시).
  **그 부위에 준 피해가 그 부위 산출을 깎는다** — 그래서 다리만 쏘면 뿔 · 가죽이 온전하다
- [x] 1. 부위를 맞힌다
  - 기준: 머리 · 몸통 · 다리가 따로 맞고, 부위마다 피해 배율이 다르다 (테스트)
  - 기준: **다리를 맞히면 느려진다** (테스트)
  - 기준: 배율은 데이터 한 곳에 있고 「임시」라고 적혀 있다
  - 기준: **조준하면** 그 동물의 **통짜 체력바와 마취 게이지**가 뜨고, 안 겨누면 사라진다 (테스트)
  - 기준: **마취가 걸리는 구간(체력 10% 아래)에서 체력바 색이 바뀐다** — 숫자 · % · 눈금은 없다 (테스트)
- [x] 2. 산출이 손상된다
  - 기준: 동물마다 부위 산출물이 데이터로 정해진다 (사슴: 머리 뿔 · 몸통 고기와 가죽 · 다리 없음) (테스트)
  - 기준: 그 부위에 준 피해만큼 **그 부위 산출이 준다** (테스트)
  - 기준: 다리만 쏴서 잡으면 머리 · 몸통 산출이 온전하다 (테스트)
- [x] 3. 도축과 점수표
  - 기준: 죽은 짐승에 다가가면 **도축 단추**가 뜨고, 누르면 산출이 나온다 (테스트)
  - 기준: 도축할 때 **부위별 몇 발 · 산출이 몇 %** 남았는지 보인다 (테스트)
  - 기준: 글자는 전부 번역 키다 (ko · en 둘 다)

## [x] G-132 마취탄과 포획
- spec: spec/04_life/hunting.md, spec/04_life/gathering.md, spec/04_life/ranching-breeding.md
- 결정 (사람 결정 2026-09-23): 마취탄은 **피해 0 · 속도를 늦춘다**. 체력 **10% 아래**에서만
  마취 게이지가 오른다. 약초 세 단(벨라도나 · 만드라고라 · 투구꽃)이 곧 마취탄 세 단이다
- [x] 1. 약초 세 종
  - 기준: 벨라도나 · 만드라고라 · 투구꽃이 난다. **벨라도나만 재배할 수 있다** (테스트)
  - 기준: 2 · 3단은 절벽의 발 디딜 틈에만 난다 (테스트)
  - 기준: 글자는 전부 번역 키다 (ko · en 둘 다)
- [x] 2. 마취탄을 만든다
  - 기준: **탄약 조합식 + 약초**로 마취탄 세 종을 만든다 (테스트)
  - 기준: 마취탄은 **아무 총에나** 들어간다. 맞아도 **피해가 0**이고 **속도가 준다** (테스트)
  - 기준: **R 을 꾹 누르면** 그 총에 맞는 탄만 뜨고 휠 · 숫자로 고른다 (테스트)
- [x] 3. 재워서 잡는다
  - 기준: 체력 **10% 위**에서는 마취 게이지가 오르지 않는다 (테스트)
  - 기준: 게이지가 다 차면 **포획된다** — 사살과 결과가 다르다 (테스트)
  - 기준: **투구꽃은 양을 넘기면 죽는다** — 포획하려다 사살이 된다 (테스트)
  - 기준: 동물마다 **재우는 데 드는 양이 다르다** (사슴 30 · 하급 마취탄 10 → 세 발) (테스트)
  - 기준: (앞 단계 QA) 탄을 바꿀 때 가방이 차면 탄이 사라진다 — Magazine.switch_to 가 남은 탄 · 쥔 탄을 bag.add 로 돌려놓는데, 가방이 가득 차 들어가지 못한 몫은 버려진다(구현 세션도 report 에 적었다). bag.add 가 돌려주는 넣은 수를 보고 못 넣은 몫이 있으면 바꾸지 않거나(false) 발밑에 떨어뜨리게 고치고, 가방을 꽉 채운 뒤 switch_to 해서 탄 총수가 줄지 않는지 단언하는 테스트를 test_tranq_ammo.gd 에 더하라
  - 기준: (앞 단계 QA) 핫바 칸에서 긴 탄 이름이 넘친다 — '벨라도나 마취탄' 같은 이름이 ItemSlot 옆 칸으로 넘친다(report 가 적은 것, '자동권총'도 같다). ItemSlot 라벨에 clip_text 나 줄임/글자 크기 맞춤을 두고, 가장 긴 ITEM_ 이름이 칸 너비를 넘지 않는지 보는 테스트를 짜라
- [x] 4. 들고 가기와 케이지
  - 기준: 재운 짐승을 **가방에 넣어 옮긴다** (테스트)
  - 기준: **마취 게이지가 계속 떨어지고**, 다 떨어지면 깨어나 도망간다 (테스트)
  - 기준: 깨는 데 걸리는 시간은 동물마다 다르다 — 값은 한 곳에 있고 「임시」다
  - 기준: **케이지**(소 · 중 · 대)를 만들어 가두면 깨어나도 도망가지 못한다 (테스트)
  - 기준: 제 덩치보다 작은 케이지에는 들어가지 않는다 (테스트)
  - 기준: (앞 단계 QA) 두 줄 이름이 칸 번호 · 개수와 겹친다 — 구현 세션이 report 에 적은 남은 틈(막는 것은 아니다). ui/hud/item_slot.gd 의 Item 라벨 상자가 칸 전체라 두 줄 이름이 왼쪽 위 Number · 오른쪽 아래 Count 글자와 겹친다. 이름 라벨의 위아래 여백을 숫자 높이만큼 두거나 숫자를 이름 밖으로 빼고, test_item_slot_fit.gd 에 Item 라벨 글자 줄 영역이 Number · Count 의 글자 영역과 겹치지 않는지 보는 단언을 더하라
  - 기준: (앞 단계 QA) item_slot.gd fit() 에 안 쓰는 변수 — fit() 안의 `var font := label.get_theme_font("font")` 가 쓰이지 않는다. 지워라

## [x] G-136 테스트가 왜 느린지 쟀다 — 고칠 것은 없었다
- **잰 것이 산출물이다** (사람 결정 2026-09-23). 「섬을 매번 새로 굽는다」는 전제로 열었는데,
  재 보니 **그런 곳이 한 곳도 없었다** — 없는 문제를 좇지 않게 여기서 닫는다
- 잰 결과 (2026-09-23):
  - 전체 **13~27분** — 같은 테스트가 맥 상태에 따라 **두 배** 흔들린다. 회차마다 도는 「바뀐 곳만」은 7~12분
  - **섬(TestIsland)을 테스트마다 다시 세우는 곳은 없다** — 134파일에서 17번, 전부 `before_all`
  - 무거운 쪽 둘: **저장본 없는 IslandShape 굽기**(한 번 **32초**, 저장본을 쓰면 0.2초)와
    **기다리기**(`wait_seconds` · 게임 시간 흐르기). 느린 열 개 중 여섯이 기다리기다
  - 가장 느린 파일: `test_island_shape` 139초(네 번 굽는다 — 그중 둘은 「같은 설계면 같은 섬」을
    확인하느라 **일부러** 두 번이다) · `test_animal_walk` 48초 · `test_tranq_ammo` 42초
- **재는 법** (다음 사람용): `TIMING=1 HARNESS_GODOT_TIMEOUT=3600 ./tools/test.sh | grep TIMING`
  — `game/tests/timing_hook.gd` 가 파일마다 걸린 초 · 섬을 몇 번 · 몇 초 세웠는지 느린 순으로 찍는다
- **왜 지금 안 고치나**: 다 고쳐야 15~20%인데 흔들림이 2배다. 기다리기를 줄이면 테스트가
  간헐적으로 실패하기 시작해 그 한 번이 아낀 것보다 비싸다
- **되살릴 조건**: 전체가 **40분**을 넘거나 회차마다 도는 검사가 **15분**을 넘으면 다시 본다.
  그때는 `TIMING=1` 로 다시 재는 것부터

## [x] G-137 저장이 빠지지 않게 — 왕복 검사
- spec: spec/01_settings/save.md
- 사람이 플레이해 보고 짚었다 (2026-09-23) — **껐다 켜면 서 있던 자리가 사라지고 탄창이 늘 가득이다.**
  의도가 아니라 spec 의 저장 목록에 아예 없었다. 오늘 하루에 들어온 건축 · 밭 · 동물 · 배고픔 중
  저장까지 챙긴 것은 절반뿐이다 — **빠뜨림이 구조적으로 생긴다**
- 결정 (사람 결정 2026-09-23): **저장은 담는 것이 기본**이고, 안 담을 것만 까닭을 적는다
- UI 묶음들보다 **먼저** 한다 — 지금도 새 기능이 계속 들어와서, 빨리 세울수록 새는 것이 적다
- [x] 1. 왕복 검사
  - 기준: 게임을 세우고 **상태를 여럿 바꾼 뒤**(움직이고 · 쏘고 · 먹고 · 짓고) 저장하고
    새로 불러오면 **값이 전부 같은지** 훑어 견주는 검사가 있다 (테스트)
  - 기준: **필드를 하나하나 적지 않는다** — 속성을 훑으므로 새 필드가 생기면 저절로 들어온다 (테스트)
  - 기준: 지금 안 담기는 것을 **목록으로 뱉는다** (고치는 것은 다음 단계)
- [x] 2. 빠진 것을 담는다
  - 기준: **어디 서 있었나**(섬 + 좌표)가 남는다 — 나갔다 들어오면 그 자리다 (테스트)
  - 기준: **총마다 남은 탄**이 남는다 — 껐다 켠다고 공짜 재장전이 되지 않는다 (테스트)
  - 기준: **배고픔 · 목마름 · 기력 · 체력**이 남는다 (테스트)
  - 기준: 단계 1의 왕복 검사가 **아무것도 못 뱉는다** — 빠진 것이 없다 (테스트)
  - 기준: (앞 단계 QA) 뱉는 목록에 값이 <없음>으로 찍히는 줄 — test_round_trip.gd 의 _at() 이 사전 키를 문자열로만 찾아서, Vector2i 키(Island/Farmland:_fields.(-397, 205))는 전·후 모두 <없음>으로 찍힌다(QA 가 섬 복원을 깨 봤을 때 드러났다 · 판정에는 영향 없다). _at 에서 키를 찾을 때 (v as Dictionary).keys() 를 돌며 str(key) == parts[i] 인 것을 고르게 하라
  - 기준: (앞 단계 QA) Island/Resources:_by_owner 가 목록에 든 까닭이 불확실하다 — 주석은 「캐릭터 가까운 칸만 짓는다」지만, 섬 복원을 끄자 이 줄이 차이에서 사라졌다(= 섬 상태와 얽혀 있다). 다음 단계에서 담을지·안 담을지 정할 때 _by_owner 의 해시가 무엇 때문에 다른지(인스턴스 id 가 섞이는지, 캔 자원이 다시 서는지) 확인하고 주석을 바로 적어라
- [x] 3. 다시 빠지지 않게
  - 기준: 일부러 안 담는 것은 **「안 담는다」 목록**에 까닭과 함께 적혀야 검사가 통과한다 (테스트)
  - 기준: 그 목록에 없는 새 상태가 안 담기면 **검사가 실패한다** — 일부러 하나 빠뜨려 확인한다 (테스트)
  - 기준: (앞 단계 QA) 죽은 채 저장하면 체력 1로 살아난다 — Health.set_left 가 clampi(value, 1, max) 로 0 을 1 로 올린다(game/combat/damage_death/health.gd). 죽음 처리 중 저장·종료되면 데스 상자 없이 체력 1로 되살아날 수 있다(막는 것은 아니다). 죽은 상태 저장을 어떻게 다룰지 확인하고(죽음이 저장 전에 늘 치러지는지), 체력 0 저장본을 불러오는 테스트를 하나 두어라
- [x] 4. (앞 단계 QA) 죽은 채 저장 결론이 spec 에 없다
  - 기준: 죽은 채 저장 결론이 spec 에 없다 — 「죽음은 take 안에서 치러져 저장본 체력은 0 이 될 수 없고, 0 이 들어오면 1 로 산다」는 결론이 health.gd 주석과 test_dead_save.gd 머리말에만 있다. spec/01_settings/save.md 의 체력 줄(31줄 「체력을 그대로 담을지…」 근처)에 한 줄로 적어 두어라 (막는 것은 아니다)

## [x] G-138 개발용 상자에 전부 들어간다
- spec: spec/01_settings/world-settings.md
- 사람이 플레이해 보고 짚었다 (2026-09-23) — **개발용 상자에 아이템이 계속 안 들어간다.**
  지금은 캐면 나오는 것(Harvest.DROPS) · 농사(Crops.CROPS) · 도구(tools.json)만 담는다.
  그래서 **제작으로만 나오는 것이 통째로 빠졌다** — 총알 네 종 · 마취탄 세 종 · 갈고리 탄 ·
  널빤지 · 석재 블록 · 철괴 · 건축물 여섯 종(나무 · 석재) · 제작대 다섯 · 케이지 셋 · 밭 · 비료 · 약초 셋
- **저장과 같은 병이다** — 「담을 것을 하나씩 적는」 방식이라 새 기능이 들어올 때마다 조용히 빠진다.
  목록을 적지 말고 **만들 수 있는 것 전부에서 끌어온다**
- [x] 1. 만들 수 있는 것이 다 들어간다
  - 기준: **레시피(recipes.json)의 모든 output** · 캐면 나오는 것 · 농사 · 도구 · 등반 장비가
    개발용 상자에 들어간다 — **목록을 손으로 적지 않는다** (테스트)
  - 기준: **하나라도 빠지면 테스트가 실패한다** — 레시피에 한 줄 더해도 저절로 들어오는지
    일부러 새 레시피를 넣어 확인한다 (테스트)
  - 기준: 상자 창이 **화면을 넘지 않는다** — 칸이 많아지면 스크롤된다 (테스트).
    찍어서 눈으로도 확인한다 (`harness/shot.sh --ui <폴더> chest`)
- [x] 2. (앞 단계 QA) spec 의 개발용 상자 줄 두 곳이 낡았다
  - 기준: spec 의 개발용 상자 줄 두 곳이 낡았다 — spec/01_settings/world-settings.md 13줄 「모든 자원 · 도구가 든 상자」와 26줄(확인 기준) 「모든 자원 · 도구가 든 상자가 선다」가 16줄의 사람 결정(2026-09-23, 만들 수 있는 것 전부)과 어긋난다. 두 줄을 「만들 수 있는 것 전부가 든 상자」로 고쳐라. 17줄에도 약초 · 레시피 재료가 담긴다는 것을 더하면 코드와 맞는다 (사람 결정 줄 자체는 지우지 말 것)
  - 기준: (앞 단계 QA) 개발용 상자 창이 조작 안내 글을 가린다 — 칸이 많으면 창이 $Center 높이를 꽉 채워 화면 맨 위(y=0)까지 올라가, 왼쪽 위 HUD 안내 「… 클릭 다시 잡기」 끝부분을 덮는다 (harness/shot.sh --ui <폴더> chest 로 보인다). 화면을 넘지는 않아 기준은 통과. chest_window.gd _fit 의 room 에 위쪽 여백(예: 안내 줄 높이)을 빼 두거나 Center 에 위 여백을 주고, test_chest_window_scroll.gd 에 창 위끝이 0 보다 아래인지 보는 단언을 더해라

## [x] G-139 데스 상자 — 한눈에 알아보고 남은 시간이 보인다
- spec: spec/08_combat/damage-death.md, spec/12_ui/hud.md
- 사람이 플레이해 보고 짚었다 (2026-09-23) — **데스 상자가 보관 상자와 똑같이 생겼다.**
  실제로 크기(0.7 · 0.6 · 0.7)도 색(0.45 · 0.31 · 0.18)도 **같은 값**이다.
  **남은 시간도 코드에만 있고**(death_chest.gd `seconds_left()`) 화면에 보여 주는 곳이 없다
- 30분이 지나면 상자와 안의 것이 **같이 사라진다** (사람 결정 2026-09-16) — 남은 시간을 모르면
  잃고 나서야 안다. 여는 동안에는 시간이 멈춘다는 것도 보여야 급하게 굴지 않는다
- [x] 1. 한눈에 다르고, 남은 시간이 보인다
  - 기준: 데스 상자가 보관 상자 · 개발용 상자와 **한눈에 구분된다** — 색이나 표시가 다르다 (테스트)
  - 기준: **조준하면 남은 시간이 보인다** (테스트)
  - 기준: 창을 열면 창에도 남은 시간이 보이고, **여는 동안 멈춘다**는 것이 드러난다 (테스트)
  - 기준: 글자는 전부 번역 키다 (ko · en 둘 다)
  - 기준: 찍어서 눈으로 확인한다 (`harness/shot.sh --ui <폴더> chest`)
  - 색은 임시다 — 사람이 보고 정한다
- [x] 2. (앞 단계 QA) 찍기 도구에 디버그 print 가 남았다
  - 기준: 찍기 도구에 디버그 print 가 남았다 — game/core/debug/ui_shots.gd 139줄 print("DBG ", player._control_locks, …) 가 남아 있고 남의 private 변수(_control_locks)까지 읽는다. 그 줄을 지워라
  - 기준: (앞 단계 QA) death_aim 자리가 세운 것을 안 치운다 — ui_shots.gd _aim_death_chest 가 ShotGround · ShotDeath · StorageChest · DevBox 를 stations 가 아니라 game 에 붙여 _reset 이 치우지 않는다. 뒤 자리(menu · ammo)를 chest 와 같이 찍으면 상자 셋과 바닥이 배경에 남는다. 자리 끝에서 free 하거나 stations 에 붙여 _reset 이 치우게 하라
  - 기준: (앞 단계 QA) 창의 '멈춰 있다' 테스트가 멈춤을 지키지 않는다 — test_chest_window_death.gd test_the_number_in_the_window_does_not_move_while_open 은 창이 _rebuild 때만 글자를 쓰므로 DeathChest.tick 이 열린 상자를 줄여도 통과한다. chest.seconds_left() 가 tick(120) 뒤에도 300 그대로인지 함께 단언하라

## [x] G-129 UI 바탕 — Theme · 재는 검사 · 찍는 폭
- **자가 피드백**: 검사가 깨끗해질 때까지 단계를 계속 붙인다 (사람 결정 2026-09-23) —
  고치고 → 다시 훑고 → 또 고친다. 맨 뒤 묶음으로 미루지 않는다
- spec: spec/12_ui/hud.md, spec/12_ui/menu.md, spec/01_settings/display.md, LOOP.md 5절
- **UI 를 고치기 전에 바탕을 세운다** (사람 결정 2026-09-23). 순서가 중요하다 —
  Theme 없이 검사를 짜면 흩어진 값에 맞춰 검사가 만들어져, Theme 을 넣을 때 검사도 다시 손봐야 한다
- 화면은 **1280×720 · 16:9 고정**이다 (project.godot · spec/01_settings/display.md 사람 결정 2026-09-17).
  해상도를 여러 개 볼 필요가 없다 — 「기준 해상도 안에 잘리지 않고 들어가나」 하나만 보면 된다
- 찍는 법: `harness/shot.sh --ui <폴더> [hud bag workbench smelter chest menu]` → **PNG 를 열어 보고** 고친다
- 나중에 사람이 ComfyUI 로 뽑은 아이콘을 끼워 넣는다 — Theme 이 서 있어야 갈아 끼워도 일관되게 보인다
- [x] 1. Theme 한 곳에 모은다
  - 기준: 색 · 여백 · 칸 크기 · 글꼴이 **Theme 리소스 한 곳**에 모인다 (지금은 Theme 을 하나도 안 쓴다)
  - 기준: 같은 값이 코드와 `.tscn` 두 곳에 적혀 있던 것이 사라진다 (막대 폭이 그랬다) (테스트)
  - 기준: 창들이 그 Theme 을 쓴다 — 글자 크기 · 칸 크기가 창마다 제각각이지 않다 (테스트)
  - 기준: 고치기 전후를 찍어 눈으로 확인한다 (bag · workbench · chest)
- [x] 2. 재는 검사
  - 기준: 열리는 창을 **전부** 열어 놓고 잰다 — ① **1280×720 안에 들어오나**
    ② 서로 **겹치나**(창이 핫바를 가린다) ③ 글자가 **칸을 넘나** (테스트)
  - 기준: **창 목록을 손으로 적지 않는다** — 새 창을 더하면 저절로 검사에 들어온다 (테스트)
  - 기준: 지금 어긋난 곳은 **목록으로 뱉기만** 한다. 고치는 것은 G-901 의 몫이다
  - 기준: (앞 단계 QA) 제작대 창이 화면 아래로 넘친다 — /tmp/g129/after/workbench.png 에서 레시피 목록이 화면 아래로 잘린다(돌격소총탄 줄이 반쯤 잘림). station_window 목록을 ScrollContainer 로 감싸거나 창 높이를 화면 안에 맞추고, 창 Rect 가 get_viewport_rect() 안에 드는지 단언하는 테스트를 이 묶음의 재는 검사 단계에 넣어라
  - 기준: (앞 단계 QA) 맨손 제작 줄이 핫바를 덮는다 — /tmp/g129/after/bag.png 에서 「횃불」 버튼이 핫바 5·6번 칸 위에 겹친다. 가방 창(bag.tscn Center/Box)의 아래 끝이 핫바 위 끝보다 위인지 재는 테스트를 짜고 배치를 고쳐라
- [x] 3. 찍는 폭
  - 기준: **한국어 · 영어 둘 다** 찍는다 — 영어가 길어 칸을 넘치는 것이 흔하다
  - 기준: **만지는 중**을 찍는다 — 칸을 끌고 있는 중 · 재료가 모자란 줄
  - 기준: **섬 위에서의 HUD** 도 찍는다 — 회색 배경이 아니라 진짜 배경 위에서 읽히는지 본다
  - 기준: (앞 단계 QA) 핫바 높이 80 이 두 곳에 적혀 있다 — 테마 Window/hotbar_room=80 과 game/ui/hud/hotbar.tscn 의 offset_top=-80.0 이 따로 산다(단계 1 이 없앤 「같은 값 두 곳」이 다시 생겼다). hotbar.gd _ready 에서 offset_top = -UiTheme.window(&"hotbar_room") 로 걸고 .tscn 값은 지우거나, 두 값이 같은지 단언하는 테스트를 tests/ui/layout 에 넣어라
  - 기준: (앞 단계 QA) 재는 검사 테스트가 고아 노드 31개를 남긴다 — test.log 의 test_station_windows_fit_the_screen 뒤에 「31 Orphans」(Button 줄들). station_window._rebuild 가 queue_free 전에 떼어낸 줄이거나 테스트가 st.free() 로 창보다 먼저 제작대를 지운 탓으로 보인다. 원인을 찾아 없애고 after_each 에서 assert_no_new_orphans 로 지켜라
- [x] 4. (앞 단계 QA) 모자란 줄이 눈으로 안 갈린다
  - 기준: 모자란 줄이 눈으로 안 갈린다 — workbench_en.png 에서 Match pistol(철괴 10 필요, 가방 6)처럼 disabled 인 줄도 다른 줄과 똑같이 흰 글자다. 레시피 글자가 Button 글자가 아니라 자식 Label 이라 disabled 색을 안 탄다(station_window.gd 레시피 줄 만드는 곳). 테마에 모자란 줄 글자색을 두고 disabled 일 때 Label 에 걸어라. 테스트: 모자란 줄의 Label 색이 고를 수 있는 줄과 다른지 단언
  - 기준: (앞 단계 QA) 영어에서 철광석·철괴 칸이 둘 다 Iron — drag_en · workbench_en 핫바/가방 칸이 'Iron' 'Iron' 으로 같아 보인다(칸 글자 줄임). 줄여도 둘이 갈리게 칸 이름 줄임 규칙이나 짧은 이름 키를 두고, 가방 칸 글자가 아이템마다 다른지 단언하는 테스트를 넣어라
  - 기준: (앞 단계 QA) 섬 위 HUD 글자가 풀밭에서 흐리다 — island_en.png 의 알림 한 줄 · Hunger/Thirst 글자가 밝은 풀 위에서 잘 안 읽힌다. 테마 Label 에 그림자(shadow_color · shadow_offset)나 바탕 판을 걸어라

## [x] G-901 UI 개선 — 창을 훑어 고친다
- spec: spec/12_ui/hud.md, spec/12_ui/menu.md, spec/01_settings/display.md
- **자가 피드백**: 검사가 깨끗해질 때까지 단계를 계속 붙인다 (사람 결정 2026-09-23) —
  고치고 → 다시 훑고 → 또 고친다. 맨 뒤 묶음으로 미루지 않는다
- **단계를 미리 적어 두지 않는다** (사람 결정 2026-09-23) — 한 회차에 다 못 고치면
  그 회차 QA 가 남긴 것이 다음 단계로 붙는다. 창이 몇 개든 깨끗해질 때까지 이어진다
- 도구는 G-129 가 세워 둔다: **재는 검사**(1280×720 안인가 · 겹치나 · 글자가 칸을 넘나)와
  `harness/shot.sh --ui <폴더> [hud bag workbench smelter chest menu]` — **찍은 PNG 를 열어 보고** 고친다
- 사람이 2026-09-22 에 찍어 본 것 (여기서 시작하면 된다):
  - 제작 창이 레시피 22줄에 위아래로 화면을 넘쳐 **출력 버퍼가 잘리고 핫바를 통째로 가린다**
  - 상자 창과 가방이 붙어 있어 어디가 어디인지 구분이 없고, 「맨손으로 만드는 것」의
    **「횃불」 단추가 핫바에 겹쳐 잘린다**
  - 칸 안에서 이름과 개수가 흩어지고, **재료가 모자란 줄이 눈에 안 띈다**
- 고치는 방향 (참고 — 그대로 할 필요는 없다):
  - 패널 높이에 상한 · 목록만 스크롤 · **남은 시간과 버퍼는 스크롤 밖 고정**
  - 창을 핫바 위쪽에 놓아 겹치지 않게 · 상자와 가방을 한 패널에 위아래로, 제목은 제 격자 바로 위
  - 칸: 이름은 위 한 줄, 개수는 오른쪽 아래 구석. 빈 칸은 테두리만, 찬 칸은 배경을 밝게
  - **색만으로 구분하지 않는다** — 모자란 줄은 색 + 표시(점 · 빗금). 색각 이상이 스무 명에 한 명이다
- [x] 1. 걸린 것을 고친다
  - 기준: **재는 검사가 아무것도 뱉지 않는다** — 화면 밖 · 겹침 · 글자 넘침이 없다 (테스트)
  - 기준: 찍어서 눈으로도 확인한다 (workbench · bag · chest · hud · menu) — 무엇을 고쳤는지 report 에 적는다
  - 기준: 한 회차에 다 못 고쳤으면 **남은 것을 QA 가 적는다** — 다음 단계로 붙는다
- [x] 2. (앞 단계 QA) 창 바탕이 비쳐 캐릭터가 보인다
  - 기준: 창 바탕이 비쳐 캐릭터가 보인다 — bag · chest · workbench 사진에서 가방 칸 뒤로 캐릭터(갈색 캡슐)가 비친다. 창 바탕 테마 알파 0.55 때문이다(report). 창 패널 StyleBox 의 bg_color 알파를 올려(예: 0.9, 임시라고 적고) 다시 찍어 캐릭터가 안 비치는지 본다. 검사로 지키려면 ui_measure 쪽에 창 패널 바탕 알파가 기준 이상인지 보는 단언을 더한다
  - 기준: (앞 단계 QA) 영어 이름이 줄바꿈되면 개수가 글자에 붙는다 — bag_en.png 에서 「Iron ingot」 · 「Iron ore」가 두 줄로 접히고 개수 6 · 12 가 둘째 줄 글자 바로 옆에 붙어 읽기 어렵다(겹침 검사엔 안 걸린다). 칸 이름 글자 크기를 줄여 한 줄에 들게 하거나 개수 라벨을 칸 오른쪽 아래 모서리에 여백을 두고 두어, ko · en 사진 모두에서 이름과 개수가 떨어져 보이게 한다
  - 기준: (앞 단계 QA) 상자 창 안내 줄이 창 가장자리에 붙는다 — chest_ko.png 의 「칸을 끌어다 놓아 가방과 주고받는다 · 우클릭으로 닫는다」가 창 폭 끝에서 끝까지 닿아 좌우 여백이 없다(데스 상자 창도 같다). 창 패널에 안쪽 여백(content_margin 또는 MarginContainer)을 두어 글자가 창 테두리에서 떨어지게 한다
- [x] 3. (앞 단계 QA) 제작대 판 뒤로 캐릭터가 아주 희미하게 남는다
  - 기준: 제작대 판 뒤로 캐릭터가 아주 희미하게 남는다 — 알파 0.96 이라 workbench_ko.png 를 확대하면 「펌프 샷건 · 반자동 샷건」 줄 오른쪽(x≈760~850, y≈480~590)에 캐릭터 윤곽이 4% 남아 보인다. 보통 크기로는 거의 안 보여 기준은 통과로 쳤다. 완전히 가리려면 game/ui/theme/naru_theme.tres 의 window_panel bg_color 알파를 1.0 으로 올리고 MIN_BACK_ALPHA 는 그대로 둔다
  - 기준: (앞 단계 QA) 가장 긴 한국어 이름은 틈 없이 맞춘다 — ItemSlot.fit 은 DIGIT_GAP 을 둔 채로 안 드는 이름(벨라도나 마취탄 · 볼트액션 저격총 · 만드라고라 마취탄)을 틈 0 으로 되돌린다. test_in_korean_names_keep_off_the_count 의 STOCK 에는 그런 이름이 없어 이 경우는 지키지 않는다. 그 이름들로 칸을 채워 찍어 보고, 개수와 붙어 읽히면 칸 이름 글자 최소 크기(MIN_FONT_SIZE)나 두 줄 배치를 손본다

## [x] G-140 건축 손보기 — 사람이 플레이해 보고 짚은 셋
- spec: spec/06_build/building.md, spec/01_settings/input.md
- **자가 피드백**: 깨끗해질 때까지 단계를 계속 붙인다
- 사람이 플레이해 보고 짚었다 (2026-09-24, 사진 둘):
  토대가 **지면을 따라 들쭉날쭉**하게 깔린다 · 맞닿은 변에 **겹벽**이 선다 · 계단이 **칸에 안 맞는다**
- [x] 1. 토대가 옆에 붙는다
  - 기준: 이미 놓인 토대가 곁에 있으면 **그 변에 먼저 붙는다** — 땅보다 우선이다 (테스트)
  - 기준: **마우스 휠로 높낮이를 올리고 내린다** — 건축물을 들고 있는 동안에만 (테스트)
  - 기준: **땅에서 5m 를 넘으면 놓이지 않는다** — 옆에 붙이는 중이어도 (테스트)
  - 기준: 찍어서 눈으로 확인한다 — 토대 여럿이 **수평으로 이어진다**
- [x] 2. 겹벽이 서지 않는다
  - 기준: 토대 둘이 맞닿은 선에는 **벽이 하나만** 선다 — 양쪽에서 하나씩 놓이지 않는다 (테스트)
  - 기준: 이미 벽이 선 변에는 미리보기가 **놓을 수 없는 색**이다 (테스트)
  - 기준: (앞 단계 QA) input.md 휠 줄이 구현과 어긋난다 — spec/01_settings/input.md 19줄은 「건축물을 들고 있으면 놓을 높낮이」인데 Buildings._holding_foundation 은 토대일 때만 휠을 받는다 (building.md 14줄은 토대 항목 아래라 구현이 맞다). input.md 줄을 「토대를 들고 있으면」으로 좁혀 적고(사람 결정 줄이니 뜻은 그대로 두고 괄호로 「벽 · 기둥 등은 변 · 바닥에 스냅되어 높낮이가 없다」를 덧붙인다), test_the_wheel_does_nothing_unless_a_foundation_is_in_hand 에 벽(wood_wall)을 든 경우도 넣어 raise()==0 을 단언하라 — 지금은 재료(wood)와 빈손만 본다
  - 기준: (앞 단계 QA) shots.gd _set_up_row 에 print 가 남았다 — game/core/debug/shots.gd 의 print("ROW …") 는 확인용 출력이다. 다른 자리 설정 함수처럼 조용히 두거나, 남길 거면 까닭 주석을 달아라
- [x] 3. 계단이 한 장에 들어가고 아래가 뚫린다
  - 기준: 계단이 **토대 한 장(3×3칸)** 안에 들어간다 — 지금은 두 장을 먹는다 (테스트)
  - 기준: **계단 아래로 걸어 지나갈 수 있다** — 속이 꽉 찬 쐐기가 아니다 (테스트). 난간은 없다
  - 기준: 걸어서 위층에 오르는 것이 그대로 된다 (테스트)
  - 기준: **계단 밑을 지나다녀도 층이 새지 않는다** — 방 판정은 그대로다 (테스트)
  - 기준: 찍어서 눈으로 확인한다 — 칸 밖으로 안 삐져나오고 밑이 비어 보인다
- [x] 4. (앞 단계 QA) building.md 계단 결정 줄의 「지금은」이 낡았다
  - 기준: building.md 계단 결정 줄의 「지금은」이 낡았다 — spec/06_build/building.md 18~20줄이 아직 「지금은 두 장을 먹고 속이 꽉 찬 쐐기라 계단 밑이 죽은 공간이다」라고 말한다. 사람 결정 문장(「계단은 토대 한 장(3×3칸)에 들어가고, 아래가 뚫려 있다 (사람 결정 2026-09-24)」 · 난간 없음 · 아트 단계)은 그대로 두고, 「지금은 …」 절만 「(G-140.3 에서 한 장 · 얇은 비탈 판으로 바꿨다)」처럼 지난 일로 고쳐라
  - 기준: (앞 단계 QA) _stairs_edges 가 늘 빈 목록을 돌려준다 — game/build/buildings.gd 의 _stairs_edges 는 STAIRS_PLATES=1 이 된 뒤로 쓰이는 데가 없다(주석에 「다시 늘면 쓰인다」). 한 장 계단이 정해졌으니 함수와 부르는 곳을 지우거나, 남길 거면 테스트 하나로 한 장일 때 빈 목록임을 못박아라

## [x] G-142 저장이 새지 않게 — 갈아 끼우기 · 창 닫기 · 자동 저장
- spec: spec/01_settings/save.md
- 사람이 플레이해 보고 짚었다 (2026-09-24) — **게임을 완전히 껐다 켜면 인벤토리가 사라진다.**
  확인해 보니 저장이 **메인 화면으로 나갈 때와 「게임 끄기」를 누를 때만** 일어난다
  (`game_root.gd` `_leave_to_main_menu` · `_quit_game`). **창의 X 나 ⌘Q 는 저장 없이 꺼진다**
- **순서가 중요하다** — 3 → 1 → 2 가 아니라, **갈아 끼우기를 먼저** 한다.
  자주 저장하게 만들면 **덮어쓰다 죽을 기회도 그만큼 늘기** 때문이다
- 저장이 새면 플레이 QA 자체가 안 되므로 목록 맨 앞에 둔다
- [x] 1. 갈아 끼우듯 저장한다
  - 기준: 저장본에 **바로 덮어쓰지 않는다** — 임시 파일에 다 쓴 뒤 이름을 바꾼다 (테스트)
  - 기준: **쓰다 말고 죽어도 옛 저장본이 멀쩡하다** — 임시 파일만 남기고 일부러 끊어 확인한다 (테스트)
  - 기준: 저장에 실패하면 옛 저장본을 지우지 않는다 (테스트)
- [x] 2. 창을 닫을 때도 저장한다
  - 기준: **창의 X 로 끄면 저장된다** — 자동 종료를 끄고 닫기 알림을 직접 받는다 (테스트)
  - 기준: 저장이 끝난 뒤에 꺼진다 — 쓰다 만 채로 죽지 않는다 (테스트)
  - 기준: 저장에 실패해도 꺼진다 — 못 끄고 갇히는 것이 더 나쁘다 (테스트)
- [x] 3. 자동 저장
  - 기준: **하루가 바뀔 때 · 섬을 옮길 때 · 죽었을 때 · 5분마다** 저장한다 (테스트)
  - 기준: 매 순간 저장하지 않는다 — 주기는 값 한 곳에 있고 「임시」라고 적는다
  - 기준: 저장하는 동안 조작이 멈추지 않는다 (테스트)
  - 기준: (앞 단계 QA) save.md 의 창 닫기 줄이 낡았다 — spec/01_settings/save.md 21줄 「창을 닫을 때(X · ⌘Q) — 지금은 이 길에서 저장이 안 된다」가 G-142.2 로 틀린 말이 됐다. 이 줄은 「사람 결정 2026-09-24」 목록 안이므로 줄을 지우지 말고 「지금은 이 길에서 저장이 안 된다」 부분만 「(한다 — 게임 안에서는 자동 종료를 끄고 닫기 알림을 받아 저장 뒤 끈다 · 메인 화면은 그냥 꺼진다)」처럼 고쳐라

## [x] G-143 크기 시험 — 4×4 토대와 2×2 밭을 하나씩
- spec: spec/06_build/building.md, spec/04_life/farming.md
- 사람이 플레이해 보고 「밭이 너무 잘다」고 했다 (2026-09-24). 지금은 **토대 3×3칸 · 밭 1칸**이라
  한 장에 밭이 아홉이다. **4×4 토대에 2×2 밭이면 네 장**이라 훨씬 시원하다 — 숫자도 딱 맞는다
- **바꾸는 것이 아니라 재 보는 것이다** — 지금 것(3×3 토대 · 1칸 밭)은 **그대로 두고**,
  **임시로 하나씩만** 더해 찍어 본다. 사람이 그림을 보고 정한다 (루프는 「보기 좋은가」를 판단하지 못한다)
- 고르고 나면 이 묶음은 닫고, 정해진 크기로 바꾸는 일은 따로 올린다
- [x] 1. 하나씩 만들어 찍는다
  - 기준: **임시 4×4 토대**를 하나 놓을 수 있다 — 지금 3×3 토대는 그대로 남는다 (테스트)
  - 기준: **임시 2×2 밭**을 하나 놓을 수 있다 — 지금 1칸 밭은 그대로 남는다 (테스트)
  - 기준: 4×4 토대 위에 2×2 밭이 **정확히 네 장** 들어간다 (테스트)
  - 기준: `harness/shot.sh` 로 **나란히 찍는다** — 3×3+1칸 과 4×4+2×2 를 한눈에 견줄 수 있게.
    찍은 자리 이름을 report 에 적는다
  - 기준: 임시라고 코드에 적는다 — 사람이 고르면 하나는 지운다

## [x] G-144 지붕은 받치는 것 바로 위만
- spec: spec/06_build/building.md
- **자가 피드백**: 깨끗해질 때까지 단계를 계속 붙인다
- 사람 결정 2026-09-24 — 지금은 벽 하나가 지붕 **6장**, 기둥 하나가 **9장**을 받친다. 너무 멀리 뻗는다.
  **벽은 2장(1×2, 양쪽 한 장씩) · 기둥은 제 판 1장**으로 줄인다. 넓게 덮으려면 그만큼 받쳐야 한다
- [x] 1. 장 수를 줄인다
  - 기준: **벽 하나가 받치는 지붕은 두 장** — 벽이 선 그 자리에서 양쪽으로 한 장씩 (테스트)
  - 기준: **기둥 하나가 받치는 지붕은 한 장** — 기둥이 선 제 판만 (테스트)
  - 기준: 문도 벽과 똑같다 (테스트)
  - 기준: 받침이 없어진 지붕은 그대로 무너진다 — 연쇄 붕괴가 새 장 수로도 맞는다 (테스트)
  - 기준: 방 판정이 새 장 수로도 맞는다 — 사방과 위가 막히면 방이다 (테스트)
  - 기준: 찍어서 확인한다 — 벽 한 장 · 기둥 한 개를 세우고 지붕 미리보기가 딱 그만큼만 초록이다
- [x] 2. 계단도 지붕을 받친다
  - 사람이 플레이해 보고 「계단에 지붕이 안 붙는다」고 했다 (2026-09-24).
    지금 `roof_plates()` 는 기둥 · 벽 · 문만 보고 계단은 빈 배열이라, 계단 둘레에 지붕이 안 올라간다
  - 기준: **계단이 올라가 닿는 판 한 장**에 지붕이 놓인다 — 계단이 받친다 (테스트)
  - 기준: **계단이 선 제 판 위는 뚫린 채**다 — 거기가 2층으로 올라가는 구멍이다.
    계단 판 위에 지붕을 놓으려 하면 안 놓인다 (테스트)
  - 기준: 계단을 부수면 그것이 받치던 지붕이 무너진다 (테스트)
  - 기준: 계단으로 2층에 올라가는 것이 그대로 된다 — 머리가 막히지 않는다 (테스트)
  - 기준: 찍어서 확인한다 — 계단 올라간 자리에 지붕이 덮이고 계단 위는 뚫려 있다

## [x] G-141 총을 쏘는 맛 — 연사 딜레이 · 거리 · 반동 · 무게
- spec: spec/08_combat/tools-as-weapons.md, spec/02_player/inventory-hotbar.md
- **자가 피드백**: 깨끗해질 때까지 단계를 계속 붙인다
- 지금 열여섯 자루가 **탄 수와 재장전만 다르다** — 피해 · 연사 · 거리 · 퍼짐 · 반동 · 소리가 없다.
  사람이 표를 정해 spec 에 올렸다 (2026-09-24, 전부 임시)
- [x] 1. 연사 딜레이와 자동
  - 기준: 좌클릭을 아무리 빨리 눌러도 **총마다 정해진 간격**보다 빨리 안 나간다 (테스트)
  - 기준: **돌격소총 3 · 4단만** 꾹 누르면 이어 나간다. 나머지는 눌러야 한 발이다 (테스트)
  - 기준: 값은 데이터 한 곳(tools.json)에 있고 「임시」라고 적는다
- [x] 2. 피해 · 거리 · 퍼짐 · 반동
  - 기준: **총마다 피해가 다르고** 부위 배율이 곱해진다 (테스트)
  - 기준: **유효 거리 밖에서 피해가 줄고**, 산탄총은 급하게 준다 (테스트)
  - 기준: 산탄총은 **알이 여럿** 나간다 (테스트)
  - 기준: 퍼짐이 **조준 절반 · 걸으며 1.5배 · 웅크리면 0.6배** (테스트)
  - 기준: **반동**으로 총구가 올라가고 안 쏘면 돌아온다. 연사는 발마다 쌓인다 (테스트)
  - 기준: 저격총 탄속만 400m/s 다 (테스트)
  - 기준: (앞 단계 QA) 좌클릭 도우미가 여섯 파일에 복사됐다 — _release_click() 과 「ready_in() 이 0 이 될 때까지 기다림」이 test_magazine · test_per_gun_magazine · test_attack · test_bullet_trace · test_ammo_label · test_ammo_label_by_gun 에 똑같이 붙었다. 다음에 좌클릭 규칙(반동 등)이 바뀌면 여섯 곳을 고쳐야 한다. game/tests/ 아래 공용 도우미(예: tests/combat/tools_as_weapons/click_helper.gd 의 static func click(tree, attack))로 모아 여섯 파일이 부르게 하라 (막는 것은 아니다)
- [x] 3. 한 발씩 장전
  - 기준: 「한 발씩」인 총은 **아무 때나 끊고 쏠 수 있다** — 두 발만 넣고 쏘는 것이 된다 (테스트)
  - 기준: 「통째로」인 총은 다 찰 때까지 못 쏜다 (테스트)
  - 기준: **총은 닳지 않는다** — 내구도가 없다 (테스트)
  - 기준: (앞 단계 QA) spec 「미정」 줄이 낡았다 — spec/08_combat/tools-as-weapons.md:101 「재장전 시간 · 조준 배율 · 흩어짐 · 탄착 남는 시간 미정」인데 조준 배율(0.5) · 흩어짐(총마다 퍼짐)은 2026-09-24 표로 정해졌다. 그 둘을 빼고, 대신 이번에 지어낸 반동 되돌림 값(Attack.RECOIL_HOLD 0.15초 · RECOIL_RETURN 20°/초 · RECOIL_MAX 15°)과 Projectile.max_range(유효 거리×2, 최소 60m)가 미정이라고 한 줄 적어라. 사람 결정 줄은 건드리지 마라
  - 기준: (앞 단계 QA) 기다림 루프가 도우미 밖에 두 곳 더 있다 — game/tests/combat/tools_as_weapons/test_aiming.gd:122 이 「ready_in() > 0 or recoil_degrees() > 0」을 따로 돌고, game/tests/settings/save/test_round_trip.gd:309 는 ready_in() 만 기다린다(반동은 안 기다림). 둘 다 click_helper.gd 의 Click.click 을 부르게 하거나, 기다림만 떼어 click_helper 에 static settle(tree, attack) 로 두고 부르게 하라
- [x] 4. 무게가 속도를 바꾼다
  - 기준: **모든 아이템에 무게**가 있다 — 데이터에 없으면 테스트가 실패한다 (테스트)
  - 기준: 가진 무게가 기준을 넘으면 **1kg 마다 -1%, 최대 -40%** 로 느려진다 (테스트)
  - 기준: **배고픔 · 목마름이 정한 속도에 곱해진다** — 둘이 섞이지 않는다 (테스트)
  - 기준: **칸 제한은 그대로다** — 무게로 막지 않는다 (테스트)
  - 기준: 기준 무게와 비율은 임시다. 한 곳에 있고 「임시」라고 적는다
  - 기준: (앞 단계 QA) spec 「재장전 중에는 쏘지 못한다」 줄이 낡았다 — spec/08_combat/tools-as-weapons.md:15 「R 로 재장전한다. 재장전 중에는 쏘지 못한다」는 이제 「통째로」 총에만 맞는다. 사람 결정 줄(2026-09-22 묶음) 안의 문장이니 지우지 말고, 뒤에 「(「한 발씩」인 총은 넣는 중에도 쏠 수 있다 — 아래 표 「장전」)」를 덧붙여라
  - 기준: (앞 단계 QA) spec 미정의 「재장전 시간 미정」이 낡았다 — spec/08_combat/tools-as-weapons.md:101 은 「재장전 시간 · 탄착 남는 시간 미정」이라고 적었지만, 재장전 시간은 이제 2026-09-24 표의 「한 발당 · 가득」 칸에 (임시로) 있다. 「탄착 남는 시간 미정」만 남기고 재장전 시간은 「위 표, 임시」로 옮겨 적어라

## [x] G-902 칸 옮기기가 편하다 — 다른 게임에 있는 것들
- **자가 피드백**: 검사가 깨끗해질 때까지 단계를 계속 붙인다 (사람 결정 2026-09-23) —
  고치고 → 다시 훑고 → 또 고친다. 맨 뒤 묶음으로 미루지 않는다
- spec: spec/06_build/storage.md, spec/02_player/inventory-hotbar.md
- **G-901 뒤에 한다** — 지금은 한 칸씩 끌어다 놓는 것뿐이라 불편하다.
  코어 키퍼 · 스타듀 · 마인크래프트에 다 있는 손버릇을 들여온다
- [x] 1. Shift + 클릭 — 통째로 건너간다
  - 기준: 상자가 열려 있을 때 가방 칸을 Shift+클릭하면 그 칸이 **통째로 상자로** 간다 (테스트)
  - 기준: 상자 칸을 Shift+클릭하면 통째로 가방으로 온다. 자리가 모자라면 들어간 만큼만 간다 (테스트)
  - 기준: 창이 닫혀 있으면 아무 일도 없다 (테스트)
- [x] 2. 우클릭 — 반만 집고, 한 개씩 놓는다
  - 기준: 칸을 우클릭하면 **반만** 집힌다 (홀수면 많은 쪽을 집는다) (테스트)
  - 기준: 집은 것을 칸에 우클릭하면 **한 개씩** 놓인다 (테스트)
  - 기준: 창 바깥 우클릭은 지금처럼 창을 닫는다 — 규칙이 부딪히지 않는다 (테스트)
  - 기준: (앞 단계 QA) spec 에 Shift+클릭이 안 적혔다 — spec/06_build/storage.md 와 spec/02_player/inventory-hotbar.md 어디에도 Shift+클릭(통째로 건너감 · 같은 것 칸부터 · 모자라면 들어간 만큼만 · 창이 닫혀 있으면 없음 · 잡은 짐승 칸은 빈 칸 하나로만)이 없다. 코드가 규칙이 되지 않게 storage.md 의 상자 칸 옮기기 줄 옆에 한 단락으로 적어라 (사람 결정 줄은 건드리지 말 것)
- [x] 3. 창 밖에 놓으면 버린다
  - 기준: 칸을 **창 밖으로 끌어다 놓으면** 그것이 **캐릭터 앞 바닥에 떨어진다** (테스트)
  - 기준: 떨어진 것은 **다시 주울 수 있다** — 밟으면 줍는다 (spec/02_player/pickup.md) (테스트)
  - 기준: 우클릭으로 **반만 집어** 밖에 놓으면 반만 버려진다 (테스트)
  - 기준: 버린 것은 저장본에 남는다 — 나갔다 들어와도 그 자리에 있다 (테스트)
  - 실수로 버려도 되돌릴 수 있어야 한다 — 없어지는 것이 아니라 **바닥에 놓이는 것**이다
    (무너진 건축물이 바닥에 떨어지는 길을 그대로 쓴다, spec/06_build/building.md)
  - 기준: (앞 단계 QA) 낡은 테스트 경로 주석 — game/player/inventory_hotbar/slot_hand.gd 의 머리 주석이 tests/player/inventory_hotbar/test_slot_hand.gd 를 가리키는데 그 파일은 없다. 실제 테스트인 tests/world/storage/test_chest_right_click.gd 로 고쳐라
  - 기준: (앞 단계 QA) 집어 든 것이 화면에 안 보인다 — 우클릭으로 집으면 칸 개수만 줄고 든 것(아이콘 · 개수)이 어디에도 보이지 않아, 사람은 무엇을 몇 개 들었는지 모른다. 든 것을 보여 주는 Control(mouse_filter IGNORE)을 창에 붙여라 — 커서 위치가 필요하면 LOOP.md 2절대로 Pointer 를 거쳐야 한다(커서를 직접 읽지 말 것). 들면 보이고 다 놓으면 숨는지 테스트로 확인하라
- [x] 4. 모으기 · 정렬 · 빠른 채우기
  - 기준: 같은 아이템 칸을 **더블클릭**하면 흩어진 것이 한 칸으로 모인다 (테스트)
  - 기준: 상자에 **정렬** 단추가 있다 — 누르면 같은 것끼리 모이고 이름순으로 선다 (테스트)
  - 기준: **빠른 채우기** 단추 — 상자에 이미 있는 종류만 가방에서 밀어 넣는다 (테스트)
  - 기준: 단추 글자는 번역 키다 (ko · en 둘 다). 찍은 그림으로 자리를 확인한다

## [x] G-903 한 바퀴가 이어지는가 — 빈손에서 끝까지
- spec: spec/00_core/roadmap.md (프로토타입 수용 기준 — 한 회차를 끊김 없이 돈다)
- **QA 가 못 잡는 것을 잡는 묶음이다.** 단계마다의 QA 는 「그 기준을 지키나」를 보지만,
  **길이 이어지는가**는 못 본다 — 제작 창이 며칠 동안 게임에서 열 길이 없었던 것이 그 예다
- 이미 `game/tests/craft/test_from_empty_hands.gd` 가 **맨손 → 나무 · 돌 → 제작대 →
  도끼 · 곡괭이 → 철광석 → 제련로 → 철괴**까지 실제로 친다. 거기서 이어 간다
- **중간에 아이템을 한 개도 넣어 주지 않는다** — 그 파일의 규칙을 그대로 지킨다
- [x] 1. 총과 탄까지 이어진다
  - 기준: 철괴에서 **총 · 탄약**을 만들어 실제로 쏜다 — 중간에 아이템을 넣어 주지 않는다 (테스트)
  - 기준: R 로 재장전하고, 탄이 없으면 안 채워진다 (테스트)
- [x] 2. 밭과 음식까지 이어진다
  - 기준: 돌을 캐 **흙**을 얻고, 석회로 **비료**를 만들어 **밭**을 놓는다 (테스트)
  - 기준: 심고 · 물 주고 · 수확해 **먹는다** — 배고픔이 찬다 (테스트)
  - 기준: (앞 단계 QA) 유황을 캐러 섬을 건너는 길은 치지 않는다 — 유황은 개척 섬에서 난다(roadmap 「자원」). 그런데 test_from_empty_hands.gd 는 유황 광석을 내 섬의 평평한 줄에 놓아 캤다. 연구대 → 해금 아이템 → 선착장 → 배로 건너가는 길은 이 한 바퀴 테스트가 치지 않는다. 이번 기준 밖이니 G-903 단계 4 「한 바퀴가 한 번도 끊기지 않는다」나 G-900 에서, 개척 섬으로 건너가 유황을 캐는 데까지 실제로 치는 테스트를 이어 붙여라
- [x] 3. 사냥과 포획까지 이어진다
  - 기준: 동물을 찾아 **부위를 맞혀** 잡고 **도축**한다 (테스트)
  - 기준: 약초로 **마취탄**을 만들어 체력 10% 아래에서 **재워 잡는다** (테스트)
  - 기준: (앞 단계 QA) 섬 건너 유황·석회 캐는 길을 단계 4에서 쳐라 — test_from_empty_hands.gd 는 개척 섬에서만 나는 유황과 석회를 둘 다 내 섬의 평평한 줄(LINE 의 "sulfur", "lime")에 놓고 캔다. G-903 단계 4에서는 연구대 → sulfur_dock_key → 선착장 → 배로 개척 섬에 건너가 유황과 석회를 캐는 데까지 실제로 치고, LINE 에서 두 줄을 빼라
## [x] G-145 건축을 다시 짠다 — 도면 · 등급 · 이어 붙이는 개수
- 차선: 3d-start (대화 세션이 나눴다 2026-09-24 — 3d-start 는 건축 쪽, 3d-lane2 는 겹치지 않는 것)
- 결정: **턱은 0.3m 까지 걸어서 오른다** (사람 결정 2026-09-24, spec/02_player/movement-controls.md).
  세션이 「캐릭터가 0.2m 토대 위로 걸어 오르지 못한다」고 물어 온 것에 대한 답이다.
  **다만 근거는 토대 두께가 아니다** (사람이 바로잡음 2026-09-24) — 토대 높이는 휠로 정하고,
  그것을 오르는 것은 **계단이 할 일**이다. 턱 0.3m 는 **지형의 작은 턱 · 돌부리**를 위한 값이다 —
  테스트에서 빼거나 토대를 땅에 묻지 말고, **턱 오르는 높이를 게임 규칙으로 넣어라**.
  이것부터 하면 HEAD 에서 떨어지던 test_from_empty_hands.gd 의 건축 부분이 함께 풀린다.
  과녁이 길을 막는 것은 테스트만 고치면 된다
- spec: spec/06_build/building.md, spec/04_life/farming.md, spec/05_craft/recipes.md
- **자가 피드백**: 깨끗해질 때까지 단계를 계속 붙인다
- 사람이 플레이해 보고 「건축은 전체적으로 다시 손봐야 한다」고 했다 (2026-09-24).
  뿌리는 **「한 장이 몇 장을 받친다」(ROOF_SPAN · 장 수)** 였다 — 받침이 범위를 뿌리니
  처마가 멋대로 생기고, 기둥은 판 한가운데라 쓸모가 없고, 미리보기가 짚은 자리와 어긋났다.
  러스트 · 발하임처럼 **소켓 + 이어 붙이는 개수**로 바꾼다
- **먼저 개수, 그다음 도면, 그다음 크기** 순서다 — 크기를 먼저 바꾸면 어긋난 것이 더 커진다
- [x] 1. 장 수를 버리고 이어 붙인 개수로 잰다
  - 기준: `ROOF_SPAN` 과 「받치는 장 수」가 **코드와 spec 어디에도 없다** (테스트)
  - **이름 규칙**: 「걸음」을 쓰지 않는다 — 코드에서 이미 **진짜 걸음**(동물 · 플레이어 이동)에 쓰인다.
    `step` 도 쓰지 않는다 — 루프의 **단계**와 헷갈린다. 「이어 붙인 개수」가 드러나는 이름을 쓴다
  - 기준: 땅에 닿은 토대가 **0**, 거기 붙은 것이 1, 그 옆이 2 다 — **받침에서 이어 붙인 개수**다 (테스트)
  - 기준: **제 등급의 개수를 넘으면 놓이지 않는다** — 나무 3개 · 석재 4개 · 철 5개 (임시) (테스트)
  - 기준: **기둥이 선 자리는 다시 0** 이 된다 — 거기서 다시 뻗을 수 있다 (테스트)
  - 기준: 받침을 부숴 개수가 넘어가면 **무너진다** — 연쇄로 (테스트)
  - 기준: 미리보기가 초록 · 빨강으로 갈린다. 찍어서 확인한다 — 벽 한 장에서 뻗어 나간 지붕이
    세 장째까지 초록, 네 장째부터 빨강이다
- [x] 2. 붙을 자리를 정한다 (소켓)
  - 기준: 벽 · 문 · 계단은 판의 **변**, 지붕은 판 **위**, 기둥은 격자의 **모서리**(네 판이 만나는 자리)에
    붙는다 (테스트). 기둥이 판 한가운데에만 서던 것을 푼다
  - 기준: 붙을 자리가 없는 허공에는 **미리보기가 뜨지 않는다** (테스트)
  - 기준: **계단이 토대에 붙는다** (사람 결정 2026-09-24) — **토대를 짚으면 그 변에서 계단이
    뻗어 내려간다**. 땅을 짚어 올리는 것이 아니다 — 붙는 쪽은 늘 받침이 있는 쪽이다 (테스트)
  - 기준: 지금 `plan_stairs_at` 은 **제 판 안에서 한 층을 오르는 것**뿐이라, 휠로 올린 토대에
    걸어 올라갈 길이 없다. 토대 밖으로 내려가는 계단이 놓여야 한다 (테스트)
  - 기준: **땅에 박혀도 놓인다** (사람 결정 2026-09-24, 팰월드 그림을 봤다) — 땅에 딱 닿는지
    따지지 않는다. 비탈이든 평지든 늘 놓인다. 「땅에 안 닿아서 못 놓는다」를 두지 않는다 (테스트)
  - 기준: 더 높으면 계단을 **이어 붙여** 내려갈 수 있다 (테스트)
  - 기준: **미리보기와 놓인 것의 상자(AABB)가 같다** — 여섯 가지 전부, 크기도 자리도 (테스트).
    사람이 「오버레이랑 실제 크기가 계속 다르다」고 했다. 어긋나면 불합격이다
  - 기준: 찍어서 확인한다 — 토대 한 장 네 모서리에 기둥이 선 그림
  - 기준: (앞 단계 QA) 빈손 테스트에 TMPSTEP 디버그 줄이 남았다 — game/tests/craft/test_from_empty_hands.gd 에 gut.p("TMPSTEP …") 가 25줄 남아 로그를 어지럽힌다(이번 회차에 TMPDBG 두 줄만 지웠다). 전부 지워라
  - 기준: (앞 단계 QA) 0.3m 턱 테스트가 0.29m 로 잰다 — test_step_up.gd test_walks_up_a_0_3m_ledge 는 _walk_into_ledge(0.29) 를 쓴다. 규칙은 「0.3m 까지」라 경계값 0.3 자체가 오르는지 보지 않는다. 0.3 으로 올리고 안 되면 _step_up 의 들어 올리는 높이에 작은 여유를 더해 0.3 이 정확히 오르게 하라(0.4 가 막히는 단언은 그대로 둔다)
- [x] 3. 도면 하나로 짓는다
  - 기준: **도면을 들면** 지을 것 여섯이 뜬다 — 토대 · 벽 · 문 · 지붕 · 계단 · 기둥 (테스트)
  - 기준: 놓으면 **가방의 재료가 그 자리에서 빠진다**. 모자라면 못 놓는다 (테스트)
  - 기준: **건축물 레시피를 없앤다** — `craft/recipes/recipes.json` 에서 열두 개가 사라지고,
    가방에 건축물 아이템이 들어갈 길이 없다 (테스트)
  - 기준: **저장본에 남은 건축물 아이템은 재료로 바꿔 돌려준다** — 옛 저장을 열어도 잃지 않는다 (테스트)
  - 기준: 철거하면 **들어간 재료가 전부** 돌아온다. 회전 · 철거에 시간 제한이 없다 (테스트)
  - 기준: (앞 단계 QA) stairs_down 그림이 계단을 옆에서만 본다 — .loop/out/stairs_down.png 는 올린 토대를 옆에서 찍어 계단이 얇은 선으로만 보이고 토대 변에 붙은 자리가 잘 안 보인다. game/core/debug/shots.gd 의 stairs_down 카메라를 계단 정면 비스듬히(남서 위) 로 옮기면 사람이 한눈에 확인할 수 있다 (막는 것은 아니다)
- [x] 4. 토대 4×4칸 · 밭 2×2칸
  - **등급보다 먼저 한다** (사람 지적 2026-09-24) — 나중에 하면 단계 3·4 에서 짠 테스트를 또 고친다
  - 사람이 G-143 사진을 보고 골랐다 (2026-09-24)
  - 기준: 토대 한 변이 **4칸(4m)** 이다 — 벽 · 문 · 지붕 · 계단도 4m 다 (테스트)
  - 기준: 밭 한 변이 **2칸(2m)** 이고, 토대 한 장에 **정확히 네 장** 들어간다 (테스트)
  - 기준: 임시 시험 코드를 **지운다** — `build/size_trial.gd` · 그 테스트 · `shots.gd` 의
    `size_trial` · `size_trial_top`. 남은 곳이 없다 (테스트)
  - 기준: (앞 단계 QA) test_blueprint 에 고아 노드가 남는다 — test_what_was_spent_is_saved_and_an_old_save_counts_the_piece_price 가 restore 를 두 번 불러 BuildPiece 6개가 고아로 잡힌다(test.log). restore 뒤 await wait_process_frames(1) 을 넣거나 끝에 남은 조각을 free 해 고아가 0 이 되게 하라
  - 기준: (앞 단계 QA) 시간 제한 테스트가 10초만 기다린다 — test_rotating_and_breaking_have_no_time_limit 는 600 물리 프레임(약 10초)만 보낸다. 지금은 잠금 코드가 아예 없어 문제없지만, 나중에 누가 분 단위 잠금을 넣으면 못 잡는다. BuildPiece 에 놓은 시각을 두지 않는다는 것을 직접 보거나(예: 놓인 조각에 placed_at 같은 메타가 없다) 시계를 몇 분 앞당겨 보는 단언으로 바꿔라
- [x] 5. 등급 셋 — 나무 · 석재 · 철
  - 기준: 같은 건축물을 **나무 · 석재 · 철** 세 등급으로 짓는다 (테스트)
  - 기준: **도면으로 그 자리에서 등급을 올린다** — 망치 같은 도구를 새로 두지 않는다 (테스트)
  - 기준: 등급을 올리면 **이을 수 있는 개수도 늘어난다** (테스트)
  - 기준: 올린 것을 철거하면 **올리며 쓴 재료까지 전부** 돌아온다 (테스트)
  - 기준: (앞 단계 QA) 옛 테스트가 1m 칸을 밭 판으로 넘긴다 — tests/settings/save/test_round_trip.gd:330 과 tests/world/day_night/test_day_in_world.gd:40 이 Grid.world_to_cell(...) 로 구한 1m 칸을 farmland.till · crops.plant 에 그대로 넘긴다. 이제 그 인자는 2m 밭 판 좌표라 밭이 뜻한 자리의 두 배 먼 곳에 서고, 높이도 엉뚱한 자리(Grid.cell_to_world(cell))에서 잰다. 지금은 우연히 통과한다. Farmland.plate_of(pos) 로 판을 구하고 높이는 Farmland.center_of(plate) 자리에서 재도록 고쳐라
  - 기준: (앞 단계 QA) 시각 이름 검사의 'age' 가 엉뚱한 것을 잡는다 — test_blueprint.gd _time_marks 는 부분 문자열로 찾아서 damage · stage · storage · image 같은 변수가 BuildPiece · Buildings 에 생기면 시각이 아닌데도 깨진다. 이름을 '_' 로 쪼갠 낱말이 정확히 age · time · lock 등과 같은지로 보거나, placed_at · placed_time · lock_time 같은 정확한 이름 목록으로 바꿔라
- [x] 6. 휠로 올려도 옆에 붙는다
  - 「토대의 높이를 휠로 올렸을 경우 옆에 스냅이 안 붙는다」 — `plan_at` 이 이웃 윗면에
    휠 값(`_raise`)을 **또 더한다**. `_raise` 는 손에서 내려놓을 때만 0 이라 이어 붙일수록
    계단처럼 올라가고 곧 5m(`MAX_HEIGHT`)에 걸려 빨개진다
  - 기준: **옆에 붙을 때는 이웃 윗면에 딱 맞춘다** — 휠 값을 더하지 않는다 (테스트)
  - 기준: **한 장 놓을 때마다 휠 값이 0 으로 돌아간다** (테스트)
  - 기준: 이웃을 고를 때 **올린 높이** 기준으로 고른다 (테스트)
  - 기준: 휠로 이웃과 **다른 높이**로 놓는 것은 그대로 된다. 5m 제한도 그대로 (테스트)
  - 기준: (앞 단계 QA) spec 의 「등급을 내린다」를 만들 길이 없다 — spec/06_build/building.md 수용 기준(80줄)은 「등급을 올리고 내릴 수 있고」인데 이번 단계는 올리기만 만들었다. 이 묶음 뒤 단계나 G-900 에서 도면으로 한 등급 내리는 손짓(임시 키)과 그때 돌려줄 재료 규칙(철거처럼 올린 몫을 그대로 돌려주나)을 만들고, 내린 뒤 LINK_LIMIT 가 줄어 넘치는 것이 무너지는지 보는 테스트를 짜라. 규칙이 spec 에 없으면 「미정」에 한 줄 적어라
  - 기준: (앞 단계 QA) Buildings._grade 가 이제 거의 쓰이지 않는다 — buildings.gd:113 의 _grade 는 손으로 놓는 것이 늘 나무가 된 뒤 pick() 으로만 바뀐다. 테스트 편의로만 남았으면 주석에 그렇게 적거나, 필요 없으면 걷어내라

## [x] G-149 계단 위에는 아무것도 못 짓는다 — 플레이하고 찾은 것
- 차선: 3d-start (건축이라 차선 1)
- spec: spec/06_build/building.md
- 사람이 플레이해 보고 찾았다 (2026-09-25): **계단 위에 토대를 지을 수 있다.**
  「건축은 아무것도 없어야 한다」 — 계단이 선 판에는 어떤 건축물도 놓이면 안 된다
- **자가 피드백**: 깨끗해질 때까지 단계를 계속 붙인다
- [x] 1. 계단이 선 판에는 놓이지 않는다
  - 기준: 계단이 선 판에 **토대 · 벽 · 문 · 지붕 · 계단 · 기둥** 어느 것도 놓이지 않는다 — 휠로 높이를 올려도,
    옆 토대에 붙여도 마찬가지다. 미리보기가 빨갛다 (테스트 — 여섯 가지 전부)
  - 기준: 계단 **위 공간**(계단이 오르는 한 층 높이 안)에도 놓이지 않는다 — 계단 구멍을 막는 지붕 · 2층 바닥이 생기지 않는다
    (spec 「계단이 선 제 판 위는 뚫린 채로 둔다」) (테스트)
  - 기준: 계단 위에 **설치물**(제작대 · 상자 · 횃불 · 밭)도 놓이지 않는다 (테스트)
  - 기준: 계단 **옆 변**에 벽을 세우거나 옆 판에 토대를 붙이는 것은 그대로 된다 — 막는 것은 계단이 선 판 하나다 (테스트)
  - 기준: `spec/06_build/building.md` 에 사람 결정 줄을 적는다 — 「**계단이 선 판에는 아무것도 짓지 못한다** (사람 결정 2026-09-25)」
  - 기준: 찍어서 확인한다 — 계단 위를 겨눈 토대 미리보기가 빨갛다
- [x] 2. (앞 단계 QA) 횃불을 놓게 되면 계단 판도 막아야 한다
  - 기준: 횃불을 놓게 되면 계단 판도 막아야 한다 — 지금은 횃불을 놓는 길이 아예 없다 (stations.catalog 에 torch 가 없다). 그래서 이번 기준은 통과지만, 나중에 횃불을 놓는 길을 만들 때 BuildPiece.stairs_in(self, BuildPiece.plate_of(spot), spot.y, spot.y) 확인을 넣어야 한다. test_building_stairs_plate.gd 의 assert_false(stations.catalog.has("torch")) 도 그때 '횃불 aimed_spot 이 null' 단언으로 바꿔라
  - 기준: (앞 단계 QA) stairs_blocked 그림이 남지 않았다 — 구현 세션이 찍었다고 한 stairs_blocked.png 가 .loop/out 에도 없고 다른 곳에서도 찾지 못했다. 앞으로 그림은 .loop/out/ 에 저장해 QA 가 볼 수 있게 하라 (harness/shot.sh .loop/out stairs_blocked)

## [x] G-148 한 바퀴 마무리 — 새 건축으로 집까지
- 차선: 3d-start (대화 세션이 나눴다 2026-09-24 — 3d-start 는 건축 쪽, 3d-lane2 는 겹치지 않는 것)
- spec: spec/00_core/roadmap.md, spec/06_build/building.md
- **자가 피드백**: 깨끗해질 때까지 단계를 계속 붙인다
- G-903 단계 4 에서 **건축만 떼어 온 것**이다 (사람 결정 2026-09-24) — 그때는 건축을 다시 짜는
  중이라 옛 건축으로 도는 것이 버리는 일이었다. **G-145 가 선 뒤에** 새 건축으로 친다
- 결정: **한 바퀴 테스트의 집 짓기 꼬리(22~25)를 이 묶음 전까지 뺀다** (사람 결정 2026-09-24) —
  G-147 · G-910 의 마지막 전체 검사가 옛 건축 꼬리에 늘 걸렸다. `test_from_empty_hands.gd` 의
  `HOUSE_UNTIL_G148` 로 pending 처리했다 (3d-lane2). **이 묶음에서 새 건축으로 되살리고 그 표시를 지운다**
- [x] 1. 빈손에서 집까지 끊기지 않는다
  - 기준: `HOUSE_UNTIL_G148` pending 이 **없다** — 집 짓기 꼬리가 다시 돈다 (테스트)
  - 기준: 한 바퀴 테스트가 **도면으로** 토대 · 벽 · 지붕을 세워 **방**이 생기는 데까지 친다 (테스트)
  - 기준: 재료를 캐서 도면으로 짓는 길이 **한 번도 끊기지 않는다** — 막히면 그 자리를 report 에 적는다
  - 기준: 지은 것이 **저장하고 새로 불러와도 그대로**다 — 등급까지 (테스트)
  - 기준: 철거하면 들어간 재료가 전부 돌아오고, 그것으로 다시 지을 수 있다 (테스트)
- [x] 2. 저장까지 이어진다 — G-903 에서 옮겨 왔다
  - G-903 단계 4 를 통째로 옮긴 것이다 (사람 결정 2026-09-24) — 건축을 다시 짜는 중에
    옛 건축으로 한 바퀴를 도는 것이 버리는 일이라 묶음째 건너뛰었다
  - 기준: 저장하고 새로 불러와도 **지은 것 · 가진 것 · 서 있던 자리**가 그대로다 (테스트)
  - 기준: 한 바퀴가 **한 번도 끊기지 않는다** — 어디서 막히면 그 자리를 report 에 적는다
  - 기준: (앞 단계 QA) **섬을 건너가 캔다** — 연구대 → sulfur_dock_key → 선착장 → 배로
    개척 섬에 건너가 **유황 넷과 석회 하나**를 캐는 데까지 test_from_empty_hands.gd 가 실제로 치고,
    LINE 에서 "sulfur" · "lime" 줄을 뺀다 (테스트). 지금 LINE 의 유황은 넷이다 (권총탄 3 · 마취탄 1)
  - 기준: (앞 단계 QA · 이미 들어갔다 65fb024) 잡아 비운 사슴 집은 저장하고 불러와도 다음 날까지 비어 있다 — `island_animals.gd` 가 빈 집 · 비운 날을 담는다. 한 바퀴 테스트 25단계 끝의 단언이 **다시 돌면서** 지키는지만 본다 (테스트)
- [x] 3. (앞 단계 QA) 배 건너기 순서가 테스트에 복사돼 있다
  - 기준: 배 건너기 순서가 테스트에 복사돼 있다 — test_from_empty_hands.gd 의 _on_boarded · _on_key_inserted · _island_open 이 core/game_root.gd 의 sail_to · insert_key · island_open 을 손으로 베낀 것이라, GameRoot 쪽이 바뀌면(예: 로딩 · 캐릭터 몸 상태 넘기기 차례) 한 바퀴 테스트는 모른 채 통과한다. 지금은 같은 차례임을 확인했다(막는 것은 아니다). 베낀 함수 머리 주석에 'GameRoot.sail_to 를 바꾸면 여기도 맞춘다'를 적거나, IslandState · PlayerState 담기/되돌리기를 GameRoot 와 테스트가 함께 부르는 정적 함수 하나로 모아라
  - 기준: (앞 단계 QA) 개척 섬 캔 자리 단언이 빈 값끼리도 통과한다 — test_from_empty_hands.gd 25단계의 assert_eq(sulfur_now.get("gone"), sulfur_left.get("gone")) 는 둘 다 비어 있어도 참이다. 그 앞에 assert_eq((sulfur_left.get("gone", PackedStringArray()) as PackedStringArray).size(), SULFUR_LINE.size(), "개척 섬에서 캔 다섯 자리가 담겼다") 를 넣어라
- [x] 4. (앞 단계 QA) GameRoot.island_open · insert_key 머리에 베낌 표시가 없다
  - 기준: GameRoot.island_open · insert_key 머리에 베낌 표시가 없다 — sail_to 머리(game_root.gd 287줄)에만 '한 바퀴 테스트가 베껴 쓴다'가 있고 island_open(226줄) · insert_key(240줄) 머리에는 없다. 고치는 쪽에서 보이도록 두 함수 머리에도 'tests/craft/test_from_empty_hands.gd 의 _island_open / _on_key_inserted 가 베껴 쓴다 — 바꾸면 거기도 맞춘다' 한 줄씩 적어라 (막는 것은 아니다)
  - 기준: (앞 단계 QA) 한 바퀴 테스트가 옆 차선과 겹쳐 돌면 흔들린다 — report 에 따르면 naru-lane2 가 godot 6개를 돌리는 동안 test_from_empty_hands.gd 가 10단계(밭 놓기) · 5단계(걷기)에서 떨어졌고 혼자 돌리면 통과했다. 걷기 · 조준을 프레임 수가 아니라 도착 여부(거리 조건 + 넉넉한 상한)로 기다리게 바꾸거나, 떨어진 단계에서 무엇이 모자랐는지 단언 메시지에 위치 · 남은 거리를 찍어 원인을 가려라
- [x] 5. (앞 단계 QA) _put_down 은 조준이 맞기 전에 놓을 수 있다
  - 기준: _put_down 은 조준이 맞기 전에 놓을 수 있다 — test_from_empty_hands.gd 의 _put_down(700줄 근처)은 _look 뒤 곧바로 _right_click 하고 stations.aimed_station() 을 한 번만 읽는다. 조준 대기(_until_aims_ground 로 땅을 짚을 때까지 기다린 뒤 우클릭)를 넣고 assert_not_null 메시지에 _where() 를 붙여라 — 이번 기준(5 · 10단계)은 풀렸지만 같은 흔들림이 제련로 · 연구대 놓기에서 날 수 있다

## [x] G-146 플레이하고 짚은 것 — 밤 · 횃불 · 오버레이 · 탄 고르기
- 차선: 3d-start (대화 세션이 나눴다 2026-09-24 — 3d-start 는 건축 쪽, 3d-lane2 는 겹치지 않는 것)
- spec: spec/03_world/lighting.md, spec/06_build/building.md, spec/08_combat/tools-as-weapons.md
- 설치물 오버레이(옛 4번)는 G-150 으로 옮겼다 — 놓기 코드를 G-150 과 같이 만져 차선이 갈리면 부딪힌다 (2026-09-26)
- **자가 피드백**: 깨끗해질 때까지 단계를 계속 붙인다
- 사람이 플레이해 보고 짚었다 (2026-09-24). 넷 다 「있는데 안 된다」쪽이다
- [x] 1. 밤이 가장 어둡다
  - 「낮→저녁 되는 노을에 어둠이 엄청 어두운데 밤은 엄청 밝다」 — 지금은 순서가 뒤집혀 있다
  - 기준: 밝기가 **낮 → 노을 → 밤 → 새벽 → 낮** 으로 이어지고 **밤이 가장 어둡다** (테스트)
  - 기준: 노을 · 새벽이 밤보다 어둡지 않다 (테스트)
  - 기준: 하루를 네 때로 나눠 찍어 나란히 본다 — 눈으로도 밤이 가장 어둡다
- [x] 2. 노을부터 새벽까지 내내 어둡다 — 한밤이 밝지 않다
  - 사람이 다시 짚었다 (2026-09-26): 「18~19시는 아예 어두운데 20시쯤은 엄청 밝다. 노을 → 밤이 어둡다가 밤이 밝아 어색하다」
  - 결정: **밤은 내내 어둡다** (사람 결정 2026-09-26) — 노을 · 한밤 · 새벽 모두 노을 때처럼 어둡다.
    초반엔 횃불, 나중엔 연구로 야간투시경 (spec/03_world/lighting.md)
  - 기준: 해가 진 뒤부터 뜨기 전까지 **어느 시각에도** 한밤이 노을보다 밝지 않다 — 18 · 19 · 20 · 22 · 0 · 3 · 5시를 재서 견준다 (테스트)
  - 기준: 밤 밝기가 광원 없이 「못 나갈 만큼」은 아니다 — 가까운 땅의 윤곽은 보인다 (day-night.md) (테스트: 최소 밝기 문턱)
  - 기준: spec/03_world/lighting.md · day-night.md 의 「밤이 가장 어둡다」 줄을 「밤은 내내 어둡다 (사람 결정 2026-09-26)」로 바꾼다
  - 기준: 하루를 18 · 20 · 22 · 3 · 5시로 찍어 나란히 본다 — 눈으로도 밤 내내 어둡다
- [x] 3. 횃불이 제 둘레를 밝힌다
  - 「횃불이 횃불 역할을 안 한다」 — 들어도 놓아도 둘레가 밝아지지 않는다
  - 기준: 횃불을 **놓으면** 그 둘레가 밝아진다 — 반경 밖은 그대로다 (spec/03_world/lighting.md) (테스트)
  - 기준: 횃불을 **손에 들어도** 둘레가 밝아진다 (테스트)
  - 기준: 밤에 횃불 하나를 놓고 찍는다 — 둘레만 밝고 섬 반대편은 그대로다
  - 기준: (앞 단계 QA) SKY_SHARE 에 「임시」 표시가 없다 — game/world/day_night/day_light.gd 의 const SKY_SHARE := 0.7 주석에 **임시** 가 빠졌다. report.json 의 temporary 에는 올라 있다. DAY_FILL · NIGHT_BELOW 처럼 주석에 **임시** 를 붙여라
  - 기준: (앞 단계 QA) 테스트의 땅 빛에 하늘 복사광이 빠졌다 — DayLight.ground_light 는 햇빛+달빛+평평한 주변광만 더하고 주변광의 70%(하늘 몫)는 뺀다. 지금은 사진으로 순서가 맞음을 확인했지만, Sky3D 가 밤하늘을 밝게 바꾸면 테스트는 못 잡는다. shots.gd 가 이미 재는 땅 밝기(_brightness(shot, GROUND))로 time_* 네 장의 순서를 단언하는 가벼운 검사를 더하거나, ground_light 주석에 이 빈틈을 적어 두어라
- [x] 4. R 꾹 — 없는 탄은 안 보인다
  - 「R 꾹 눌렀을 때, 있지 않은 탄의 종류는 표시되지 않는다」 — 지금은 0 개인 것도 다 뜬다
  - 기준: 탄 고르기에 **가방에 있는 탄만** 뜬다 (테스트)
  - 기준: 들고 있는 총이 쓰는 계통의 탄만 뜬다 — 지금 규칙 그대로다 (테스트)
  - 기준: 쓸 탄이 하나도 없으면 고르기가 뜨지 않는다 (테스트)
  - 기준: (앞 단계 QA) shots.gd 의 밤 판정이 단언이 아니라 출력뿐이다 — game/core/debug/shots.gd 의 TIME_ORDER · TORCH_NIGHT 는 「틀렸다」를 print 만 하고 종료 코드는 그대로다. 사진을 찍는 세션이 줄을 읽지 않으면 놓친다. 틀렸을 때 push_error 로 남기거나 quit(1) 로 끝내 harness/shot.sh 가 실패로 돌려주게 하라
  - 기준: (앞 단계 QA) test_torch.gd 머리 주석의 단계 번호가 틀렸다 — game/tests/world/lighting/test_torch.gd 1줄 「G-146.2」 는 이번 단계(G-146.3)다. shots.gd 의 torch_night 주석(「(G-146.2, spec/03_world/lighting.md)」)도 같다. G-146.3 으로 고쳐라
- [x] 5. (앞 단계 QA) ammo_picker.gd 머리 주석 한 줄이 너무 길다
  - 기준: ammo_picker.gd 머리 주석 한 줄이 너무 길다 — game/ui/hud/ammo_picker.gd 7줄에 「가방에 없는 탄도 뜨지 않는다 (Attack.usable_rounds).」 를 앞줄에 이어 붙여 다른 줄보다 훨씬 길어졌다. 옆 줄들처럼 두 줄로 나눠라

## [x] G-147 지도에서 자리를 찾는다 — 좌표 읽기 · 좌표로 찾기
- 차선: 3d-lane2 (대화 세션이 나눴다 2026-09-24 — 3d-start 는 건축 쪽, 3d-lane2 는 겹치지 않는 것)
- spec: spec/12_ui/hud.md, spec/01_settings/input.md
- **자가 피드백**: 깨끗해질 때까지 단계를 계속 붙인다
- 결정: 마지막 전체 검사를 막던 한 바퀴 테스트의 건축 꼬리는 **G-148 까지 뺐다** (사람 결정 2026-09-24, (a)).
  G-147 은 기준을 다 만족했으니 전체 검사만 다시 돌면 된다
- 사람이 청했다 (2026-09-24) — 지도(M)는 이미 밝히기와 「내 자리」까지 되어 있다.
  **어디가 어디인지 읽을 길**과 **좌표로 찾아가는 길**만 없다
- **마우스는 Pointer 로만 읽는다** (LOOP.md 2절) — `map_window.gd` 머리에도 적혀 있다.
  커서를 직접 읽으면 하네스 검사가 떨어진다
- [x] 1. 마우스 올린 자리의 좌표가 보인다
  - 기준: 지도 위에 마우스를 올리면 **그 자리의 세계 좌표**가 보인다 (테스트)
  - 기준: 마우스가 지도 밖으로 나가면 사라진다 (테스트)
  - 기준: 좌표는 **`game/core/input/` 의 Pointer 를 거쳐** 읽는다 — 커서를 직접 읽지 않는다 (테스트)
  - 기준: 지도를 닫았다 열어도 어긋나지 않는다 — 지도 네모와 세계 좌표가 늘 같은 잣대다 (테스트)
  - 기준: 찍어서 확인한다 — 글자가 지도 밖으로 잘리지 않고 다른 것을 덮지 않는다
- [x] 2. 좌표를 넣으면 그 자리에 표가 뜬다
  - 기준: 지도 **위쪽에 X · Y 칸**이 있다. 넣으면 그 자리에 표가 뜬다 (테스트)
  - 기준: **칸을 지우면 표도 사라진다** — 잠깐 보는 것이라 남지 않는다 (테스트)
  - 기준: 지도를 닫으면 표도 사라진다. **저장본에 담지 않는다** (테스트)
  - 기준: 지도 밖 좌표를 넣으면 표가 뜨지 않는다 — 빨갛게든 비어서든 알려 준다 (테스트)
  - 기준: 숫자가 아닌 것을 넣어도 터지지 않는다 (테스트)
  - 기준: 칸 이름은 번역 키다 (ko · en 둘 다)
  - 기준: 찍어서 확인한다 — 표가 어디에 떴는지 눈으로 보인다
- 표시를 **남겨 두는 마커**(아이콘 · 이름 · 저장)는 이 묶음에 넣지 않는다 —
  아이콘이 나온 뒤에 따로 한다 (사람 결정 2026-09-24)
  - 기준: (앞 단계 QA) 밝힌 칸이 지도 네모 밖으로 삐져나온다 — .loop/shots/map/map_ko.png 오른쪽 아래를 보면 (200,200) 언저리 밝힌 칸이 지도 테두리(아래·오른쪽) 바깥까지 그려진다. 이번 단계(좌표 글자) 밖의, 원래 있던 _draw 흠이다. game/ui/hud/map_window.gd _draw 에서 밝힌 칸 Rect 를 map_rect() 로 잘라(Rect2.intersection) 그리고, tests/ui/hud/test_map_window.gd 에 「섬 가장자리 칸을 밝혀도 그린 칸이 map_rect 를 넘지 않는다」 단언을 더하라

## [x] G-150 건축 손보기 — 사람이 플레이하고 짚은 것 (2차)
- 차선: 3d-lane2 (사람 결정 2026-09-26 — 건축은 Fable 이 맡는다)
- spec: spec/06_build/building.md, spec/06_build/rooms.md, spec/06_build/storage.md, spec/04_life/farming.md
- 사람이 플레이해 보고 짚었다 (2026-09-26)
- **자가 피드백**: 깨끗해질 때까지 단계를 계속 붙인다
- [x] 1. 토대를 부수면 그 위 설치물도 무너진다
  - 「토대 위에 밭을 설치하고 토대를 부수면 밭이 공중부양한다 — 다른 것들도 이런지 다 확인하고 다 부서져야 한다」
  - 기준: 토대(와 그 위 지붕 · 2층 바닥)를 부수면 그 위에 놓인 **모든 설치물**(밭 · 제작대 계열 전부 · 상자 · 횃불 · 앞으로 생길 것)이
    같이 무너져 바닥에 떨어진다 — **놓이는 것 전부에서 끌어와** 검사한다. 하나라도 떠 있으면 실패 (테스트)
  - 기준: 밭에 심긴 작물도 같이 사라지거나 떨어진다 — 떠 있는 작물이 없다 (테스트)
  - 기준: 찍어서 확인한다 — 토대를 부순 뒤 공중에 남은 것이 없다
- [x] 2. 계단은 토대에만 붙고, 토대 위에도 놓인다 — Tab 으로 스냅을 바꾼다
  - 「계단은 맨 땅에 설치하는 게 아니다. 스냅이 토대에 붙는 것일 뿐」 · 「토대 위에 계단 설치가 안 된다 — 스냅이 토대 아래로 잡힌다」
  - 기준: 계단은 **맨땅에 놓이지 않는다** — 토대(또는 2층 바닥)의 변에 붙을 때만 놓인다 (테스트)
  - 기준: 토대 변에서 **아래로 내려가는 계단**과 **토대 위에서 위층으로 올라가는 계단** 둘 다 놓인다 (테스트)
  - 기준: **Tab** 으로 붙는 자리(위로 · 아래로)를 바꾼다 — 미리보기가 바로 바뀐다. 입력은 InputMap 액션,
    spec/01_settings/input.md 표에 한 줄 (사람 결정 2026-09-26) (테스트)
  - 기준: 계단이 선 판에는 아무것도 못 짓는다(G-149)는 그대로다 (테스트)
  - 기준: (앞 단계 QA) spec 무너짐 줄에 밭이 빠져 있다 — spec/06_build/building.md 60줄 「그 위에 놓인 설치물(제작대 · 상자 · 횃불)까지 연쇄로 내려앉는다」에 밭(과 그 작물이 바닥에 쏟아진다)을 한 단어 더해라. 코드(Buildings.everything_placed)와 테스트는 밭까지 무너뜨리는데 spec 문장만 옛것이다
- [x] 3. 지붕은 하늘을 보고 놓는다
  - 「지붕은 하늘 보고 건축을 하는데 땅을 봐야 설치가 된다」
  - 기준: 지붕을 들고 **위를 보면** 조준선이 닿는 벽 윗면 · 기둥 위 자리에 미리보기가 뜬다 — 땅을 보지 않아도 놓인다 (테스트)
  - 기준: 땅을 보고 놓던 길도 그대로 된다 (테스트)
- [x] 4. 설치물은 1칸 단위로 자유롭게 놓인다
  - 「밭은 2×2 인데, 토대 두 개 사이에 걸치거나 한 토대 가운데에 놓을 수 있어야 한다. 지금은 네 사분면에만 딱딱 들어맞는다」
  - 기준: 설치물(밭 · 제작대 · 상자 · 횃불 …)은 **1칸(1m) 격자**에 스냅된다 — 토대의 사분면 · 토대 경계에 묶이지 않는다 (테스트)
  - 기준: 밭 2×2 가 **토대 두 장에 걸쳐** 놓이고, **한 토대의 가운데**에도 놓인다 (테스트)
  - 기준: 겹쳐 놓이지는 않는다 — 이미 놓인 것과 칸이 겹치면 빨갛다 (테스트)
  - 기준: `spec/06_build/building.md` 또는 farming.md 에 「설치물은 1칸 격자 (사람 결정 2026-09-26)」를 적는다
  - 기준: (앞 단계 QA) 벽 윗면 테스트가 새 길을 지나지 않는다 — test_building_roof_skyward.gd 의 test_looking_up_at_the_wall_top_… 은 25° 로 벽 **옆면**에 광선이 닿아(BuildPiece 를 짚음) 고친 것 없이도 통과한다(roof 분기를 꺼도 통과했다). 벽 **위 하늘**을 보는 경우(Aim.at 이 비는 각도로 올리거나 벽에서 더 떨어져 서서)를 하나 더 두어 _roof_level_crossing 이 벽 꼭대기 자리를 짚는지 assert_true(sky.is_empty()) 대조와 함께 단언하라
- [x] 5. 설치물도 놓을 자리가 보인다 (G-146 에서 옮겼다)
  - 「모든 설치물들은 오버레이를 둘 것. 밭 · 연구대 · 상자 · 조리대 다」 — 사람이 다시 짚었다 (2026-09-26: 「해 달라고 했는데 안 했다」)
  - 기준: **놓을 수 있는 것 전부**가 손에 들면 놓일 자리 미리보기를 띄운다 — 밭 · 상자 ·
    제작대(연구대 · 조리대 · 화로 …) · 횃불. **하나라도 빠지면 검사가 실패한다** (테스트).
    「목록에 적어 둔 것만」이 아니라 **놓이는 것 전부에서 끌어온다**
  - 기준: 놓을 수 있으면 초록 · 없으면 빨강이다 — 건축물과 같은 색 규칙 (테스트)
  - 기준: **미리보기 크기가 놓인 것과 같다** (테스트) — G-145.1 과 같은 잣대다
  - 기준: 찍어서 확인한다
  - 기준: (앞 단계 QA) 상자를 토대 위에 놓을 수 없다 — game/world/storage/chests.gd aimed_spot 이 놓인 것(토대 포함)을 짚으면 null 을 돌려줘 상자가 토대 윗면에 안 놓인다. 제작대(Stations.aimed_spot)는 놓인다. 토대 윗면(BuildPiece.is_floor)을 짚으면 그 1m 칸 한가운데를 돌려주게 고치고, test_field_free_grid.gd 의 test_chests_snap_to_the_one_meter_grid 를 토대 위에서도 도는 것으로 넓혀라 (지금은 「맨땅에서 격자만 본다」고 주석으로 피해 갔다)
- [x] 6. 건축 전체 QA — 한 바퀴 훑어 흠을 찾는다 (G-153 을 옮겼다 2026-09-26)
  - 사람이 플레이할 때마다 건축에서 새로 걸렸다 (G-140 · G-145 · G-149 · G-150). **사람보다 먼저 찾는다**
  - 보는 법: **실제 입력(도면 들기 · 휠 · Tab · 우클릭 · 좌클릭)으로만** 짓고, 그림으로 찍어 본다. 함수를 직접 불러 맞추지 않는다
  - 기준: 아래를 **전부** 실제 입력으로 해 보고, 되는 것 · 안 되는 것을 report 에 표로 적는다 —
    토대 · 벽 · 문 · 지붕 · 계단 · 기둥 여섯 × 나무 · 석재 · 철 세 등급 놓기 / 등급 올리기 · 내리기 / 회전 / 철거(재료가 전부 돌아오나) ·
    옆에 붙이기 · 휠로 높이기(5m 한도) · 계단 위 · 아래(Tab) · 지붕을 하늘 보고 놓기 · 이어 붙인 개수를 넘기면 빨강 ·
    받침을 부수면 연쇄로 무너지고 **위의 설치물까지** 떨어지나 · 방이 생기고 없어지나 · 2층 · 저장하고 불러오면 그대로인가 (등급까지)
  - 기준: 찾은 흠 중 **그 자리에서 고칠 수 있는 것**은 고치고 테스트를 단다 (테스트)
  - 기준: 나머지는 QA 가 적어 다음 단계로 붙인다
  - 기준: 찍어서 확인한다 — 여섯 가지가 선 집 한 채 · 무너진 뒤 · 불러온 뒤
  - 기준: (앞 단계 QA) 임시 표시 누락 — game/build/placeable.gd 의 FLOOR_TOP_NORMAL 은 report.json 에 임시(눈대중)라 적혔는데 코드 주석에는 「임시」가 없다. 주석에 「**임시**다 — 눈대중」을 붙여라
  - 기준: (앞 단계 QA) 찍은 그림이 남지 않았다 — 구현 세션이 place_green · place_red 를 찍어 확인했다 했으나 .loop/out/shots/ 에 없어 QA 가 다시 찍었다. 다음부터 harness/shot.sh .loop/out/shots/<이름> … 으로 찍어 그림을 남겨라
- [x] 7. (앞 단계 QA) 무너짐 테스트의 상자를 손으로 놓았다
  - 기준: 무너짐 테스트의 상자를 손으로 놓았다 — test_building_qa_sweep.gd:264 가 StorageChest.make() 와 global_position 대입으로 지붕 위 상자를 세운다. 단계 참고 「함수를 직접 불러 맞추지 않는다」에 어긋난다. G-150.5 가 상자 미리보기를 붙였으니 상자를 손에 들고(set_slot) 지붕 위를 짚어 우클릭으로 놓게 바꾸고, 놓인 뒤 held_up(chest) 단언은 그대로 둬라
  - 기준: (앞 단계 QA) 저장 · 불러오기가 저장 파일을 거치지 않는다 — test_building_qa_sweep.gd:374·389 가 buildings.standing() → restore() 만 잇는다. 기준 「저장하고 불러오면」을 끝까지 보려면 세계 저장(SaveGame 류)에 써서 새 GameRoot 로 읽어 오는 한 번을 더 두거나, 이미 그 길을 도는 테스트(tests/world/storage 쪽)에 등급 있는 건축물을 한 장 끼워라
- [x] 8. 사슬 끝 4 · 5번째 자리는 그 등급으로 바로 놓인다 (결정 대기 → 사람 결정 2026-09-26)
  - 「석재 · 철 사슬을 올려도 4 · 5번째 자리에 새 것을 못 놓는다」 — 손으로 놓는 것이 늘 나무라 나무 한도 3 에 걸렸다. 사람이 ② 를 골랐다
  - 기준: 나무 한도(3)를 넘는 자리를 겨누면 미리보기가 **그 자리에 설 수 있는 가장 낮은 등급**으로 뜬다 — 4번째는 석재 · 5번째는 철 (테스트)
  - 기준: 놓으면 그 등급으로 서고 **재료도 그 등급으로** 든다. 그 재료가 모자라면 빨강이다 (테스트)
  - 기준: 한도 안의 자리는 지금대로 나무로 놓인다 — 달라지는 것이 없다 (테스트)
  - 기준: 철 한도(5)를 넘는 자리는 여전히 빨강이다 (테스트)
  - 기준: spec/06_build/building.md 104줄 「손으로 놓는 것은 늘 나무」를 「한도를 넘는 자리는 설 수 있는 등급으로 바로 놓인다 (사람 결정 2026-09-26)」로 고친다
  - 기준: 찍어서 확인한다 — 석재로 올린 사슬 끝에 석재 미리보기가 뜬 그림
- [x] 9. (앞 단계 QA) grade_color 섞는 비율 0.5 에 「임시」 표시가 없다
  - 기준: grade_color 섞는 비율 0.5 에 「임시」 표시가 없다 — game/build/buildings.gd 의 grade_color 머리말 주석에 report.json 처럼 「**임시** — 눈에 갈리게 0.5 로 정했다」를 적어라

## [x] G-151 카메라 · 장비칸 — 사람이 플레이하고 짚은 것
- 차선: 3d-start (사람 결정 2026-09-26 — Opus 가 G-151 부터 쭉 돈다. Fable 은 G-150 까지)
- spec: spec/02_player/movement-controls.md, spec/01_settings/display.md, spec/02_player/equipment.md, spec/12_ui/hud.md
- 사람이 플레이해 보고 짚었다 (2026-09-26)
- **자가 피드백**: 깨끗해질 때까지 단계를 계속 붙인다
- [x] 1. 캐릭터를 화면 왼쪽 아래로 — 평소에도 조준할 때도
  - 「총을 조준할 때 캐릭터가 가운데 있어서 표적이 안 보인다. 오른손잡이라 생각하고 캐릭터를 왼쪽 아래로」 ·
    「플레이어가 너무 가운데 있어 부자연스럽다. 왼쪽 아래로 조금」
  - 기준: 평소 카메라에서 캐릭터가 화면 가운데보다 **왼쪽 아래**에 선다 — 조준점(화면 가운데)을 가리지 않는다 (테스트 — 화면 좌표로 잰다)
  - 기준: 우클릭 조준 때도 캐릭터가 조준점을 가리지 않는다 (테스트)
  - 기준: 보이는 범위는 누구에게나 같다(display.md) · 벽에 막히면 당겨진다는 규칙은 그대로다 (테스트)
  - 기준: spec 에 「캐릭터는 왼쪽 아래 — 오른손잡이 어깨 너머 (사람 결정 2026-09-26)」를 적는다
  - 기준: 찍어서 확인한다 — 평소 · 조준 두 장
- [x] 2. 인벤토리는 핫바를 포함해 20칸 (2줄)
  - 「기본 인벤토리 2줄(20칸)인데 왜 3줄이지」 — **핫바 10 + 가방 한 줄 10 = 20칸** (사람 결정 2026-09-26, 지금은 30칸)
  - 기준: 새 캐릭터의 인벤토리가 핫바 포함 20칸이다. 가방 창에 두 줄이 보인다 (테스트)
  - 기준: **옛 저장본의 21~30칸에 든 물건을 잃지 않는다** — 빈 칸으로 옮기고, 자리가 없으면 캐릭터 발밑에 떨어뜨린다 (테스트)
  - 기준: 무게 규칙 · 줍기 · 칸 옮기기 · 상자 Shift+클릭이 20칸에서 그대로 된다 (테스트)
  - 기준: spec/02_player/inventory-hotbar.md 수치 「가방 30칸」을 「20칸 (핫바 포함, 사람 결정 2026-09-26)」으로
  - 기준: 찍어서 확인한다 — 가방 창이 두 줄이다
  - 기준: (앞 단계 QA) spec 이 값 자리를 player_config.tres 라고 한다 — movement-controls.md 21줄 「값은 player_config.tres, 임시」인데 shoulder_right · shoulder_up 은 player_config.tres 에 없고 player_config.gd 의 기본값(0.9 · 0.4)에만 있다. spec 을 「player_config.gd」로 고치거나 tres 에 두 값을 적어라
  - 기준: (앞 단계 QA) 조준 사진에서 캐릭터가 핫바를 덮는다 — .loop/out/shoulder/shoulder_aim_ko.png 에서 좁힌 시야로 캐릭터 몸이 커져 핫바 2~3번 칸 위를 가린다. 조준점은 비지만 핫바 글자가 캐릭터 뒤에 묻힌다. 조준 때 캐릭터를 더 왼쪽으로 두거나(shoulder_right 를 조준 때만 늘리기) 몸이 핫바 네모(화면 좌표)와 겹치지 않는지 보는 단언을 test_shoulder_camera.gd 에 더해라
- [x] 3. 장비칸
  - 「인벤토리 칸 위에 있고, 왼쪽에 캐릭터가 보이고 그 주변으로 장비칸이 있는 느낌」
  - 기준: 가방 창의 **인벤토리 칸 위**에 장비 영역이 있다 — **왼쪽에 캐릭터 모습**, 그 둘레에 장비칸 (테스트)
  - 기준: 장비칸은 **신발 · 하의 · 상의 · 모자 · 귀걸이 · 목걸이 · 반지 · 가방** 여덟 (사람 결정 2026-09-26) (테스트)
  - 기준: 그 부위에 맞는 것만 들어간다 — 스파이크 신발은 신발칸 (테스트). 장비칸은 저장본에 담긴다 (테스트)
  - 기준: 칸 이름은 번역 키 (ko · en)
  - 기준: **가방칸에 가방을 끼우면 인벤토리 칸이 는다** (사람 결정 2026-09-26) — 늘어나는 수 · 가방 등급은 임시.
    가방을 빼려는데 늘어난 칸에 물건이 있으면 빠지지 않는다 (잃지 않게) (테스트)
  - 기준: `spec/02_player/equipment.md` 에 여덟 칸 · 가방칸 규칙을 사람 결정으로 적는다. 장비마다의 스탯은 「미정」
  - 기준: 찍어서 확인한다 — 창이 잘리지 않고 1280×720 안에 들어간다
  - 기준: (앞 단계 QA) test_shoulder_camera 주석이 옛 방식을 말한다 — game/tests/player/movement_controls/test_shoulder_camera.gd 의 핫바 단언 위 주석 「조준 때는 어깨를 더 비킨다」는 버린 방식이다. 지금은 좁힌 시야만큼 카메라가 시선을 따라 물러난다(Player.set_camera_fov). 주석을 「조준 때는 좁힌 시야만큼 카메라가 물러난다」로 고쳐라
- [x] 4. (앞 단계 QA) 배낭 레시피·무게가 spec 에 「임시」로 없다
  - 기준: 배낭 레시피·무게가 spec 에 「임시」로 없다 — recipes.json 의 backpack(제작대, 가죽 4 · 목재 2) 과 weights.json 의 backpack 1.0kg 은 지어낸 값인데 report 에만 임시라고 적혔다. spec/02_player/equipment.md 수치 절에 「배낭 레시피 제작대 가죽 4 · 목재 2 · 무게 1.0kg — 임시」 한 줄을 더해라
  - 기준: (앞 단계 QA) 장비칸을 맞지 않는 칸에 놓으면 받는 척한다 — slot_drop.gd _can_drop_data 가 equip 데이터면 chest==null 만 보고 true 라, 스파이크를 끌어 목재가 든 칸에 놓으면 드롭 표시가 뜨고 Equipment.unequip 이 조용히 false 를 낸다. 빈 칸이거나 fits(slot, 그 칸)일 때만 true 로 하고, test_bag_gear 에 「목재 칸은 신발칸 것을 안 받는다」 단언을 더해라

## [x] G-154 계단이 걸린다 — 오르막이 느리다 · 토대 위로 안 붙는다 (사람이 플레이하고 짚었다 2026-09-26)
- 차선: 3d-lane2 (사람 결정 2026-09-26 — 차선 2 가 G-152 다음에 한다)
- spec: spec/06_build/building.md, spec/02_player/movement.md
- 사람이 플레이해 보고 짚었다 (2026-09-26 오후) — G-150 을 합친 판(92638c3)에서
- **자가 피드백**: 깨끗해질 때까지 단계를 계속 붙인다
- [x] 1. 계단에서는 이동 속도가 그대로다
  - 「계단에서 오르막이 느리다. 계단은 이동속도 상관 없이 해야 할듯함」
  - 어디가 그런가: `game/player/movement_controls/player.gd` 의 `slope_factor()` 가 `get_floor_normal()` 로 비탈을 재는데,
    계단은 비탈면이라 `uphill_slowdown`(0.5, player_config.gd)이 그대로 걸려 **오를 때 속도가 절반**이 된다.
    땅 비탈에 쓰라고 만든 배율이 건축물에까지 걸린 것이다
  - 기준: **계단 위에 서 있으면 비탈 배율이 1 이다** — 오를 때도 내려갈 때도 평지와 같은 속도다 (테스트)
  - 기준: **땅 비탈은 그대로** 느려지고 빨라진다 — 계단만 예외다 (테스트)
  - 기준: 계단을 딛고 선 것을 무엇으로 아는지 코드에 적는다 (바닥이 BuildPiece 의 계단인가). 지형과 건축물을 가르는 길이
    이미 있으면 그것을 쓴다 — 새로 만들면 왜 필요한지 주석에 남긴다
  - 기준: `spec/02_player/movement.md`(없으면 building.md 계단 절)에 「계단에서는 비탈 배율이 걸리지 않는다 (사람 결정 2026-09-26)」를 적는다
- [x] 2. 계단은 **딛고 선 바닥**에 받쳐진다 — 지붕이 아니라
  - 「계단 스냅이 지붕 혹은 토대 아래에 붙는데 토대 위로도 붙어야함」 ·
    「탭을 했을 때 스냅이 바뀌는 건 돼. 근데 계단의 스냅 기준이 사진처럼 토대 윗부분이 안 되어 있어서 그런 거고,
    지금은 스냅이 지붕으로만 되어 있어서 지붕을 부수면 계단이 같이 부서져」 (사람이 플레이하고 짚었다 2026-09-26)
  - 무엇이 거꾸로인가: 위로 가는 계단이 **올라가 닿는 지붕 · 위층 바닥에 매달린다** (`stairs_host`) —
    `Buildings.plan_stairs_at` 의 up 갈래 · `test_building_stairs_flip.gd` 가 「위층 바닥에 매달렸다」 ·
    「매달린 바닥이 무너지자 계단도 무너졌다 · 계단 밑 토대 A 는 그대로다」로 이 동작을 **지키고 있다**.
    그래서 (ㄱ) 지붕을 부수면 계단이 같이 죽고 (ㄴ) 매달릴 위층 바닥이 없으면 아예 못 놓는다(`why = "no_support"`)
  - 기준: 계단의 **받침은 딛고 선 바닥**(토대 · 2층 바닥)이다 — `stairs_host` · `link_count` · `held_up` 이 전부 그것을 본다 (테스트)
  - 기준: **지붕(또는 위층 바닥)을 부숴도 계단은 그대로 선다** (테스트) — 지금은 같이 무너진다
  - 기준: **딛고 선 바닥을 부수면 계단이 무너진다** (테스트) — 받침이 바뀌었다고 떠 있는 계단이 생기면 안 된다 (spec 「받쳐야 선다」)
  - 기준: **위층 바닥이 하나도 없어도** 토대 위에서 위로 가는 계단이 놓인다 (테스트) — `no_support` 로 막지 않는다
  - 기준: 앞의 동작을 지키던 **옛 단언을 고친다** — `test_building_stairs_flip.gd` 의 「위층 바닥에 매달렸다」 ·
    「매달린 바닥의 개수 + 1」 · `test_stairs_up_need_an_upper_floor_to_hang_from` 이 새 규칙으로 바뀌어 있다.
    지우기만 하면 안 된다 — 같은 자리를 새 규칙으로 물어야 한다
  - 기준: 「계단이 선 판에는 아무것도 못 짓는다」(G-149)는 그대로다 (테스트)
  - 기준: `spec/06_build/building.md` 에서 「올라가 닿는 바닥에 매달린다」를 「딛고 선 바닥에 받쳐진다 (사람 결정 2026-09-26)」로
    고친다 — 옛 줄이 남지 않는다 (grep)
  - 기준: 찍어서 확인한다 — 토대 한 장 위로 오르는 계단이 선 그림 · 그 위 지붕을 부순 뒤에도 계단이 선 그림
- [x] 3. (앞 단계 QA) stairs_roof 머리 주석이 낡았다
  - 기준: stairs_roof 머리 주석이 낡았다 — game/tests/build/test_building_stairs_roof.gd 3~5행 「그래서 계단이 올라가 닿는 판에는 늘 바닥이 있다」는 이제 틀렸다(위층 바닥 없이도 위로 가는 계단이 놓인다). 그 문장을 지우거나 「위층 바닥 변에서 놓을 때는」으로 좁혀라
  - 기준: (앞 단계 QA) test_rooms 주석에 옛 까닭이 남았다 — game/tests/build/test_rooms.gd 92행 「(사람 결정 2026-09-24 — 붙는 쪽은 늘 받침이 있는 쪽이다)」는 G-154.2 로 덮였다. 「받침은 딛고 선 A (사람 결정 2026-09-26)」로 고쳐라
  - 기준: (앞 단계 QA) stairs_host 와 _link_of 의 바닥 판정이 두 벌이다 — BuildPiece.stairs_host() 는 서 있는 바닥만 보고 Buildings._link_of 는 무너진 것까지 본다(_floor_under). 지금은 무너진 계단이 곧 사라져 드러나지 않지만, 한 곳(예: Buildings 의 정적 함수)으로 모아 두 규칙이 어긋나지 않게 하라
- [x] 4. (앞 단계 QA) 받침 한 벌 테스트의 무너짐 단언이 헛돈다
  - 기준: 받침 한 벌 테스트의 무너짐 단언이 헛돈다 — test_building_stairs_flip.gd::test_stairs_host_and_link_count_read_one_floor_rule 은 C 를 _link_counts 의 무너짐 표({c: true})에만 넣는다. C 노드는 트리에 그대로 서 있다. 그래서 그 뒤의 assert_eq(s.stairs_host(), own, "…두 판정이 어긋나지 않는다") 는 옛 「서 있는 바닥만」 반복문으로 되돌려도 그대로 통과한다. 무너짐을 흉내 내려면 C 를 트리에서 떼거나(remove_child · 무너짐 처리 중 상태), 무너진 C 를 담은 pieces 로 stairs_floor_under 를 직접 불러 true 인지 단언하라. 그 단언이 있어야 이름대로 「한 규칙」을 지킨다. 기준 3 자체는 코드상 하나로 모였으므로 막는 것은 아니다

## [x] G-910 루프가 찾은 것 — 스스로 갚는다 (2차)
- 차선: 3d-start (사람 결정 2026-09-26 — Opus 가 G-151 부터 쭉 돈다. Fable 은 G-150 까지)
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- **차선 2(Fable)가 맡는다** (사람 결정 2026-09-24) — 차선 1(Opus)의 G-145 와 겹치는 것은 G-911 로 뺐다.
  건축(`build/`) · 가방 · 도면 · `player.tscn` · `world/storage/chests.gd` 는 **건드리지 않는다**
- [x] 1. 다쳐서 도망간 뒤 진정하지 않는다 (G-130 단계 3)
  - 기준: 다쳐서 도망간 뒤 진정하지 않는다 — spec/04_life/hunting.md 22행 「한동안 뒤 진정한다」가 아직 없다. panicked() 는 체력만 보므로 30% 아래인 짐승은 영원히 쫓지도 물지도 않고 소리마다 9 m/s 로 달아난다(체력이 차오르는 길도 없다). animal.gd 에 도망 시작 시각을 두고 CALM_AFTER(임시 값, spec 수치 줄에도 「임시」로)가 지나면 panicked() 가 false 가 되게 하라. 테스트: 25% 로 다친 적대가 CALM_AFTER 뒤에 보이는 사람을 다시 CHASE 하는지, 그 전에는 안 하는지 단언하라
- [x] 2. spec 「미정」 줄이 낡았다 (G-130 단계 3)
  - 기준: spec 「미정」 줄이 낡았다 — spec/04_life/hunting.md 68행이 「도망 속도 … 미정」인데 55행에 임시 값 9 m/s 가 섰다. 「도망 속도 (지금 9 m/s 임시)」처럼 고쳐 두 줄이 어긋나지 않게 하라 (막는 것은 아니다)
- [x] 3. 총을 들면 단추의 「(우클릭)」이 틀린다 (G-131 단계 3)
  - 기준: 총을 들면 단추의 「(우클릭)」이 틀린다 — 막는 문제는 아니다. HUNT_BUTCHER 가 「도축 (우클릭)」/「Butcher (Right-click)」인데, 총을 들었을 때 우클릭은 조준이다(사람 결정 2026-09-22, test_right_click_with_a_gun_aims_and_the_button_still_butchers). ButcherPanel._refresh 에서 손에 총이 있으면 「(우클릭)」이 없는 키(예: HUNT_BUTCHER_PLAIN, ko·en 둘 다)를 쓰든지 안내 글자를 없애라. 총을 든 채 패널을 띄웠을 때 단추 글자에 우클릭 안내가 없는지 보는 단언을 test_butchery.gd 에 더하라
- [x] 4. 도축하지 않은 주검이 영원히 남는다 (G-131 단계 3)
  - 기준: 도축하지 않은 주검이 영원히 남는다 — 막는 문제는 아니다. Animal._on_died 가 queue_free 를 하지 않으니 도축하지 않은 사슴(그리고 적대 동물)은 CARCASSES 무리에 쌓이고, Butchery._nearest 는 매 프레임 그 무리를 다 돈다. spec/04_life/hunting.md 「미정」에 「주검이 사라지는 시간」 한 줄을 적어 두고, 임시 값(예: 하루)과 그것을 확인하는 테스트를 넣어라
- [x] 5. 상자에 넣은 짐승은 영영 안 깬다 (G-132 단계 4)
  - 기준: 상자에 넣은 짐승은 영영 안 깬다 — Captives._process 는 가방(player.inventory) 칸만 돌며 게이지를 깎는다. 그런데 world/storage/chest.gd 는 SlotMove.drop 으로 칸을 통째로(kind · sedation · cage 까지) 상자에 옮기므로, 잡은 짐승을 상자에 넣어 두면 게이지가 멈춰 케이지 없이도 영영 가둘 수 있다. 상자에 captive_ 칸을 못 넣게 하거나(Chest 넣기에서 Captives.is_captive 면 거절) 상자 칸도 깎게 하고, test_carry_cage.gd 에 '잡은 짐승을 상자에 넣고 시간이 가도 게이지가 멈추지 않는다(또는 넣을 수 없다)' 단언을 더하라
- [x] 6. 케이지 제작을 실제로 해 보는 테스트가 없다 (G-132 단계 4)
  - 기준: 케이지 제작을 실제로 해 보는 테스트가 없다 — test_three_cages_are_crafted 는 레시피가 있는지만 본다(막는 것은 아니다). 제작대 레시피 경로로 목재 · 철괴를 넣어 cage_small 이 가방에 들어오는지 한 번 보는 단언을 더하라
- [x] 7. 테스트의 안내 줄 아래끝 40 이 박힌 값이다 (G-138 단계 2)
  - 기준: 테스트의 안내 줄 아래끝 40 이 박힌 값이다 — test_chest_window_scroll.gd 의 assert_gte(panel.position.y, 40.0) 와 chest_window.gd TOP_MARGIN 주석이 player.tscn HUD/Hint 의 offset_bottom(40)을 숫자로 베껴 두었다. 안내 줄이 옮겨지거나 두 줄이 되면 테스트는 통과한 채 다시 덮는다. player.tscn 을 불러 HUD/Hint 의 offset_bottom 을 읽어 비교하거나, 적어도 test_ 쪽에 그 값의 출처를 상수로 두고 Hint 의 offset_bottom 과 같은지 보는 단언을 더해라
- [x] 8. chest 찍기에 제작대 창이 남아 겹친다 (G-129 단계 1)
  - 기준: chest 찍기에 제작대 창이 남아 겹친다 — /tmp/g129/after/chest.png 에 앞 자리의 제작대 창이 그대로 깔린다. ui_shots 가 제작대를 먼저 치워 StationWindow._refresh 가 풀린 제작대로 can_start 를 부르는 SCRIPT ERROR 가 난다(보고서 기준, 고치기 전에도 났다). 자리를 바꿀 때 창을 먼저 닫거나 _refresh 에서 is_instance_valid 로 막고, 찍은 로그에 SCRIPT ERROR 가 없는지 보라
- [x] 9. 조준점 겹침은 창이 뜨면 해가 없을 수 있다 (G-129 단계 2)
  - 기준: 조준점 겹침은 창이 뜨면 해가 없을 수 있다 — UIMEASURE 목록 4곳 중 3곳이 Crosshair/H 를 가린다는 줄이다. 창이 열리면 조준점을 숨기는지(게임 쪽) 아니면 재는 검사가 Crosshair 를 가려도 되는 쪽으로 빼야 하는지 G-901 에서 정하고, 어느 쪽이든 목록이 진짜 흠만 뱉게 하라
- [x] 10. 제작대 목록 휠 스크롤이 확인되지 않았다 (G-129 단계 2)
  - 기준: 제작대 목록 휠 스크롤이 확인되지 않았다 — 구현 세션이 headless push_input 휠로 스크롤을 못 움직였다고 적었다. Scroll 이 PASS 라 휠을 받는지, scroll_vertical 을 직접 바꿔 아래 줄이 보이는 자리로 오는지 단언하는 테스트를 test_station_window.gd 에 넣어라
- [x] 11. ui_shots 의 _reset 이 제작대 창을 못 닫는다 (G-129 단계 2)
  - 기준: ui_shots 의 _reset 이 제작대 창을 못 닫는다 — stations.open(null) 은 무시되어 chest 사진에 앞 제련로 창이 남고 can_start SCRIPT ERROR 가 난다(구현 세션 보고). stations.close() 로 바꾸고 찍는 폭 단계에서 chest.png 를 열어 확인하라
- [x] 12. 섬 사진 테스트가 얕다 (G-129 단계 3)
  - 기준: 섬 사진 테스트가 얕다 — test_island_shot_builds_the_real_island 는 island_scene 이 null 이 아닌지만 본다. 세운 뒤 game.island()(세운 섬)가 있고 HUD(hotbar)가 보이는지까지 단언하거나, 이름대로 '섬을 세운다'를 확인하게 고쳐라
- [x] 13. 녹이는 곳 창 바닥이 넓게 비어 있다 (G-901 단계 1)
  - 기준: 녹이는 곳 창 바닥이 넓게 비어 있다 — report 가 남긴 것: 레시피가 둘뿐인데 창이 크게 잡혀 아래가 빈다. 창 높이를 레시피 수에 맞춰 줄이고(제작대 창이 핫바 위에 머무는 규칙은 지킨다) smelter 사진으로 확인한다
- [x] 14. 테스트가 GameRoot 의 밑줄 함수를 직접 부른다 (G-901 단계 1)
  - 기준: 테스트가 GameRoot 의 밑줄 함수를 직접 부른다 — test_window_layout.gd 의 _open · _close_all 과 core/debug/ui_shots.gd 가 game._on_chest_open_changed(...) 를 부른다. 시그널 받는 쪽 함수라 이름이 바뀌면 조용히 깨진다. GameRoot 에 open_chest_window(chest)(null 이면 닫기) 같은 드러난 함수를 두고 셋 다 그것을 부르게 한다
- [x] 15. 낡은 Godot 함수 경고의 출처를 적어 둔다 — `instance_reset_physics_interpolation()` 은 deprecated 다
  - 확인함 (2026-09-24 대화 세션): **우리 GDScript 는 이 함수를 부르지 않는다.** `game/addons/terrain_3d/bin/` 의
    Terrain3D 1.0.2 바이너리 안에서 부른다 — `_build_terrain` 이 Terrain3D 를 만들 때 경고가 뜨는 까닭이다
  - 기준: `game/` 에서 애드온을 뺀 곳에 이 함수가 없다는 것을 검사로 지킨다 (테스트 — 소스를 읽어 본다)
  - 기준: `THIRD_PARTY.md` 의 Terrain3D 줄에 「1.0.2 는 deprecated 함수를 불러 경고가 뜬다 — 새 판으로 올릴 때 사라진다」를 적는다
  - 애드온을 새 판으로 올리는 것은 이 단계가 아니다 (사람이 정한다)
- [x] 16. 갈고리총이 3발이다 — 사람 결정은 4발이다 (2026-09-25 대화 세션 점검)
  - `spec/08_combat/tools-as-weapons.md` 수치 「갈고리총 **4발** (사람 결정 2026-09-22)」인데 `game/life/climbing/grapple.gd:30` 은 `magazine := 3` 이다.
    테스트가 탄창을 상대값으로만 견줘 못 잡았다
  - 기준: 갈고리총에 **4발**이 들어간다 — 숫자 4 를 단언하는 테스트가 있다 (테스트)
  - 기준: `grapple.gd` 의 「임시」 표시를 지운다 — 사람이 정한 값이다. `spec/04_life/climbing.md` 수치 절의 「탄창은 grapple.gd」 줄도 「4발 (사람 결정)」으로 맞춘다
- [x] 14. test.log 합계 한 줄이 어긋난다 (G-910 단계 3)
  - 기준: test.log 합계 한 줄이 어긋난다 — 전체 결과가 Tests 1430 · Passing 1429 · Failing 0 · Pending 0 이라 하나가 어느 쪽에도 안 든다. 단언 없는(GUT 의 risky) 테스트가 하나 있을 것이다. 이번 단계와 무관하다 (test_butchery 는 10개 다 통과·단언 110). test.sh 가 남기는 파일별 로그에서 'risky' 를 찾아 그 테스트에 진짜 단언을 넣어라
- [x] 13. 죽음 상자 안에서는 풀린 짐승의 게이지가 멈춘다 (G-910 단계 5)
  - 기준: 죽음 상자 안에서는 풀린 짐승의 게이지가 멈춘다 — combat/damage_death/death_chest.gd:43 은 가방을 _items 에 통째로 옮기므로 accepts 를 거치지 않는다. 30분 뒤 상자가 사라지니 「영영」은 아니지만(사람 결정 2026-09-16), 열어 두면 상자 시간도 멈추므로 그동안 게이지가 얼어 있다. spec/04_life/hunting.md 「미정」에 「죽었을 때 풀린 짐승 — 죽음 상자에 얼린 채 두나, 그 자리에 놓아 도망가게 하나」 한 줄을 적고, 지금 행동(얼린 채 죽음 상자에 간다)을 test_death_chest.gd 에 단언 하나로 못 박아라
- [x] 17. 전체 합계에서 테스트 하나가 통과도 실패도 아니다 (G-910 단계 5)
  - 기준: 전체 합계에서 테스트 하나가 통과도 실패도 아니다 — test.log 합친 결과가 Tests 1440 · Passing 1439 · Failing 0 · Pending 0 이다. GUT 의 risky(단언 없는) 테스트가 하나 있는 것으로 보이는데 tools/test.sh 가 임시 폴더를 지워 어느 파일인지 남지 않는다. test.sh 의 합치는 awk 에 Risky 줄도 더하고, Tests 와 Passing+Failing+Pending 이 안 맞으면 그 파일 이름을 맨 뒤에 찍게 하라
- [x] 18. 줄 · 버퍼 칸 누르기에는 지킴이가 없다 (G-910 단계 8)
  - 기준: 줄 · 버퍼 칸 누르기에는 지킴이가 없다 — station_window.gd 의 _press 와 _collect 는 _station == null 만 보고 _drop_freed() 를 안 부른다. 제작대가 풀린 그 프레임 안(_process 의 _refresh 가 돌기 전)에 줄이나 버퍼 칸이 눌리면 풀린 객체로 start_craft · collect 를 불러 같은 SCRIPT ERROR 가 난다. _press · _collect 첫머리에 _drop_freed() 를 넣고, test_station_window.gd 의 test_a_station_freed_while_the_window_is_open_is_not_asked_anything 에서 station.free() 직후 wait_process_frames 없이 row(AXE).pressed.emit() · buffer_slot(0).pressed.emit() 을 먼저 한 번 더 눌러 조용한지 단언하라 (지금은 station_kind() 가 먼저 불려 null 이 된 뒤에만 누른다)
- [x] 19. station_window.gd 머리말의 줄 수가 낡았다 (G-910 단계 10)
  - 기준: station_window.gd 머리말의 줄 수가 낡았다 — 25행 「제작대 레시피가 스물 몇 줄이라 화면 아래로 넘쳤다」인데 recipes.json 의 가공대 레시피는 44줄이다(이번 테스트 주석은 「마흔 몇 줄」로 맞다). 「마흔 몇 줄」로 고쳐라 (막는 것은 아니다)
- [x] 20. 탄창 주석이 spec 에 없는 말을 한다 (G-910 단계 16)
  - **줄기에는 아직 없는 코드다** — 이 묶음의 브랜치 `g/G-910`(99ab8a9)이 넣은 주석이다. 그 브랜치에서 이어 하면 보인다 (2026-09-26 확인)
  - 기준: 탄창 주석이 spec 에 없는 말을 한다 — game/life/climbing/grapple.gd:29 「등급이 생기면 등급표가 덮는다」는 spec/04_life/climbing.md 에 없다. 등급마다 다른 것은 **최대 거리**(사람 결정 2026-09-17)뿐이고 탄창은 4발로 정해졌다. 그 문장을 지워라 (test_the_magazine_is_not_marked_temporary 는 「임시」·「사람 결정」만 보니 지워도 통과한다)
- [x] 21. test.sh 합계의 Pending 이 늘 0 으로 찍힌다 (G-910 단계 14)
  - 기준: test.sh 합계의 Pending 이 늘 0 으로 찍힌다 — tools/test.sh 끝의 awk 가 '^Pending +[0-9]' 를 찾는데 GUT(addons/gut/summary.gd:64) 는 그 줄을 'Risky/Pending N' 으로 찍고 0 이면 아예 안 찍는다. 그래서 pending·risky 가 있어도 합계에 안 보이고 Tests 와 Passing 만 어긋나 보인다 — 이번 기준의 「하나가 어느 쪽에도 안 든다」가 바로 그것이었다. awk 패턴을 '^(Risky\/)?Pending +[0-9]' 로 바꾸고 $NF 를 더하라. 테스트: pending() 하나만 있는 임시 테스트 파일로 test.sh 를 돌려 합계의 Pending 이 1 로 찍히는지 보는 셸 테스트(tests/ 아래 GD 로 어렵다면 harness/ 의 python 테스트)를 짜라
- [x] 22. test.sh 가 파일별 로그를 남기지 않는다 (G-910 단계 14)
  - 기준: test.sh 가 파일별 로그를 남기지 않는다 — 기준이 「test.sh 가 남기는 파일별 로그에서 risky 를 찾아라」고 했지만 tools/test.sh 는 mktemp 폴더에 두고 trap 으로 지운다. 구현 세션이 찾을 수 없어 파일마다 godot 을 따로 띄워 되짚었다. 떨어졌을 때만이 아니라 늘 .loop/out/test_files/ 같은 곳에 파일별 로그를 복사해 두거나(하네스 쪽이면 [결정] 아님 — 세션이 tools/test.sh 를 고칠 수 있다), 아니면 위 awk 고침으로 합계에 Risky/Pending 이 보이게 해서 로그 없이도 찾게 하라
- [x] 23. 지키는 테스트 머리말이 실제 검사보다 세게 말한다 (G-910 단계 14)
  - **줄기에는 아직 없는 파일이다** — `test_from_empty_hands_asserts_to_the_end.gd` 는 `g/G-910` 에만 있다. 덧붙여 그 파일이 전제한 「마지막 걸음 = 25」도 낡았다 — 지금 test_from_empty_hands.gd 는 27번(돌아온 재료로 다시 짓는다)까지 간다 (2026-09-26 확인)
  - 기준: 지키는 테스트 머리말이 실제 검사보다 세게 말한다 — test_from_empty_hands_asserts_to_the_end.gd 머리말·단언문은 「중간에 return 으로 빠져 뒤가 비지 않는다」고 하지만 검사는 글자 줄 수만 센다. '# 25.' 바로 아래에 `return` 을 넣어도 통과했다(일부러 깨 봄). 몸통에 들여쓴 `return` 줄이 없는지도 같이 단언하거나(_body_of 결과에서 strip_edges()=='return' 줄 0개), 머리말을 「25번 아래에 단언 글줄이 있다」로 낮춰 적어라
- [x] 24. 옛 시각 상수가 테스트에 남았다 (G-146 단계 2)
  - 기준: 옛 시각 상수가 테스트에 남았다 — test_day_light.gd 에 DUSK_AT 18.5 · DAWN_AT 5.5 가 새 SUNSET_AT 18.0 과 같이 있다. 앞 테스트(test_brightness_runs_…)가 아직 쓰고 있어서 틀린 것은 아니다. 다만 「노을」이 두 시각이라 읽기가 헷갈린다. 주석에 둘이 무엇이 다른지 한 줄 적거나 하나로 합쳐라
- [x] 25. 횃불이 미리보기 그물에 없다 (G-150 단계 5 — 2026-09-26 다시 쟀다)
  - 앞 전제는 사라졌다: 횃불 놓기는 G-146.3 으로 생겼다 (`world/lighting/torches.gd` place_held · place_at, 불빛은 torch_light.gd)
  - 기준: `Torches` 가 `Placeable.PLACERS` 에 든다 — 지금 드는 것은 stations · chests · farmland · buildings 넷뿐이라
    횃불만 미리보기가 없다. 들면 놓일 자리가 초록 · 빨강으로 뜬다 (테스트)
  - 기준: `game/tests/build/test_placeable_previews.gd` 의 무대에 **Torches 를 세운다** — 지금은 세우지 않아
    「놓이는 것 전부」를 훑는다는 그물에 횃불이 아예 안 걸린다 (테스트)
  - 기준: `spec/03_world/lighting.md` 14줄 「지금은 들어도 · 놓아도 아무것도 안 밝다」를 지운다 — 횃불이 들어와 낡은 줄이다 (grep)
- [x] 10. 진짜 섬 물로 채우는 테스트가 없다 (G-152 단계 1)
  - 기준: 진짜 섬 물로 채우는 테스트가 없다 — 채우기 테스트는 모두 가짜 연못(fresh_water_level 을 테스트가 넣은 Callable)이다. test_island.gd 가 crops.fresh_water_level 을 shape.fresh_water_level_at 에 이었는지는 아무 테스트도 안 본다. tests/world/terrain/test_island_in_game.gd 같은 섬 테스트에 「섬의 crops.fresh_water_level 이 유효하고 섬의 연못 한 자리에서 수면 높이를 낸다」는 단언을 더하라
- [x] 8. _process 주석의 줄 수도 낡았다 (G-910 단계 19)
  - 기준: _process 주석의 줄 수도 낡았다 — game/ui/hud/station_window.gd 160행 「줄이 스물 몇이라 세는 값이 싸다」도 가공대 레시피 45줄과 안 맞는다. 「줄이 마흔 몇이라」로 고쳐라 (주석만 — 막는 것은 아니다)
- [x] 8. 테스트 사이 빈 줄이 하나뿐이다 (G-910 단계 20)
  - 기준: 테스트 사이 빈 줄이 하나뿐이다 — game/tests/life/climbing/test_grapple_magazine_size.gd 의 test_the_magazine_does_not_vary_by_grade 끝과 다음 ## 주석(test_the_check_catches_the_old_shape) 사이에 빈 줄이 하나다. 파일의 다른 함수들처럼 두 줄로 맞춰라 (모양만의 문제다)
- [x] 8. 테스트가 매번 사용자 휴지통에 폴더를 버린다 (G-910 단계 21)
  - 기준: 테스트가 매번 사용자 휴지통에 폴더를 버린다 — test_test_sh_counts_pending.gd 의 after_each 가 OS.move_to_trash(_dir) 를 불러 테스트가 돌 때마다 macOS ~/.Trash 에 naru_pending_only 폴더가 하나씩 쌓인다(사람 환경을 건드린다). 임시 파일은 DirAccess.remove_absolute(_file) 뒤 DirAccess.remove_absolute(_dir) 로 바로 지워라
- [x] 9. 안쪽 test.sh 가 --import 를 다시 부른다 (G-910 단계 21)
  - 기준: 안쪽 test.sh 가 --import 를 다시 부른다 — 이 테스트는 전체 실행 도중 test.sh 를 한 번 더 띄우므로 안쪽 test.sh 머리의 godot --import 가 다른 godot 들이 같은 game/.godot/ 를 쓰는 동안 돈다. 지금은 통과했지만 겹치면 흔들릴 수 있다. 안쪽 실행에서 import 를 건너뛰는 환경 변수(예: TEST_SKIP_IMPORT=1)를 test.sh 에 두고 이 테스트가 그것을 넘기게 하라
- [x] 8. 단언문 하나에 낡은 25번이 남았다 (G-910 단계 23)
  - 기준: 단언문 하나에 낡은 25번이 남았다 — test_from_empty_hands_asserts_to_the_end.gd 의 test_the_check_catches_the_old_tail 에서 `assert_eq(_assert_lines(_after_last_step(fine, LOOP_FUNC)), 1, "25번 아래의 단언을 못 센다")` 의 글이 아직 25번이라고 한다. LAST_STEP 은 이제 27 이니 「마지막 걸음 아래의 단언을 못 센다」로 고쳐라
- [x] 3. 다른 테스트 셋도 휴지통에 버린다 (G-910 단계 8)
  - 기준: 다른 테스트 셋도 휴지통에 버린다 — game/tests/core/tools/test_test_totals_awk.gd:29 · game/tests/core/tools/test_test_sh_keeps_file_logs.gd:32 · game/tests/ui/hud/test_map_pin.gd:218 이 여전히 OS.move_to_trash 를 불러 돌 때마다 사람의 ~/.Trash 에 폴더를 쌓는다. test_test_sh_counts_pending.gd 의 after_each 처럼 안의 파일(logs/ 같은 하위 폴더 포함)을 DirAccess.remove_absolute 로 지운 뒤 폴더를 지워라. 지키는 것으로는 game/ 아래 .gd 전체에 "OS." + "move_to_trash" 가 0 번인지 보는 테스트 하나를 두어라 (지금은 파일마다 제 소스만 본다)
- [x] 3. 파일 머리 주석이 새 테스트를 말하지 않는다 (G-910 단계 9)
  - 기준: 파일 머리 주석이 새 테스트를 말하지 않는다 — test_test_sh_counts_pending.gd 1~5줄 머리 주석은 Pending 합계만 설명한다. 이제 휴지통 안 쓰기 · 안쪽 import 건너뛰기(test_inner_test_sh_skips_import)도 지키므로 머리 주석에 한 줄씩 덧붙여라
- [x] 2. 파일마다 보던 옛 검사가 겹쳐 남았다 (G-910 단계 3)
  - 기준: 파일마다 보던 옛 검사가 겹쳐 남았다 — game/tests/core/tools/test_test_sh_counts_pending.gd:86 의 src.count("OS." + "move_to_trash") 단언은 이제 test_no_move_to_trash.gd 가 game/ 전체를 보므로 겹친다. 지워도 되고 남겨도 해는 없다 — 남긴다면 주석에 전체 검사가 따로 있다고 적어 두어라
- [x] 3. 다른 테스트가 TMPDIR 에 임시 폴더를 남긴다 (G-910 단계 3)
  - 기준: 다른 테스트가 TMPDIR 에 임시 폴더를 남긴다 — 구현 세션 보고대로 TMPDIR 에 naru_key_* · naru_locked_* · naru_island_animals_* 따위가 쌓인다. game/tests 에서 OS.get_temp_dir() 로 폴더를 만드는 테스트를 찾아 after_each 에서 안의 파일 → 폴더 순으로 DirAccess.remove_absolute 로 지우게 하고, 돌린 뒤 그 폴더가 없는지 단언하라
- [x] 3. 휴지통 줄의 번호 표가 어긋난다 (G-910 단계 3)
  - 기준: 휴지통 줄의 번호 표가 어긋난다 — test_test_sh_counts_pending.gd 7줄과 after_each 위 23줄 주석이 휴지통 고침을 「G-910.21」로 적었지만 G-910.21 은 2줄의 Pending 합계 항목이고 휴지통 고침은 커밋 0342a6c 「G-910.8 … (G-910 단계 21)」이다. 8줄처럼 「G-910 단계 21」로 맞추고, 7줄에도 지키는 테스트 이름 test_after_each_removes_the_temp_dir_without_the_trash 를 적어 8줄과 꼴을 맞춰라
- [x] 3. 테스트 이름이 휴지통을 안 본다 (G-910 단계 2)
  - 기준: 테스트 이름이 휴지통을 안 본다 — test_test_sh_counts_pending.gd 의 test_after_each_removes_the_temp_dir_without_the_trash 는 이제 폴더가 지워졌는지만 본다. 휴지통 검사는 test_no_move_to_trash.gd 로 옮겨 갔으니 이름을 test_after_each_removes_the_temp_dir 로 줄이면 읽는 사람이 헷갈리지 않는다 (막는 것은 아니다)
- [x] 3. build/ 두 파일은 아직 지웠는지 단언이 없다 (G-910 단계 3)
  - 기준: build/ 두 파일은 아직 지웠는지 단언이 없다 — tests/test_temp_dirs_are_cleaned.gd 의 NOT_OURS 가 tests/build/test_building_qa_sweep.gd · test_building_roof_stairs.gd 를 뺐다(차선 1 G-145 가 build/ 를 쥐고 있어서 — 이번 묶음 제약상 옳다). 그 차선이 풀린 뒤 두 파일의 지우는 곳을 TempDir.clean(self, path) 로 바꾸고 NOT_OURS 를 비워라
- [x] 4. 훑기 검사가 글자 하나만 본다 (G-910 단계 3)
  - 기준: 훑기 검사가 글자 하나만 본다 — _leaks 는 파일 안에 TempDir.clean(self, 가 한 번이라도 있으면 통과로 친다. 한 파일이 임시 폴더를 둘 만들고 하나만 clean 에 넘겨도 못 잡는다. 지금 그런 파일은 없어 보이나, 막으려면 테스트 뒤 TMPDIR 에 naru_* 가 새로 생겼는지 보는 검사(예: tools/test.sh 가 돌기 전후 TMPDIR 의 naru_* 목록을 비교해 늘었으면 알린다)를 더하라
- [x] 2. _erase 를 지운 자리에 빈 줄이 남았다 (G-910 단계 3)
  - 기준: _erase 를 지운 자리에 빈 줄이 남았다 — game/tests/build/test_building_qa_sweep.gd 의 _apply_island_state 와 _stand_and_aim 사이에 빈 줄이 셋이다(함수 사이는 둘). game/tests/build/test_building_roof_stairs.gd 는 _held_count 뒤 파일 끝에 빈 줄이 하나 더 남았다. 하나씩 지워라
- [x] 1. test_foundation_wheel_attach.gd 에 빈 줄 셋 (G-910 단계 2)
  - 기준: test_foundation_wheel_attach.gd 에 빈 줄 셋 — game/tests/build/test_foundation_wheel_attach.gd 51~53행이 빈 줄 셋이다(함수 사이는 둘). 하나를 지워라. 이번 기준 밖이고 build/ 는 차선 1 몫이니 그쪽 묶음 단계로 넘겨도 된다
- [x] 1. 다른 파일에도 빈 줄이 셋 이상 남았다 (G-910 단계 1)
  - 기준: 다른 파일에도 빈 줄이 셋 이상 남았다 — addons/ 를 빼고 훑으면 game/build/placeable.gd 125~127행(return {at = spot, ok = true, why = ""} 뒤, '## 이 놓인 것을 부수면' 앞 빈 줄 셋), game/tests/life/hunting/test_animal_walk.gd 254~257행(파일 끝 빈 줄 넷 — 끝의 빈 줄은 모두 지워 줄바꿈 하나로 끝내라), game/tests/world/frontier_islands/test_dock_building.gd 386~388행(함수 사이 빈 줄 셋)이 걸린다. 함수 사이는 둘로 맞춰라. build/placeable.gd 는 차선 1 몫이니 그쪽 묶음 단계로 넘긴다. 다시 생기지 않게 하려면 tests/core/tools 에 '.gd 파일(addons 제외)에 빈 줄이 셋 이상 이어지는 곳이 없다'는 훑기 테스트를 두면 된다
- [x] 1. placeable.gd 빈 줄 셋이 아직 남았다 (G-910 단계 1)
  - 기준: placeable.gd 빈 줄 셋이 아직 남았다 — game/build/placeable.gd 125~127행(return {at = spot, ok = true, why = ""} 뒤, '## 이 놓인 것을 부수면' 앞)의 빈 줄 셋을 둘로 줄여라. build/ 를 맡은 쪽 단계에서 할 일이다. 고친 뒤에는 game/tests/core/tools/test_no_triple_blank_lines.gd 의 LEFT_FOR_LANE_1 에서 "build/placeable.gd" 를 지워 빈 목록으로 두고, test_no_script_under_game_has_three_blank_lines_in_a_row 가 그대로 통과하는지 확인하라
- [x] 2. 예외 목록이 낡아도 알 수 없다 (G-910 단계 1)
  - 기준: 예외 목록이 낡아도 알 수 없다 — test_no_triple_blank_lines.gd 의 LEFT_FOR_LANE_1 에 든 파일을 고쳐도 목록에서 지우지 않으면 그 파일은 계속 검사 밖에 남고, 이를 알리는 것이 없다. 목록의 파일마다 assert_false(_triple_blanks(...).is_empty(), "<파일>은 이제 고쳐졌다 — LEFT_FOR_LANE_1 에서 지워라") 를 두어, 고친 뒤 목록이 남아 있으면 테스트가 떨어지게 하라
- [x] 2. 빈 LEFT_FOR_LANE_1 과 낡은 주석이 남았다 (G-910 단계 1)
  - 기준: 빈 LEFT_FOR_LANE_1 과 낡은 주석이 남았다 — game/tests/core/tools/test_no_triple_blank_lines.gd 7~8행의 '아직 남은 채로 두는 파일 — 건축(build/)은 차선 1 몫이라 …' 주석과 빈 상수 LEFT_FOR_LANE_1, 그리고 테스트 안의 'if rel in LEFT_FOR_LANE_1: continue' 는 이제 지키는 것이 없다. 예외가 없으니 셋을 모두 지우고 훑기가 예외 없이 game/ 전체를 본다는 것을 머리 주석에 적어라 (예외 자리가 남아 있으면 다음에 파일을 슬쩍 올려 두기 쉽다)
- [x] 2. 새 테스트 뒤 빈 줄이 하나뿐이다 (G-910 단계 2)
  - 기준: 새 테스트 뒤 빈 줄이 하나뿐이다 — game/tests/core/tools/test_no_triple_blank_lines.gd 의 test_files_left_for_lane_1_still_have_three_blank_lines 끝(pass_test 줄)과 func test_the_check_catches_three_blank_lines 사이 빈 줄이 하나다. 파일의 다른 함수 사이처럼 둘로 맞춰라

## [x] G-913 G-910 에서 뗀 셋 — 차선 2 가 같이 한다 (사람 결정 2026-09-26)
- 차선: 3d-lane2
- **이 묶음의 브랜치 `g/G-913` 은 `g/G-910` 끝에서 갈라 두었다** — 셋 다 G-910 단계 23 · 25 가 만든 코드를
  고치는 일이라 줄기에서 갈라지면 그 파일이 아예 없다 (대화 세션이 미리 만들었다 2026-09-26)
- G-910 을 빨리 끝내려고 사람이 셋을 떼어 차선 2 에 맡겼다. 나머지는 차선 1 이 이어서 한다
- [x] 1. 머리말의 옛 단언 수가 낡았다 (G-910 단계 23)
  - 기준: 머리말의 옛 단언 수가 낡았다 — 같은 파일 머리말 6~7줄의 「지금은 814개를 단언하고 끝까지 간다 (전체 1507 · 통과 1507)」가 지금(1092 단언 · 전체 1665)과 다르다. 「G-148 때는」처럼 그때 일로 적거나 숫자를 빼라
- [x] 2. PLACERS 주석이 놓는 곳을 넷만 적는다 (G-910 단계 25)
  - 기준: PLACERS 주석이 놓는 곳을 넷만 적는다 — game/build/placeable.gd 25줄 「(Stations · Chests · Farmland · Buildings)」에 Torches 가 빠졌다. 이 묶음은 build/ 를 건드리지 않아 남겼다. 「(Stations · Chests · Farmland · Buildings · Torches)」로 고쳐라
- [x] 3. 토대 위 횃불 놓기에 테스트가 없다 (G-910 단계 25)
  - 기준: 토대 위 횃불 놓기에 테스트가 없다 — torches.gd aimed_spot 이 Placeable.plan_cell 로 합쳐지면서 토대 · 지붕 윗면에도 횃불이 놓이게 됐는데(전에는 BuildPiece 위를 막았다), 이를 지키는 테스트가 없다. tests/world/lighting/test_torch.gd 에 토대를 하나 놓고 윗면을 짚어 place_held 가 횃불을 그 윗면 높이에 세우는지, 벽 옆면을 짚으면 안 놓이는지 단언을 넣어라


## [x] G-911 G-910 에서 뺀 것 — 차선을 나누느라 갈라졌던 것 (2026-09-26 다시 합쳤다)
- 차선: 3d-lane2 (사람 결정 2026-09-26 — 차선 2 가 G-154 다음에 한다)
- **G-912 를 여기로 되돌렸다** (사람 결정 2026-09-26) — 단계 6 · 7 · 8 이 그것이다. 나눈 까닭(차선 둘이 같은 파일을
  동시에 고치는 것)이 사라졌다: G-145 는 끝났고, 목록에 남은 것은 모두 차선 1(3d-start · Opus) 몫이다
- G-910 을 차선 2 가 맡으면서 차선 1(G-145 건축)과 겹치는 것을 뺐다 (사람 결정 2026-09-24).
  G-145 가 합쳐진 뒤에 한다. shot.sh 둘은 하네스라 **사람이** 고친다
- [x] 1. shot.sh 기본 해상도가 1280x720 이 아니다 — 대화 세션이 고쳤다 (2026-09-24) (G-129 단계 1)
  - 뺀 까닭: harness/shot.sh — 세션은 하네스를 못 고친다
  - 기준: shot.sh 기본 해상도가 1280x720 이 아니다 — 찍힌 PNG 가 1600x900 이다. 화면은 1280×720 고정(spec/01_settings/display.md)이라 찍는 폭도 그에 맞춰라 (이 묶음 「찍는 폭」 단계의 몫)
- [x] 2. shot.sh 쓰임 줄이 낡았다 — 대화 세션이 고쳤다 (2026-09-24) (G-129 단계 3)
  - 뺀 까닭: harness/shot.sh — 세션은 하네스를 못 고친다
  - 기준: shot.sh 쓰임 줄이 낡았다 — harness/shot.sh 5줄의 자리 목록(hud · bag · workbench · smelter · chest · menu)에 drag · ammo · island 가 없고 _ko/_en 두 장이라는 말도 없다. ui_shots_kit.gd PLACES 를 가리키게 고쳐라
- [x] 3. 창이 떠 있어도 위 안내 줄이 걷기 안내다 (G-901 단계 1)
  - 뺀 까닭: player.tscn HUD 안내 줄 — G-145 가 player.tscn · 입력 안내를 고치는 중
  - 기준: 창이 떠 있어도 위 안내 줄이 걷기 안내다 — 모든 창 사진 맨 위에 「WASD 이동 · … · Esc 마우스 풀기 · 클릭 다시 잡기」가 그대로 떠 있다. 창이 떠 있을 때는 이 줄을 숨기거나 창 조작 안내로 바꿀지 spec/12_ui/hud.md 를 보고 맞춘다 — spec 에 없으면 「미정」에 한 줄 적어 둔다
- [x] 4. 테스트가 임시 폴더를 안 지운다 (2026-09-24 대화 세션 점검)
  - $TMPDIR 에 naru_locked_* 155 · naru_key_* 154 · naru_island_animals_* 22 개가 쌓였다 — 돌 때마다 는다
  - 까닭 (확인함): 지우는 코드는 있는데 `_erase` 가 **파일만 지우고 하위 폴더(worlds/)를 못 지운다** — 그래서
    마지막 `remove_absolute(폴더)` 도 실패해 통째로 남는다 (test_locked_island.gd:261 · test_island_key.gd:302)
  - 기준: 임시 폴더를 **하위 폴더까지** 지운다 — 한 곳에 재귀로 지우는 함수를 두고 테스트들이 그것을 쓴다 (테스트: 지운 뒤 폴더가 없다)
  - 기준: 테스트 폴더 전체에서 `OS.get_temp_dir()` 아래 만든 것을 안 지우는 곳이 없다 — 찾아서 같이 고친다
  - 기준: (앞 단계 QA) 상자 창 여백 주석이 낡았다 — game/ui/hud/chest_window.gd:35 주석이 「왼쪽 위 조작 안내 줄(HUD/Hint)을 덮지 않게 위 여백을 비워 둔다」고 하지만, 이제 창이 뜨면 HUD/Hint 는 숨는다(G-911.3). 주석을 지금 사정에 맞게 고치거나(안내 줄은 숨지만 자리는 비워 둔다 등 까닭을 적기) 여백이 더 필요 없으면 여백과 주석을 함께 정리하고 test_window_layout 이 그대로 통과하는지 본다
  - 기준: (앞 단계 QA) 함수 이름이 하는 일과 어긋난다 — player.gd _show_crosshair 가 이제 조준점과 위 안내 줄을 같이 다룬다. _show_walk_hud 처럼 둘을 아우르는 이름으로 바꾸고 부르는 곳(lock_controls · unlock_controls)도 함께 고친다 (막는 것은 아니다)
- [x] 5. 섬 저장본(island_cache)이 옛 판을 안 지운다 (2026-09-24 대화 세션 점검)
  - user://island_cache 에 24 개 · 379MB. 굽는 코드가 바뀔 때마다 새 이름으로 하나씩 는다 (VERSION · SOURCES 해시)
  - 기준: 새로 구워 저장할 때 **같은 섬의 옛 판**을 지운다 — 섬마다 최신 하나만 남는다 (테스트)
  - 기준: 다른 섬(개척 섬 · 시험 섬)의 저장본은 지우지 않는다 (테스트)
  - 기준: 여러 godot 이 한꺼번에 돌 때(tools/test.sh) 남이 쓰는 중인 파일을 지워 터지지 않는다 — 없으면 다시 굽는 것으로 족하다
  - 기준: (앞 단계 QA) 임시 폴더 지킴 테스트가 글자만 본다 — test_temp_dir.gd:test_every_test_that_makes_temp_erases_it_through_one_place 는 파일에 「TempDir.erase(」 글자가 한 번이라도 있으면 통과라, 한 파일이 임시 폴더를 둘 만들고 하나만 지워도 못 잡는다. 지금 57 개 파일은 맞게 고쳐져 있어 막는 것은 아니다. 더 굳히려면 before_all/after_all 에서 $TMPDIR 의 naru_* 목록을 테스트 앞뒤로 비교하는 식(또는 OS.get_temp_dir() 가 나온 횟수 ≤ erase 가 닿는 변수 수)을 검토하라
  - 기준: (앞 단계 QA) 이미 쌓인 옛 임시 폴더가 남아 있다 — $TMPDIR 에 naru_locked_* · naru_key_* · naru_island_animals_* 가 수백 개 남았다(보고서: 639개). 새 코드는 더 쌓지 않지만 옛 것은 손으로 지워야 한다: rm -rf "$TMPDIR"/naru_* (다른 체크아웃의 테스트가 돌지 않을 때)
- [x] 6. 연쇄 되풀이를 지키는 테스트가 없다 (G-133 단계 3)
  - 뺀 까닭: buildings.gd collapse — G-145 가 건축을 다시 짜는 중
  - 기준: 연쇄 되풀이를 지키는 테스트가 없다 — buildings.gd collapse() 의 while 루프에서 `more = true` 를 지워 한 번만 훑게 해도 tests/build 53개가 다 통과했다. 지금 테스트는 받치는 것이 늘 받쳐지는 것보다 먼저 놓여(무리 순서가 유리해) 한 번 훑기로도 잡힌다. 받침이 나중에 놓인 경우를 짜라: 토대 A · 벽 W1(A) · 지붕 R1 · R1 위 기둥 P · 지붕 R2 를 놓은 뒤, 옆 토대 C 와 R1 을 받치는 벽 W2(C) 를 나중에 놓고 W1 을 손으로 부순다. 그다음 C 를 부수면 W2 · R1 · P · R2 가 전부 무너지는지 단언하라 (한 번 훑기면 R1 · P · R2 가 떠서 남는다)
  - 기준: (앞 단계 QA) 코드로 만든 설계 값이 저장본 한 칸을 나눠 쓴다 — IslandCache.island_key 가 resource_path 가 빈 설계 값을 모두 「unnamed」 하나로 쳐서, 캐시를 켠(use_cache=true) 서로 다른 코드 설계 값 둘이 번갈아 저장하면 서로의 저장본을 옛 판이라며 지운다. 지금은 캐시를 켜는 곳(test_island.gd · test_lime · test_volcano · test_island_resources · test_dock_building · test_hunger_thirst)이 모두 .tres 를 load 한 것이라 드러나지 않는다(막는 것은 아니다). 고치려면 island_cache.gd 의 prune 에서 key 가 "unnamed" 이면 지우지 않거나(주석으로 까닭을 적기), island_key 주석에 「unnamed 는 한 칸 — 캐시를 켜려면 .tres 로 두라」고 적고, test_island_cache.gd 에 unnamed 를 저장해도 다른 unnamed 저장본에 대한 기대(남는지/지우는지)를 단언으로 박아 둔다. 또 test_save_leaves_other_islands_alone 의 「시험 섬(코드로 만든 설계 값)」 설명은 틀렸다 — 시험 섬은 test_island_blueprint.tres 이다. 설명을 「코드로 만든 설계 값」으로 고친다
  - 기준: (앞 단계 QA) 차선끼리 저장본 폴더를 나눠 써 옛 형식이 다시 생긴다 — user://island_cache 는 모든 체크아웃이 같이 쓴다. QA 시점(18:08)에 옛 코드를 도는 다른 차선이 섬 이름 없는 옛 형식(4_39d4…bin 26MB · 4_7658…bin 5.2MB)을 다시 만들었고, 이 차선이 다음에 구우면 또 지워 저쪽이 다시 굽는다. 합친 뒤에는 사라지지만, 합친 뒤에도 차선마다 굽는 코드(SOURCES 해시)가 다르면 같은 섬의 두 판을 서로 지우며 번갈아 굽는다(틀리진 않고 느려진다). island_cache.gd 의 prune 주석에 이 사정을 한 줄 적어 두면 다음 사람이 헤매지 않는다
- [x] 7. spec 에 낡은 줄 둘이 남았다 (2026-09-24 md 점검 — 2026-09-26 다시 재어 셋은 이미 됐다)
  - 뺀 까닭: G-145 가 같은 spec 을 고치는 중이었다 (그 뒤 G-145 는 끝났다)
  - **이미 된 것 (2026-09-26 확인)**: farming.md 9줄이 「밭 2×2 · 토대 4×4 위에 네 장」으로 고쳐졌다 ·
    rooms.md 11줄이 「토대 한 장 (… 3×3 에서 키웠다 2026-09-24)」로 날짜와 함께 남았다 ·
    spec 에 「3×3」이 토대 크기로 남은 곳이 없다 (남은 둘은 「3×3 에서 키웠다」는 지난 결정 기록이다)
  - 기준: `spec/06_build/building.md` 78줄의 옛 줄 「**건축물 = 벽 · 문 · 지붕 · 계단.**」을 지운다 —
    같은 파일 8줄에 「건축물 = 토대 · 벽 · 문 · 지붕 · 계단 · 기둥」이 이미 있어 여섯 가지와 어긋난다
  - 기준: `spec/04_life/farming.md` 「## 미정」 절 40줄의 「밭 한 장은 2×2칸 (사람 결정 2026-09-24)」을 지운다 —
    규칙 절(9줄)로 옮겨졌는데 사본이 남아 **정해진 값이 아직 「미정」에 앉아 있다**
  - 기준: 정해진 것이 「미정」 절에 남은 곳이 farming.md 에 더 없다 (읽어서 확인)
  - 기준: (앞 단계 QA) 새 연쇄 테스트의 머리 주석이 틀린 까닭을 적는다 — test_building_collapse.gd 의 test_a_support_placed_later_still_brings_down_the_whole_chain 위 첫 줄이 「한 번 훑기로는 R1 · P · R2 가 떠서 남는다」라고 하지만, 같은 주석 아래와 구현 보고대로 G-145 뒤에는 건축물은 _link_counts 가 다 잡고 떠 남는 것은 상자(설치물)다. 첫 줄을 「한 번 훑기로는 그 위 설치물(상자)이 떠서 남는다」로 고쳐라
  - 기준: (앞 단계 QA) unnamed 저장본이 끝없이 쌓일 수 있다 — island_cache.gd prune 이 unnamed 끼리 안 지우므로, 코드로 만든 설계 값을 use_cache=true 로 굽는 곳이 생기면 user://island_cache 에 unnamed 판이 SOURCES 해시가 바뀔 때마다 늘어난다(지금은 그런 곳이 없어 막는 것은 아니다). 코드 설계 값에서 use_cache 를 켜면 push_warning 하거나, island_key 주석처럼 테스트 하나로 「캐시를 켜는 곳은 모두 .tres」를 단언해 두면 막힌다
- [x] 8. (앞 단계 QA) spec/README.md 에 건축물 네 가지 줄이 남았다
  - 기준: spec/README.md 에 건축물 네 가지 줄이 남았다 — 30줄 「building.md — 벽 · 문 · 지붕 · 계단 건축」이 building.md 8줄의 여섯 가지(토대 · 벽 · 문 · 지붕 · 계단 · 기둥)와 어긋난다. 「building.md — 토대 · 벽 · 문 · 지붕 · 계단 · 기둥 건축」으로 고쳐라

## [x] G-152 농사가 된다 — 물 주기 · 비료
- 차선: 3d-lane2 (사람 결정 2026-09-26 — 차선 2 를 Opus 로 바꿔 이 묶음을 맡긴다)
- **밭을 만지는 것은 이 묶음 하나로 모은다** — `farmland.gd` 를 같이 고치면 차선이 부딪힌다.
  그래서 G-911 에 있던 「밭 거르기」를 단계 2 로 옮겨 왔다 (사람 결정 2026-09-26)
- spec: spec/04_life/farming.md
- 사람이 플레이해 보고 짚었다 (2026-09-26): 「밭에 물을 어떻게 뿌리는 건지, 비료도 안 되고 되는 게 없다」
- **자가 피드백**: 깨끗해질 때까지 단계를 계속 붙인다
- [x] 1. 물 주기 · 비료가 게임 안에서 실제로 된다
  - 기준: **빈손에서 시작해** 밭을 놓고 → 심고 → **물을 주고** → 비료를 주고 → 자라 → 수확하는 길을 **실제 입력(클릭 · 키)만으로** 친다 —
    중간에 함수를 직접 부르지 않는다 (테스트)
  - 기준: 물은 어디서 어떻게 얻고 어떻게 뿌리는지 게임 안에서 알 수 있다 — 물을 들고 밭을 겨누면 안내 한 줄이 뜬다 (번역 키) (테스트)
  - 기준: 비료를 주면 무엇이 달라지는지 눈에 보인다 (자라는 속도 · 표시) (테스트)
  - 기준: 안 되는 까닭(물이 없다 · 이미 줬다 · 심지 않았다)을 한 줄로 띄운다 — 「아무 일도 안 일어남」이 없다 (테스트)
  - 기준: 찍어서 확인한다 — 물 준 밭 · 안 준 밭이 달라 보인다
- [x] 2. 밭 거르기가 지형 확인이 아닌 제외 목록이다 (G-134 단계 2)
  - 뺀 까닭: 밭 거르기 — G-145 단계 4 가 밭 크기를 바꾼다
  - 기준: 밭 거르기가 지형 확인이 아닌 제외 목록이다 — 앞 단계 QA 는 「collider 가 지형(또는 흙판)일 때만」 치라고 했는데, is_field_ground 는 몸통 · Placeable · 절벽 법선 · 높이(GROUND_TOLERANCE 0.5m 임시)로 걸러낸다. 테스트 땅과 장애물이 둘 다 맨 StaticBody3D 라는 까닭은 이해되나, 윗면이 0.5m 아래인 낮은 몸통(바위 · 작은 자원 노드)이 밭 칸에 걸쳐 있으면 그 윗면을 쳐도 밭이 깎인다. 막는 것은 아니다. 섬 지형·자원 노드에 그룹이나 메타를 달아 받아들일 것을 지형으로 좁히거나, 낮은 StaticBody3D(높이 0.3m)를 밭 위에 세우고 HITS 번 쳐도 is_tilled 가 참인지 보는 테스트를 더하라
  - 기준: (앞 단계 QA) spec 에 새 규칙이 안 적혔다 — spec/04_life/farming.md 「규칙」에 이번에 만든 규칙이 없다: 물은 물뿌리개에 담아 뿌린다(연못 · 개울 민물을 겨누고 우클릭해 채움, 바닷물 안 됨) · 비료를 심은 밭에 주면 거둘 때까지 물 준 날마다 두 단계 자란다 · 안 되는 까닭은 한 줄로 띄운다. 이 세 줄을 「규칙」에 더하고, 「미정」에 「비료 효과의 크기(지금 임시 2배) · 물뿌리개 한 통 크기(지금 임시 5번치)」를 한 줄 적어라
  - 기준: (앞 단계 QA) 가득 찬 물뿌리개로 물을 겨누면 「밭이 아니다」가 뜬다 — crops.gd interact() 에서 catalog.waters(held) 이고 밭이 아니면 fill_held() or _no_field(ground) 인데, 가득 차 fill_held 가 false 를 내면 연못 바닥을 겨눈 것이라 _no_field 가 FARM_NO_FIELD 를 띄우고 같은 클릭으로 Drinking 이 물을 마신다. 민물을 겨눴을 때(Drinking.aims_at_fresh_water 가 참)는 _no_field 를 부르지 않게 하고, test_filling_the_can_does_not_also_drink 끝에 「마실 때 last_said 가 FARM_NO_FIELD 가 아니다」 단언을 더하라

## [x] G-990 점검 — 한 바퀴 돌아보고 빠진 것을 찾는다
- 차선: 3d-start (대화 세션이 나눴다 2026-09-24 — 3d-start 는 건축 쪽, 3d-lane2 는 겹치지 않는 것)
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다 (사람 결정 2026-09-23)
- **목록이 비었을 때 도는 자리다.** 기능을 새로 만들지 않는다 — **끊긴 곳 · 빠진 것을 찾아 잇는다**
- 보는 법 셋: ① 한 바퀴 테스트(G-903)를 돌려 어디서 막히는지 ②`harness/shot.sh --ui` 로 창을 찍어
  눈으로 ③ spec 의 「수용 기준」을 훑어 **적혀 있는데 없는 것**을 찾는다
- 사람에게: 루프가 「할 일을 모두 처리했다」로 끝나면 **이 묶음을 다시 `[ ]` 로 열면** 또 한 바퀴 돈다
- [x] 1. 한 바퀴 돌아본다
  - 기준: 위 셋으로 훑고, **빠진 것 · 끊긴 곳을 목록으로** 뱉는다 (report 에 적는다)
  - 기준: 그중 **그 자리에서 고칠 수 있는 것**은 고친다 (테스트)
  - 기준: 나머지는 QA 가 적어 다음 단계로 붙인다 — 깨끗해질 때까지
- [x] 2. (앞 단계 QA) 설치물의 맞은 대수가 저장되지 않는다
  - 기준: 설치물의 맞은 대수가 저장되지 않는다 — Placeable 의 HITS_LEFT meta(제작대 · 상자 · 횃불 · 건축물 · 밭)가 저장본에 안 담긴다. save.md 는 담는 것이 기본이다. 설치물을 저장할 때 남은 대수를 같이 담고, tests/settings/save/test_round_trip.gd 에서 설치물을 한 번 친 뒤 저장 → 불러오기 해서 남은 대수가 같은지 단언하라
  - 기준: (앞 단계 QA) 제작대가 만들던 것이 저장되지 않는다 — CraftingStation._job/_buffer 가 저장본에 안 담긴다(stations.gd:174 에 「임시」라고 적혀 있다). 담고, 왕복 테스트에서 제작을 시작하고 버퍼가 찬 상태로 저장 → 불러오기 해서 같은지 단언하라
  - 기준: (앞 단계 QA) 반쯤 캔 자원의 내구도가 저장되지 않는다 — Harvest._left 가 저장본에 안 담긴다. 나무를 몇 번 친 뒤 저장 → 불러오기 해서 남은 대수가 같은지 보는 왕복 테스트와 함께 담아라. 담지 않을 거라면 save.md 「안 담는다」 목록에 까닭을 적어라
- [x] 3. (앞 단계 QA) 횃불 · 건축물 · 선착장의 남은 대수 왕복 테스트가 없다
  - 기준: 횃불 · 건축물 · 선착장의 남은 대수 왕복 테스트가 없다 — 코드는 torches.gd · buildings.gd · dock_builder.gd 모두 with_hits/restore_hits 로 담지만, test_round_trip.gd 의 test_hits_left_on_placed_things_come_back 은 station · chest · field 셋만 친다. _play 에서 횃불(island.torches.place_at)과 건축물 하나를 놓고 한 대씩 친 뒤 _placed_hits 에 'torch' · 'building' 을 더해 HITS-1 로 돌아오는지 단언하라 (선착장은 시험 섬에 놓을 수 있으면 같이)

## [x] G-156 상자 안에서 아이템이 안 옮겨진다 (사람이 플레이하고 짚었다 2026-09-27)
- 차선: 3d-start
- spec: spec/06_build/storage.md, spec/02_player/inventory-hotbar.md
- **자가 피드백**: 깨끗해질 때까지 단계를 계속 붙인다
- 「상자 안에서도 아이템이 합쳐지게 만들어줘. 상자 안에서 아이템 이동이 안되더라고.」
- 어디가 빈 자리인가: 가방은 칸끼리 옮기기 · 바꾸기 · 합치기가 다 된다 (`tests/ui/hud/test_bag_move_slots.gd`).
  상자는 **가방↔상자**만 있고(`tests/world/storage/test_chest_slots.gd`) **상자 안 칸끼리**는 테스트도 없다
- [x] 1. 상자 안에서 칸끼리 옮기고 · 바꾸고 · 합친다
  - 기준: 상자 칸을 **같은 상자의 빈 칸**으로 끌면 그리로 옮겨진다 (테스트)
  - 기준: **다른 아이템이 든 칸**으로 끌면 둘이 자리를 바꾼다 (테스트)
  - 기준: **같은 아이템이 든 칸**으로 끌면 합쳐진다 — 한 칸 최대(STACK_MAX)를 넘는 몫은 끌어온 칸에 남는다 (테스트)
  - 기준: 칸 밖(창의 빈 곳)에 놓으면 **원래 자리에 그대로 있는다** — 사라지지 않는다 (테스트)
  - 기준: 가방에서 하는 것과 **같은 규칙**이다 — 두 곳이 같은 길을 쓰는지 보고, 베껴 쓴 것이면 한 곳으로 모은다
  - 기준: 옮긴 뒤 저장하고 불러오면 그대로다 (테스트)
  - 기준: 찍어서 확인한다 — 합치기 앞 · 뒤 두 장
- [x] 2. (앞 단계 QA) 상자 창 안내 글이 가방과만 주고받는다고 한다
  - 기준: 상자 창 안내 글이 가방과만 주고받는다고 한다 — i18n/ko.po 177행 「칸을 끌어다 놓아 가방과 주고받는다 · 우클릭으로 닫는다」(en.po 짝 포함)가 이제 상자 안에서도 옮기고 합친다는 것을 말하지 않는다. 틀린 말은 아니나 낡았다. 바꿀 거면 ko · en 둘 다 고치고 tests/ui/layout/test_window_layout.gd 184행의 같은 글과 창 폭(잘림)도 같이 본다

## [x] G-155 사람이 정한 것을 spec 과 코드에 반영한다 (사람 결정 2026-09-27)
- 결정: **유황은 그대로 둔다** (사람 결정 2026-09-27) — 섬마다 무슨 자원을 둘지는 사람이 나중에 정한다. 단계 1 의 기준을 약초 쪽으로 좁혔다
- 차선: 3d-lane2 (사람 결정 2026-09-27 — 약초 · 채집을 만지는 것은 이 묶음 하나로 모은다)
- spec: spec/04_life/gathering.md, spec/04_life/farming.md, spec/02_player/movement-controls.md, spec/02_player/inventory-hotbar.md, spec/03_world/spawn-conditions.md, spec/05_craft/crafting-stations.md
- 결정 대기 아홉 건을 풀며 사람이 정한 것들이다 (archive/decisions-done.md 2026-09-27)
- [x] 1. 벨라도나는 산비탈에 야생으로 난다 — 밭을 안 거친다
  - 「벨라도나가 마취약인데 왜 석회 얘기가 나오는지 모르겠는데 그냥 산쪽에 풀이랑 같이 자라게 만들면 될 것 같은데」
  - **석회 사슬 자체는 안 끊긴다** (사람이 짚었다 2026-09-27) — 밭 ← 비료(석회 2) ← 석회(개척 섬) 는 그대로다.
    이 단계는 **벨라도나가 그 사슬을 안 타게** 할 뿐이다. 사슬 자체는 따로 정할 일이다
  - 지금: gathering.md 16 · 21줄이 「1단 벨라도나는 **재배한다**」라 밭 ← 비료 ← 석회 ← 개척 섬으로 줄줄이 이어져,
    「등반을 못 해도 포획을 시작할 수 있게」라는 본뜻과 달리 **원정을 다녀와야** 마취탄을 만들 수 있다
  - 기준: **1단 벨라도나가 메인섬 산비탈 풀밭에 야생으로 난다** — 등반 장비 · 밭 · 비료 없이 캔다 (테스트)
  - 기준: **빈손에서 시작해 벨라도나를 캐는 데까지** 실제 입력으로 닿는다 — 등반 · 밭 · 섬 건너기 없이 (테스트)
  - **마취탄까지는 보지 않는다** — 조합식에 든 유황은 개척 섬 것이지만 **섬마다 무슨 자원을 둘지는 사람이 아직 안 정했다**
    (사람 결정 2026-09-27 — 「그대로 두면 된다. 나중에 알아서 말한다」). 이 단계는 약초 쪽만 본다
  - 기준: 2 · 3단(만드라고라 · 투구꽃)은 그대로 절벽 턱에만 난다 — 등반의 값어치가 남는다 (테스트)
  - 기준: `spec/04_life/gathering.md` 의 「재배한다」 예외를 「산비탈 풀밭에 야생으로 난다 (사람 결정 2026-09-27)」로 고치고
    `farming.md` 16줄도 맞춘다 — 낡은 줄이 남지 않는다 (grep)
  - 기준: 찍어서 확인한다 — 산비탈에 벨라도나가 난 그림
- [x] 2. 약초는 며칠 뒤 다시 난다
  - 「캔 약초는 다시 나는가?」 — 지금은 한 번 캐면 그 턱이 영영 비고 내 섬의 20포기가 평생 전부다 (G-920 이 여기서 막혔다)
  - 기준: `spec/03_world/resources-regrowth.md` 의 재생 표에 **약초**가 든다 — 나무 · 돌과 같은 자리 (grep)
  - 기준: 캔 자리에서 며칠 뒤 다시 난다 — 날이 지나면 나고, 그 전에는 안 난다 (테스트)
  - 기준: 날수는 **임시**다 — 코드에 「임시」를 적고 report 의 temporary 에도 적는다 (사람이 플레이해 보며 맞춘다)
  - 기준: (앞 단계 QA) gathering.md 「약초 자리 20곳」이 모호해졌다 — spec/04_life/gathering.md 14줄 「약초 자리는 정해진 스팟이다 — 내 섬에 20곳 (사람 결정 2026-09-19)」은 이제 턱 자리 수인데 섬에는 약초가 28포기(턱 20 + 산비탈 8, 임시) 난다. 사람 결정 줄은 지우지 말고 끝에 「— 턱(2 · 3단) 자리다. 벨라도나의 산비탈 자리는 따로 센다 (임시 8곳, island_blueprint.gd slope_herb_spot_count)」처럼 덧붙여라
  - 기준: (앞 단계 QA) 옛 저장본의 벨라도나 씨앗이 이름 없는 물건이 된다 — belladonna_seed 를 레시피 · ko/en.po · weights.json 에서 뺐지만 옛 가방 · 상자에 든 씨앗은 그대로 불러와져 ITEM_belladonna_seed 글자가 번역 없이 뜬다. 불러올 때 모르는 물건을 버리거나 벨라도나로 바꾸는 처리를 하거나, spec/01_settings/save.md 에 「옛 벨라도나 씨앗은 버린다」를 적고 그 처리를 테스트로 박아라 (밭에 심긴 벨라도나는 Crops.restore 가 이미 건너뛴다)
- [x] 3. 약초를 캐도 가방에 아무것도 안 들어온다 (G-990 단계 1)
  - 기준: 약초를 캐도 가방에 아무것도 안 들어온다 — Gathering.closed(herb, success) 를 듣는 곳이 tests/life/gathering/test_gathering.gd 말고 없다. 그래서 만드라고라 · 투구꽃은 개발 상자로만 구할 수 있다(gathering.md:7 · hunting.md:46). 게임에서 closed 를 받아 success 면 그 약초 아이템을 가방에 넣고 스팟을 캔 상태로 바꿔라. 게임 안 테스트로 스킬체크 성공 → 가방에 약초 1개, 실패 → 0개를 단언하라
  - 기준: (앞 단계 QA) 다시 나는 약초가 선 사람을 안 가린다 — resources-regrowth.md 「플레이어 몸이 서 있는 칸에도 안 자란다」는 표 전체의 규칙인데 Herbs.next_day 는 그 자리에 플레이어가 서 있어도 바로 심는다. 약초는 LAYER 3 이라 몸에 걸리지는 않지만 규칙과 어긋난다. next_day 에서 플레이어가 자리 반경 안이면 그날은 건너뛰게 하고(_picked 에 남겨 다음 날 다시 봄) test_herb_regrowth.gd 에 「선 자리에는 안 난다」 단언을 더하거나, spec 에 약초는 예외라고 적어라
  - 기준: (앞 단계 QA) world_data.gd 주석의 「캔 약초의 턱」이 낡았다 — herbs_picked 는 이제 턱과 산비탈 자리 번호를 함께 담는다. game/settings/save/world_data.gd 23줄 「herbs_picked(캔 약초의 턱) · herbs_picked_days(그 턱을 캔 날)」을 「캔 약초 자리(턱 · 산비탈)」로 고쳐라
- [x] 4. spec 의 「가공대」를 「제작대」로 맞춘다 (사람 결정 2026-09-27)
  - 게임 안 이름(ITEM_workbench)은 「제작대」인데 spec 은 「가공대」라 서로 달랐다. 사람이 보는 이름에 맞춘다
  - 기준: `spec/05_craft/crafting-stations.md` 17줄 · `spec/06_build/rooms.md` 20 · 37 · 38줄 ·
    `spec/08_combat/tools-as-weapons.md` 70줄의 「가공대」를 「제작대」로 고친다 — 사람 결정 줄은 날짜와 함께 남긴다
  - 기준: spec 에 「가공대」가 남지 않는다 (grep)
  - 기준: (앞 단계 QA) test_map_pin 저장본 검사가 글자 꼴 좌표를 놓친다 — game/tests/ui/hud/test_map_pin.gd _is() 는 String 값이 「173」과 통째로 같을 때만 잡는다. 표가 "173,-211" 같은 한 글자열로 저장되면 통과해 버린다(예전 contains 검사는 잡았다). String 값은 split(",")·공백으로 쪼갠 조각 하나하나를 str(n) 과 비교하도록 _is 를 넓혀라 — 소수 속 우연한 173 은 조각이 "173.52" 라 걸리지 않는다


## [x] G-991 코드를 다듬는다 — 파일 하나씩 (사람 결정 2026-09-27)
- 차선: 3d-lane2 (Fable high 로 돌려 본다)
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다 — 깨끗해질 때까지
- **한 단계 = 한 파일이다** (사람 결정 2026-09-27). 처음엔 「코드와 테스트를 전부 훑어라」(67,000줄)였는데
  세션이 10분 읽다 토큰 한도에 걸려 죽었고, report 를 못 남겨 **그 10분이 통째로 버려졌다** — 두 번 그랬다.
  세션은 회차마다 기억이 없으니 **훑고 고치는 것을 한 단계에 묶는다** — 그래야 단계마다 커밋으로 남는다
- **buildings.gd 하나만 한다** (사람 결정 2026-09-27) — 처음엔 큰 파일 열 개를 두었는데, 줄 수로만 골라
  자동화가 건드리지도 않는 것(shots.gd 찍기 도구 · test_island.gd 테스트 발판 · attack.gd 전투)까지 들어갔다.
  이 파일만 남긴 까닭 둘: (1) 1453 줄 한 곳에 놓기 · 무너짐 · 계단 · 등급 · 방이 다 들어 있어 **차선 충돌이 계속 여기서 났다**
  (2) 다음에 올 **NPC 는 방이 배치 단위**라(spec/07_automation/npc-villagers.md) 결국 이 파일 위에 올라탄다.
  나머지 파일의 잔 것은 루프가 G-9xx(루프가 찾은 것)로 이미 계속 갚고 있다 — 따로 묶음을 두지 않는다
- **이 묶음은 기능을 더하지 않는다.** 겉으로 드러나는 행동이 하나도 달라지지 않아야 한다
- **넘지 말아야 할 선** (모든 단계에 걸린다):
  - **테스트를 고쳐서 통과시키지 않는다.** 고쳐야 통과하면 리팩토링이 아니라 행동이 바뀐 것이다 —
    되돌리고 report 의 how 에 무엇이 걸렸는지 적는다. **단언의 값 · 개수를 바꾸면 안 된다**
  - **전체 테스트 통과 개수가 줄지 않는다** — 단계마다 앞뒤 숫자를 report 에 적는다
  - 「사람 결정」이 적힌 줄 · 「임시」 표시를 지우지 않는다
  - 한 단계에 **한 파일만** — 옆 파일이 눈에 걸리면 고치지 말고 report 에 적는다 (QA 가 단계로 붙인다)
- [x] 1. buildings.gd (1453줄) 를 다듬는다
  - 보는 곳: `game/build/buildings.gd` 와 **그것을 부르는 쪽 · 그것을 무는 테스트**. 이 파일 하나만 이번 단계의 몫이다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 2. (앞 단계 QA) 전체 실행의 뒤 숫자가 로그로 남지 않았다
  - 기준: 전체 실행의 뒤 숫자가 로그로 남지 않았다 — report 는 1718/1718 이라 하지만 .loop/out 에 앞 숫자(full_test_G991_1.log)만 있고 뒤 것은 없다 (test_files/ 는 루프의 바뀐 곳 실행이 덮어썼다). 이 묶음의 다음 단계부터 전체를 돌릴 때 `./tools/test.sh > .loop/out/full_test_G991_<단계>_after.log` 로 남겨라 — QA 가 산술 말고 로그로 확인할 수 있게
  - 기준: (앞 단계 QA) build_piece.gd 주석이 없는 이름을 가리킨다 — 81 · 385 줄 「(Buildings.link_counts)」인데 실제 함수는 Buildings._link_counts 다. build_piece.gd 를 다듬는 단계에서 두 줄을 _link_counts 로 고쳐라
  - 기준: (앞 단계 QA) build_piece.gd stairs_host 가 건축물 목록을 제 손으로 만든다 — 372~378 줄에서 Placeable.GROUP 을 훑어 BuildPiece 만 모으는데 Buildings._pieces_of 와 같은 일이다. build_piece.gd 단계에서 Buildings._pieces_of(get_tree().get_nodes_in_group(Placeable.GROUP)) 로 바꿔라 (겉 행동 그대로 · 단언 값 · 개수 그대로)

## [x] G-920 루프가 찾은 것 — 스스로 갚는다
- 결정: **약초는 며칠 뒤 다시 난다** (사람 결정 2026-09-27) — resources-regrowth.md 재생 표에 약초를 넣는다. 날수는 임시(코드에 「임시」를 적는다). 약초는 마취탄 재료라 안 나면 포획이 영영 막힌다
- 차선: 3d-start (사람 결정 2026-09-27 — 약초 쪽은 G-155 로 옮겼다. 여기는 약초를 안 만진다)
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. 벽에 박은 앵커가 저장되지 않는다 (G-990 단계 1)
  - 기준: 벽에 박은 앵커가 저장되지 않는다 — 박은 앵커를 섬마다 저장하고, 왕복 테스트로 같은 자리 · 같은 방향으로 돌아오는지 단언하라
- [x] 2. 웅크림 · 시선 방향이 저장되지 않는다 (G-990 단계 1)
  - 기준: 웅크림 · 시선 방향이 저장되지 않는다 — Player._crouching/_yaw/_pitch 와 절벽에 붙은 상태가 저장본에 없다. 담거나, 담지 않을 거라면 save.md 「안 담는다」 목록에 까닭을 적어라. 담는다면 왕복 테스트의 _play() 에서 웅크리고 고개를 돌린 뒤 비교하라
- [x] 3. 막힌 장착에 알림이 안 뜬다 (G-990 단계 1)
  - 기준: 막힌 장착에 알림이 안 뜬다 — hud.md:29 는 막힌 동작에 알림 줄을 띄운다고 적었는데, Equipment.equip/unequip 이 false 를 돌려줘도 ui 의 equip_slot.gd · slot_drop.gd 가 무시한다(예: 가방에 뭐가 들어 있어 가방을 못 벗을 때). false 일 때 notice 로 까닭 번역 키(ko · en)를 띄우고, 가방이 찬 채로 벗기를 시도하면 알림이 뜨는지 단언하는 테스트를 짜라
- [x] 4. 휘두를 때 캐릭터가 조준 쪽으로 안 돈다 (G-990 단계 1)
  - 기준: 휘두를 때 캐릭터가 조준 쪽으로 안 돈다 — movement-controls.md:18 은 휘두르기 · 쏘기 · 상호작용 때 캐릭터가 조준 방향을 본다고 적었다. 동작을 시작할 때 몸의 yaw 를 카메라 yaw 로 맞추고, 뒤를 보다가 휘두르면 몸이 카메라 쪽으로 돌아 있는지 단언하라
- [x] 5. 사망 상자를 아무나 연다 (G-990 단계 1)
  - 기준: 사망 상자를 아무나 연다 — damage-death.md:15,33 「죽은 본인만 열고 꺼낼 수 있다」가 코드에 없다. 사망 상자에 주인 캐릭터 id 를 담고(저장본에도), 다른 id 는 열기를 거부하게 하고, 두 캐릭터로 여는 테스트를 짜라
- [x] 6. 제작대 목록 글이 붙어 읽기 어렵다 (G-990 단계 1)
  - 기준: 제작대 목록 글이 붙어 읽기 어렵다 — 「Axe 1 Needs Wood 3 Stone 2」처럼 이름 · 개수 · 재료가 구분 없이 이어진다. 이름과 재료를 줄이나 칸으로 나누고 shot.sh --ui 로 ko · en 을 찍어 확인하라
- [x] 7. 앵커를 박는 길이 왕복 테스트에서 빠졌다 (G-920 단계 1)
  - 기준: 앵커를 박는 길이 왕복 테스트에서 빠졌다 — test_round_trip.gd 의 _play 는 drive_anchor() 대신 climb_gear.place_at() 으로 곧장 박는다. drive_anchor 도 place_at 을 거치니 저장 쪽에는 빈틈이 없다. 다만 「가방에서 하나 빠지고 벽에 하나 남는다」를 한 번에 보려면 tests/life/climbing/test_anchor.gd 에 drive_anchor 성공 뒤 climb_gear.standing().size() 가 1 늘었는지 보는 단언을 더하면 된다
- [x] 4. 우클릭 상호작용 때 몸이 안 돈다 (G-920 단계 4)
  - 기준: 우클릭 상호작용 때 몸이 안 돈다 — movement-controls.md:19 는 휘두르기 · 쏘기 · 상호작용 때 조준점 쪽을 본다고 적었지만 이번엔 좌클릭(Attack.swing · Harvest.swing)만 face_aim() 을 부른다. 우클릭 상호작용이 시작되는 곳(INTERACT 액션을 받는 _unhandled_input)에서도 player.face_aim() 을 부르고, test_face_aim.gd 처럼 뒤를 본 채 상호작용하면 몸이 카메라 쪽인지 단언하는 테스트를 더하라
- [x] 5. 연사 중 몸이 조준을 안 따라간다 (G-920 단계 4)
  - 기준: 연사 중 몸이 조준을 안 따라간다 — attack.gd:124 의 _process 는 방아쇠를 누르고 있는 동안 shoot() 을 곧장 불러 face_aim() 을 건너뛴다. 자동총을 누른 채 카메라를 돌리면 몸은 첫 발 방향에 남는다. shoot() 안(또는 _process 의 연사 줄 앞)에서 face_aim() 을 부르고, 자동총 방아쇠를 누른 채 카메라 yaw 를 바꾼 뒤 몸이 새 카메라 쪽인지 단언하라
- [x] 6. 절벽에 붙은 채 휘두르면 벽에서 돈다 (G-920 단계 4)
  - 기준: 절벽에 붙은 채 휘두르면 벽에서 돈다 — Player.face_aim() 은 등반 중에도 몸을 카메라 쪽으로 돌려, climbing.gd 가 face() 로 벽을 마주 보게 한 것을 덮는다. 등반 중(_climb.has_hold())에는 face_aim() 이 돌리지 않게 하든지 spec 대로 정하고, 붙은 채 좌클릭해도 몸이 벽을 보는지 테스트하라
- [x] 6. 남의 상자를 우클릭해도 아무 알림이 없다 (G-920 단계 5)
  - 기준: 남의 상자를 우클릭해도 아무 알림이 없다 — Death.interact 가 남의 데스 상자에서 입력을 삼키고 조용히 끝난다(game/combat/damage_death/death.gd interact/open). 플레이어는 왜 안 열리는지 모른다. G-920.3(막힌 장착 알림)처럼 거절 때 번역 키 알림(한국어·영어)을 띄우고, 미르로 우클릭하면 알림이 뜨는지 단언하는 테스트를 붙여라
- [x] 7. 조준 시간 표시 주석이 낡았다 (G-920 단계 5)
  - 기준: 조준 시간 표시 주석이 낡았다 — game/ui/hud/chest_timer_label.gd:10 「열리는 상자에만 뜬다」인데 이제 남의 데스 상자는 조준은 되지만 열리지 않는다. 남의 상자에도 시간이 뜨는 것이 맞는지 주석을 지금 동작에 맞춰 고쳐라
- [x] 7. 새 테스트의 단언 글이 낡은 이름을 쓴다 (G-920 단계 6)
  - 기준: 새 테스트의 단언 글이 낡은 이름을 쓴다 — game/tests/ui/hud/test_station_row_columns.gd 의 test_needs_stands_in_one_column_in_every_language 에서 workbench(제작대)를 여는데 단언 글이 「가공대는 줄이 많다」다. 「제작대는 줄이 많다」로 고쳐라
- [x] 4. spec 에 등반 중 조준 규칙이 없다 (G-920 단계 6)
  - 기준: spec 에 등반 중 조준 규칙이 없다 — face_aim 이 붙은 동안 안 돈다는 규칙이 코드 주석에만 있다. spec/02_player/movement-controls.md 의 「휘두르기 · 쏘기 · 상호작용 때 캐릭터가 조준점 쪽을 본다」 줄 옆(또는 spec/04_life/climbing.md)에 「벽에 붙었거나 줄에 걸린 동안은 몸이 벽 · 줄 쪽을 그대로 본다」 한 줄을 적어라
- [x] 2. 옆 테스트 주석이 제작대를 가공대라 부른다 (G-920 단계 7)
  - 기준: 옆 테스트 주석이 제작대를 가공대라 부른다 — 이번 기준 밖이다. game/tests/ui/hud/test_station_output.gd(12·14·16·53행) · test_station_short_rows.gd(10·12행) · test_station_window.gd(11·14·16·18행)의 주석이 workbench 를 옛 이름 「가공대」로 부른다. 가리키는 것은 workbench 상수이고 ITEM_workbench 는 「제작대」다. 이 주석들의 「가공대」를 「제작대」로 고쳐라. test_station_window.gd 11행 「가공대 · 제련로 — 서로 다른 레시피를 든 두 제작대」는 「제작대(workbench) · 제련로 — …」처럼 이름과 갈래가 헷갈리지 않게 적어라
- [x] 1. 다른 코드 주석에도 가공대가 남았다 (G-920 단계 2)
  - 기준: 다른 코드 주석에도 가공대가 남았다 — workbench 를 「가공대」로 부르는 주석이 game/ui/hud/station_window.gd(13·24행), game/tests/ui/hud/test_station_collect_note.gd, game/tests/craft/stations/test_station_recipes.gd · test_stations.gd, game/tests/craft/recipes/test_recipes.gd, game/tests/life/food/test_food.gd, game/tests/life/hunting/test_carry_cage.gd, game/tests/life/farming/test_farmland.gd, game/tests/build/test_room_chests.gd · test_room_kinds.gd 에 남아 있다. `grep -rn 가공대 game --include='*.gd'` 로 찾아 workbench 를 가리키는 것만 「제작대」로 고쳐라 (갈래 이름으로 쓴 곳은 「제작대(workbench)」처럼 풀어 적는다)
- [x] 1. spec 에도 옛 이름 가공대가 남았다 (G-920 단계 1)
  - 기준: spec 에도 옛 이름 가공대가 남았다 — 이번 기준은 .gd 파일만이라 기준 밖이다. spec/06_build/rooms.md 20·37·38행(「제작소 | 가공대」, 「가공대만 있는 막힌 공간」, 「가공대 + 제련로」), spec/05_craft/crafting-stations.md 17행(「1판 구상: 가공대(열 없는 가공) · …」), spec/08_combat/tools-as-weapons.md 70행(「총알은 가공대에서 만든다」)이 workbench 를 옛 이름으로 부른다. 「제작대(workbench)」로 고쳐라 — crafting-stations.md 17행은 갈래 목록이라 「제작대(workbench, 열 없는 가공)」처럼 적는다. tools-as-weapons.md 70행은 사람 결정 줄이니 이름만 바꾸고 날짜 · 뜻은 그대로 둔다. 테스트 test_no_old_workbench_name.gd 는 .gd 만 보니 spec 을 지키려면 res://../spec 의 .md 도 훑도록 넓히거나 따로 검사를 두어라

## [x] G-157 NPC 가 어떤 존재인지 spec 에 적는다 (사람 결정 2026-09-28)
- 차선: 3d-start
- spec: spec/07_automation/npc-villagers.md, spec/00_core/roadmap.md
- 사람이 정해 준 것이다 (2026-09-28). 지금 npc-villagers.md 미정 절의
  「주민 목록 · 들어오는 이벤트 목록 미정 — 자세한 것은 사람이 G-148 뒤에 말해 주기로 했다」가 이것으로 채워진다
- 단계 1 은 **적어 두기만** 한다. 단계 2 부터 **일하는 NPC 한 명**을 실제로 만든다 (사람 결정 2026-09-28)
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다 — 깨끗해질 때까지
- [x] 1. NPC 를 「개인」으로 적는다
  - 사람이 준 글 그대로:
    - [노동] 담당: NPC — 밭에 물 주기, 광산에서 캐기, 제작, 요리 등
    - NPC 는 **림월드식 「개인」**이다 — 나이 · 특성 · 스트레스 · 관계 · 결혼 · 아기 · 죽음
    - **습격 / 구출 / 합류**로 얻는다 (**노예 톤 아님**)
    - NPC 는 자동화의 수단이지만, **잃으면 아픈 존재**
  - 기준: `spec/07_automation/npc-villagers.md` 규칙 절에 위 넷을 적는다 — 「사람 결정 2026-09-28」을 붙인다
  - 기준: 앞의 「이벤트로 하나씩 들어온다 (사람 결정 2026-09-24)」와 **어긋나지 않게 잇는다** —
    습격 · 구출 · 합류가 그 「이벤트」의 갈래다. 옛 줄을 지우지 말고 날짜와 함께 남긴다
  - 기준: **「노예 톤 아님」을 규칙으로 적는다** — 부리는 것이 아니라 같이 사는 사람이다. 앞으로 글 · 이름을 지을 때의 잣대다
  - 기준: 미정 절을 고친다 — 채워진 것(주민이 무엇인가 · 어떻게 들어오나)은 빼고,
    아직 안 정한 것만 남긴다: **특성 · 스트레스 · 관계 · 결혼 · 아기가 각각 무엇을 바꾸나 · 습격이 어떻게 도나 · 죽음이 무엇을 남기나**
  - 기준: `spec/00_core/roadmap.md` 의 「NPC 가 기계보다 먼저다」와 맞는지 보고, 어긋나면 한 줄로 잇는다
  - 기준: md 가 150줄을 넘지 않는다
  - 기준: 이 단계에서는 코드 · 테스트를 만들지 않는다 — spec 만 바뀐다 (git diff 로 보인다)
- [x] 2. NPC 한 명이 섬에 산다
  - **가장 작은 한 명**부터다 (사람 결정 2026-09-28) — 특성 · 스트레스 · 관계 · 결혼 · 아기는 **아직 안 만든다**
  - 무엇을 밑그림으로 쓰나: 스스로 움직이는 개체는 `game/life/hunting/animal.gd` 가 이미 있다.
    베껴 쓰지 말고 **같이 쓸 것이 있으면 한 곳으로** 모은다 (걷기 · 길 찾기 · 저장)
  - 기준: 개발용 상자(또는 디버그 키)로 NPC 를 한 명 섬에 세울 수 있다 — 서 있고, 땅에 붙어 있고, 물에 안 빠진다 (테스트)
  - 기준: **이름과 나이**를 갖는다 — 세울 때 정해지고 게임 안에서 보인다 (번역 키) (테스트)
  - 기준: **저장하고 불러오면 그대로다** — 이름 · 나이 · 자리 (테스트, spec/01_settings/save.md)
  - 기준: 가까이 가면 누구인지 보인다 — 이름표든 말 걸기든, 한 가지 길로 (테스트)
  - 기준: `spec/07_automation/npc-villagers.md` 에 여기까지 만든 것을 적는다
  - 기준: 찍어서 확인한다 — 섬에 선 NPC 한 장
  - 기준: (앞 단계 QA) 목적 절 「동물의 숲 참고」가 림월드식과 겹친다 — npc-villagers.md 4행 목적은 「하나씩 섬에 들어와 일하는 주민 (동물의 숲 참고)」인데 규칙은 이제 림월드식 「개인」이다. 줄을 지우지 말고 무엇을 어디서 따왔는지 갈라 적어라: 예 「들어오는 모습은 동물의 숲, 사람됨(나이·특성·죽음)은 림월드」
  - 기준: (앞 단계 QA) 일하는 목록 두 줄이 겹친다 — 규칙 절 「[노동] 담당은 NPC (2026-09-28) — 물 주기·광산·제작·요리」와 「일을 한다 — 광질·제작·나무·돌 캐기·물 주기 (2026-09-24)」가 따로 있다. 둘 다 남기되 2026-09-28 줄에 「요리가 더해졌다 — 아래 2026-09-24 목록과 합쳐 본다」처럼 잇는 한 줄을 달아라
- [x] 3. 그 NPC 가 밭에 물을 준다
  - 왜 물 주기인가: spec 이 NPC 의 일로 콕 집은 것이고(npc-villagers.md), 물 주기는 G-152 로 이미 만들어져 있다
  - 기준: NPC 에게 **밭을 맡기면** 물뿌리개를 채워 마른 밭에 물을 준다 — 사람이 하는 것과 **같은 길**을 쓴다
    (함수를 따로 만들지 않는다 — 같은 곳을 부른다) (테스트)
  - 기준: **맡기는 길**이 게임 안에 있다 — 방에 넣든 밭을 짚어 시키든 한 가지로. 무엇을 골랐는지 spec 에 적는다 (테스트)
  - 기준: 물이 없으면 **연못 · 개울로 가서 채운다** — 사람이 하는 것과 같은 규칙 (테스트)
  - 기준: 줄 밭이 없으면 **멈춰 선다** — 헛돌거나 떨지 않는다 (테스트)
  - 기준: 맡긴 것이 **저장된다** — 껐다 켜도 하던 일을 이어 한다 (테스트)
  - 기준: 찍어서 확인한다 — NPC 가 밭에 물을 주는 그림 · 마른 밭과 준 밭이 다르게 보인다
  - 기준: (앞 단계 QA) IslandState 가 섬을 get 으로 묻는다 — island_state.gd 의 of · apply 는 다른 칸은 island.animals 처럼 바로 읽는데, 주민만 island.get(&"villagers") 로 묻는다. 주석에는 「주민이 없는 땅도 있어서」라고 적혀 있지만, 그런 땅이 어디인지(TestIsland 말고 IslandState 를 받는 섬이 있는지) 확인해라. 없다면 island.villagers 로 맞추고, 있다면 주석에 그 섬 이름을 적어라
- [x] 4. (앞 단계 QA) 찍기 주석이 그림과 다르다
  - 기준: 찍기 주석이 그림과 다르다 — game/core/debug/shots.gd 의 villager_water 주석(「둘을 준 때(젖은 밭 둘 · 마른 밭 둘)」)과 달리 실제 첫 장은 젖은 셋 · 마른 하나다(물 2번치를 다 쓰고 이미 채워 하나 더 준 뒤 찍힌다). 주석을 「물 두 번치가 떨어져 연못에서 채우러 간 뒤, 젖은 밭 · 마른 밭이 섞인 때」처럼 실제 모습으로 고치거나, want 를 셋째 밭 전에 잡히게 기다림 조건을 고쳐라
  - 기준: (앞 단계 QA) water_spots 주석이 낡았다 — game/automation/villagers/field_care.gd 의 `var water_spots` 주석이 「연못 가운데 · 개울 점」이라 적혀 있는데, TestIsland._fresh_water_spots 는 연못 가운데가 아니라 물가 선의 점들을 준다. 「연못 물가 점 · 개울 점」으로 고쳐라

## [x] G-158 낚시 — 떡밥 · 낚싯대 · 찌 (사람 결정 2026-09-28)
- 차선: 3d-lane2
- spec: spec/04_life/fishing.md, spec/00_core/roadmap.md (1단계에 남은 것)
- **지금은 기본만 한다** (사람 결정 2026-09-28) — 「낚시는 지금은 기본적인 것만 하자」
- **작살총은 나중이다** — 「바다 나가서 하는 게 저거고 이번엔 낚시대만 하면 돼」.
  fishing.md 의 작살총 줄은 **지우지 말고** 나중 것으로 남긴다. 미정의 「F-2 던지고 기다리는 낚싯대를 남길 것인가」는 이것으로 답했다
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다 — 깨끗해질 때까지
- 사람이 준 규칙 그대로:
  - 낚싯대 낚시는 **떡밥**이 있어야 한다
  - 떡밥을 풀면 **그 영역 안에** 물고기가 생긴다. 마리 수는 **떡밥 등급**에 따라 다르고, **가장 낮은 등급은 3~5마리**
  - 물고기 **종류는 랜덤** — 종류 목록은 동물과 마찬가지로 **사람이 나중에 알려 준다**
  - **그 영역 안에 낚싯대를 던져야** 물고기가 문다
  - **미끼는 없다. 찌를 바꿀 수 있다** — 기본은 찌 없음. 찌는 **나중에** 부착해 낚시를 더 쉽게 만든다
  - 영역이 끝나는 때: **다 잡거나 10분이 지나거나** — 먼저 오는 쪽 (사람 결정 2026-09-28)
  - 실패하면(게이지 0): **그 물고기 한 마리를 놓친다** — 떡밥 영역은 남고 다시 던질 수 있다
- [x] 1. 낚싯대 낚시를 spec 에 적는다
  - 기준: `spec/04_life/fishing.md` 에 위 규칙을 적는다 — 「사람 결정 2026-09-28」을 붙인다
  - 기준: 작살총 줄은 **그대로 남긴다** — 「나중에 한다 (사람 결정 2026-09-28)」를 붙인다. 미정의 F-2 는 답한 것으로 옮긴다
  - 기준: 미정에 남길 것 — **물고기 종류 목록** · **떡밥 등급이 몇 단이고 등급마다 몇 마리인가** ·
    **찌가 무엇을 얼마나 쉽게 만드나** · 떡밥 · 낚싯대를 어떻게 얻나
  - 기준: md 가 150줄을 넘지 않는다
  - 기준: 이 단계는 코드를 만들지 않는다 — spec 만 바뀐다
- [x] 2. 떡밥을 풀면 물고기가 생기고, 그 안에 던져야 문다
  - 기준: 떡밥을 **물에 대고 우클릭**하면 그 자리에 영역이 생긴다 — 눈에 보인다 (물결 · 표시) (테스트)
  - 기준: 영역에 물고기가 **3~5마리** 생긴다 (가장 낮은 등급). 마리 수는 떡밥 등급에서 온다 — 등급이 하나뿐이면 그 하나로 (테스트)
  - 기준: **영역 안에 던져야** 문다 — 밖에 던지면 아무 일도 없고 왜 안 되는지 한 줄이 뜬다 (번역 키) (테스트)
  - 기준: 영역은 **다 잡으면** 사라진다 · **10분이 지나면** 사라진다 — 먼저 오는 쪽 (테스트, 시계를 돌려 본다)
  - 기준: 떡밥 · 낚싯대를 얻는 길이 게임 안에 있다 — 재료 · 개수는 **임시**로 두고 코드와 report 에 「임시」를 적는다
    (씨앗을 임시 규칙으로 둔 것과 같다 — spec/04_life/farming.md 미정)
  - 기준: 영역과 남은 마리 수가 **저장된다** — 껐다 켜도 그대로 (테스트, spec/01_settings/save.md)
  - 기준: 찍어서 확인한다 — 떡밥을 푼 물과 안 푼 물이 다르게 보인다
- [x] 3. 물면 미니게임으로 낚는다
  - 팰월드식이다 (사람이 그림을 줬다 2026-09-28):
    - 아래에 **초록 바**가 있고 **플레이어가 그것을 움직인다**
    - 그 위에서 **물고기가 왔다 갔다** 한다
    - 물고기가 **초록 바 안에 있으면** 동그란 게이지가 **올라간다**
    - 물고기가 **바 밖으로 나가면** 게이지가 **줄어든다**
    - **100%면 낚고, 0이면 실패**다
  - 기준: 물면 미니게임 창이 뜬다 — 초록 바 · 왔다 갔다 하는 물고기 · 동그란 게이지가 다 보인다 (테스트)
  - 기준: 물고기가 바 안이면 게이지가 오르고, 밖이면 내린다 (테스트 — 자리를 넣어 값을 잰다)
  - 기준: **100%면 물고기가 가방에 든다** — 가방이 차면 발밑에 떨어진다 (다른 것과 같은 규칙) (테스트)
  - 기준: **0이면 그 물고기 한 마리를 놓친다** — 영역의 남은 수가 하나 줄고, 떡밥 영역은 남는다 (테스트)
  - 기준: 초록 바를 **무엇으로 움직이나**를 정해 `spec/01_settings/input.md` 표에 한 줄 더한다.
    지금은 **임시**로 둔다 — 마우스 왼쪽을 누르고 있으면 오른쪽, 놓으면 왼쪽으로 밀린다 (팰월드와 같다)
  - 기준: 바 너비 · 물고기 속도 · 게이지가 차고 빠지는 속도는 전부 **임시**다 — 코드와 report 에 「임시」를 적는다
  - 기준: 창이 떠 있는 동안 조작이 잠긴다 — 다른 창과 같은 규칙 (테스트)
  - 기준: 찍어서 확인한다 — 게이지가 오르는 중 · 다 찬 뒤 두 장
- [x] 4. (앞 단계 QA) 낚시 사진의 알림이 다른 말로 찍힌다
  - 기준: 낚시 사진의 알림이 다른 말로 찍힌다 — /tmp/fishshots2/fishing_en.png 에 「떡밥을 풀었다 — 물고기 5마리가 모인다」(한국어), fishing_full_ko.png 에 「Spread the groundbait — 3 fish gather」(영어)가 찍혔다. game/core/debug/ui_shots.gd 가 fishing_caught 만 again 으로 알림을 다시 띄우고 fishing · fishing_full 은 안 띄운다. fishing · fishing_full 에도 again = Notice.say(get_tree(), Fishing.SAY_SPREAD, [마리 수]) 를 달거나, 사진에 안 필요하면 _fishing() 끝에서 알림을 지워라. 막는 것은 아니다 — 창 자체는 두 말 다 맞게 찍혔다
- [x] 5. (앞 단계 QA) 낚시 알림 테스트가 ui_shots 를 돌려 보지 않는다
  - 기준: 낚시 알림 테스트가 ui_shots 를 돌려 보지 않는다 — test_fishing_shots_say_the_notice_again_in_each_language 의 뒤쪽 절반은 Notice.say 만 직접 불러 보고 ui_shots 의 _fishing 은 부르지 않는다. 소스 글자 검사로 회귀는 막지만, SHOTS.new() 로 shots._fishing(player, stations, "fishing") 을 불러 shots._fish_notice == [Fishing.SAY_SPREAD, [fishing.fish_left(...)]] 이고 "fishing_caught" 면 [Fishing.SAY_LANDED, [0 이상]] 인지 단언하는 검사를 덧붙이면 마리 수가 실제 영역과 맞는지까지 지킨다

## [x] G-159 NPC 자동화 — 직무를 준다 (사람 결정 2026-09-28)
- 차선: 3d-start
- spec: spec/07_automation/npc-villagers.md, spec/06_build/rooms.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다 — 깨끗해질 때까지
- 사람이 준 규칙 그대로:
  - 방은 **특정 구조물을 쓸 수 있고 버프가 부여된다**
  - **자동화는 해당 사람에게 직무를 부여하는 것**이다
  - 나무꾼: 나무를 벤다
  - 광부: 광물을 캔다
  - 약초꾼: 채집물들을 채집한다
  - 농부: 농사, 비료제작대를 쓴다
  - 연구원: 연구 작업대류를 쓴다
  - 관리인: 상자를 정리하고 건물을 수리한다
  - 청소부: 청소한다
- **방에 무엇이 들어가야 하는지는 사람이 나중에 알려 준다** (2026-09-28) — 묻지 말고 미정으로 남긴다
- 지금 있는 것: `game/automation/villagers/villager.gd` (이름 · 나이 · 저장 · 움직임, G-157) ·
  밭 물 주기(G-157.3) · 나무 · 돌은 `life/harvest/` · 약초는 `life/gathering/`
- [x] 1. 직무를 spec 에 적는다
  - 기준: `spec/07_automation/npc-villagers.md` 에 **일곱 직무**와 각자 무엇을 하는지 적는다 — 「사람 결정 2026-09-28」
  - 기준: **자동화 = 직무 부여**라고 적는다 — 기계가 아니라 사람에게 일을 맡기는 것이 이 게임의 자동화다.
    앞의 「NPC 가 기계보다 먼저다」(2026-09-24)와 이어 적는다
  - 기준: `spec/06_build/rooms.md` 에 **방이 구조물을 쓰게 하고 버프를 준다**를 적는다
  - 기준: 미정으로 남길 것 — **방마다 무엇이 들어가야 하나 · 버프가 무엇인가**(사람이 나중에 알려 준다) ·
    한 사람이 직무를 **하나만** 갖나 여럿 갖나 · 직무마다 솜씨가 느나
  - 기준: **연구원 · 관리인 · 청소부는 「쓸 것이 아직 없다」**고 적는다 — 연구대 · 수리 · 청소가 게임에 없다. 만들 때 함께 연다
  - 기준: md 가 150줄을 넘지 않는다 · 이 단계는 코드를 만들지 않는다
- [x] 2. 직무를 줄 수 있고, 농부가 그 틀로 돈다
  - 지금 물 주기는 G-157.3 이 만든 한 갈래다. 그것을 **직무 틀**로 옮겨 다른 직무가 같은 길을 타게 한다
  - 기준: NPC 에게 **직무를 줄 수 있다** — 게임 안에서 고르는 길이 있다 (테스트)
  - 기준: **농부** 직무를 주면 지금 하던 물 주기를 한다 — G-157.3 의 테스트가 그대로 통과한다 (테스트)
  - 기준: 직무가 **없으면 아무 일도 안 한다** — 멈춰 선다 (테스트)
  - 기준: 직무가 **저장된다** — 껐다 켜도 하던 직무를 이어 한다 (테스트)
  - 기준: 직무를 더하는 자리가 **한 곳**이다 — 새 직무를 넣을 때 무엇을 적어야 하는지 주석에 남긴다.
    베껴 쓰기가 생기지 않게 (`tests/temp_dir.gd` 가 본보기다)
  - 기준: 한 사람이 직무를 하나만 갖는지 여럿 갖는지는 **임시**로 정하고 코드에 「임시」를 적는다
- [x] 3. 나무꾼 · 광부 · 약초꾼이 일한다
  - 기준: **나무꾼**은 나무를 벤다 · **광부**는 광물을 캔다 — `life/harvest/` 의 길을 쓴다 (사람이 하는 것과 같은 곳) (테스트)
  - 기준: **약초꾼**은 약초를 캔다 — `life/gathering/` 의 길을 쓴다. 스킬체크가 있는 것은 어떻게 하는지 정하고 적는다 (테스트)
  - 기준: 캔 것을 **어디에 두나**를 정한다 — 상자에 넣든 그 자리에 쌓든 한 가지로. spec 에 적는다 (테스트)
  - 기준: 벨 나무 · 캘 광물이 없으면 멈춰 선다 — 헛돌지 않는다 (테스트)
  - 기준: 찍어서 확인한다 — 직무를 받은 NPC 가 일하는 그림
  - 기준: (앞 단계 QA) 직무 틀 채움 단언이 물려받은 것도 센다 — test_villager_jobs.gd test_every_listed_job_has_a_name_a_work_and_a_turn 가 get_script_method_list() 로 tick · halt 등을 찾는데, 이 목록은 JobWork 에서 물려받은 메서드도 담아 일 스크립트가 틀을 비워 둬도 통과할 수 있다. 막는 것은 아니다 — 스크립트 소스(source_code)에서 "func tick(" 등을 찾거나 get_base_script() 의 목록을 빼고 세도록 고쳐라
- [x] 4. (앞 단계 QA) 나무꾼이 바위를 안 치는 단언이 약하다
  - 기준: 나무꾼이 바위를 안 치는 단언이 약하다 — test_villager_gathering.gd test_the_woodcutter_fells_the_tree_the_way_the_player_does 는 나무를 다 벤 바로 그 순간 바위 내구도를 본다. 바위(ROCK_AT)가 나무보다 멀어 나무꾼이 바위도 치게 깨 봐도(woodcutting.gd groups=["tree","stone"]) 이 테스트는 통과했다 (다른 테스트가 잡아 기준은 지켜진다). 나무를 벤 뒤 _drift(2.0) 만큼 더 돌린 다음 바위 내구도가 그대로인지 보거나, 바위를 나무보다 가깝게 놓아라

## [x] G-160 주민 설계를 고정한다 — spec 을 사람이 만든 판으로 (사람 결정 2026-09-28)
- 차선: 3d-start
- spec: spec/07_automation/npc-villagers.md, spec/06_build/rooms.md
- **사람이 만든 설계다. 이대로 고정한다 — 낡은 것은 지운다** (2026-09-28)
- **약초꾼은 없앤다** — 직무 목록에서 빠졌다. 코드 · 테스트 · spec 에서 걷어낸다 (채집 자체는 사람이 하는 것으로 그대로 둔다)
- 사람이 준 규칙 그대로:
  - 방은 **특정 구조물을 쓸 수 있고 버프가 부여된다**. 자동화는 **해당 사람에게 직무를 부여**하는 것이다
  - **수면**: 최대 100. 깨어 있으면 내려간다. **30부터 스트레스가 오른다. 0 이면 스트레스가 대폭 오른다**
  - **스트레스**: 100 이 되면 그 주민의 **부정 특성이 발동**한다
  - **경고**: **50 과 80 에 경고**. 경고는 **사라지지 않고 오른쪽 위에 로그로 계속 뜬다**
  - **70 부터는 느리게 일한다**
  - **부정 특성**: 부수기 · 불피우기 · 가출 · 폭식
  - **긍정 특성**: 협상가(상인에게 사고 파는 비용 완화) · 외향인(말 걸면 스트레스 완화, **내향인에겐 효과 없음**) ·
    내향인(방에서 일하면 버프) · 천부적인 재능(그 직업 경험치 증가) · 대식가(음식 2배지만 작업 효율 증가) ·
    소식가(음식을 적게 먹음) · 완벽주의(높은 품질, **실패하면 파괴**) · 큰손(생산품 제작 시 +1)
  - **특성은 변하지 않는다** (사람 결정 2026-09-28)
  - **직업**: 부여하면 그 작업을 알아서 하는 자동화. 계속 하면 **레벨이 오르고, 오르면 쓸 수 있는 것이 늘어난다**
  - 나무꾼(범위 안의 나무를 벤다) · 광부(범위 안의 광물, 광산을 쓴다) · 연구원(연구) · 제작(제작 기기) ·
    관리인(수리 · 청소 · 상자 정리 · 재고 관리) · 의약(치료 · 약 제조) · 요리(음식 · 음료) ·
    건축(건축 속도 증가) · 농사(농사)
  - **재고 관리**: 플레이어가 **상자에 카테고리별 재고를 정한다**. 부족하면 관리인이 그 구조물에 가서 **우선순위를 높인다**.
    **작업대 우선순위는 플레이어가 못 정한다 — 관리인만 정한다** (사람 결정 2026-09-28)
  - **스텟**: 레벨이 있고 **5레벨마다 +n%**. **최대 100레벨**
  - 힘(운반 중량 · 채광 속도 · 벌목 속도 · 근접 데미지) · 민첩(이동 속도 · 회피 · 근접 공격 속도 · 낙하 데미지 감소) ·
    체력(hp · 질병 확률 감소 · 굶주림 감소) · 지식(연구 속도 · 의약 품질/속도 · 요리 품질/속도) ·
    손재주(제작 속도 · 제작품 품질 · 제작 실패율 감소)
  - **스텟은 무슨 일로든 오른다** — 벌목을 해도 광질을 해도 힘이 오르고, 그 힘이 나중에 근접 전투 같은 데 쓰인다.
    **직업 레벨은 쓸 수 있는 것이 늘어나는 것**이고 스텟은 말 그대로 스텟이다 (사람 결정 2026-09-28)
- [x] 1. 새 설계를 spec 에 고정하고 낡은 것을 지운다
  - 기준: `spec/07_automation/npc-villagers.md` 를 위 규칙으로 **다시 쓴다** — 새 설계와 어긋나는 낡은 줄은 지운다
  - 기준: **약초꾼이 spec 어디에도 없다** (grep) — 직무 목록 · 「지금 만든 것」 · 수치 · 수용 기준 전부
  - 기준: 「지금 만든 것 (G-157 · G-159)」 절들은 **무엇이 실제로 도는가**만 남기고 줄인다 —
    설계는 규칙 절에 있고, 여기는 만들어진 것만 적는다
  - 기준: 미정으로 남길 것 — **방마다 무엇이 들어가나 · 버프가 무엇인가**(사람이 나중에 알려 준다) ·
    스텟이 어떤 일로 얼마나 오르나 · 직업 레벨이 무엇을 여나 · 특성을 어떻게 얻나
  - 기준: `spec/06_build/rooms.md` 에 방이 구조물을 쓰게 하고 버프를 준다가 적혀 있다 (없으면 적는다)
  - 기준: md 가 150줄을 넘지 않는다 · 이 단계는 코드를 만들지 않는다
- [x] 2. 약초꾼 직무를 걷어낸다
  - 기준: `J` 로 도는 직무에서 약초꾼이 빠진다 — 코드 · 테스트에서 지운다 (테스트)
  - 기준: **사람이 약초를 캐는 길은 그대로다** — `life/gathering/` 의 스킬체크가 그대로 돈다 (테스트)
  - 기준: 약초꾼만 쓰던 코드가 남지 않는다 (grep) — 다른 직무가 같이 쓰는 것은 남긴다
  - 기준: 전체 테스트 통과 개수가 줄어든 만큼이 **약초꾼 테스트뿐**이다 — report 에 앞뒤 숫자와 지운 테스트 이름을 적는다
  - 기준: (앞 단계 QA) spec 몇 곳에 옛 말 「직무」가 남았다 — npc-villagers.md 는 「직업」으로 맞췄는데, spec/01_settings/input.md 22행 「그 주민의 직무를 다음 것으로 — 없음 → 농부 → …」와 spec/01_settings/save.md 13행 「주민(이름 · 나이 · 자리 · 직무 · …)」이 아직 「직무」 · 「농부」다. 「직업」 · 「농사」로 바꿔라. npc-villagers.md 83행의 「채집 직무」 줄은 앞 QA 말대로 G-160.2 에서 코드를 걷어낼 때 함께 지운다. 끝나면 grep -rn '직무' spec/ 에 12행(자동화는 직무를 부여) 말고는 없어야 한다
- [x] 3. (앞 단계 QA) 코드 주석 · 테스트 이름에 옛 말 「직무」가 남았다
  - 기준: 코드 주석 · 테스트 이름에 옛 말 「직무」가 남았다 — spec 은 「직업」으로 맞췄는데 jobs.gd · villager.gd · villagers.gd · gather_work.gd 주석과 test_villager_jobs.gd 의 단언 글(「약초꾼이 직무 목록에 없다」 등), gather_work.gd 가 가리키는 spec 절 이름 「직무」(지금은 없는 절이다)에 옛 말이 그대로다. 주석과 글에서 「직무」를 「직업」으로 바꾸고 spec 절을 가리키는 곳은 지금 npc-villagers.md 에 있는 절 이름으로 고쳐라. 막는 것은 아니다
  - 기준: (앞 단계 QA) test_villager.gd 가 임시 폴더를 남긴다 — test.log 에 TMPDIR naru_villager_2240277995 가 남았다고 나온다. 이름꼴 naru_villager_%d 는 tests/automation/villagers/test_villager.gd:32 의 것이다. after_each 의 TempDir.clean(self, _dir)(59행)이 모든 경로에서 불리는지(이른 return · await 도중 끝남) 보고, _dir 을 만드는 테스트마다 확실히 지워지게 고쳐라. 이번 변경과는 상관없다

## [x] G-161 주민 창 — 한눈에 보고 직무를 준다 (사람 결정 2026-09-28)
- 차선: 3d-lane2
- spec: spec/07_automation/npc-villagers.md, spec/12_ui/hud.md, spec/01_settings/input.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다 — 깨끗해질 때까지
- 왜 창인가 (사람이 짚었다 2026-09-28): 「J 해서 부여하는 게 아니라고. 그렇게 만들면 **UX 가 부족한** 거라고.
  **여러 정보들을 한번에 볼 수 있어야** 해당 NPC 의 적합한 직무를 찾는다고」
- 창에서 하는 일: **주민 목록 · 그 주민의 스텟과 특성 보기 · 직무 주기 · 방출할지 정하기**
- `J`(겨눠서 직무를 넘기는 임시 키, G-159)는 **이 창이 대신한다** — 창이 서면 뺀다
- [x] 1. 주민 창을 연다 — 목록과 그 주민을 한눈에
  - 기준: 키 하나로 주민 창이 열리고 닫힌다 — 어느 키인지 정해 `spec/01_settings/input.md` 표에 한 줄 (테스트)
  - 기준: **왼쪽에 주민 목록** · 고르면 **오른쪽에 그 주민**이 뜬다 — 이름 · 나이 · 지금 직무 (테스트)
  - 기준: **스텟 다섯**(힘 · 민첩 · 체력 · 지식 · 손재주)이 레벨과 함께 보인다 (테스트)
  - 기준: **특성**이 보인다 — 긍정 · 부정을 가려 볼 수 있다 (테스트)
  - 기준: **수면 · 스트레스**가 보인다 — 숫자든 막대든 (테스트)
  - 기준: 주민이 없으면 「아직 없다」가 뜬다 — 빈 창이 아니다 (테스트)
  - 기준: 창이 떠 있는 동안 조작이 잠기고, 걷기 안내 줄이 숨는다 — 다른 창과 같은 규칙 (테스트)
  - 기준: 글자는 번역 키다 — ko · en 둘 다 (테스트)
  - 기준: 찍어서 확인한다 — 주민을 고른 창 한 장
- [x] 2. 창에서 직무를 주고 방출한다
  - 기준: 창에서 **직무를 고르면 그 주민이 그 일을 시작한다** — 지금 도는 직무(농사 · 나무꾼 · 광부)가 다 된다 (테스트)
  - 기준: 직무를 **없음**으로 되돌릴 수 있다 (테스트)
  - 기준: **방출**할 수 있다 — 물어보는 한 단계를 거친다 (실수로 눌러 잃지 않게) (테스트)
  - 기준: 방출하면 그 주민이 어떻게 되는지 정하고 spec 에 적는다 — NPC 는 「**잃으면 아픈 존재**」다 (사람 결정 2026-09-28)
  - 기준: 직무 · 방출이 **저장된다** (테스트)
  - 기준: **`J` 키를 뺀다** — 창이 그 일을 한다. `input.md` 표에서도 지운다 (grep)
  - 기준: 찍어서 확인한다 — 직무를 고르는 중 한 장
  - 기준: (앞 단계 QA) npc-villagers.md 의 옛 줄이 낡았다 — spec/07_automation/npc-villagers.md 43행(「지금 만든 것 (G-157.2)」)이 「특성 · 스트레스 · 관계 · 결혼 · 아기는 아직 없다」라고 한다. 이제 특성 · 스트레스는 들고 있다(G-161.1). 「관계 · 결혼 · 아기는 아직 없다 — 특성 · 스트레스는 G-161.1 에서 들게 됐다」처럼 고쳐라
  - 기준: (앞 단계 QA) TMPDIR 에 naru_island_cache_* 가 남는다 — test.log 에 naru_island_cache_2640387327 이 TempDir.clean 으로 지워지지 않고 남았다고 나온다. .loop/out/test_files/ 로그에서 그 캐시를 만든 테스트를 찾아 after_all/after_each 에서 TempDir.clean 을 불러라 (이번 변경 탓인지는 확인 못 했다)
- [x] 3. (앞 단계 QA) 낡은 주석
  - 기준: 낡은 주석 — 진짜 좌클릭이라 적었다 — game/tests/automation/villagers/test_villager_jobs.gd 의 _pick 주석이 「직무 단추를 진짜 좌클릭으로 누르고」라고 하지만 VillagerClicks.click 은 보이는지 · 화면 안인지 본 뒤 pressed.emit() 을 낸다. 「창의 단추를 누른다(pressed — villager_clicks.gd 참고)」로 고쳐라
  - 기준: (앞 단계 QA) 주민 목록에 같은 이름이 둘 선다 — villagers_release_en.png 에 Nuri 가 두 줄이다(settle 이 이름 여섯 가운데 겹쳐 뽑는다). 창에서 누가 누구인지 가려 고르기 어렵다. Villagers.settle 이 이미 섬에 있는 이름을 먼저 피해 고르게 하고(다 쓰였을 때만 겹친다), 세 명을 세우면 이름이 다 다른지 보는 테스트를 test_villager.gd 에 더하라
- [x] 4. (앞 단계 QA) 이름이 다 쓰였을 때 갈래에 테스트가 없다
  - 기준: 이름이 다 쓰였을 때 갈래에 테스트가 없다 — Villagers._free_name 의 `if free.is_empty(): free = NAMES` 갈래(일곱째 주민부터 겹쳐 뽑기)를 아무 테스트도 거치지 않는다. test_villager.gd 에 주민 일곱을 세워 settle 이 null 이 아니고 이름이 NAMES 안에 있는지, 앞 여섯의 이름은 다 다른지 보는 테스트를 더하라 (막는 것은 아니다 — 이번 기준은 셋까지만 요구했다)

## [x] G-162 방을 눈으로 본다 — 오버레이 · 방 페이지(O) · 이름 바꾸기 (사람 결정 2026-09-29)
- 차선: 3d-start
- spec: spec/06_build/rooms.md, spec/12_ui/hud.md, spec/01_settings/input.md, spec/01_settings/save.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다 — 깨끗해질 때까지
- 사람이 준 규칙 그대로:
  - 플레이어가 쉽게 볼 수 있게 **오버레이**를 보여 준다
  - **O 키로 방이 뭐가 있는지 볼 수 있는 페이지**를 만든다
  - **방의 이름을 바꿀 수 있어야** 한다
  - **생성된 방만** 표시한다
  - UI 에는 **방의 이름 · 방의 역할**만 일단 둔다 (더 생각나면 사람이 말해 준다)
- 지금 있는 것: `game/build/rooms.gd` 가 벽 · 문 · 지붕으로 막힌 판을 방으로 잰다 (그때그때 잰다 — 따로 놓는 손짓이 없다).
  방 종류 다섯은 `rooms.json` 한 곳에 있다 (농장 · 작업실 · 대장간 · 연구실 · 주방, 없거나 섞이면 잡실)
- **먼저 풀 것 — 방을 무엇으로 알아보나**: 방은 그때그때 재는 것이라 **고정된 정체가 없다**.
  벽을 헐면 사라지고 다시 지으면 새로 잰다. 이름을 붙이려면 **같은 방인지 알아보는 열쇠**가 있어야 한다.
  `Rooms._key(판, 높이)` · `id_at` 이 이미 그 비슷한 것을 쥐고 있으니 거기서 출발한다
- [x] 1. 방이 눈에 보인다 — 오버레이
  - 기준: 방 안에 서면(또는 방을 겨누면) 그 방의 **바닥 판이 물들어 보인다** — 어디부터 어디까지가 한 방인지 한눈에 (테스트)
  - 기준: **방 종류마다 다른 빛깔**이다 — 잡실도 제 빛깔이 있다 (테스트)
  - 기준: **방이 아닌 곳은 안 물든다** — 사방이 안 막혔으면 아무것도 안 보인다 (테스트)
  - 기준: 벽을 헐면 **그 자리에서 사라진다** — 다시 재는 것과 어긋나지 않는다 (테스트)
  - 기준: 오버레이를 **켜고 끌 수 있다** — 늘 떠 있으면 거슬린다. 무엇으로 켜고 끄는지 `input.md` 에 적는다
  - 기준: 찍어서 확인한다 — 방 둘이 서로 다른 빛깔로 물든 그림
- [x] 2. O 로 방 페이지를 연다
  - 기준: **O** 로 방 페이지가 열리고 닫힌다 — `spec/01_settings/input.md` 표에 한 줄 (테스트)
  - 기준: **생성된 방만** 줄줄이 보인다 — 방이 없으면 「아직 없다」가 뜬다 (테스트)
  - 기준: 줄마다 **방의 이름 · 방의 역할**이 보인다 (테스트) — 지금은 이 둘뿐이다
  - 기준: 벽을 헐어 방이 없어지면 페이지에서도 사라진다 — 열어 둔 채로도 따라 바뀐다 (테스트)
  - 기준: 창이 떠 있는 동안 조작이 잠기고 걷기 안내 줄이 숨는다 — 다른 창과 같은 규칙 (테스트)
  - 기준: 글자는 번역 키다 — ko · en 둘 다 (테스트)
  - 기준: 찍어서 확인한다 — 방 몇 개가 뜬 페이지 한 장
  - 기준: (앞 단계 QA) rooms.md 수치 절이 「없음」으로 남았다 — spec/06_build/rooms.md 의 「## 수치 - 없음」인데 이번에 임시 수치(방 빛깔 다섯 · 잡실 MISC_COLOR · RoomOverlay.ALPHA 0.45 · REFRESH 0.5초)가 생겼다. 코드 · rooms.json 에는 「임시」라 적혔으나 spec 수치 절에는 없다. 그 절에 이 넷을 「(임시)」로 한 줄씩 적어라
- [x] 3. 방 이름을 바꾸고 저장한다
  - 기준: **같은 방인지 알아보는 열쇠**를 정하고 `rooms.md` 에 적는다 — 무엇이 같으면 같은 방인가 (테스트)
  - 기준: 페이지에서 **이름을 고쳐 쓸 수 있다** (테스트)
  - 기준: 이름을 안 지으면 **방 종류가 이름을 대신한다** — 빈 줄이 아니다 (테스트)
  - 기준: **저장된다** — 껐다 켜도 지은 이름이 그대로다 (테스트, spec/01_settings/save.md)
  - 기준: 벽을 헐어 방이 사라졌다가 **같은 자리에 다시 지으면** 그 이름이 돌아오는지 아닌지를 정하고 적는다 —
    어느 쪽이든 테스트로 못 박는다 (테스트)
  - 기준: 찍어서 확인한다 — 이름을 바꾼 뒤의 페이지
  - 기준: (앞 단계 QA) ROOM_FOR_FRAME 이 spec 수치 절에 없다 — game/ui/hud/room_window.gd 의 ROOM_FOR_FRAME 120px 는 코드에 「임시」라 적혔으나 spec/06_build/rooms.md(또는 spec/12_ui/hud.md) 「## 수치」에는 없다. 앞 단계 QA 가 짚은 것과 같은 꼴이다 — rooms.md 수치 절에 「방 페이지 목록 밖 창 몫 높이 RoomWindow.ROOM_FOR_FRAME 120px (임시) — 눈으로 잡았다」 한 줄을 더해라

## [x] G-163 주민은 방을 기준으로 일한다 (사람 결정 2026-09-29)
- 차선: 3d-start (G-162 와 같은 방 파일을 만진다 — 한 차선에 모은다)
- spec: spec/07_automation/npc-villagers.md, spec/06_build/rooms.md, spec/06_build/storage.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다 — 깨끗해질 때까지
- **지금의 「물뿌리개를 건네면 일한다」를 없앤다** — 한 주민이 한 가지밖에 못 맡게 되는 길이었다 (사람이 짚었다 2026-09-29)
- 사람이 준 규칙 그대로:
  - 주민은 **방을 기준으로** 일한다. 크게 두 가지다
  - **농부** — **방 안**을 작업한다. 방 안 상자에 물뿌리개나 비료가 있으면 꺼내 쓴다
  - 일을 마치면 **갖고 있는 모든 것을 그 방 상자에 둔다**
  - 상자가 **꽉 차서 더 못 넣으면 멈추고** 플레이어에게 알린다 — 「○○방에 상자가 부족하다」
  - 상자가 더해질 때까지 주민은 **idle 로 돌아다닌다**
  - **벌목 · 채광은 밖에서 일할 수도 있다** — 이때도 방 기준이고, 보통 **침실**을 쓴다.
    주민의 **거주지를 침실로 두고** 그 둘레 반경에서 일해 그 방 상자에 쌓는다
  - 반경을 고를 수 있다 — **네모난 범위로 짧음 · 중간 · 멂**
  - 보기: 나무가 많은 자리에 침실을 두고 「나무파밍」이라 이름 짓고, 주민의 거주지를 거기로 두고 나무꾼을 맡기면
    둘레 나무를 그 침실 상자에 쌓는다. 나중에 관리인이 옮긴다
- 사람이 정한 값 (2026-09-29): **농부는 방 안의 밭만** 돌본다 · 반경은 **24 / 48 / 96m** (전부 **임시**)
- **한 방에 주민 여럿을 둘 수 있다** (사람 결정 2026-09-29) — 거주지를 같이 쓰고, 상자도 같이 쓴다
- [x] 1. 침대를 만들고 침실을 둔다
  - 기준: **침대**를 만들어 놓을 수 있다 — 다른 설치물과 같은 길(제작 · 놓기 · 미리보기 · 부수기 · 저장) (테스트)
  - 기준: **침대 1 개가 들면 침실**이다 — `game/build/rooms.json` 에 한 줄 (주방 · 대장간과 같은 꼴) (테스트)
  - 기준: 침실이 방 페이지(G-162)와 오버레이에 제 빛깔로 뜬다 (테스트)
  - 기준: 재료 · 개수는 **임시**다 — 코드와 report 에 「임시」를 적는다
  - 기준: `spec/06_build/rooms.md` 표에 침실을 더한다
- [x] 2. 주민에게 거주지 방과 반경을 준다
  - 기준: **주민 창(V)에서 거주지 방을 고른다** — 만들어진 방만 보인다 (G-162 의 목록을 쓴다) (테스트)
  - 기준: **반경 세 단계**를 고른다 — 짧음 24m · 중간 48m · 멂 96m, **네모난 범위**. 값은 한 곳에 두고 「임시」를 적는다 (테스트)
  - 기준: 거주지 · 반경이 **저장된다** (테스트)
  - 기준: 거주지가 없으면 **일을 시작하지 않는다** — 왜 안 하는지 **창 안에** 남는다 (잠깐 떴다 사라지는 줄이 아니다) (테스트)
  - 기준: **한 방에 주민 여럿**을 둘 수 있다 (사람 결정 2026-09-29) — 몇이 사는지 방 페이지에 보인다 (테스트)
  - 기준: 방을 헐어 거주지가 없어지면 주민이 멈추고 알린다 (테스트)
  - 기준: (앞 단계 QA) regrowth 주석에 침대가 빠졌다 — game/world/resources_regrowth/regrowth.gd:28 의 「놓인 것(제작대 · 상자 · 횃불 · 선착장 · 건축물 — Placeable.GROUP)」 목록에 침대가 없다. 침대도 Placeable.GROUP 에 들어 재생을 막으니 목록에 「침대」를 더하라 (막는 것은 아니다)
- [x] 3. 방 상자에서 꺼내 쓰고, 방 상자에 쌓는다
  - 기준: **물뿌리개를 손으로 건네는 길을 없앤다** — `Villagers.entrust_aimed` 와 그 테스트를 걷어낸다 (grep)
  - 기준: **농부**는 거주지 방 안 상자에서 **물뿌리개 · 비료를 꺼내 쓴다** — 없으면 그 일만 건너뛴다 (테스트)
  - 기준: **농부는 방 안의 밭만** 돌본다 (사람 결정 2026-09-29) — 밖의 밭은 안 건드린다 (테스트)
  - 기준: **나무꾼 · 광부**는 거주지에서 **반경 안**의 나무 · 광물을 캔다 (테스트)
  - 기준: 일을 마치거나 손이 차면 **갖고 있는 것을 전부 그 방 상자에 둔다** (테스트)
  - 기준: **같은 방에 둘 이상이 살아도 서로 방해하지 않는다** — 한 밭 · 한 나무를 둘이 붙들지 않고,
    같은 상자에 같이 넣고 꺼내도 물건이 겹쳐 사라지거나 늘지 않는다 (테스트)
  - 기준: 방에 상자가 없으면 시작하지 않고 왜인지 창에 남는다 (테스트)
  - 기준: (앞 단계 QA) 방 페이지 머리 주석이 「둘뿐이다」 그대로다 — game/ui/hud/room_window.gd:6 이 「줄마다 방의 이름 · 방의 역할 … 둘뿐이다」라 적고 두 줄 아래 8줄에 셋째 칸(사는 주민)을 적어 서로 어긋난다. 6줄의 「둘뿐이다」를 「이름 · 역할 · 사는 주민 수(G-163.2)」로 고쳐라 (spec/06_build/rooms.md 는 이미 고쳤다)
- [x] 4. 상자가 차면 멈추고 알린다
  - 기준: 넣을 자리가 없으면 **멈춘다** — 캔 것을 바닥에 버리지 않는다 (테스트)
  - 기준: 플레이어에게 **「○○방에 상자가 부족하다」**가 뜬다 — 방 이름(G-162)을 쓴다. 번역 키 (테스트)
  - 기준: 그 알림은 **사라지지 않고 남는다** — 오른쪽 위 로그. 주민 창에서도 그 주민이 왜 멈췄는지 보인다 (테스트)
  - 기준: 상자를 더 놓으면 **스스로 다시 일한다** — 사람이 다시 맡기지 않아도 된다 (테스트)
  - 기준: 멈춰 있는 동안 **idle 로 돌아다닌다** — 굳어 서 있지 않는다 (테스트)
  - 기준: 찍어서 확인한다 — 상자가 차서 멈춘 주민과 그 알림
  - 기준: (앞 단계 QA) 디버그 찍기 장면 넷이 틀린 채 남았다 — game/core/debug/shots.gd 의 villager_water · villager_water_done · villager_woodcut · villager_woodcut_done 이 거주지 방 · 방 상자 없이 주민을 세운다. G-163.2 · G-163.3 부터는 일을 시작하지 않으니 찍으면 _wrong 에 남는다 (구현 세션도 그렇게 적어 뒀다). _set_up_villager_water 와 _set_up_woodcutter 에서 주민 곁에 막힌 방(토대 · 벽 · 지붕)을 짓고, 그 안에 물뿌리개(또는 빈 상자)를 넣은 상자를 두고, villagers.house 로 거주지를 줘라. 두 장면이 다시 젖은 밭 · 벤 나무를 찍는지 확인하라
  - 기준: (앞 단계 QA) 주민의 걸을 땅이 방 벽을 모른다 — AnimalNav 내비메시가 건축물을 안 넣어서 주민이 방 벽 너머로 곧장 걷다 막힌다. spec/07_automation/npc-villagers.md 에는 적혀 있다. 테스트는 자원을 방의 한쪽(동쪽)에만 둬서 이 문제를 비켜 간다(test_villager_gathering.gd 의 ROCK_AT 주석). 방 기준 일은 방 안팎을 드나드는 것이 전제다. 내비메시를 구울 때 건축물(벽 · 토대)을 넣고 문으로 드나들게 하라. 테스트: 자원을 방 반대쪽(서쪽)에 두고 주민이 방 안 상자와 오가며 캐는지 본다
- [x] 5. (앞 단계 QA) npc-villagers.md 가 150줄 꽉 찼다
  - 기준: npc-villagers.md 가 150줄 꽉 찼다 — spec/07_automation/npc-villagers.md 가 정확히 150줄이다(넘지는 않았다). 다음 단계에서 한 줄만 더해도 넘으니, G-163 묶음을 마칠 때 G-163.x 절(방 기준 일 · 상자 참 · 문 드나들기)을 spec/07_automation/ 아래 새 md(예: villager-rooms.md)로 떼어 내고 원래 파일에는 가리키는 한 줄만 남겨라. 사람 결정 줄은 옮기되 지우지 마라
- [x] 6. (앞 단계 QA) G-163 내용을 가리키는 낡은 spec 경로가 남았다
  - 기준: G-163 내용을 가리키는 낡은 spec 경로가 남았다 — 옮긴 절(거주지 · 방 상자 · 상자 참 멈춤 · 돌아다니기)을 가리키는 곳이 아직 npc-villagers.md 를 적고 있다: game/automation/villagers/stroll.gd 3줄(G-163.4 idle 돌아다니기), game/automation/villagers/field_care.gd 3줄(G-163.3 방 안의 밭), game/automation/villagers/gather_work.gd 4줄(G-163.3), game/build/beds/bed.gd 4줄 · game/tests/build/test_bed.gd 2줄 · spec/06_build/rooms.md 24줄(침실이 거주지가 된다), spec/01_settings/save.md 13줄(거주지 방 · 반경 · 멈췄나). 이 경로들을 spec/07_automation/villager-rooms.md 로 고치거나 둘 다 적어라 (npc-villagers.md 가 한 줄로 넘겨 주니 막히지는 않는다)

## [x] G-930 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-start (루프가 가져갔다 2026-09-29 09:46)
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. 낡은 주석 둘 (G-163 단계 3)
  - 기준: 낡은 주석 둘 — game/automation/villagers/villager.gd:69 「직업을 바꿔도 남는다 (든 물뿌리개가 사라지지 않게)」와 :240 「농부를 내려놓아도 든 물뿌리개는 그대로다」. 이제 물뿌리개는 일(JobWork)이 아니라 손(hands)에 있다. 두 줄을 손 기준으로 고쳐라
- [x] 2. NAV_MARGIN 에 「임시」 표시가 없다 (G-163 단계 3)
  - 기준: NAV_MARGIN 에 「임시」 표시가 없다 — game/automation/villagers/villagers.gd:45 의 NAV_MARGIN := 5.0 에 주석이 없다. 보고서는 이 값을 임시라 했고 spec 수치 절에도 없다. 줄 위에 「**임시**」 주석을 달고 spec/07_automation/npc-villagers.md 「수치」에 한 줄 더하라 (CHEST_REACH 도 spec 수치에 없다)
- [x] 2. to_dict 주석의 「일마다 쥔 것」이 비었다 (G-930 단계 1)
  - 기준: to_dict 주석의 「일마다 쥔 것」이 비었다 — villager.gd:285 는 「일마다 쥔 것(work: 직업 id → 그 일의 save_state)」을 담는다고 적지만, 지금 save_state 를 덮어쓰는 JobWork 가 하나도 없어(job_work.gd:36 만 {} 를 돌려준다) work 줄은 저장본에 늘 안 생긴다. 88줄 「직업을 바꿔도 남는다」도 이제 지킬 까닭이 적혀 있지 않다. 두 줄에 「지금은 담을 것이 있는 일이 없다 — 앞으로 제 상태를 가진 일이 생기면 담는다」처럼 지금 사정을 적어 두거나, 까닭 없이 남기는 것이면 그렇다고 적어라 (막는 것은 아니다 — 주석만의 일)
- [x] 2. NAV_HALF 의 5.0 이 이름 없는 임시 수치다 (G-930 단계 2)
  - 기준: NAV_HALF 의 5.0 이 이름 없는 임시 수치다 — game/automation/villagers/villagers.gd:54 `const NAV_HALF := FieldCare.WATER_RADIUS + 5.0` 의 5.0 은 NAV_MARGIN 과 같은 여유값인데 숫자로 박혀 있고 「임시」 표시도 spec 수치 줄도 없다. NAV_MARGIN 을 NAV_HALF 위로 올려 `FieldCare.WATER_RADIUS + NAV_MARGIN` 으로 쓰거나, 다른 값이면 주석에 **임시** 와 까닭을 달고 npc-villagers.md 「수치」 NAV_MARGIN 줄에 「걸을 땅은 적어도 민물 반경 60m + 5m」를 더하라

## [x] G-164 농부가 심고 거둔다 — 농사 한 바퀴를 닫는다 (사람 결정 2026-09-29)
- 차선: 3d-start
- spec: spec/07_automation/npc-villagers.md, spec/04_life/farming.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다 — 깨끗해질 때까지
- 「심는 것도 해야 되는데?」 (사람이 플레이하고 짚었다 2026-09-29)
- 지금은 농부가 **물 주기 · 비료 주기만** 한다 (field_care.gd). 그래서 **심지도 거두지도 않아** 농사가 한 바퀴를 못 돈다 —
  사람이 손으로 심어 줘야 시작되고, 다 자라도 사람이 거둬야 한다
- 쓸 것은 이미 있다: `Crops.plant` · `Crops.harvest` (사람이 쓰는 그 길). 농부가 그것을 그대로 부르면 된다
- [x] 1. 농부가 방 상자의 씨앗으로 빈 밭에 심는다
  - 기준: 거주지 방 **안의 빈 밭**에 심는다 — 방 밖의 밭은 안 건드린다 (물 · 비료와 같은 규칙) (테스트)
  - 기준: 씨앗은 **그 방 상자에서 꺼낸다** — 없으면 그 일만 건너뛴다 (물뿌리개 · 비료와 같다) (테스트)
  - 기준: **사람이 심는 것과 같은 길**을 부른다 (`Crops.plant`) — 주민 몫으로 새로 짜지 않는다 (테스트)
  - 기준: 상자에 씨앗이 여러 종류면 **무엇을 심나**를 정하고 spec 에 적는다 (테스트)
  - 기준: 남은 씨앗은 일을 마칠 때 **그 방 상자에 도로 둔다** (테스트)
- [x] 2. 농부가 다 자란 것을 거둔다
  - 기준: 거주지 방 안의 **다 자란 밭**을 거둔다 — `Crops.harvest` 그대로 (테스트)
  - 기준: 거둔 것을 **그 방 상자에 둔다** — 상자가 차면 멈추고 알린다 (G-163.4 와 같은 길) (테스트)
  - 기준: 거둔 자리에 **다시 심는다** — 심을 씨앗이 상자에 있으면 (테스트)
- [x] 3. 한 바퀴가 돈다 — 빈 밭에서 거둘 때까지
  - 날을 넘기는 것은 `Crops.next_day()` 를 쓴다 — 이미 그렇게 돼 있다 (대화 세션이 시계를 돌리라고 잘못 적었다가 뺐다 2026-09-29)
  - 기준: **심기 → 물 → 비료 → 거두기 → 다시 심기**가 사람 손 없이 이어진다 —
    빈 밭 · 씨앗 든 상자 · 농부 하나로 시작해 거둔 것이 상자에 쌓일 때까지 실제로 돌려 본다 (테스트)
  - 기준: 할 일을 고르는 차례를 spec 에 적는다 — 무엇을 먼저 하나 (심기 · 물 · 비료 · 거두기)
  - 기준: 찍어서 확인한다 — 빈 밭 · 심은 뒤 · 거둔 뒤 세 장

## [x] G-165 주민이 닿아야 한다 — 벽 너머로 일하지 않는다 (사람이 플레이하고 짚었다 2026-09-29)
- 차선: 3d-start
- spec: spec/07_automation/npc-villagers.md
- **자가 피드백**: 찾은 것을 계속 붙인다 — 깨끗해질 때까지
- 사람이 본 것 그대로:
  - **밭 하나만 하고 안 움직인다**
  - **방 안으로 들어오지도 않는다**
  - **벽 밖에서 씨앗을 심는다**
  - **씨앗 · 물뿌리개를 꺼낼 때 상자 앞으로 안 간다**
  - 「이거 심는 걸 **닿을 수 있어야** 하게 만들어야 될 것 같아」
- 어디가 그런가: `field_care.gd` 의 `_field_in_reach` 는 **직선 거리(Farmland.REACH 4m)만** 잰다.
  벽이 가로막혀 있어도 4m 안이면 「닿는다」로 보고 그 자리에서 해 버린다 — 그래서 방 밖에서 심고, 걸어 들어오지 않는다.
  사람은 조준선이 벽에 막혀 못 하는데 주민은 그것을 안 본다. 상자도 같은 꼴로 보인다 (`chests_in_reach`)
- [x] 1. 닿는다는 것이 사람과 같아진다
  - 기준: **막힌 것이 있으면 안 닿는다** — 주민과 대상 사이에 벽 · 문 · 지붕이 있으면 그 자리에서 하지 않고 걸어간다 (테스트)
  - 기준: **밭 · 상자 · 물 모두 같은 잣대**다 — 닿는지 재는 곳이 **한 곳**이어야 한다. 세 군데에 따로 두지 않는다 (테스트)
  - 기준: 사람이 못 하는 자리에서는 주민도 못 한다 — **같은 길로 잰다** (테스트)
  - 기준: 방 밖에 선 채로 방 안 밭에 심지 않는다 — 걸어 들어와서 심는다 (테스트)
  - 기준: 상자에서 꺼낼 때 **상자 앞으로 걸어간다** — 멀리서 꺼내지 않는다 (테스트)
- [x] 2. 밭 하나를 하고 멈추지 않는다
  - 기준: 방 안에 밭이 여럿이면 **하나를 마치고 다음 밭으로 간다** — 할 일이 남았는데 서지 않는다 (테스트)
  - 기준: 왜 멈췄는지 알 수 있다 — 할 일이 없어 선 것인지, 못 가서 선 것인지 주민 창에 남는다 (테스트)
  - 기준: 길이 막혀 못 가는 밭은 **건너뛰고 다음 것을 한다** — 하나에 붙들려 멈추지 않는다 (테스트)
  - 기준: 찍어서 확인한다 — 여러 밭을 차례로 돌본 뒤의 방
  - 기준: (앞 단계 QA) 앞을 막은 주민 판정이 넓다 — Villager.blocked_by_mate 는 1.2m 안 · 대상 쪽 반평면(dot>0)이면 참이라, 옆으로 거의 나란히 선 주민만 있어도 끼였을 때 4m 로 닿는다. 열린 문 밖에서 옆 주민과 나란히 문틀에 끼이면 3m+ 상자에서 꺼낼 수 있다. 대상 쪽 좁은 부채꼴(예: 방향 cos>0.7)로 좁히고, 나란히 선 주민(옆 90°)은 막은 것이 아닌지 보는 단언을 test_villager_reach.gd 의 test_stuck_behind_a_mate_… 에 더하라
  - 기준: (앞 단계 QA) 한 곳 테스트가 소스 글자를 본다 — test_fields_chests_and_water_are_measured_in_one_place 는 파일 글자에 'in_reach(' 가 있는지 · 'CHEST_REACH' 가 없는지만 본다. 이름만 바꾼 새 거리 비교는 못 잡는다. 행동으로도 지키려면 Villager.HAND 를 바꿔(예: 테스트에서 in_reach 를 가로채는 대신) 밭 · 상자 · 물 셋이 함께 바뀌는지 보는 식으로 보강하라 (벽 테스트가 이미 행동으로 셋을 보니 막는 것은 아니다)
- [x] 3. 사람이 플레이하는 그대로 한 번 돌려 본다
  - 기준: **벽 · 문 · 지붕으로 막은 진짜 방**을 짓고, 그 안에 밭 여럿 · 상자(씨앗 · 물뿌리개 · 비료)를 두고
    주민에게 거주지와 농부를 맡겨, **문으로 걸어 들어와** 밭을 차례로 다 돌보는지 실제로 돌려 본다 (테스트)
  - 기준: 찍어서 확인한다 — 주민이 방 안에서 일하는 그림 · 밖에서 심는 일이 없다
  - 기준: (앞 단계 QA) SHORE_STEP 에 「임시」 표시가 없다 — game/world/terrain/test_island.gd 의 SHORE_STEP(0.5m) 주석에 **임시** 를 달아라. report 의 temporary 목록에는 있는데 코드 주석에는 없다
  - 기준: (앞 단계 QA) 찍은 그림을 남기지 않았다 — 구현 세션이 farm_planted 를 다시 찍었다고 했으나 그 PNG 가 어디에도 없다(.loop/out/farm 은 G-164 때 것). 찍은 뒤 .loop/out/<단계>/ 에 남겨 QA 가 볼 수 있게 하라 (이번에는 QA 가 .loop/out/qa_g1652 에 다시 찍었다)
- [x] 4. (앞 단계 QA) npc-villagers.md 가 딱 150줄이다
  - 기준: npc-villagers.md 가 딱 150줄이다 — 이번에 두 줄을 한 줄로 합쳐 겨우 맞췄다. 다음에 한 줄만 더해도 넘는다. 「지금 만든 것」 · 「멈추지 않는다」 같은 단계별 기록 절을 spec/07_automation/ 아래 따로 떼어(예: npc-villagers-history.md) 본문을 120줄 안팎으로 줄여 두어라
- [x] 5. (앞 단계 QA) 코드 주석이 옮긴 단계 기록을 본문으로 가리킨다
  - 기준: 코드 주석이 옮긴 단계 기록을 본문으로 가리킨다 — villager_window.gd:3 · game_root.gd:361 · input_actions.gd:42 (G-161.1), shots.gd:199 (G-157.2), field_care.gd:3 (G-157.3) 이 `spec/07_automation/npc-villagers.md` 를 적고 있다. 그 단계의 「지금 만든 것」은 이제 npc-villagers-history.md 에 있다. 본문 「단계별 기록」 절이 기록 파일을 가리키니 따라가면 닿기는 한다. 그래도 이 다섯 주석은 `spec/07_automation/npc-villagers-history.md 「지금 만든 것 (G-…)」` 로 바꿔 두어라. 「직업」 절을 가리키는 jobs.gd · job_work.gd · woodcutting.gd · mining_work.gd · harvest_work.gd · gather_work.gd 는 본문에 그 절이 남았으니 그대로 둔다

## [x] G-166 주민이 계단을 오른다 (사람이 플레이하고 짚었다 2026-09-29)
- 차선: 3d-start
- spec: spec/07_automation/npc-villagers.md, spec/06_build/rooms.md
- **자가 피드백**: 찾은 것을 계속 붙인다 — 깨끗해질 때까지
- 사람이 본 것: 「턱이 있어서 **계단을 설치**해 줬고 그래도 못 갔어. 방은 2×2칸에 밭이랑 상자를 설치했어.
  **상자가 벽에 붙어 있긴 해**」 · 앞서 「직선으로 상자에 다가갔지만 닿지 못해서 **길이 막혔다**고 나온다」
- **찾았다**: `game/life/hunting/animal_nav.gd:123` — 「지붕 · **계단** · 기둥은 넣지 않는다 — 주민은 1층만 다닌다
  (위층 · 계단을 오르는 길은 아직 없다)」. 걸을 땅에 계단이 없어 **턱 위의 방으로 길이 안 난다**.
  그래서 직선으로 다가가다 턱에 막혀 「길이 막혔다」가 뜬다
- 대화 세션이 만든 검사(test_villager_door.gd)는 **평평한 땅**이라 이 자리를 못 잡았다 — 계단이 필요 없었다
- [x] 1. 걸을 땅이 계단을 딛는다
  - 기준: **계단이 걸을 땅에 든다** — 계단 윗면이 딛는 바닥이고, 아래 판과 위 판이 계단으로 **이어진다** (테스트)
  - 기준: **턱 위에 지은 방**으로 밖에서 길이 난다 — 계단을 밟고 올라 문으로 들어간다 (테스트)
  - 기준: 주민이 실제로 **걸어 올라간다** — 길만 나는 것이 아니라 끝까지 간다 (테스트)
  - 기준: 계단이 없으면 여전히 못 간다 — 턱을 뛰어넘지 않는다 (테스트)
  - 기준: 짐승은 그대로다 — 짐승이 계단으로 집에 들어오지 않는다면 그 규칙을 지키고, 바뀌면 spec 에 적는다 (테스트)
- [x] 2. 벽에 붙은 상자에도 손이 닿는다
  - 기준: **상자를 벽에 붙여 놓아도** 주민이 손 닿는 데까지 간다 (테스트) — 상자는 걸을 땅에서 도려내지므로
    벽과 상자 사이에 설 자리가 없을 수 있다
  - 기준: 2×2 판 방에 밭과 상자를 다 놓아도 다닐 자리가 남는다 — 사람이 그렇게 지었다 (테스트)
  - 기준: 그래도 설 자리가 없으면 **왜인지 창에 남는다** — 말없이 서 있지 않는다 (테스트)
  - 기준: (앞 단계 QA) 짐승의 목적지가 주민 층 바닥에 붙을 수 있다 — game/life/hunting/animal.gd 의 _pick_spot · _away_from 이 NavigationServer3D.map_get_closest_point 로 목적지를 당기는데 이 함수는 navigation_layers 를 거르지 않아, 이제 층 2 에 선 주민의 걸을 땅(방 바닥 · 계단 비탈)으로 당겨질 수 있다. 길은 층 1 로 묻으니 방에 들지는 않지만 벽 밑에서 헛걸음한다. 짐승 층(1) 지역만 보고 가장 가까운 점을 고르거나(예: 당긴 점으로 층 1 길을 묻어 끝점을 목적지로 쓴다), 당긴 점이 층 1 밖이면 다시 고르게 하고 — 방 곁 짐승의 _pick_spot 이 방 바닥 높이를 돌려주지 않는지 단언하는 테스트를 붙여라
- [x] 3. 사람이 지은 그대로 돌려 본다
  - 기준: **턱 + 계단 + 2×2 방 + 벽에 붙은 상자 + 밭**을 그대로 지어, 밖에 선 주민이 계단을 올라 문으로 들어가
    상자에서 꺼내 밭을 돌보는지 끝까지 돌려 본다 (테스트)
  - 기준: 찍어서 확인한다 — 계단을 오르는 주민 · 방 안에서 일하는 주민
  - 기준: (앞 단계 QA) 나무꾼·광부의 「설 자리 없음」 테스트가 없다 — gather_work.gd 에 why_still 과 WHY_NO_ROOM_AT_CHEST · WHY_CANT_GET_THERE 를 적는 갈래(두러 가다 PathWalk.ARRIVED 인데 손이 안 닿을 때)를 새로 넣었지만 어느 테스트도 이 길을 밟지 않는다. test_villager_chest_wall.gd 에 나무꾼(Jobs 의 나무꾼)이 통나무를 들고 빽빽이 쌓인 상자 구석(씨앗 테스트와 같은 ROWS×ROWS 배치, 둘 자리 있는 상자는 맨 구석 하나만)으로 두러 가 villagers.why_still 이 WHY_NO_ROOM_AT_CHEST 를 절반 넘는 프레임 동안 돌려주는지 단언하는 테스트를 붙여라
  - 기준: (앞 단계 QA) test_animal_walk 의 거리 단언이 부하에 흔들린다 — 구현 세션이 바뀐 곳 그물에서 test_wanders_around_home 이 21.07m < 21.0m 로 한 번 떨어졌다고 적었다(이번 test.log 에서는 통과). 옛 animal.gd 로도 통과해 이번 변경 탓은 아닐 수 있으나 _snap 이 길 끝점을 고르면서 WANDER_RADIUS 밖 점을 돌려줄 수 있는지 확인하지 않았다. test_animal_walk.gd 의 그 단언 여유를 다시 재거나, _pick_spot 이 home 에서 WANDER_RADIUS + 한 걸음 안의 점만 돌려주는지 단언하는 테스트를 붙여라
- [x] 4. (앞 단계 QA) _pick_spot 다시 고르기 길을 아무 테스트도 안 밟는다
  - 기준: _pick_spot 다시 고르기 길을 아무 테스트도 안 밟는다 — test_picked_spots_stay_within_the_wander_radius 는 옛 _pick_spot(다시 고르기 없음)으로 돌려도 통과했다(집 셋에서 가장 멀리 19.96 · 19.79 · 19.84m). 고른 점이 물 · 바위에 막혀 _snap 이 반경 밖을 돌려주는 집이 없어서다. test_animal_walk.gd 에 집 둘레 반쪽이 물인 자리(예: 못 한가운데 가까이, 또는 바닷가 끝)를 골라 옛 코드면 반경 밖 점이 나오는지 먼저 재고, 그런 집에서 새 코드가 WANDER_RADIUS + STEP 안인지 단언해라. 그런 자리가 섬에 없다면 PICK_TRIES 갈래는 지워도 된다
  - 기준: (앞 단계 QA) AsBuiltRoom 의 밭은 심기까지만 본다 — 기준의 「밭을 돌보는지」를 심기로 읽었다. water_spots 가 빈 목록이라 물 주기는 이 판에서 안 돈다. 방 안 밭에 물을 떠다 주는 길(계단을 두 번 오르내림)도 보려면 AsBuiltRoom 에 물가를 두고 test_villager_as_built.gd 에 물 준 밭을 단언하는 테스트를 더해라

## [x] G-167 벽에 붙은 상자에도 손이 닿는다 — G-166.2 가 안내만 만들었다 (사람이 플레이하고 짚었다 2026-09-30)
- 차선: 3d-start
- spec: spec/07_automation/npc-villagers.md
- **자가 피드백**: 찾은 것을 계속 붙인다 — 깨끗해질 때까지
- 사람이 본 것: 「지금도 **직선으로 이동 방향을 정하고 가서 상자 곁에 설 자리가 없어 손이 안 닿는다 —
  상자 둘레를 비워 주면 간다**. 이렇게 적혀 있는데?」 (방은 2×2 판 · 밭과 상자 · 상자는 벽에 붙어 있다)
- **G-166.2 는 기준 둘 가운데 하나만 했다** — 「벽에 붙여 놓아도 손이 닿는다」는 안 되고,
  「그래도 안 되면 왜인지 창에 남긴다」(VILLAGERS_NO_ROOM_AT_CHEST_WHY)만 만들어졌다. **안내가 고침을 대신했다**
- 숫자로는 닿아야 한다: 상자를 WALL_PAD 0.45m 도려내고 몸 반지름 0.5m 가 더 깎여 **상자에서 1m 쯤** 떨어져 서는데,
  주민 손은 **2.0m** 다. 그런데도 안 닿는다 — 벽까지 같은 폭으로 깎여 **좁은 방에서 설 자리가 통째로 없어지는** 것으로 보인다
- [x] 1. 왜 설 자리가 없는지 재서 밝힌다
  - 기준: **사람이 지은 그대로** 만든다 — 턱 + 계단 + 2×2 판 방 + 문 + **벽에 붙은 상자** + 밭.
    그 자리에서 걸을 땅이 상자 둘레 어디까지 남는지 **숫자로 재어** report 에 적는다 (테스트)
  - 기준: 안 닿는 까닭을 **하나로 짚는다** — 상자 도려내기 · 벽 도려내기 · 몸 반지름 · 방이 좁아서 중 무엇인가.
    짐작이 아니라 잰 값으로 적는다
  - 기준: 이 단계에서는 고치지 않는다 — 재서 밝히기만 한다
- [x] 2. 닿게 고친다
  - 기준: **벽에 붙은 상자에 손이 닿는다** — 2×2 판 방에 밭과 상자를 다 놓아도 주민이 가서 꺼내고 둔다 (테스트)
  - 기준: 「상자 곁에 설 자리가 없다」가 **이 짜임에서는 안 뜬다** — 안내는 정말 막혔을 때만 (테스트)
  - 기준: 고친 값(도려내는 폭 · 손 거리 …)은 **왜 그 값인지** 주석에 적고 「임시」를 붙인다
  - 기준: 상자를 뚫고 다니지는 않는다 — G-164.3 이 막은 것이 그대로다 (테스트)
  - 기준: 찍어서 확인한다 — 벽에 붙은 상자 앞에 선 주민
  - 기준: (앞 단계 QA) 합성 판 테스트가 흠을 재현하지 못한다 — test_villager_chest_room_measure.gd 는 머리 주석에 「사람이 지은 그대로(AsBuiltRoom)」라 적지만 거기서는 길 끝이 방 안이라 assert_true(room.in_room(end)) 가 통과한다. 흠은 저장본 판(test_villager_chest_saved_room.gd)에서만 난다. 머리 주석에 「AsBuiltRoom 판은 방 바닥과 벽 밖 땅의 높이 차가 달라 흠이 안 난다 — 흠 재현은 test_villager_chest_saved_room.gd」라고 적거나, 까닭 가리기가 saved_room 과 겹치니 이 파일을 지우고 한 곳으로 모아라
  - 기준: (앞 단계 QA) 까닭 가리기 헬퍼가 두 파일에 겹친다 — _bake · _area · _nearest · _gaps 가 test_villager_chest_room_measure.gd 와 test_villager_chest_saved_room.gd 에 거의 그대로 둘이다. 위 정리 때 한 파일로 모으거나 tests 쪽 공용 헬퍼로 빼라
- [x] 3. 좁은 방에서 한 바퀴가 돈다
  - 기준: 2×2 판 방에서 **심기 → 물 → 비료 → 거두기**가 끝까지 돈다 — 사람이 지은 크기 그대로 (테스트)
  - 기준: 밭과 상자를 방에 꽉 채워도 다닐 자리가 남는다 — 안 남으면 놓을 때 막거나 알린다 (테스트)
  - 기준: (앞 단계 QA) 지운 파일의 스테이징이 짝이 안 맞는다 — test_villager_chest_room_measure.gd 삭제는 인덱스에 스테이징됐고(git rm --cached) 짝 .uid 삭제는 워킹트리에만 있다. 커밋할 때 둘 다 `git add -A game/tests/automation/villagers/` 로 함께 담아라 (내용은 둘 다 지운 상태로 맞다)
- [x] 4. 길이 없으면 직선으로 가지 않는다
  - 사람이 짚었다 (2026-09-30): 「**NPC 가 그냥 직선으로 가면 안 되는데** 이건 못 고치는 거야?」
  - 어디가 그런가: `core/path_walk.gd` 의 `plan(body, target, straight)` 은 **길을 못 찾으면 목표로 곧장 가는 길**을 만든다
    (`path = PackedVector3Array([target])`). 주민이 걷는 다섯 곳이 전부 `straight = true` 로 부른다 —
    `field_care.gd` 347 · 373 · 314, `gather_work.gd` 220 · 234, `stroll.gd` 50.
    그래서 벽 · 계단 때문에 길이 없어도 **막힌 줄 모르고 직선으로 가다 처박힌다**
  - 기준: **주민은 길이 없으면 직선으로 가지 않는다** — 가지 않고 「길이 막혔다」로 선다 (테스트)
  - 기준: 직선 되돌림이 **왜 있는지** 밝히고, 정말 필요한 자리만 남긴다 — 내비메시가 아직 안 구워졌을 때처럼.
    남긴 자리마다 까닭을 주석에 적는다 (그냥 다 없애면 다른 곳이 멈춘다)
  - 기준: **짐승은 그대로다** — 짐승이 직선으로 다니던 것이 바뀌면 spec 에 적고 테스트로 못 박는다 (테스트)
  - 기준: 벽 · 계단으로 막힌 자리에서 주민이 **벽에 처박혀 비비지 않는다** (테스트)
- [x] 5. (앞 단계 QA) 벽 거리 여유가 2mm 뿐이다
  - 기준: 벽 거리 여유가 2mm 뿐이다 — test_villager_no_straight.gd 의 땅 재기가 지금 0.502m 로 WALL_CLEAR 0.5 를 겨우 넘는다(report). 굽는 칸 0.25m 에 맞춰 난 값이라 엔진 · 굽기 설정이 조금만 달라져도 흔들릴 수 있다. WALL_CLEAR 를 몸 반지름 0.4 보다 넉넉한 0.45 쯤으로 낮추거나(0.45 로 되돌리면 0.40 이 나므로 여전히 잡는다) 그 이유를 WALL_CLEAR 주석에 적어라
  - 기준: (앞 단계 QA) 상자 대조 단언의 뜻이 뒤집혔다 — test_villager_chest_saved_room.gd 의 대조가 이제 「길 끝이 방 안이다」(assert_true)라 G-167.1 이 잰 흠(방 밖)을 더는 재현하지 않는다. 주석에 까닭은 적혔으나 테스트 이름 · 머리 주석(test_measure_the_saved_room 이 흠을 잰다는 설명)이 옛 뜻이면 맞춰 고쳐라
  - 기준: (앞 단계 QA) 벽 몫 변경이 짐승 걸을 땅도 바꾼다 — AnimalNav.WALL_PAD 0.6 은 짐승 굽기에도 쓰인다(짐승은 벽에서 0.15m 더 물러난다). 전체 테스트는 통과했으나 spec/07_automation/npc-villagers.md 「짐승은 그대로다」 줄 곁 또는 짐승 spec 에 이 한 줄(벽 둘레 짐승 걸을 땅도 0.6m 로 물러났다)을 적어라

## [x] G-168 놓인 것 전부를 걸을 땅에서 도려낸다 — 횃불에 막힌다 (사람이 찍어 보냈다 2026-09-30)
- 차선: 3d-start
- spec: spec/07_automation/npc-villagers.md
- **자가 피드백**: 찾은 것을 계속 붙인다 — 깨끗해질 때까지
- 사람이 찍어 보냈다: 「상자에 가는 중간에 **횃불이 있으면 막혔다고 일을 안 해**.
  이런 경우가 많을 텐데 **횃불 말고도 중간에 장애물이 있으면 피해야** 되는데」
- 어디가 그런가: `game/life/hunting/animal_nav.gd` 가 도려내는 것은 **`BuildPiece` 와 `StorageChest` 둘뿐**이다 (137~163줄).
  횃불 · 제작대 · 밭처럼 나머지 놓인 것은 걸을 땅에 없어 **길이 그것들을 뚫고 나고**, 걸어가다 몸으로 부딪혀 멈춘다
- **손으로 적은 목록이라 새 물건이 생길 때마다 또 빠진다** — G-150.1 에서 밭이 무너짐 목록에서 빠졌던 것과 같은 꼴이다
- [x] 1. 놓이는 것 전부를 도려낸다
  - 기준: **목록이 아니라 무리에서 끌어온다** — `Placeable.GROUP`(또는 `Buildings.everything_placed`)를 훑는다.
    새로 놓이는 것이 생겨도 저절로 든다 (테스트)
  - 기준: **검사가 목록이 아니라 전부를 훑는다** — 놓이는 클래스를 프로젝트에서 끌어와 하나라도 안 도려내지면 떨어진다
    (`tests/build/test_collapse_takes_everything_placed.gd` 가 본보기다) (테스트)
  - 기준: 도려내는 폭은 **물건마다 제 몸 크기**를 따른다 — 가는 것(횃불)을 벽만큼 크게 도려내 좁은 방을 막지 않는다 (테스트)
  - 기준: **밭은 밟고 지나간다** — 밭은 땅처럼 밟는 것이라 도려내면 밭일을 못 한다. 도려낼 것과 밟을 것을 가르고 까닭을 적는다 (테스트)
- [x] 2. 횃불을 사이에 두고 상자로 간다
  - 기준: **사람이 찍어 보낸 그 짜임** — 방 안에 횃불이 길 한가운데 서 있고 상자는 구석. 주민이 **돌아서** 상자에 닿는다 (테스트)
  - 기준: 「길이 막혔다」 · 「설 자리가 없다」가 이 짜임에서는 안 뜬다 (테스트)
  - 기준: 횃불을 나중에 놓아도 걸을 땅이 다시 구워진다 — 놓자마자 길이 돌아간다 (테스트)
  - 기준: 찍어서 확인한다 — 횃불을 돌아 상자로 가는 주민
  - 기준: (앞 단계 QA) 트리 전체 node_added 를 듣는다 — Villagers._enter_tree 가 get_tree().node_added/node_removed 에 붙어 총알 · 떨어진 물건 · 이펙트 등 트리에 드는 모든 노드마다 _changes_ground 를 부른다. 지금은 `is CollisionObject3D` 로 먼저 걸러 싸지만, Placeable 쪽에 신호(예: Placeable 무리에 들 때 한 번 알리기)로 좁히거나 까닭을 주석에 적어 두면 좋다
  - 기준: (앞 단계 QA) _changed_at 의 저장 까닭이 흩어졌다 — test_round_trip.gd 의 DONT_SAVE 에서 Island/Villagers:_changed_at 줄을 뺐는데 villagers.gd 주석은 「담지 않는다」로 시작한다. 왕복에서 값이 같아져 빠진 것이면 주석 첫 마디를 「저장 목록에 둘 까닭이 없다 — 불러오면 섬이 설 때 같은 자리가 다시 모인다」처럼 맞춰 헷갈리지 않게 하라
- [x] 3. (앞 단계 QA) 찍기 짜임이 테스트 짜임과 다르다
  - 기준: 찍기 짜임이 테스트 짜임과 다르다 — ui_shots.gd _torch_between 은 횃불 하나만 세운다. 테스트는 하나로는 몸이 비벼 지나가 막힘이 안 난다며 줄을 세웠으니, 사진은 「돌아간다」보다 「곁을 스친다」를 보인다. 사람이 볼 사진이면 테스트의 _row() 처럼 횃불 줄을 세워 찍게 맞추면 좋다 (막는 것은 아니다)
- [x] 4. (앞 단계 QA) 낡은 횃불 사진이 .loop/out 에 남았다
  - 기준: 낡은 횃불 사진이 .loop/out 에 남았다 — .loop/out/torch_shot/villager_torch_{ko,en}.png 는 06:25(G-168.2 이전) 것으로 횃불 하나만 보인다. 새 사진은 /tmp/g1683/ 에만 있다. 사람이 볼 곳에 두려면 .loop/out/torch_shot/ 를 새 사진으로 바꾸거나 지워라

## [x] G-169 주민이 상자를 붙들고 놓지 않는다 — 사람도 못 집는다 (사람이 플레이하고 짚었다 2026-09-30)
- 차선: 3d-start
- spec: spec/07_automation/npc-villagers.md, spec/06_build/storage.md
- **자가 피드백**: 찾은 것을 계속 붙인다 — 깨끗해질 때까지
- 사람이 본 것 그대로: 「물뿌리개를 **클릭하고 있는 것 같은데 안 집어지는지** 상자를 보면 **엄청 클릭하는 게 보이고**,
  심지어 **나도 안 집어져**. **NPC 를 떠나보내면 내가 집어져**」
- 짐작(확인 안 됨): 주민이 물뿌리개를 **꺼냈다 도로 넣기를 되풀이**해, 그 사이에 칸이 비어 사람 손이 헛도는 것으로 보인다.
  떠나보내면 풀리는 것도 이와 맞는다. **단정하지 말고 단계 1 에서 재서 밝힌다**
- **찾았다** (대화 세션 2026-09-30) — 사람이 짚어 준 「**비료랑 씨앗은 되는데 물뿌리개만 안 된다**」가 열쇠였다.
  `hands.gd:61 add()` 는 **상태를 안은 물건**(물뿌리개 — 물을 담는다)을 **빈 칸이 있을 때만** 받는다.
  씨앗 · 비료는 쌓이는 물건이라 이미 있는 칸에 얹혀 늘 들어간다. 손은 4칸(`SLOTS`)뿐이라
  거둔 밀 · 씨앗 · 비료로 차면 물뿌리개가 **영영 못 들어간다**. 그런데 `take_from` 은 0 을 돌려주고
  주민은 「손이 찼다」로 끝낸 뒤 **곧바로 다시 꺼내려 든다** — 그것이 「엄청 클릭하는」 것으로 보이고
  그 사이 칸이 흔들려 사람도 못 집는다. 단계 1 은 이 짐작을 **재서 확인**하는 것이다
- [x] 1. 무엇이 되풀이되는지 재서 밝힌다
  - 기준: 사람이 본 짜임을 만든다 — 방 · 상자에 물뿌리개 · 농부. **상자 칸이 몇 초에 몇 번 바뀌는지 세어** report 에 적는다 (테스트)
  - 기준: 되풀이가 맞으면 **한 바퀴가 어디서 어디로 도는지** 짚는다 — 꺼냈다가 왜 도로 넣나.
    되풀이가 아니면 사람이 못 집는 진짜 까닭을 짚는다 (붙들어 둔 것 · 잠금 · 다른 것)
  - 기준: **사람이 같은 칸을 집으려 할 때 무엇이 막는지** 잰다 — 칸이 비어서인가, 막아 둔 것이 있어서인가
  - 기준: 이 단계에서는 고치지 않는다 — 재서 밝히기만 한다
- **사람이 고를 길을 정했다** (사람 결정 2026-09-30): **손 칸을 늘리고**, **거둔 것을 먼저 상자에 두게** 한다.
  못 들면 되풀이하지 않는 것은 그 위에 받침으로 둔다 — 두 가지를 해도 언젠가는 손이 찬다
- [x] 2. 되풀이를 멈춘다
  - 기준: **손 칸을 늘린다** (`hands.gd` SLOTS 4) — 몇 칸으로 늘릴지 정하고 **왜 그 수인지** 주석에 적고 「임시」를 붙인다 (테스트)
  - 기준: **거둔 것을 먼저 상자에 둔다** — 손이 차기 전에 두러 가서, 물뿌리개를 들 자리를 비워 둔다 (테스트)
  - 기준: **밀을 한가득 거둔 뒤에도 물뿌리개를 든다** — 사람이 본 그 자리가 풀린다 (테스트)
  - 기준: 주민이 **꺼냈으면 쓰거나, 못 쓰면 다시 꺼내지 않는다** — 같은 것을 몇 초 안에 되풀이해 꺼내지 않는다 (테스트)
  - 기준: 못 쓰는 까닭이 있으면 **멈추고 창에 남긴다** — 말없이 되풀이하지 않는다 (테스트)
  - 기준: 상자 칸이 **초당 몇 번씩 바뀌지 않는다** — 단계 1 에서 잰 값과 견준다 (테스트)
  - 기준: (앞 단계 QA) 되풀이 기대는 고칠 때 뒤집어야 한다 — test_villager_can_churn.gd 의 세 테스트(물 없음 · 사람 막힘 · 물 막힘)는 지금의 흠을 단언한다(assert_gt(changes, ticks/2), flips>=18, 헛돎>0). 고치는 단계에서 이 단언을 「초당 몇 번 이하 · 물뿌리개가 상자에 머문다 · Shift+클릭 30번 다 된다」로 뒤집어야 한다. 머리 주석에 적혀 있으나 다음 단계 기준에 꼭 넣어라
  - 기준: (앞 단계 QA) 상자 칸 그림 깜박임은 재지 않았다 — report 는 창 칸 그림이 Chest.changed 마다 다시 그려져 틱마다 깜박인다(사람이 본 「엄청 클릭한다」)고 적었으나 테스트로 확인하지 않았다. 고치는 단계에서 상자 창을 연 채 칸 그림이 다시 그려지는 횟수를 세는 단언을 붙이면 사람이 본 증상까지 막힌다
- [x] 3. 사람과 주민이 같은 상자를 같이 쓴다
  - 기준: **주민이 일하는 중에도 사람이 그 상자에서 집을 수 있다** (테스트)
  - 기준: 사람이 먼저 집으면 주민은 없는 대로 하고, 주민이 먼저 집으면 사람은 남은 것을 집는다 —
    **물건이 겹쳐 사라지거나 늘지 않는다** (테스트)
  - 기준: 주민을 떠나보내지 않아도 된다 — 사람이 그것 때문에 주민을 내보내는 일이 없다 (테스트)
  - 기준: 찍어서 확인한다 — 주민이 일하는 중에 사람이 상자를 연 창
  - 기준: (앞 단계 QA) REFETCH(_shun) 만 지키는 테스트가 없다 — field_care.gd 의 `_shun[k] = REFETCH` 를 `pass` 로 바꿔도 test_villager_can_churn.gd 가 다 통과했다. 물 막힘 짜임에서는 _wet_can 미리 보기가 먼저 막아 REFETCH 가 한 번도 안 쓰인다. 씨앗 · 비료처럼 미리 보기가 없는 것을 꺼냈다가 못 쓰는 짜임(예: 방 상자에 씨앗, 심을 밭이 꺼낸 뒤 가다 막혀 빠짐)을 만들어 도로 둔 뒤 REFETCH 동안 다시 안 꺼내는지 · why_still==WHY_PUT_BACK 인지 단언하라
  - 기준: (앞 단계 QA) WHY_PUT_BACK 까닭을 단언하는 테스트가 없다 — grep 에서 Villagers.WHY_PUT_BACK 을 보는 테스트가 0개다. 위 REFETCH 테스트에 함께 넣으면 된다
- [x] 4. 열린 문이 다시 닫힌다
  - 사람이 짚었다 (2026-09-30): 「문도 이상한 게 **NPC 가 연 문이라서 그런 건지 게임을 껐다 켜서 그런 건지**
    모르겠는데 **열리면 안 닫히는데?**」
  - 찾은 것: 주민은 제가 연 문을 `_opened` 에 적어 두었다가 멀어지면 닫는다 (`villager.gd:588 _mind_doors`).
    그런데 **`_opened` 가 저장에 안 담긴다** (`to_dict` 에 없다). 껐다 켜면 목록이 비어 **열어 둔 문을 아무도 안 닫고**,
    주민은 「이미 열린 문」을 건너뛰니 영영 그대로다
  - 기준: **껐다 켜도 주민이 제가 연 문을 닫는다** — `_opened` 를 저장하거나, 저장하지 않고도 닫히게 한다 (테스트)
  - 기준: **사람이 열린 문을 닫을 수 있다** — 겨누고 우클릭하면 닫힌다. 주민이 연 문이든 사람이 연 문이든 같다 (테스트)
  - 기준: 주민이 지나간 뒤 문이 **열린 채로 남지 않는다** — 멀어지면 닫는다 (테스트)
  - 기준: 사람이 일부러 열어 둔 문을 주민이 제 것으로 알고 닫지는 않는다 — 누가 열었나를 가른다 (테스트)
  - 기준: 방 판정은 그대로다 — 문은 열려 있어도 닫힌 것으로 본다 (spec/06_build/rooms.md) (테스트)
  - 기준: (앞 단계 QA) 사람이 짚은 물뿌리개 다툼을 일하는 중 집기로 안 잰다 — test_a_person_takes_from_the_chest_while_the_farmer_works 는 농부가 안 건드리는 밀 칸만 집는다. 사람이 막혔던 것은 물뿌리개였다(G-169 머리말). 손이 거의 찬 농부(밀 · 씨앗 · 비료를 든 채)가 물 주기에 들어가는 동안 사람이 물뿌리개 칸을 Shift+클릭하는 경우를 test_villager_chest_shared.gd 에 더해, 한 번에 집히고 물뿌리개가 상자+손+가방 합 1 을 지키는지 단언하라 (막는 것은 아니다 — 되풀이 자체는 test_villager_can_churn.gd 가 지킨다)
  - 기준: (앞 단계 QA) REFETCH 테스트가 벽시계로 기다린다 — test_seeds_put_back_unused_are_not_taken_again 이 Time.get_ticks_msec 5000ms 를 기다려 「게임 시간 20초」를 가정한다. 프레임이 늦으면 게임 시간이 짧아져 REFETCH 가 풀렸어도 못 볼 수 있다. 게임 시간으로(물리 프레임 수 × delta 합으로) 20초를 재도록 바꿔라
- [x] 5. (앞 단계 QA) 이 변경 전의 저장본에서 주민이 열어 둔 문은 계속 열려 있다
  - 기준: 이 변경 전의 저장본에서 주민이 열어 둔 문은 계속 열려 있다 — 예전 저장본에는 folk 키가 없어서, 불러오면 folk_opened=false 가 되고 주민이 그 문을 사람이 연 문으로 여긴다. 사람이 이미 겪은 그 저장본의 문은 우클릭으로 닫아야 한다. 막는 문제는 아니다. 옮겨 오는 처리를 두지 않을 거라면 spec/01_settings/save.md 의 문 줄에 「G-169.4 전 저장본의 열린 문은 사람이 연 것으로 본다」 한 줄을 적어 두어라

## [x] G-940 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-start (루프가 가져갔다 2026-09-30 16:26)
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. SPARE 상수에 「임시」 표시가 없다 (G-169 단계 2)
  - 기준: SPARE 상수에 「임시」 표시가 없다 — hands.gd 의 `const SPARE := 1` 주석에 **임시** 를 붙여라 (report temporary 와 spec 수치에는 적혀 있다)
- [x] 2. villager-rooms.md 한 줄에 마침 구분이 빠졌다 (G-169 단계 2)
  - 기준: villager-rooms.md 한 줄에 마침 구분이 빠졌다 — 「물뿌리개는 빈 칸에만 든다 (G-169.2) 상자가 차서…」 사이에 「.」 또는 「 · 」 를 넣고 줄이 너무 길면 나눠라

## [x] G-170 컵 — 물을 담아 다닌다 (사람 결정 2026-09-30)
- 차선: 3d-start
- spec: spec/02_player/hunger-thirst.md, spec/05_craft/recipes.md
- **자가 피드백**: 찾은 것을 계속 붙인다 — 깨끗해질 때까지
- 왜 지금인가: 「**물이 없어서 너무 느린데** 일단 물 좀 만들어야겠다」 — 민물이 연못 셋뿐이라 멀면 계속 목마르다
- 사람이 준 규칙 그대로:
  - 컵은 **둘**이다 — **나무컵**과 **철로 만든 컵**
  - 나눈 까닭: **나중에 음료를 만들 때 더 세분화하려고**
  - **나무컵은 3모금 · 철컵은 8모금**
  - **빈 컵은 겹쳐 놓을 수 있다**
  - **물을 포함한 음료가 든 컵은 1개씩만** 들고 다닐 수 있다
- 이름은 **쇠컵**으로 적는다 — 「철 컵」이 어색하다고 했다 (사람이 짚었다 2026-09-30). 다른 이름을 원하면 사람이 말한다
- 물뿌리개가 이미 같은 결이다 — 든 물을 칸에 적는다 (`{item, count, water}`, `watering_can.gd`). **그 길을 그대로 쓴다**
- [x] 1. 컵을 만들고 물을 담는다
  - 기준: **나무컵 · 쇠컵**을 만들 수 있다 — 재료 · 개수는 **임시**로 두고 코드와 report 에 「임시」를 적는다 (테스트)
  - 기준: 컵을 들고 **민물(연못 · 개울)을 우클릭하면 가득 찬다** — 바닷물은 안 된다 (사람이 마시는 규칙과 같다) (테스트)
  - 기준: **든 물이 칸에 적힌다** — 물뿌리개와 같은 꼴 (`{item, count, water}`). 새로 짜지 않고 그 길을 쓴다 (테스트)
  - 기준: **저장하고 불러오면 든 물이 그대로다** (테스트)
  - 기준: **개발용 상자에 물이 가득 든 쇠컵**이 들어 있다 (사람 결정 2026-09-30 — 「물이 계속 부족해서 이동 속도가 너무 느려」).
    개발용 상자는 레시피에서 저절로 채워지지만 **빈 채로** 들어간다 — 물을 채워 넣는 줄을 따로 둔다 (테스트)
  - 기준: 물뿌리개도 개발용 상자에 **물이 든 채로** 들어가는지 보고, 아니면 같이 채운다 — 시험할 때마다 물 뜨러 가지 않게 (테스트)
- [x] 2. 겹쳐 놓기 — 빈 것은 겹치고 든 것은 하나씩
  - 기준: **빈 컵은 겹쳐진다** — 한 칸에 여러 개 (테스트)
  - 기준: **물이 든 컵은 한 칸에 하나뿐**이다 — 겹치지 않는다 (테스트)
  - 기준: 물을 다 마셔 비면 **다시 겹칠 수 있다** (테스트)
  - 기준: 나무컵과 쇠컵은 **서로 겹치지 않는다** — 다른 물건이다 (테스트)
- [x] 3. 모금 수대로 마신다
  - 기준: **나무컵 3모금 · 쇠컵 8모금** — 한 모금에 목마름이 얼마나 차는지는 지금 값(`HungerThirst.SIP`)을 쓴다 (테스트)
  - 기준: 컵을 들고 **우클릭하면 한 모금** 마시고 남은 모금이 준다 (테스트)
  - 기준: **다 마시면 빈 컵이 된다** — 컵이 사라지지 않는다 (테스트)
  - 기준: 빈 컵으로는 마실 수 없다 — 왜 안 되는지 한 줄이 뜬다 (번역 키) (테스트)
  - 기준: 모금 수 · 재료는 **임시**다. `spec/02_player/hunger-thirst.md` 에 컵 표를 적는다 (사람 결정 2026-09-30)
  - 기준: **주민도 컵을 쓰지는 않는다** — 지금은 사람만. 나중에 주민이 마시게 되면 그때 연다 (spec 미정에 한 줄)

## [x] G-171 계단 · 벽에 붙은 상자가 실제 플레이에서 여전히 안 된다 (사람이 찍어 보냈다 2026-09-30)
- 차선: 3d-start
- spec: spec/07_automation/npc-villagers.md
- **자가 피드백**: 찾은 것을 계속 붙인다 — 깨끗해질 때까지
- 사람이 찍어 보냈다: 「**아직도 계단이 있는 건 인식을 못 하는데?** 벽에 딱 붙여서 상자를 두니
  **NPC 가 저기에 도착하고 손이 안 닿는다**고 하네」 — 사진에서 주민은 **건물 밖**에 서 있다
- **고친 것은 이미 들어가 있는데 실제로는 안 된다** — 이것이 이 묶음의 핵심이다:
  - `animal_nav.gd:15` 계단 윗면을 딛는 바닥으로 넣었다 (G-166.1)
  - `villagers.gd:451 chest_spot` 은 「벽에 붙은 상자는 가장 가까운 걸을 땅이 **벽 밖 땅**일 수 있다」를 알고
    손이 닿고 길이 이어지는 자리를 고르게 돼 있다 (G-167.2)
  - 그런데도 사람이 지은 자리에서는 둘 다 안 듣는다. **검사는 통과하는데 플레이는 안 되는 것이 세 번째다**
- [x] 1. 사람이 지은 그대로를 재현한다 — 안 되면 고치지 못한다
  - 기준: **사진 그대로 짓는다** — 비탈에 휠로 올린 토대 · 그 옆에 붙인 계단 · 벽에 붙인 상자 · 문.
    검사 안에서 짓고, 주민이 상자에 닿는지 · 계단으로 오르는지 잰다 (테스트)
  - 기준: **안 되면 거기서 멈추고**, 무엇이 지금 검사들과 다른지 report 에 적는다 —
    걸을 땅이 어디까지 구워졌나 · 계단이 그 안에 들었나 · `chest_spot` 이 무엇을 골랐나. **숫자로** 적는다
  - 기준: **재현되면** 그것이 이 묶음의 검사가 된다. 재현이 안 되면 사람에게 물어 저장본을 받는다 (report 에 무엇이 필요한지 적는다)
  - 기준: 이 단계에서는 고치지 않는다 — 재현하고 밝히기만 한다
- [x] 2. 재현된 것을 고친다
  - 기준: 단계 1 에서 밝힌 까닭을 고친다 — **짐작으로 고치지 않는다**
  - 기준: 주민이 **계단을 밟고 올라** 방에 든다 (테스트)
  - 기준: **벽에 붙은 상자**에 손이 닿는다 — 벽 밖이 아니라 **방 안쪽**에 선다 (테스트)
  - 기준: 「손이 안 닿는다」 · 「길이 막혔다」가 이 짜임에서 안 뜬다 (테스트)
  - 기준: 찍어서 확인한다 — 계단을 오르는 주민 · 벽에 붙은 상자 앞에 선 주민
- [x] 3. 왜 검사는 통과하는데 플레이는 안 됐나
  - 기준: 지금 검사들이 **무엇을 안 보고 있었는지** 적는다 — 평평한 땅만 썼나 · 방이 너무 넓었나 · 상자를 벽에서 떼 놓았나
  - 기준: 그 빈자리를 메우는 검사를 단다 — 다음에 같은 일이 안 나게 (테스트)
  - 기준: (앞 단계 QA) ground_ready 가 매 물리 프레임 길을 묻는다 — Villager._physics_process 가 주민마다 매 프레임 Villagers.ground_ready 를 부르고, 그 안에서 자식 훑기 + NavigationServer3D.map_get_path + map_get_closest_point_owner 를 한다. 한 번 참이 되면 굽기가 바뀔 때까지 참이니, 걸을 땅(AnimalNav) 노드별로 「들었다」를 기억해 두고 새로 구울 때만 지우는 식으로 줄여라. 주민이 늘면 비용이 쌓인다
  - 기준: (앞 단계 QA) 게이트 테스트가 「길이 막혔다」 재현은 못 본다 — test_the_farmer_waits_for_the_walk_ground_before_working 은 게이트를 빼도 막힌 까닭이 [] 로 남고 움직인 거리(0.25m)로만 떨어졌다. report 가 말한 「벽에 처박혀 STUCK」까지는 이 테스트 안에서 안 일어난다. 게이트를 뺐을 때 why_still 이 WHY_CANT_GET_THERE 가 되는 판(예: 더 오래 · 다시 굽기 직후 곧장 일을 시키기)을 찾아 단언을 더하거나, 주석을 실제로 재는 것(기다림)에 맞춰라

## [x] G-992 자동화와 건축을 다듬는다 — 파일 하나씩 (사람 결정 2026-10-11)
- 차선: 3d-start
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다 — 깨끗해질 때까지
- **G-991 의 틀을 그대로 쓴다** (buildings.gd 하나로 해 봤고 통했다 2026-09-27).
  한 단계 = **한 파일**(아주 작은 것은 한 집안) · **훑기와 고치기를 한 단계에 묶는다** —
  세션은 회차마다 기억이 없으니 훑어만 두면 그 회차가 통째로 버려진다 (처음엔 67,000줄을 한 번에 읽히다 두 번 죽었다)
- **이 묶음은 기능을 더하지 않는다.** 겉으로 드러나는 행동이 하나도 달라지지 않아야 한다
- **왜 이 파일들인가**: 자동화(NPC)는 G-157~G-171 동안 **플레이에서 걸릴 때마다 덧댄 자리**가 많다 —
  「검사는 통과하는데 플레이는 안 된다」가 세 번 났고(G-171), 그때 짐작으로 넣은 길과 끝내 고친 길이 겹쳐 남아 있다.
  다음에 올 기계 자동화가 이 위에 올라탄다 (spec/07_automation/npc-villagers.md)
- **안 하는 것** (줄 수로만 고르지 않는다 — G-991 에서 한 잘못):
  - `game/world/terrain/island_*.gd` (1,600여 줄) — **섬 모양은 사람과 같이 다시 빚는다**(spec/00_core/roadmap.md). 지금 다듬으면 버려진다
  - `game/core/debug/shots.gd` · `ui_shots.gd` · `game/world/terrain/test_island.gd` (2,500여 줄) — 찍기 · 검사 발판이라 게임 행동이 아니다
  - `game/addons/*` — 남의 코드
- **넘지 말아야 할 선** (모든 단계에 걸린다):
  - **테스트를 고쳐서 통과시키지 않는다.** 고쳐야 통과하면 리팩토링이 아니라 행동이 바뀐 것이다 —
    되돌리고 report 의 how 에 무엇이 걸렸는지 적는다. **단언의 값 · 개수를 바꾸면 안 된다**
  - **전체 테스트 통과 개수가 줄지 않는다** — 단계마다 앞뒤 숫자를 report 에 적는다
  - **밖에서 부르는 이름은 바꾸지 않는다** — 지금 두 차선이 같이 돌고, 다른 차선의 파일이 그 이름을 부른다.
    안에서만 쓰는 것(`_` 로 시작하는 것)은 마음대로 해도 된다
  - 「사람 결정」이 적힌 줄 · 「임시」 표시를 지우지 않는다
  - 한 단계에 **그 단계의 파일만** — 옆 파일이 눈에 걸리면 고치지 말고 report 에 적는다 (QA 가 단계로 붙인다)
- **고치려는 것이 다른 파일과 이어져 있으면** (사람 물음 2026-10-11 「함수가 다른 파일과 연결돼 있으면 연결해서 고쳐야 할 텐데」):
  - 안에서만 쓰는 이름(`_` 로 시작)은 마음대로 해도 된다 — 밖에서 안 보인다
  - **밖에서 부르는 이름**을 바꾸거나 함수를 옮겨야 하면, **부르는 쪽이 전부 이 묶음에 적힌 파일일 때만** 같이 고친다.
    검사 파일은 어느 차선 것이든 같이 고쳐도 된다 — 단 **단언의 값 · 개수는 못 바꾼다**
  - 부르는 쪽에 **다른 묶음 · 다른 차선 파일**이 하나라도 있으면 **손대지 않는다**. report 에 「경계를 넘는 것」으로
    적는다 — 무엇을 · 어디서 부르나(파일 수) · 왜 한쪽에서 못 하나. 그것으로 이 단계는 통과다.
    **G-994** 가 두 차선이 합쳐진 뒤에 갚는다 (차선 2 가 맨 뒤에 한다) — 사람을 기다리지 않는다
  - 겹친 코드가 **두 파일에 걸쳐** 있으면(같은 일을 둘이 각자 적었다) 역시 G-994 로 넘긴다
  - 재어 본 값 (2026-10-11): `Player` 를 부르는 코드가 44 파일 · `GameRoot` 24 · 가방 집안 32 · 상자 19 ·
    `Rooms` 집안 23 · `BuildPiece` 16 · `Villagers` 17. **이름 하나를 바꾸면 그만큼이 같이 움직인다**
- [x] 1. villagers.gd (843줄) 를 다듬는다
  - 보는 곳: `game/automation/villagers/villagers.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - 주민 전체를 쥔 자리다 — 방 · 상자 · 걸을 땅 · 왜 멈췄나(`WHY_*`)가 한 파일에 다 있다
  - `_geom_dirty` · `_rooms_now` · `chests_in_reach` · `chest_spot` 이 서로 겹쳐 훑는 데가 없나 본다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 2. villager.gd (629줄) 를 다듬는다
  - 보는 곳: `game/automation/villagers/villager.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - `in_reach` · `_mind_doors` · `_physics_process` 가 매 프레임 하는 일이 겹치지 않나 본다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 3. field_care.gd (591줄) 를 다듬는다
  - 보는 곳: `game/automation/villagers/field_care.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - G-164~G-171 이 전부 이 파일을 지나갔다 — 심기 · 물 · 비료 · 거두기 길이 서로 베껴 쓴 데가 있나 본다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) 문을 곧장 지나는 길을 지키는 테스트가 없다 — QA 가 thread_path 의 `out.append_array(_straight_crossings(doors, through, a, b))` 줄을 `pass` 로 바꿔 TEST_DIR=res://tests/automation/villagers 를 돌렸더니 175/175 가 그대로 통과했다 (되돌렸다). 모서리 없이 문구멍을 가로지르는 길(a·b 가 둘 다 DOOR_LANE 밖, 선분이 문 폭/2+0.5 안을 지남)에 thread_path 가 _door_points 두 점을 넣는지 보는 테스트를 tests/automation/villagers/ 에 짜라 — 문 하나를 놓고 path=[문 앞 2m, 문 뒤 2m] 을 넣어 결과가 4점이고 가운데 둘이 문 한가운데 줄(_door_lane x≈0)에 있는지 단언
  - 기준: (앞 단계 QA) villagers.gd ground_ready 가 매 물리 프레임 불린다 — villager.gd _physics_process 가 _fit 인 주민마다 매 틱 _island.ground_ready(self) 를 부르고, 그때마다 home_room · room_center · _navs_at(자식 훑기)를 한 뒤 GROUND_IN 메타를 본다 (구현 report 의 옆 파일 지적 1). villagers.gd 쪽에서 땅이 들었음을 주민마다 기억해 두고 바로 true 를 돌려주게 다듬어라 — 겉 행동은 같아야 한다
- [x] 4. 일거리 여섯 (463줄) 을 다듬는다 — 한 집안이라 같이 본다
  - 보는 곳: `game/automation/villagers/` 의 `gather_work.gd` · `harvest_work.gd` · `job_work.gd` · `mining_work.gd` · `woodcutting.gd` · `stroll.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - 같은 틀을 여섯이 나눠 쓴다 — **같은 일을 여섯 번 적은 데**가 있으면 `job_work.gd` 로 올린다
  - mining_work.gd(8줄) · woodcutting.gd(7줄) 은 껍데기다. 아직 비어 있는 것이면 **지우지 말고** 왜 비었나 적는다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) tick 에서 _needs 를 두 번 부른다 — field_care.gd tick 의 `elif doing == TO_FIELD and not _needs(...)` 를 지난 뒤 _do_in_reach 의 TO_FIELD 갈래가 _needs(_cell, _deed) 를 또 부른다. 행동은 같고 가볍지만, 하나로 줄이려면 _do_in_reach 의 _needs 검사를 stuck 일 때만 하게(`(not stuck or _needs(...))`) 하거나 주석으로 두 번인 까닭을 적어라
- [x] 5. hands.gd · persona.gd · jobs.gd (385줄) 를 다듬는다
  - 보는 곳: `game/automation/villagers/` 의 `hands.gd` · `persona.gd` · `jobs.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - `hands.gd` 는 G-170 에서 칸 수 · 겹치기 · 빈 칸 규칙이 한꺼번에 들어왔다 — 규칙이 한 곳에 모여 있나 본다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) villagers.gd:118 주석이 없는 HerbWork 를 가리킨다 — 「섬이 넣어 준다 (HarvestWork · HerbWork)」인데 HerbWork 라는 클래스는 villagers.gd 그 줄 말고 어디에도 없다. villagers.gd 는 G-992.1 단계 몫이라 이번엔 못 고쳤다. 지금 gathering 을 쓰는 실제 일거리(grep Gathering / island.gathering)로 이름을 바꾸거나 HerbWork 를 지워라
- [x] 6. build_piece.gd (472줄) 를 다듬는다
  - 보는 곳: `game/build/build_piece.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - **G-991 이 여기에 둘을 남겨 뒀다**: 81 · 385 줄 주석이 `Buildings.link_counts` 라 하는데 실제 이름은 `_link_counts` ·
  - `stairs_host` 가 제 손으로 BuildPiece 목록을 또 만든다 (`Buildings._pieces` 와 같은 일)
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 7. game_root.gd (864줄) 를 다듬는다
  - 보는 곳: `game/core/game_root.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - 전부를 잇는 자리다 — 잇는 줄만 남고 규칙은 제 파일에 있어야 한다. 여기로 흘러든 규칙이 있나 본다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) buildings.gd:1116 주석이 낡았다 — 「(BuildPiece.roof_plates 가 제 변에 같은 셈을 한다)」인데 G-992.6 이후 roof_plates 는 이 _edge_plates 를 부른다. 「BuildPiece.roof_plates 도 이것을 쓴다」로 고쳐라 (buildings.gd 를 다루는 단계 몫)
  - 기준: (앞 단계 QA) Buildings 의 _ 이름이 밖에서 불린다 — build_piece.gd 가 Buildings._pieces_of(356·377줄) · _edge_plates(392줄)를 부른다. 밖에서 쓰니 _ 를 뗀 공개 이름이 맞다. 두 파일과 부르는 쪽을 같이 고쳐야 하니 G-994 에서 이름을 바꿔라
- [x] 8. placeable.gd · rooms.gd · beds.gd (600줄) 를 다듬는다
  - 보는 곳: `game/build/placeable.gd` · `game/build/rooms.gd` · `game/build/beds/beds.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - `rooms.gd` 는 NPC 가 방을 보는 길이다 (`find` · `holds` · `id_at` · `_key`)
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 9. (앞 단계 QA) 놓는 곳 뼈대가 네 파일에 겹쳤다
  - 기준: 놓는 곳 뼈대가 네 파일에 겹쳤다 — Beds · Torches · Chests · Stations(build/beds/beds.gd · world/lighting/torches.gd · world/storage/chests.gd · craft/stations/stations.gd)가 미리보기 _ready · refresh_preview · preview · preview_color · aimed_* · plan_spot · aimed_spot · standing/restore 를 거의 줄마다 같이 적었다 (G-992.8 report 「경계를 넘는 것」). 차선을 넘으니 G-994 에서 Placeable 쪽(또는 공통 놓는 곳 뼈대)으로 모으고, 네 파일의 기존 검사가 단언 그대로 통과하는지 본다

## [x] G-993 생활과 화면을 다듬는다 — 파일 하나씩 (사람 결정 2026-10-11)
- 차선: 3d-lane2
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다 — 깨끗해질 때까지
- **G-991 의 틀을 그대로 쓴다** (buildings.gd 하나로 해 봤고 통했다 2026-09-27).
  한 단계 = **한 파일**(아주 작은 것은 한 집안) · **훑기와 고치기를 한 단계에 묶는다** —
  세션은 회차마다 기억이 없으니 훑어만 두면 그 회차가 통째로 버려진다 (처음엔 67,000줄을 한 번에 읽히다 두 번 죽었다)
- **이 묶음은 기능을 더하지 않는다.** 겉으로 드러나는 행동이 하나도 달라지지 않아야 한다
- **왜 이 파일들인가**: 농사 · 사냥 · 낚시 · 가방 · 창은 로드맵 1단계가 한 바퀴 지나간 자리다.
  고친 자리마다 검사가 붙어 1,700개가 넘었으니, 코드만 손대도 깨지는지 바로 드러난다.
  **차선 1(G-992)과 파일이 겹치지 않게 갈라 뒀다** — 같이 돌아도 부딪히지 않는다
- **안 하는 것** (줄 수로만 고르지 않는다 — G-991 에서 한 잘못):
  - `game/world/terrain/island_*.gd` (1,600여 줄) — **섬 모양은 사람과 같이 다시 빚는다**(spec/00_core/roadmap.md). 지금 다듬으면 버려진다
  - `game/core/debug/shots.gd` · `ui_shots.gd` · `game/world/terrain/test_island.gd` (2,500여 줄) — 찍기 · 검사 발판이라 게임 행동이 아니다
  - `game/addons/*` — 남의 코드
- **넘지 말아야 할 선** (모든 단계에 걸린다):
  - **테스트를 고쳐서 통과시키지 않는다.** 고쳐야 통과하면 리팩토링이 아니라 행동이 바뀐 것이다 —
    되돌리고 report 의 how 에 무엇이 걸렸는지 적는다. **단언의 값 · 개수를 바꾸면 안 된다**
  - **전체 테스트 통과 개수가 줄지 않는다** — 단계마다 앞뒤 숫자를 report 에 적는다
  - **밖에서 부르는 이름은 바꾸지 않는다** — 지금 두 차선이 같이 돌고, 다른 차선의 파일이 그 이름을 부른다.
    안에서만 쓰는 것(`_` 로 시작하는 것)은 마음대로 해도 된다
  - 「사람 결정」이 적힌 줄 · 「임시」 표시를 지우지 않는다
  - 한 단계에 **그 단계의 파일만** — 옆 파일이 눈에 걸리면 고치지 말고 report 에 적는다 (QA 가 단계로 붙인다)
- **고치려는 것이 다른 파일과 이어져 있으면** (사람 물음 2026-10-11 「함수가 다른 파일과 연결돼 있으면 연결해서 고쳐야 할 텐데」):
  - 안에서만 쓰는 이름(`_` 로 시작)은 마음대로 해도 된다 — 밖에서 안 보인다
  - **밖에서 부르는 이름**을 바꾸거나 함수를 옮겨야 하면, **부르는 쪽이 전부 이 묶음에 적힌 파일일 때만** 같이 고친다.
    검사 파일은 어느 차선 것이든 같이 고쳐도 된다 — 단 **단언의 값 · 개수는 못 바꾼다**
  - 부르는 쪽에 **다른 묶음 · 다른 차선 파일**이 하나라도 있으면 **손대지 않는다**. report 에 「경계를 넘는 것」으로
    적는다 — 무엇을 · 어디서 부르나(파일 수) · 왜 한쪽에서 못 하나. 그것으로 이 단계는 통과다.
    **G-994** 가 두 차선이 합쳐진 뒤에 갚는다 (차선 2 가 맨 뒤에 한다) — 사람을 기다리지 않는다
  - 겹친 코드가 **두 파일에 걸쳐** 있으면(같은 일을 둘이 각자 적었다) 역시 G-994 로 넘긴다
  - 재어 본 값 (2026-10-11): `Player` 를 부르는 코드가 44 파일 · `GameRoot` 24 · 가방 집안 32 · 상자 19 ·
    `Rooms` 집안 23 · `BuildPiece` 16 · `Villagers` 17. **이름 하나를 바꾸면 그만큼이 같이 움직인다**
- [x] 1. crops.gd (626줄) 를 다듬는다
  - 보는 곳: `game/life/farming/crops.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - `next_day` 가 한 줄기로 길지 않나 · 작물표가 코드에 박혀 있지 않나 본다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 2. farmland.gd (438줄) 를 다듬는다
  - 보는 곳: `game/life/farming/farmland.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 3. animal.gd (561줄) 를 다듬는다
  - 보는 곳: `game/life/hunting/animal.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) report 의 바뀐 곳 개수가 test.log 와 다르다 — report.how 에는 '43 파일 · 312개'라고 적었지만 루프의 .loop/out/test.log 에는 48 파일 · 357개다. 세션이 따로 돌린 changed_G993_2.log 숫자를 옮긴 것으로 보인다. 다음부터는 report 에 루프가 남긴 test.log 숫자를 적어라(막는 것은 아니다)
  - 기준: (앞 단계 QA) Stations.REACH 주석이 Farmland 를 안 적는다 — 이제 Farmland.REACH 가 game/craft/stations/stations.gd:32 의 Stations.REACH 를 그대로 쓰는데, stations.gd:31 주석은 'Harvest.REACH 와 같은 값'만 적었다. 이 값이 밭의 손 닿는 거리도 정한다고 한 줄 더하라. Aim.REACH · Harvest.REACH 와 4.0 을 따로 적은 겹침은 report 대로 G-994 몫
- [x] 4. animal_nav.gd 와 사냥 나머지 (약 900줄) 를 다듬는다
  - 보는 곳: `game/life/hunting/` 의 `animal_nav.gd` · `animal_kinds.gd` · `captives.gd` · `island_animals.gd` · `butchery.gd` · `body_parts.gd` · `sound.gd` · `cage.gd` · `tranq.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - `animal_nav.gd` 는 주민도 같이 쓴다 (G-165~G-168) — **밖에서 부르는 이름을 바꾸면 차선 1 이 깨진다**
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) test_wanders_around_home 이 흔들린다 — game/tests/life/hunting/test_animal_walk.gd:144 의 검사는 짐승의 _rng 가 _ready 에서 randomize() 라(game/life/hunting/animal.gd:151) 돌 때마다 길이 달라, 앞 시도에서 22.61 < 22.5 로 한 번 떨어졌다가 같은 코드로 다시 통과했다. 검사 쪽에서 _put 뒤 animal._rng.seed 를 고정값으로 박아(그 값으로 통과하는지 확인) 결과가 늘 같게 하라. 단언의 값(WANDER_RADIUS + DETOUR) · 개수는 바꾸지 않는다. 같은 파일의 다른 검사도 _rng 에 기대는지 함께 본다
- [x] 5. fishing.gd (495줄) 를 다듬는다
  - 보는 곳: `game/life/fishing/fishing.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - G-158 에서 떡밥 · 영역 · 작은 놀이가 한꺼번에 들어왔다 — 셋이 한 함수에 섞여 있나 본다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) butchery.gd 머리 주석이 같은 말을 두 번 한다 — game/life/hunting/butchery.gd:11 「조준보다 뒤에 붙어 먼저 받는다 (뒤에 붙은 노드가 먼저 받는다, world/terrain/test_island.gd)」. 괄호 안을 「(Godot 은 뒤에 붙은 노드부터 입력을 준다, world/terrain/test_island.gd)」처럼 까닭으로 바꾸거나 괄호를 줄여라. 막는 것은 아니다
  - 기준: (앞 단계 QA) report 의 「수치가 한 비트도 안 바뀐다」는 정확하지 않다 — animal_nav.gd _carve_box 에서 _carve_rect 를 합치며 중간값이 Vector2(float32) 를 거치지 않게 되어 끝자리 비트가 다를 수 있다. 행동·테스트에는 영향 없음(2083/2083). 다음 report 에는 「더하기 순서는 같다」 정도로만 적어라
- [x] 6. player.gd (646줄) 를 다듬는다
  - 보는 곳: `game/player/movement_controls/player.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - 걷기 · 뛰기 · 배고픔으로 느려짐 · 계단이 여기서 만난다 (G-166)
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 7. 가방 집안 (1,069줄) 을 다듬는다
  - 보는 곳: `game/player/inventory_hotbar/inventory.gd` · `game/ui/hud/bag.gd` · `item_slot.gd` · `slot_drop.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - 겹쳐 놓기 · 칸 옮기기 규칙이 네 파일에 흩어져 있나 본다 (G-170 에서 또 손댔다)
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) facing() 주석이 부르는 곳 하나를 빠뜨렸다 — game/player/movement_controls/player.gd:430 「저장 · 앞으로 버리기에 쓴다」인데 game/core/debug/debug_tools.gd:64 (주민을 앞에 세우기)도 facing() 을 부른다. 「저장 · 버리기 · 디버그 주민 세우기에 쓴다」로 넓히거나 「앞쪽이 필요한 곳에 쓴다」처럼 목록을 빼라. 행동과 상관없다
- [x] 8. villager_window.gd (555줄) 를 다듬는다
  - 보는 곳: `game/ui/hud/villager_window.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - G-161 에 한 번에 지어졌다 — 글자가 전부 번역 키인지도 같이 본다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 9. 창 집안 (1,260줄) 을 다듬는다
  - 보는 곳: `game/ui/hud/` 의 `station_window.gd` · `map_window.gd` · `room_window.gd` · `chest_window.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - 창 넷이 **여닫기 · 칸 그리기**를 각자 적고 있나 본다 — 같으면 한 곳으로 올린다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 10. 상자 (580줄) 를 다듬는다
  - 보는 곳: `game/world/storage/chest.gd` · `chests.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - 사람과 주민이 같은 상자를 같이 쓴다 (G-169.3) — 그 규칙이 한 곳에 있나 본다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) round_trip 검사가 임시 폴더를 남긴다 — test.log 끝에 TMPDIR 에 naru_round_trip_* 가 하나 남았다고 나온다. game/tests/settings/save/test_round_trip.gd 에서 만든 임시 폴더를 after_each/after_all 에서 TempDir.clean 으로 지우게 고쳐라 (이번 단계 파일과는 상관없다)
- [x] 11. 전투 (1,196줄) 를 다듬는다
  - 보는 곳: `game/combat/tools_as_weapons/` 의 `attack.gd` · `tool_catalog.gd` · `bullet_trace.gd` · `magazine.gd`. **이 단계의 몫은 여기뿐이다** — 부르는 쪽과 무는 테스트는 읽기만 한다
  - **맨 뒤에 둔 까닭**: 지금 하는 일(자동화 · 생활)이 이 파일을 안 지나간다. 위가 다 끝난 뒤에 한다
  - 기준: 이 파일에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) 상자의 「제 상태 칸」 경로를 지키는 검사가 없다 — 이번에 하나로 합친 chest.gd _take_in 의 SlotMove.plain 이 아닌 갈래(빈 칸 하나에 entry 를 그대로 넣는 곳)를 `return count` 로 바꿔(넣지 않게) tests/world/storage 11파일 106개와 tests/life/hunting 101개를 돌렸더니 전부 통과했다 (되돌렸다). 행동은 줄마다 맞대 같음을 확인했지만 다음에 누가 건드리면 못 잡는다. tests/world/storage/test_chest_shift_click.gd 에 테스트 둘을 더하라: (1) 가방 칸에 제 상태를 안은 것(케이지에 든 짐승처럼 item·count 밖의 키가 있는 칸)을 두고 quick_from_bag → 상자의 첫 빈 칸이 그 사전과 같고(assert_eq 전체 사전) 가방 칸이 비었는지, 같은 item 이 든 칸에 겹치지 않았는지 (2) 상자에 빈 칸이 없을 때 quick_from_bag 은 0, store 는 count 를 돌려주고 아무것도 안 바뀌는지. store(entry) 로도 (1)을 한 번 본다
  - 기준: (앞 단계 QA) 주민 손의 상자 칸 베끼기가 겹친다 — automation/villagers/hands.gd 의 fits_into · takes_some 이 상자 칸을 VillagerHands 로 베끼는 같은 네 줄을 각자 적었다(구현 세션이 report 에 적음). 이 묶음 밖 파일이니 hands.gd 를 맡는 단계에서 안쪽 함수 하나로 올려라 — 이름이 _ 로 시작하면 밖에 안 보인다
- [x] 12. (앞 단계 QA) hands.gd 상자 칸 베끼기 네 줄 겹침
  - 기준: hands.gd 상자 칸 베끼기 네 줄 겹침 — game/automation/villagers/hands.gd 의 fits_into 와 takes_some 이 상자 칸을 VillagerHands 로 베끼는 같은 네 줄을 각자 적었다. 그 네 줄을 _ 로 시작하는 안쪽 함수 하나(예: _copy_of(chest))로 올리고 두 곳에서 부르게 하라. 밖 이름은 그대로 두고, tests/automation/villagers 가 그대로 통과하는지 본다 (G-993 마지막 단계의 기준에 실려 왔지만 이 묶음 밖 파일이라 못 했다)
  - 기준: (앞 단계 QA) round_trip 의 임시 폴더 검사 비켜가기 — game/tests/settings/save/test_round_trip.gd _sweep_stale 이 `var root := OS.get_temp_dir()` 를 `_dir.get_base_dir()` 로 바꿔 test_temp_dir 검사기의 눈을 피했다(주석에 그렇게 적혀 있다). 행동은 같고 root 는 만든 자리가 아니라 오해(검사기의 헛짚음)였지만, 글자만 바꿔 피한 것이라 다음에 같은 꼴(임시 폴더를 둘러보며 TempDir.clean 으로 지우는 자리)이 또 걸린다. tests/test_temp_dir.gd 의 _unerased 가 「그 변수 아래를 TempDir.clean/erase 로 지우는 자리」(root.path_join(..) 을 TempDir.clean 에 넘김)를 지운 것으로 치게 하고, 그 경우를 test_checker_* 에 하나 더해 검사기 자체를 고쳐라 — 단, 기존 단언의 값·개수는 바꾸지 않는다
- [x] 13. (앞 단계 QA) naru_file_logs 가 TMPDIR 에 또 남는다
  - 기준: naru_file_logs 가 TMPDIR 에 또 남는다 — test.log 끝에 「TMPDIR 에 새로 남은 naru_* 1 개: naru_file_logs」가 떴고 full_test_G993_5_after.log 에도 같은 줄이 있다. game/tests/core/tools/test_test_sh_keeps_file_logs.gd 는 _dir 을 OS.get_temp_dir().path_join("naru_file_logs") 로 이름을 고정해 두었고(test.sh 의 걸린 시간 기록 때문) after_each 에서 TempDir.clean 한다 — 그런데도 남는 것은 두 차선이 같은 이름을 같이 쓰다 겹치거나, 판 도중에 남은 것을 test.sh 가 세는 것으로 보인다. 어느 쪽인지 test.sh 의 「새로 남은」 셈과 이 파일의 before_each/after_each 를 보고 가려라 — 차선마다 다른 이름(예: 프로젝트 경로 해시를 붙임)으로 하거나 before_each 에서 남은 것을 먼저 지우게 하고, 그 뒤 전체 돌림 끝에 이 줄이 안 뜨는지 확인하라. 이번 기준과는 상관없다

## [x] G-995 아직 안 본 나머지를 다듬는다 — 차선 1 (사람 물음 2026-10-12)
- 차선: 3d-start
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다 — 깨끗해질 때까지
- 왜 생겼나: 차선 1 이 **G-992 · G-950 을 끝내고 할 일이 없어 멈췄다** (2026-10-12 03:08). 차선 2 는 아직
  G-993 이 넷 남았고 그 뒤에 G-994 가 있다. G-994 는 **G-993 이 합쳐진 뒤**에 해야 하니 차선 1 에 줄 수 없다 —
  대신 **아직 아무 묶음도 안 본 파일 109 개(11,485줄)**를 차선 1 이 맡는다
- **G-993 · G-994 의 파일과 하나도 겹치지 않는다** (2026-10-12 세어 갈랐다) — 같이 돌아도 안 부딪힌다
- G-992 와 같은 틀: 한 단계 = 한 집안 · **훑기와 고치기를 한 단계에** · 기능을 더하지 않는다
- **안 하는 것**:
  - `core/debug/*` (876줄 + shots.gd · ui_shots.gd) — 찍기 · 검사 발판이라 게임 행동이 아니다
  - `world/terrain/*` 의 섬 빚기 (island_* · tree_mesh · vegetation_* · resource_view, 2,200여 줄) —
    **섬 모양은 사람과 같이 다시 빚는다** (spec/00_core/roadmap.md). 지금 다듬으면 버려진다
  - `game/addons/*` — 남의 코드
- **넘지 말아야 할 선** (G-992 와 같다):
  - **테스트를 고쳐서 통과시키지 않는다** — 단언의 값 · 개수를 바꾸면 안 된다
  - **전체 테스트 통과 개수가 줄지 않는다** — 단계마다 앞뒤 숫자를 report 에 적는다
  - 「사람 결정」이 적힌 줄 · 「임시」 표시를 지우지 않는다
  - 한 단계에 **그 단계의 파일만**
- **파일 밖과 이어져 있으면** G-992 와 똑같이 한다: 안쪽 이름(`_`)은 마음대로 · 밖에서 부르는 이름은
  **부르는 쪽이 전부 이 단계의 파일일 때만** 같이 고친다. 아니면 **고치지 말고 report 에 「경계를 넘는 것」으로
  적고 통과한다** — G-994 가 갚는다
- [x] 1. HUD 나머지 19 파일 (1,350줄) 을 다듬는다
  - 보는 곳: `game/ui/hud/` 에서 G-993 이 안 본 것 전부 — butcher_panel · fishing_window · alert_log · target_bars · hotbar · ammo_picker · blueprint_picker · skill_check_ring · need_gauge · equip_slot · stamina_bar · ammo_label · notice · hand_view · health_bar · needs_gauges · chest_timer_label · clock_label · loading_screen. **막대 · 눈금이 여럿인데 같은 일을 각자 적고 있나**를 특히 본다
  - 기준: 이 파일들에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일들과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 2. 설 11 파일 (858줄) 을 다듬는다
  - 보는 곳: `game/ui/menu/` 전부 — menu_root · character_create · appearance_options · pause_menu · world_select · character_select · world_create · character_preview · world_settings_screen · main_menu · settings_screen
  - 기준: 이 파일들에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일들과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) 테스트가 TMPDIR 에 naru_move_* 를 남긴다 — test.log 끝에 naru_move_376950170 이 TempDir.clean 없이 남았다고 나온다(이번 단계 변경과는 무관). game/tests/ui/hud/test_bag_move_slots.gd 가 naru_move_ 임시 폴더를 만든다 — after_each 에서 TempDir.clean 을 부르게 하라
  - 기준: (앞 단계 QA) NeedGauge.sweep() 가 그리기와 따로 논다 — _draw 가 draw_ring 안에서 TAU*ratio 를 다시 셈하고 sweep() 은 이제 테스트만 부른다. test_needs_gauges 의 sweep 단언이 실제 그린 호를 지키지 않는다. draw_ring 에 각도를 넘기거나(sweep() 을 _draw 에서 쓰기) sweep 의 주석에 「테스트용 · 그리기와 같은 셈」이라 적어 어긋남을 막아라
- [x] 3. 등반 (732줄) 을 다듬는다
  - 보는 곳: `game/life/climbing/` 의 climbing.gd(321) · climb_gear · stamina · anchor · grapple
  - 기준: 이 파일들에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일들과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) report 가 test.sh 에 적었다는 말이 없다 — report.how 는 「test.sh 도 그렇게 섞일 수 있다고 적어 둔다」(두 차선이 같은 TMPDIR 을 같이 쓴다)라고 하지만 tools/test.sh 는 바뀌지 않았다. tools/test.sh 의 임시 폴더 검사 근처에 「다른 차선(naru-lane2)이 같은 TMPDIR 에서 돌면 남의 naru_* 가 섞여 보일 수 있다」는 주석을 달거나, 차선마다 TMPDIR 을 따로 주게 하라. 하네스 쪽이라면 사람에게 보낼 일이다
  - 기준: (앞 단계 QA) SavedList 는 delete_id 가 비면 Delete 단추를 안 단다 — 전에는 WorldSelect 가 월드마다 Delete 를 늘 달았다. 지금은 w.id 가 빈 월드에는 Delete 가 없다. WorldData.id 가 늘 차 있으면 상관없지만, 그렇다면 saved_list.gd 의 _make_row 주석에 「빈 칸(새 캐릭터)만 id 가 비어 있다」고 적어 뜻을 못박아 두라
- [x] 4. 개척 섬 · 선착장 (725줄) 을 다듬는다
  - 보는 곳: `game/world/frontier_islands/` 의 dock_builder · dock · island_state · islands
  - 기준: 이 파일들에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일들과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) test_dev_box 가 TMPDIR 에 naru_dev_box_* 를 남긴다 — 이번 전체 그물 test.log 끝에 naru_dev_box_3829645723 이 남았다고 나온다. game/tests/core/debug/test_dev_box.gd 는 after_all 에서 g.free() 뒤 TempDir.clean(self, _dir) 를 부르지만, G-950.2 의 test_dev_box_open 처럼 지운 뒤 게임이 내려가며 다시 저장해 폴더가 되살아나는 것으로 보인다(다른 차선 것일 수도 있으나 이름이 이 테스트 꼴이다). test_dev_box_open.gd 45줄 근처에서 고친 방법과 같이, 게임을 내린 뒤 프레임을 기다리고 나서 TempDir.clean 을 부르거나 지운 뒤 폴더가 없는지 단언하라
- [x] 5. 제작대 · 레시피 (901줄) 을 다듬는다
  - 보는 곳: `game/craft/stations/` 의 stations.gd(382) · crafting_station · station_catalog 와 `game/craft/recipes/` 의 recipe_catalog · hand_craft
  - 기준: 이 파일들에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일들과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) Stations._aim_hit 이 Aim.at 을 베꼈다 — game/craft/stations/stations.gd 의 _aim_hit 이 Aim.at(core 쪽 Aim) 과 줄마다 같은 광선을 다시 적는다. 이 단계 파일이 아니라 report 에 「경계를 넘는 것」으로 남겼다. DockBuilder._aim_hit 처럼 `if player == null or not is_inside_tree(): return {}` 뒤 `return Aim.at(player, get_world_3d().direct_space_state, REACH)` 로 줄이고 tests/craft/stations 를 돌려라
  - 기준: (앞 단계 QA) report 의 테스트 개수 「뒤」가 일부만 적혔다 — G-995.4 report 는 뒤 숫자를 바뀐 곳 그물 33 파일 293/293 으로만 적었다. 다음 단계부터는 루프의 전체 그물이 돌았다면 test.log 의 「Passing Tests」 숫자(이번에는 2099)를 앞 숫자와 나란히 적어라
- [x] 6. 저장 (590줄) 을 다듬는다
  - 보는 곳: `game/settings/save/` 의 round_trip · save_store · world_data · character_data · old_items 와 `game/settings/world_settings/world_settings.gd`
  - 기준: 이 파일들에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일들과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) 트리 밖 테스트가 Aim 길을 안 지난다 — game/tests/craft/stations/test_stations_aim_one_path.gd 의 test_aimed_station_is_empty_handed_without_a_player_or_tree 는 player 가 null 이라 새로 넣은 `not is_inside_tree()` 갈래를 따로 보지 않는다(막는 것은 아니다). player 를 넣고 트리 밖에 둔 Stations 로 aimed_station() 이 null 이고 에러가 없는지 보는 단언을 하나 더 넣어라
- [x] 7. 다치고 죽기 (488줄) 을 다듬는다
  - 보는 곳: `game/combat/damage_death/` 의 death · death_chest · health · mob · fall_damage
  - 기준: 이 파일들에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일들과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 8. 캐기 · 거두기 · 먹기 (834줄) 을 다듬는다
  - 보는 곳: `game/life/gathering/` 의 herbs · gathering · skill_check · `game/life/harvest/harvest.gd` · `game/life/food/` 의 food_catalog · buffs · food
  - 기준: 이 파일들에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일들과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 9. 건축 나머지 (468줄) 을 다듬는다
  - 보는 곳: `game/build/` 의 room_overlay · room_catalog · blueprint · `build/beds/bed.gd` · `game/world/storage/storage_chest.gd`
  - 기준: 이 파일들에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일들과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) report 의 「뒤」 숫자가 부분 그물이다 — how 에 뒤를 362/362(바뀐 곳 37 파일)로 적었지만 이번 test.log 는 전체 2103/2103 였다. 다음 단계부터는 test.log 맨 윗줄이 「전체」면 그 숫자를 「뒤」로 적어라 (앞뒤를 같은 그물로 견준다)
- [x] 10. 플레이어 나머지 (1,128줄) 을 다듬는다
  - 보는 곳: `game/player/` 에서 G-993 이 안 본 것 — inventory_hotbar 의 slot_move · held_item · item_weights · slot_hand · hunger_thirst 의 drinking · hunger_thirst · cup · pickup 의 pickup · dropped_item · equipment · movement_controls 의 player_config · aim · player_state
  - 기준: 이 파일들에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일들과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 11. 세상 나머지 (1,138줄) 을 다듬는다
  - 보는 곳: `game/world/` 의 lighting(torches · torch · torch_light) · day_night(game_clock · day_light) · water(water_surfaces) · map(map_memory) · resources_regrowth(regrowth)
  - 기준: 이 파일들에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일들과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
  - 기준: (앞 단계 QA) report 의 「뒤」 숫자를 같은 그물로 적어라 — 이번에도 앞은 전체 2103, 뒤는 부분 559 로 적었다. 단계 9 QA 지적과 같다. 묶음 마지막 단계처럼 test.log 가 「전체」로 돌 때는 뒤에 그 숫자를 적고, 부분만 돌렸으면 「뒤: 루프 전체 그물로 확인」이라고만 적어 앞뒤를 다른 그물로 견주지 마라 (이번엔 test.log 전체가 2103/2103 이라 기준은 통과)
- [x] 12. 뼈대 나머지 (594줄) 을 다듬는다
  - 보는 곳: `game/core/` 의 path_walk · grid · input(input_actions · pointer) · `game/ui/theme/ui_theme.gd` · `game/life/fishing/fishing_gauge.gd` · `game/life/farming/` 의 field_patch · watering_can · `game/combat/tools_as_weapons/` 의 projectile · aiming
  - 기준: 이 파일들에서 **겹친 코드 · 죽은 코드(아무도 안 부른다) · 너무 긴 함수 · 낡은 주석**을 찾아
    report 에 표로 적고, **그 자리에서 고친다** (찾아만 두지 않는다)
  - 기준: **고칠 것이 없으면 없다고 적는다** — 억지로 만들지 않는다. 무엇을 보고 왜 괜찮다고 봤는지 적는다
  - 기준: **전체 테스트 통과 개수가 줄지 않았다** — report 에 앞뒤 숫자를 적는다
  - 기준: 이 파일들과 부르는 쪽에 걸리는 테스트가 전부 통과한다 (테스트)
- [x] 13. (앞 단계 QA) report 의 뒤 숫자가 바뀐 곳 그물뿐이다
  - 기준: report 의 뒤 숫자가 바뀐 곳 그물뿐이다 — 「뒤」 칸에 1089/1089 (바뀐 곳) 만 적고 전체 숫자는 루프에 맡겼다. 이번엔 루프 전체 그물이 2103/2103 이라 문제없지만, 다음 단계부터는 test.log 의 전체 숫자를 받은 뒤 report 에 「뒤: 2103/2103」처럼 같은 단위로 적어라 (막는 것은 아니다)
- [x] 14. (앞 단계 QA) 씨앗 고정이 물 들어감을 가렸을 수 있다
  - 기준: 씨앗 고정이 물 들어감을 가렸을 수 있다 — 시도 2 · 3 의 전체 그물에서 test_animal_walk.gd:234 test_never_steps_into_water_even_when_sent_there 가 (-256, -246) 근처 물 칸 4~5 개를 밟아 떨어졌다. 이번 단계는 RNG_SEED 993 으로 길을 고정해 통과시켰지만(단언은 그대로), 이는 animal.gd/animal_nav.gd 가 어떤 씨앗에서는 실제로 물에 발을 들인다는 뜻이다. 씨앗을 여러 개(예: 993 외 20 개) 돌려 물 칸을 밟는 씨앗이 있는지 보는 테스트를 test_animal_walk.gd 에 더하고, 밟으면 animal_nav 의 물 막기를 고쳐라 (G-994 쪽 · 사냥 파일)
  - 기준: (앞 단계 QA) 테스트가 짐승 안쪽 이름을 직접 만진다 — test_animal_walk.gd _put 이 animal._rng.seed · animal._rest 를 바로 쓴다. 차선 2 G-993.4 와 똑같은 내용이라 합칠 때 겹치진 않지만, Animal 에 씨앗을 받는 자리(예: spawn 인자나 set_seed())를 두고 테스트가 그것을 부르게 하면 _rest 다시 뽑기를 테스트가 흉내 낼 필요가 없다
- [x] 15. (앞 단계 QA) 다른 짐승 테스트는 옛 땅을 깐다
  - 기준: 다른 짐승 테스트는 옛 땅을 깐다 — test_animal_sound.gd:80 · test_leg_slow.gd:18 이 아직 AnimalNav.around 로 짐승 땅을 굽는다. 게임의 짐승은 이제 AnimalNav.wild 를 쓰니 두 테스트도 AnimalNav.wild(shape, at, ...) 로 바꿔 게임과 같은 땅에서 재게 하라 (단언은 건드리지 말 것)
  - 기준: (앞 단계 QA) spec 사냥 문서에 짐승 땅 대각선이 없다 — spec/04_life/hunting.md 10줄 근처 내비메시 설명에 「짐승의 땅(AnimalNav.wild)은 지형 충돌과 같은 대각선으로 깐다 · 주민 땅(around)은 옛 대각선 그대로 — 사람의 저장본 판이 그 면으로 땅 몸을 세운다」를 한 줄 더하라
  - 기준: (앞 단계 QA) 주민 땅은 실제 충돌과 대각선이 다르다 — report 의 「경계를 넘는 것」: 진짜 섬에서 주민도 Terrain3D 위를 걷는데 AnimalNav.around 는 b–c 대각선이라 47° 둔덕 반쪽을 걸을 땅으로 본다. as_played_room.gd 의 땅 몸을 Terrain3D 대각선(_surface)으로 맞추고 주민도 on_terrain 으로 굽게 하는 일을 G-994 에서 갚아라 (test_villager_as_played 가 지켜볼 것)

## [x] G-994 파일 경계를 넘는 것 — 차선 2 가 맨 뒤에 갚는다 (사람 결정 2026-10-12)
- 차선: 3d-lane2
- **사람을 기다리지 않는다** (사람 결정 2026-10-12 「결정 대기는 왜 또 생기는 거야? 자체적으로 해결하게 만들어」).
  처음엔 `[>]` 로 묶어 사람이 풀게 해 뒀는데, 그건 **순서를 기다리는 일**이지 사람이 정할 일이 아니었다.
  차선 2 는 할 일이 더 많아(11 단계 대 8 단계) **늘 나중에 끝난다**. 그 차선에 맡기고 맨 뒤에 두면,
  루프가 새 묶음을 시작하기 전에 `sync_lanes` 가 차선 1(G-992)을 이미 합쳐 온다 — 순서가 저절로 맞는다
- 왜 따로 두나: 코드가 **서로 깊이 엉켜 있다** (2026-10-11 재어 봤다) — `Player` 를 부르는 코드가 44 파일 ·
  `GameRoot` 24 · 가방 집안 32 · `Rooms` 집안 23 · `BuildPiece` 16 · `Villagers` 17.
  파일 **안**에서 끝나는 다듬기는 두 차선이 같이 해도 안 부딪히지만,
  **이름을 바꾸거나 두 파일에 걸친 겹침을 합치는 일**은 한쪽 차선에서 할 수 없다
- **무엇을 갚나**: G-992 · G-993 의 단계마다 report 에 적어 둔 「경계를 넘는 것」이다.
  report 는 커밋 글에 그대로 남으니 `git log --all --grep='경계를 넘는 것'` 으로 모아 올 수 있다.
  G-950(루프가 찾은 것)에 「아니면 G-994 로」로 붙은 것도 같이 본다
- **자가 피드백**: 한 단계에 한 가지만 갚고, 남은 것은 단계로 계속 붙인다 — 다 갚을 때까지
- **넘지 말아야 할 선** (G-992 와 같다. 더하여):
  - 이름을 바꾸면 **부르는 쪽을 전부** 같이 고친다 — grep 으로 하나도 빠뜨리지 않았음을 report 에 적는다 (검사 파일까지)
  - 한 단계에 **한 가지만** 바꾼다 (이름 하나 · 겹침 하나) — 여럿을 묶으면 떨어졌을 때 무엇이 깼는지 모른다
  - **갚을 것이 없으면 없다고 적고 통과한다** — 억지로 만들지 않는다
- [x] 1. 경계를 넘는 것을 모으고, 가장 값나가는 하나를 갚는다
  - 기준: `git log --all --grep='경계를 넘는 것'` 과 G-950 단계를 훑어 **표로 모은다** —
    무엇이 · 어느 두 파일에 걸쳐 있나 · 부르는 쪽이 몇 파일인가 (report 에)
  - 기준: 그중 **하나**를 골라 그 자리에서 갚는다. 왜 그것을 먼저 골랐는지 적는다
  - 기준: 남은 것은 report 에 **다음 단계 후보**로 남긴다 (QA 가 단계로 붙인다)
  - 기준: 전체 테스트 통과 개수가 줄지 않았다 — 앞뒤 숫자를 report 에 적는다
- [x] 2. build_piece.stairs_host 가 Buildings._pieces 와 같은 일을 한다
  - 기준: **G-991 이 적어 둔 것이다** (2026-09-27 report) — `build_piece.gd stairs_host` 가 제 손으로
    BuildPiece 목록을 또 만든다. `buildings.gd` 는 G-991 에서, `build_piece.gd` 는 G-992 단계 6 에서
    다듬었지만 **둘에 걸친 겹침은 어느 쪽도 손댈 수 없었다**
  - 기준: 한 곳으로 모은다. `Buildings._pieces` 는 안쪽 이름이니, 밖에 내놓을 이름이 필요하면 하나만 만든다
  - 기준: `build_piece.gd` 81 · 385 줄 주석의 `Buildings.link_counts` 를 실제 이름으로 고친다 (G-992 단계 6 에서 안 됐으면)
  - 기준: 전체 테스트 통과 개수가 줄지 않았다 (앞뒤 숫자)
  - 기준: (앞 단계 QA) 손 닿는 거리 4.0 을 Aim.REACH 하나로 — G-994 다음 단계(report 표 3번). Stations.REACH · Harvest.REACH 가 각자 4.0 을 적었다(부르는 쪽 Stations.REACH 10 · Aim.REACH 12 · Harvest.REACH 4 파일). 두 상수를 지우거나 `const REACH := Aim.REACH` 로 잇고, 부르는 쪽을 grep 으로 전부 고친 뒤 report 에 빠짐 없음을 적어라. tests/world/storage/test_reach_temporary_notes.gd 가 「임시」 주석을 보니 그 주석이 Aim.REACH 한 곳에 남는지 같이 고쳐라
  - 기준: (앞 단계 QA) 낚시 · 마시기의 수면 찾기 겹침 — G-994 그 다음 단계(표 5번). life/fishing/fishing.gd 의 aimed_water 와 player/hunger_thirst/drinking.gd 가 광선 → STEP 걸음 수면 찾기를 각자 적었다. 거리(CAST_RANGE ↔ Aim.REACH)만 인자로 받는 한 함수로 모으고, 두 거리에서 같은 수면을 찾는 테스트를 더해라
- [x] 3. 남은 것을 갚는다
  - 기준: 단계 1 의 표에서 아직 안 갚은 것을 **하나** 갚는다
  - 기준: 남은 것이 없으면 **없다고 적고 통과한다**
  - 기준: 전체 테스트 통과 개수가 줄지 않았다 (앞뒤 숫자)
  - 기준: (앞 단계 QA) test_from_empty_hands 가 부하 아래 들쭉날쭉하다 — 앞 시도 기계 검사에서 5-2 「물가 가까이 걸어왔다」 중 사람이 섬 밖 (-171.6, -197.3) 으로 걸어 나가 y=-63879 까지 떨어진 뒤 game/tests/craft/test_from_empty_hands.gd 909줄에서 research == null 로 스크립트 오류가 났다(이번 회차 · 전체 그물 · 단독 재실행에선 통과). 물가로 걷는 도우미가 목표를 못 찾으면 섬 밖으로 걷지 않게 멈추거나 섬 안 거리로 막고, 909줄 앞에 research 가 null 이면 assert 로 떨어지게(스크립트 오류 대신) 고쳐라
  - 기준: (앞 단계 QA) TMPDIR 에 naru_villager_* 가 남아 있다 — /var/folders/13/.../T/naru_villager_3480380615. 어느 주민 테스트가 TempDir.clean 을 안 부르는지 찾아 지우게 하라(앞 시도엔 naru_dead_save_* 도 남았다)
- [x] 4. (앞 단계 QA) 걷기 도우미의 멈춤을 지키는 검사가 없다
  - 기준: 걷기 도우미의 멈춤을 지키는 검사가 없다 — test_from_empty_hands.gd _walk_to 의 WALK_ASTRAY 멈춤은 이번에 들쭉날쭉함을 다시 일으켜 보지 못해 실제로 걸린 적이 없다. 다음에 이 파일이 떨어지면 「물가 가까이 걸어왔다」 단언의 _walked 글(선 자리)을 보고 돌아서기가 덜 먹은 것인지 확인하라. 돌아서기(_face 류)가 끝난 뒤 바라보는 방향을 단언하면 원인 쪽을 바로 막을 수 있다
- [x] 5. (앞 단계 QA) naru_file_logs 가 TMPDIR 에 또 남았다
  - 기준: naru_file_logs 가 TMPDIR 에 또 남았다 — 이번 전체 실행 test.log 끝에 「TMPDIR 에 새로 남은 naru_* 1 개: naru_file_logs」가 찍혔다(G-993.13 에서 갚은 것이 다시 났다). naru_file_logs 를 만드는 테스트(grep -rn naru_file_logs game/)를 찾아 after_each/after_all 에서 TempDir.clean 을 반드시 부르게 하고, 실패 경로(중간 return · await 중 끝남)에서도 지워지는지 보라

## [x] G-950 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-start (루프가 가져갔다 2026-10-12 02:47)
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- **경계를 넘는 것은 여기서 고치지 않는다** (사람 결정 2026-10-12): 고치려면 다른 묶음 · 다른 차선의 파일을
  같이 건드려야 하는 것이면, **고치지 말고 report 에 「경계를 넘는 것」으로 적고 통과한다** — 그것으로 이 단계는 끝이다.
  G-994(차선 2 가 맨 뒤에 한다)가 두 차선이 합쳐진 뒤에 갚는다. 억지로 고치려다 아홉 번 떨어져
  결정 대기로 넘어가는 것이 더 나쁘다
- [x] 1. villagers.gd set_home 의 halt 세 줄이 villager._halt_work 와 겹… (G-992 단계 2)
  - 기준: villagers.gd set_home 의 halt 세 줄이 villager._halt_work 와 겹친다 — set_home 안 `var w := v.work(); if w != null: w.halt()` 는 villager.gd 의 _halt_work 와 같은 일이다. 모으려면 villager 에 공개 이름이 하나 필요하다 (부르는 쪽이 이 묶음 파일뿐인지 보고, 아니면 G-994 로)
- [x] 2. 테스트가 TMPDIR 에 naru_dev_box_open_* 를 남긴다 (G-992 단계 6)
  - 기준: 테스트가 TMPDIR 에 naru_dev_box_open_* 를 남긴다 — test.log 끝에 naru_dev_box_open_2099798754 가 남았다고 나온다. dev box 를 여는 테스트(tests/core/debug/test_dev_box_everything.gd 쪽)가 만든 임시 폴더를 TempDir.clean 으로 지우게 고쳐라

## [x] G-950 루프가 찾은 것 — 스스로 갚는다
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. 창 위 여백 TOP_MARGIN 을 창 셋이 각자 (G-994 단계 1)
  - 기준: 창 위 여백 TOP_MARGIN 을 창 셋이 각자 — G-994 그 다음 단계(표 9번). chest_window · villager_window 등 ui 창이 같은 여백을 따로 적는다. ui 쪽 한 곳(레이아웃 상수)으로 모으고 test_window_layout 류에서 셋이 같은 값을 쓰는지 단언해라
- [x] 2. tools/test.sh 가 두 차선에서 같은 TMPDIR (G-994 단계 1)
  - 기준: tools/test.sh 가 두 차선에서 같은 TMPDIR — G-994 그 다음 단계(표 10번, G-950.2). 차선 1 · 2 가 동시에 돌면 임시 경로가 겹친다. .loop/lane 값으로 TMPDIR 아래 차선별 하위 폴더를 쓰게 고쳐라
- [x] 3. 차선 1 합친 뒤 남은 경계 다섯 (G-994 단계 1)
  - 기준: 차선 1 합친 뒤 남은 경계 다섯 — G-994 맨 뒤 단계들(표 2 · 4 · 6 · 7 · 8). sync_lanes 가 G-992 · G-995 를 이 줄기에 합쳐 온 뒤 하나씩 단계로: 2 놓는 곳 뼈대 → Placeable · 4 Buildings._pieces_of/_edge_plates 공개 이름 · 6 hands.gd 상자 칸 베끼기 ↔ Chest.store · 7 gather_work/field_care → JobWork · 8 ButcherPanel._holds_gun ↔ Attack.holds_gun
- [x] 3. 숫자로 베낀 여백은 못 잡는다 (G-950 단계 1)
  - 기준: 숫자로 베낀 여백은 못 잡는다 — test_windows_share_one_top_margin 은 offset_top 값이 같은지와 TOP_MARGIN · chest_top 글자만 본다. 창 스크립트가 center.offset_top = 44.0 처럼 숫자를 직접 적어도 지금 값이 같으면 통과한다. test_window_layout.gd 의 그 테스트에서 네 창 스크립트의 offset_top 대입 줄이 UiTheme.WINDOW_TOP 을 쓰는지(예: 'offset_top =' 가 든 줄마다 'WINDOW_TOP' 포함) 단언을 더하라
- [x] 4. 알림 기록 위 여백 44 가 따로 있다 (G-950 단계 1)
  - 기준: 알림 기록 위 여백 44 가 따로 있다 — game/ui/hud/alert_log.tscn:9 의 offset_top = 44.0 이 Window chest_top(44) 과 같은 값을 손으로 적었다. 같은 안내 줄 자리를 비우는 것이면 UiTheme.WINDOW_TOP 으로 이어 주거나, 다른 까닭이면 그 까닭을 주석으로 적어라
- [x] 3. 놓는 곳 넷의 파일 끝에 빈 줄이 둘 남았다 (G-950 단계 3)
  - 기준: 놓는 곳 넷의 파일 끝에 빈 줄이 둘 남았다 — beds.gd · torches.gd · chests.gd · stations.gd 끝이 \n\n\n 이다. 맨 끝 함수를 지우고 남은 빈 줄이다. 각 파일 끝을 줄바꿈 하나로 줄여라
- [x] 4. placeable.gd PLACERS 주석에 같은 말이 두 번 있다 (G-950 단계 3)
  - 기준: placeable.gd PLACERS 주석에 같은 말이 두 번 있다 — game/build/placeable.gd 26~27줄. 「(넷은 뼈대 Placer 를 잇는다)」와 바로 아랫줄 「1칸짜리 설치물을 놓는 넷…은 뼈대(build/placer.gd)가 무리에 넣는다」가 겹친다. 26줄 끝의 괄호를 지워라
- [x] 5. 넷의 REACH 상수가 이제 쓰이지 않는다 (G-950 단계 3)
  - 기준: 넷의 REACH 상수가 이제 쓰이지 않는다 — Chests/Torches/Beds.REACH(:= Stations.REACH)와 Stations.REACH 는 조준에 안 쓰이고 Placer 가 Aim.REACH 로 겨눈다. test_reach_temporary_notes 가 그 주석을 읽어서 남겼다. 차선 2 와 합친 뒤 그 검사를 Placer 쪽으로 옮기고 네 상수를 지우든지, 주석에 「겨누는 거리는 Placer(Aim.REACH) — 이 값은 기록용」이라고 적어라
- [x] 6. 총 잣대가 아직 한 곳 더 있다 (G-950 단계 3)
  - 기준: 총 잣대가 아직 한 곳 더 있다 — game/life/harvest/harvest.gd:82 가 _tools().shoots(held) 로 손에 든 것이 총인지 제 손으로 본다. Attack.gun_in_hand 와 같은 잣대인지 보고, 같으면 그것을 부르게 하고 test_butchery 처럼 소스 검사를 하나 더하라
- [x] 3. _is_skeleton 이 매번 res:// 를 다시 훑는다 (G-950 단계 4)
  - 기준: _is_skeleton 이 매번 res:// 를 다시 훑는다 — game/tests/build/test_placers_comment.gd 의 _is_skeleton 은 부를 때마다 _scan(res://) 과 _joiners()(또 _scan) 를 돈다. test_comment_lists_every_placer 의 for 안에서 클래스마다 두 번씩 훑는다. 루프 밖에서 extends_of 를 한 번 모아 뼈대 집합을 만들고 그것을 쓰게 줄여라
- [x] 3. Stations.REACH 주석이 「기록용」이라며 쓰인다고 한다 (G-950 단계 5)
  - 기준: Stations.REACH 주석이 「기록용」이라며 쓰인다고 한다 — game/craft/stations/stations.gd:33 은 「이 값은 기록용이다 (건물 · 도축 · 선착장 짓기가 이 값을 따라 쓴다)」라고 적었다. 실제로 Buildings.REACH(build/buildings.gd:70 → 770줄 Aim.at) · Butchery.REACH(life/hunting/butchery.gd:21, Captives 도 따른다) · DockBuilder.REACH(world/frontier_islands/dock_builder.gd:29 → 202줄 Aim.at)가 이 값을 진짜 조준 거리로 쓰니 「기록용」은 제작대를 놓는 조준에만 참이다. 문구를 「제작대를 놓는 조준은 Placer(Aim.REACH) 가 한다 — 이 값은 건물 · 도축 · 선착장 짓기가 거리로 쓴다」처럼 고치고, test_placer.gd 의 문구 단언은 stations.gd 만 그 문구로 따로 보게 바꿔라 (넷 다 같은 문구를 요구하는 지금 단언을 갈라야 한다)
- [x] 1. test_placer 의 함수 이름이 이제 반만 맞다 (G-950 단계 3)
  - 기준: test_placer 의 함수 이름이 이제 반만 맞다 — game/tests/build/test_placer.gd 의 test_every_placer_reach_says_it_is_only_a_record 는 이제 Stations.REACH 가 「기록용이 아니다」를 단언하는데 이름은 넷 다 기록용이라고 한다. test_every_placer_reach_doc_says_who_aims 처럼 둘을 다 담는 이름으로 바꿔라 (막는 것은 아니다)

## [x] G-200 시설 열둘로 가른다 — 뼈대 (사람 결정 2026-10-12)
- 차선: 3d-start
- spec: spec/05_craft/facilities.md (사람이 표로 준 것 · 픽스) · crafting-stations.md · recipes.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- 왜 먼저인가: 지금 레시피 57 개 중 **49 개가 작업대 하나**에 몰려 있다. 뒤의 묶음이 전부 이 위에 얹힌다
- **수량 · 시간은 다 임시다** — 「개수 같은 건 상관 없다, 지금 있는 대로 해도 된다」 (사람 2026-10-12).
  QA 는 개수가 spec 표와 다른 것을 흠으로 잡지 않는다
- [x] 1. 재료에 갈래(태그)를 넣는다
  - 기준: 레시피 재료에 `@곡물` 처럼 **태그**를 적을 수 있다. 그 갈래의 아이템 **아무 하나**로 만들어진다 (테스트)
  - 기준: 태그 목록이 **데이터 파일 한 곳**에 있다 — 한 줄 더 적으면 코드를 안 고치고 갈래가 넓어진다 (테스트)
  - 기준: 제작대 창이 태그 재료를 **갈래 이름**으로 보여 주고, 가진 아이템 중 무엇이 쓰이는지 보인다
  - 기준: `@음식` 처럼 가공된 것을 받는 태그에는 **1단 가공까지만** 담는다 — 3단 한도를 넘지 않는다 (테스트)
  - 기준: 글자는 전부 번역 키다 (ko · en)
- [x] 2. 레시피를 시설별 파일로 가른다
  - 기준: `game/craft/recipes/` 아래 **시설마다 파일 하나**가 되고, 지금 `recipes.json` 한 덩이가 그리로 간다 (테스트)
  - 기준: 파일을 더 넣으면 코드를 안 고치고 그 시설 레시피가 생긴다 — 지금 `recipes.json` 의 약속과 같다 (테스트)
  - 기준: 레시피 **개수 · 재료가 하나도 달라지지 않는다** (옮기기만 한다 — 앞뒤 개수를 report 에)
  - 까닭: 뒤의 묶음 다섯이 저마다 레시피를 더한다. 한 파일이면 **차선 둘이 같은 파일에서 부딪힌다** (2026-10-12 에 19 파일로 겪었다)
  - 기준: (앞 단계 QA) recipes.json 임시 설명에 떡밥이 아직 「밀 1」 — game/craft/recipes/recipes.json 의 _temporary 에 「낚싯대(목재 3 → 1) · 떡밥(밀 1 → 2덩이)도 임시다」가 남아 있다. 바로 뒤 문장이 @grain 으로 바꿨다고 하지만 앞 괄호가 낡았다. 「떡밥(아무 곡물 1 → 2덩이)」로 고쳐라
- [x] 3. 시설 열둘을 세운다
  - 기준: 용광로 · 제재소 · 맷돌 · 방직기 · 염전 · 양조통 · 증류기 · 축사 · 훈연기 · 약제 작업대 · 제작대 · 조리대가
    **다 놓이고 열린다** (stations.json 에 열둘 · 저마다 만드는 레시피 한 줄) (테스트)
  - 기준: 지금 작업대에 몰린 49 개가 **제 시설로** 옮겨 간다 — 옮긴 표를 report 에 적는다
  - 기준: **석공대 · 도축대는 만들지 않는다** (사람 결정) — 석재 블록은 용광로, 도축은 짐승과 상호작용
  - 기준: 연구대는 그대로 남는다 (개척 섬 해금)
  - 기준: 빈손에서 **집 · 침대 · 밥까지 가는 길이 끊기지 않는다** — 한 바퀴 검사가 통과한다 (테스트)
  - 기준: (앞 단계 QA) 데이터 파일 설명이 아직 recipes.json 을 가리킨다 — game/craft/stations/stations.json 의 _fields(「레시피가 정한다(recipes.json)」) · _recipes(「제작대마다 recipes.json 에 … 한 줄 있어야 한다」)와 game/world/frontier_islands/islands.json 의 _fields(「unlock_item=… 연구대에서만 만든다, recipes.json」)가 지운 파일 이름을 말한다. 각각 「craft/recipes/ 의 시설 파일」(islands.json 은 research_bench.json)로 고쳐라. 지키려면 game/ 아래 .json · .gd(테스트 폴더의 임시 파일 이름 test_recipe_tags.gd 는 빼고)에 'recipes.json' 이 안 나오는지 보는 단언을 test_recipes_by_station.gd 에 더해라
  - 기준: (앞 단계 QA) 맨손 파일의 임시 설명이 「도」로 시작한다 — game/craft/recipes/hand.json 의 _temporary 첫 문장이 「재료 개수 · 나오는 개수도 임시다」인데 앞 문장(5초 임시)을 맨손 파일에서는 뺐기 때문에 「도」가 받을 말이 없다. 「재료 개수 · 나오는 개수는 임시다」로 고쳐라 (막는 것은 아니다)
- [x] 4. 벌집을 농사장에 둔다
  - 기준: 벌집을 **놓을 수 있고**, 농사장(farm) 방 안에서 시간이 지나면 **밀랍**이 나온다 (테스트)
  - 기준: 벌집은 **방 종류를 정하는 핵심 오브젝트가 아니다** — 밭 + 벌집인 방이 **여전히 농사장**이다 (테스트)
  - 기준: 농사장 밖의 벌집은 아무것도 내지 않는다 (테스트)
  - 참고: 사람의 뜻은 「방이 시설을 돌리는 조건이기도 하다」다 — 농사장 **상위 방**이 생기면 벌집을 그 방의 핵심으로 올린다 (spec 에 적혀 있다)
  - 기준: (앞 단계 QA) 새 시설 일곱이 빈 창으로 열린다 — 맷돌 · 방직기 · 염전 · 양조통 · 증류기 · 축사 · 훈연기는 레시피 파일이 없어 놓고 열면 줄 없는 창이다. 이번 기준(놓이고 열린다)은 채웠지만, 빈 창일 때 「아직 만드는 것이 없다」 같은 안내 줄(번역 키 ko · en)을 station_window.gd 에 띄우거나, 이 묶음의 뒤 단계에서 각 시설 파일에 첫 레시피를 넣을 때 test_twelve_facilities.gd 에 「열둘 모두 for_station(kind) 가 비지 않는다」 단언을 더해 빈 시설이 남지 않게 지켜라
- [x] 5. (앞 단계 QA) 벌집 제자리 재기가 매초 섬 전체를 훑는다
  - 기준: 벌집 제자리 재기가 매초 섬 전체를 훑는다 — Stations._process 가 YIELD_STEP(1초)마다 Buildings.measure_rooms 로 건축물 전부의 방을 다시 잰다 (벌집이 하나라도 있으면). 건축물이 많아지면 1초마다 끊김이 생길 수 있다. 막는 것은 아니다 — 건축물이 바뀔 때만 방을 다시 재도록 Buildings 쪽 캐시(지었다/부쉈다 신호로 무효화)를 쓰거나, 큰 기지에서 measure_rooms 시간을 재는 테스트를 더해 둘 것

## [x] G-201 재료가 나오게 — 작물 · 약초 · 광석 (사람 결정 2026-10-12)
- 차선: 3d-lane2
- spec: spec/05_craft/facilities.md · 04_life/farming.md · gathering.md · mining.md
- **어디에 나는가는 사람이 나중에 정한다** — 지금은 임시 자리다. 「나오게 하면 되는 거 아니야?」 (사람 2026-10-12).
  그 단계가 보는 것은 **심어 자라나 · 캐지나**까지다. 섬의 어디에 깔리나는 묻지 않는다
- 결정: **(가) 태그 기준을 이 묶음에서 뗀다** (2026-10-12). 태그 파일은 G-200 단계 1 이 만들고 **차선 1 이 쥔다** —
  두 차선이 같은 파일을 고치면 부딪힌다 (2026-10-12 에 19 파일로 겪었다). 그래서 **담는 일은 차선 1 이 G-204 에서** 한다.
  이 묶음은 **작물 · 약초 · 광석이 나게만** 한다. 태그에 안 담겨도 이 묶음은 통과다
- [x] 1. 작물 열넷
  - 기준: 보리 · 쌀 · 감자 · 옥수수 · 토마토 · 사과 · 포도 · 사탕수수 · 카카오 · 찻잎 · 원두 · 생강 ·
    목화 · 아마를 **밭에 심어 거둔다** (테스트). 자라는 날수 · 수량은 임시다
  - 기준: 작물표가 **데이터 한 곳**에 있다 — 지금 `crops.gd` 의 `CROPS` 는 코드에 박혀 있다. 한 줄로 늘어나게 한다 (테스트)
  - 기준: 씨앗을 얻는 길이 **있다** — 지금 임시 규칙(제작대에서 목재로)을 그대로 늘린다
- [x] 2. 약초 넷
  - 기준: 알로에 · 인삼 · 쑥 · 박하가 **야생으로 나고 캔다** (테스트) — 지금 벨라도나 · 만드라고라 · 투구꽃과 같은 식
- [x] 3. 광석 여덟
  - 기준: 구리광석 · 주석광석 · 금광석 · 은광석 · 석탄 · 모래 · 석회석 · (점토는 **없다** — 사람이 뺐다)
    을 **캔다** (테스트). 석회는 이제 석회석에서 나온다
  - 기준: 캐는 바위가 다시 자란다 (03_world/resources-regrowth.md 의 약속)
  - 기준: 석회를 쓰던 곳(비료)이 끊기지 않는다 — 석회석 → 석회 길이 이어진다 (테스트)
- [x] 4. (앞 단계 QA) 석회 레시피가 지워질 파일에 들어갔다
  - 기준: 석회 레시피가 지워질 파일에 들어갔다 — game/craft/recipes/recipes.json 에 {"key": "lime", station smelter, limestone 2 → lime 2} 한 줄을 더했는데, 차선 1(G-200.2)이 이 파일을 시설별 파일로 갈라 지웠다. 합칠 때 이 줄(과 _temporary 의 석회 문장)을 용광로/제련로 쪽 시설 파일(smelter.json 등)로 옮겨야 비료 사슬이 안 끊긴다 — 합친 뒤 test_fertilizer_farm_plot.gd test_limestone_becomes_lime_becomes_fertilizer 가 그것을 지킨다
  - 기준: (앞 단계 QA) 시설 이름이 용광로 · 제련로로 섞였다 — game/life/harvest/harvest.gd DROPS 위 주석 · island_resources.gd 주석은 「용광로」, 레시피 _temporary · 테스트 · spec/04_life/mining.md 는 「제련로(smelter)」라 한다. spec/05_craft/facilities.md 의 표 이름은 용광로다. 한쪽으로 맞춰라(코드 주석을 facilities.md 와 같게, 혹은 「용광로(smelter)」로 둘 다 적기)

## [x] G-202 용광로 · 제재소 (사람 결정 2026-10-12)
- 차선: 3d-start
- spec: spec/05_craft/facilities.md
- [x] 1. 용광로 — 주괴 · 숯 · 석재 · 유리
  - 기준: 목재 → **숯**, 구리 · 주석 · 금 · 은 광석 → 주괴, 구리괴 + 주석괴 → **청동괴**, 철괴 + 석탄 → **강철괴** (테스트)
  - 기준: **돌 + 연료 → 석재 블록** (사람 결정 2026-10-12 — 석공대를 없애고 여기로 옮겼다). 지금 작업대의 석재 블록 줄은 지운다 (테스트)
  - 기준: 모래 + 석회 → **유리** (테스트)
  - 기준: **연료 칸이 있다** — 숯이나 석탄을 넣어야 도는 레시피가 있다 (강철괴는 석탄만) (테스트)
  - 기준: 벽돌 · 시멘트 · 슬래그는 **만들지 않는다** (사람이 뺐다)
- [x] 2. 제재소 — 판자 · 기둥 · 자루
  - 기준: 목재 → **판자 · 기둥 · 자루** (테스트). 개수는 임시다
  - 기준: 판자를 쓰던 곳(건축 재료)이 끊기지 않는다 — 건축 비용은 그대로 둔다 (테스트)
  - 기준: **종이 · 도면 줄은 건드리지 않는다** — 종이를 없앴으니 도면은 지금처럼 맨손 목재 1 이다 (사람 결정 2026-10-12)
  - 기준: 기둥은 **아이템**이고 건축의 기둥 조각과 **이름이 겹치지 않는다** (테스트)
  - 기준: (앞 단계 QA) 철괴만 연료 없이 굽는다 — smelter.json 의 iron_ingot(과 sulfur)은 @fuel 이 없어 구리 · 주석 · 금 · 은괴와 어긋난다. spec 표의 용광로 「연료: 초반 숯 → 나중 석탄」과도 맞지 않는다. 까닭이 「빈손 테스트 길을 그대로 두려고」인데 석재 블록처럼 test_from_empty_hands.gd 에 숯 굽는 줄과 소나무를 더하면 풀린다 — iron_ingot 에 "@fuel": 1 을 달고 빈손 테스트의 목재 셈(숯 = 철괴 수만큼)을 고친 뒤, facilities.md 수치 줄 · smelter.json _temporary 의 「철괴 · 유황은 연료 없이」 문장을 지워라
  - 기준: (앞 단계 QA) 청동괴가 이미 3단이다 — 숯(1단)을 연료로 세면 연료 든 주괴는 2단, 청동괴는 3단이라 청동으로 무엇을 만들면 3단 한도를 넘는다(facilities.md 수치 줄에 적혀 있다). 연료를 깊이에 세지 않는 쪽이 spec 의 뜻(원자재에서 몇 번 가공하나)에 맞다 — 깊이 검사(tests/craft/recipes/test_recipes.gd 쪽)가 @fuel 을 깊이 계산에서 빼게 하고, 그걸 05_craft/recipes.md 깊이 규칙에 한 줄로 적어라

## [x] G-203 방직 · 양조 · 증류 · 염전 · 훈연 (사람 결정 2026-10-12)
- 차선: 3d-start
- **차선을 하나로 몰았다** (사람 결정 2026-10-13 「차례대로 만들면 되는데 지금 나눠서 못 만든다 이 말인가」 → **그렇다**).
  시설 묶음은 저마다 **같은 공용 데이터 파일**을 만진다 — `i18n/*.po`(이름) · `weights.json`(무게) ·
  `stations.json`(시설 목록) · `tags.json`(갈래) · `crops.json`. 레시피는 시설별 파일로 갈라 막았지만(G-200.2)
  이름 · 무게 · 태그는 **아이템 하나에 네 파일**이라 가를 수 없다. 그래서 두 차선이 무엇을 하든 같은 줄에서 만난다
  - 2026-10-13 에 실제로 났다: `g/G-203` 이 없어진 `recipes.json` 에 방직기를 적어 두어 따라잡기가 부딪히고 루프가 멈췄다.
    손으로 풀어 합쳤다 (`loom.json` 으로 옮김, 3d-lane2 dff125dc) — 전체 2223/2223 통과
  - **차선 2 는 꺼 둔다.** 겹치지 않는 일(UI · 리팩토링)이 생기면 그때 다시 둘로 나눈다
- spec: spec/05_craft/facilities.md
- **물은 재료에서 뺀다** — 「물 1」이 들던 줄은 물 없이 둔다. 사람이 따로 설계한다 (배관 없이 **시설 범위로 물이 들어오는** 쪽)
- [x] 1. 방직기 — 실 · 천 · 밧줄 · 옷
  - 기준: 양털 · 목화 · 아마 → **실**, 실 → **천**, 아마 → **밧줄**, 천 → **옷** (테스트)
  - 기준: 밧줄을 쓰던 곳(활 · 낚싯대 · 사다리)이 끊기지 않는다 (테스트)
  - 기준: 색 천 · 방한복 · 돛은 **만들지 않는다** (사람이 뺐다)
  - 기준: (앞 단계 QA) 실 레시피가 재료마다 한 줄씩 셋이다 — facilities.md 「태그」 절은 줄을 베껴 늘리지 않는다고 한다. 이 차선에 태그(G-200)가 아직 없어 임시로 둔 것이고 _temporary 에 적혀 있다. 태그가 들어오는 묶음에서 thread_from_wool/cotton/flax 를 @섬유류 태그 한 줄로 합치고 test_loom.gd 의 _recipe(only_input) 조회를 태그 재료도 받게 고쳐라
  - 기준: (앞 단계 QA) 차선을 합칠 때 cotton · flax 줄이 겹칠 수 있다 — G-201(다른 차선)도 ITEM_cotton · ITEM_flax 번역과 weights.json 무게를 더한다. 합친 뒤 game/i18n/ko.po · en.po · weights.json 에서 msgid ITEM_cotton/ITEM_flax 와 "cotton"/"flax" 키가 하나씩만 남았는지 grep -c 로 보고 겹치면 하나를 지워라
- [x] 2. 양조통 · 증류기
  - 기준: 곡물 · 과일 · 꿀로 **술**이 나온다 (맥주 · 포도주 · 럼 · 청주 · 보드카 · 사이다 · 벌꿀주 · 약주) (테스트)
  - 기준: 술 → **독주**(위스키 · 브랜디 · 숙성 럼) · `@술` → **알코올** (테스트)
  - 기준: 술을 마시면 **스트레스가 내려간다** — 값은 spec 표대로(임시) (테스트)
  - 기준: 식초는 **만들지 않는다** (사람이 뺐다)
  - 기준: (앞 단계 QA) 3d-lane2 에 cotton · flax 가 두 번씩 남아 있다 — 구현 세션 보고대로 3d-lane2 의 ko.po · en.po(msgid ITEM_cotton/ITEM_flax)와 weights.json("cotton"/"flax")에 각 2개씩 있다. 차선 2 를 다시 켜거나 그 줄기를 이 줄기에 합칠 때 grep -c 로 보고 하나씩만 남겨라 (이 줄기는 깨끗하다 · 막는 것은 아니다)
- [x] 3. 염전 — 물가에 선다
  - 기준: 염전을 **물가에 놓으면** 시간이 지나 **소금**이 나온다 (테스트). 바닷물은 아이템이 아니다
  - 기준: 물가가 아니면 아무것도 내지 않고, **왜 안 되는지 보인다** (테스트)
  - 참고: 물 영역을 재는 길은 낚시에 이미 있다 (`game/life/fishing/`)
- [x] 4. 훈연기 — 초반을 넘기는 음식
  - 기준: 고기 · 생선 + 소금 → **훈제 음식** (테스트). 배고픔만 채운다 — **버프는 없다** (사람 결정 2026-10-12)
  - 기준: 창자 + 고기 → **소시지**, 소시지 → 훈제 소시지 (테스트) — 표에서 끊겨 있던 자리다
  - 기준: 부산물 **기름**이 나온다 (약제의 연고가 쓴다)
- [x] 5. (앞 단계 QA) 창자를 사슴에서 바로 낸다
  - 기준: 창자를 사슴에서 바로 낸다 — G-204 와 어긋난다: plan/list.md G-204 기준은 「도축 → 내장, 작업대에서 내장 → 창자」인데 이번에 game/life/hunting/animal_kinds.gd 사슴 BODY 에 gut 1 을 바로 넣었다. G-204 단계에서 사슴 몸통 산출을 gut → 내장(offal 등)으로 바꾸고 작업대 레시피 내장 → 창자를 더한 뒤 test_smoker.gd test_butchering_a_deer_gives_gut 를 「도축 → 내장 → 작업대 → 창자」로 고쳐라
  - 기준: (앞 단계 QA) 기름 이름 테스트가 무게만 본다 — test_smoker.gd test_oil_has_names_and_weight 는 이름에 names 를 걸었지만 ItemWeights 만 확인한다. ko.po · en.po 에 ITEM_oil · ITEM_gut · ITEM_sausage · ITEM_smoked_* 가 있는지 TranslationServer 로 확인하는 단언을 더하거나 테스트 이름을 test_oil_has_weight 로 고쳐라
  - 기준: (앞 단계 QA) 훈제 소시지가 소시지보다 가볍다 — weights.json 에서 sausage 0.4 · smoked_sausage 0.3 이다. 훈제 고기·생선(0.4)과 맞추든 소시지와 같게 두든, 임시 값이라도 뒤집혀 있지 않게 고쳐라

## [x] G-204 축사 · 도축 산출 (사람 결정 2026-10-12)
- 차선: 3d-start
- spec: spec/05_craft/facilities.md · 04_life/ranching-breeding.md · hunting.md
- [x] 1. 태그에 새 재료를 담는다 — 태그 파일은 이 차선 것이다
  - 기준: G-201 이 낸 작물 열넷 · 약초 넷을 `@곡물` · `@채소` · `@과일` · `@약초` 에 담는다 (테스트)
  - 기준: 담은 뒤 그 태그를 쓰는 레시피가 **새 재료로도** 만들어진다 (예: 보리로 맥주 · 쌀로 청주) (테스트)
  - 기준: 아직 줄기에 안 들어온 재료는 **빼고 적고**, 무엇을 뺐는지 report 에 적는다 — 없는 이름을 태그에 적지 않는다
  - 까닭: 태그 파일을 두 차선이 고치면 부딪힌다. **만든 차선(1)이 쥐고 담는다** (사람 결정 2026-10-12)
- [x] 2. 축사 — 먹이를 넣으면 나온다
  - 기준: 닭 · 오리 · 염소 · 양 · 돼지 · 소 · 물소 · 말을 축사에 두고 **먹이(`@곡물`)**를 넣으면
    알 · 우유 · 양털이 주기마다 나온다 (테스트). 주기 · 수량은 임시다
  - 기준: **풀은 없다** (사람이 뺐다) — 초식 가축도 `@곡물` 을 먹는다
  - 기준: **똥을 싼다**. 똥 → 비료가 되고, **NPC 가 치운다** (테스트)
  - 기준: 같은 종 둘 + 먹이 → **새끼** (테스트). 걸리는 시간은 임시다
  - 기준: (앞 단계 QA) 차선을 합치면 태그 테스트가 떨어진다 — G-201(3d-lane2)이 3d-start 에 합쳐지면 보리 · 옥수수 · 감자 · 토마토 · 생강 · 알로에 · 인삼 · 쑥 · 박하가 「있는 아이템」이 되어 test_tag_new_materials.gd 의 test_present_new_materials_are_in_their_group 이 일부러 떨어진다(설계대로). 합친 직후 단계로 game/craft/recipes/tags.json 에 grain += barley, corn · 새 vegetable 갈래 = potato, tomato, ginger(TAG_vegetable ko「아무 채소」/en「Any vegetable」) · herb += aloe, ginseng, mugwort, mint 를 더하고 _temporary 문구와 spec/05_craft/facilities.md 44행 아래 「줄기에 들어오면 더한다」 줄을 지워라. 같은 단계에서 축사 먹이 @grain 이 보리도 받는지 테스트에 더하라
- [x] 3. 도축 산출을 늘린다 — 도축대는 없다
  - 기준: 짐승과 **상호작용해 그 자리에서** 도축한다 (지금 코드 그대로 — 도축대를 만들지 않는다)
  - 기준: 짐승마다 **고기 · 가죽 · 뼈 · 뿔 · 깃털 · 내장**이 spec 표대로 나온다 (테스트)
  - 기준: 가죽 → **가죽끈**, 내장 → **창자** (작업대에서 — 도축대가 없으니) (테스트)
  - 기준: 뼈 도구는 **만들지 않는다** (사람이 뺐다). 뼈 · 뿔 · 이빨 · 밀랍 · 가죽끈은 **산출물로만** 둔다 — 쓰임은 사람이 나중에
  - 기준: (앞 단계 QA) 합친 뒤 태그 일을 맡을 단계가 아직 없다 — 3d-lane2 가 3d-start 에 합쳐진 직후 tags.json 에 보리 · 옥수수 · 채소 갈래 · 약초 넷을 더하는 일이 이 묶음 안에서는 일어나지 않았다(조건이 안 섰다). 합친 직후 test_tag_new_materials.gd 와 test_barn.gd test_barley_is_feed_once_it_is_in_the_trunk 가 같이 떨어진다 — 그때 앞 단계 QA 줄 그대로(grain += barley, corn · vegetable = potato, tomato, ginger + TAG_vegetable ko/en · herb += aloe, ginseng, mugwort, mint · tags.json _temporary 와 facilities.md 44행 아래 '줄기에 들어오면 더한다' 줄 삭제) 고치면 둘 다 풀린다. G-204 의 다음 단계나 합친 뒤 첫 단계 기준으로 옮겨 두어라
  - 기준: (앞 단계 QA) 축사 창에 가축 · 먹이를 넣는 길이 없다 — Stations.put_animal · put_feed 는 있으나 station_window.gd 에 단추가 없어 사람이 게임 안에서 축사에 가축을 들이거나 먹이를 넣을 수 없다(이번 기준 밖, report 에도 적혀 있다). 여덟 가축이 들판에 나지 않는 것과 함께 다음 단계에 축사 창의 '가축 들이기 · 먹이 넣기' 단추와 그 테스트를 더하라
- [x] 4. (앞 단계 QA) 합친 뒤 태그 넷 더하기가 갈 단계가 없다
  - 기준: 합친 뒤 태그 넷 더하기가 갈 단계가 없다 — G-204 마지막 단계라 옮길 '다음 단계'가 없다. 3d-lane2(G-201)가 3d-start 에 합쳐진 직후 첫 단계 기준으로: game/craft/recipes/tags.json 에 grain += barley, corn · 새 vegetable 갈래 = potato, tomato, ginger(TAG_vegetable ko「아무 채소」/en「Any vegetable」) · herb += aloe, ginseng, mugwort, mint 를 더하고 tags.json _temporary 의 해당 문구와 spec/05_craft/facilities.md 의 「줄기에 들어오면 더한다」 줄을 지운다. 그러면 test_tag_new_materials.gd test_present_new_materials_are_in_their_group 와 test_barn.gd test_barley_is_feed_once_it_is_in_the_trunk 가 같이 풀린다
  - 기준: (앞 단계 QA) hunting.md 사슴 예 줄이 낡았다 — spec/04_life/hunting.md 수치의 「부위 산출물 예(사슴: … 몸통 고기 4 · 가죽 2, 다리 없음)」 줄이 바로 위 새 「도축 산출표」(사슴 몸통 고기 4 · 가죽 2 · 뼈 2 · 내장 1)와 어긋난다. 그 줄에서 사슴 예 괄호를 지우고 「개수는 위 표 · 한 부위에 체력 가득만큼 들어가면 …」 규칙만 남겨라. animal_kinds.gd 12~14행 주석(「사슴만 예가 적혀 있다: … 몸통 고기 · 가죽」 · 「동물 목록 · 부위 산출물이 미정」)도 같은 까닭으로 표를 가리키게 고쳐라
  - 기준: (앞 단계 QA) 축사 가축 여덟을 게임 안에서 얻을 길이 좁다 — 닭 · 오리 · 염소 · 양 · 돼지 · 소 · 물소 · 말이 들판에 나지 않아 축사 단추와 깃털 산출이 실제 플레이에서는 닿지 않는다. spec/04_life/ranching-breeding.md 또는 hunting.md 「미정」에 '가축 여덟이 어디서 나는가(스폰 자리 · 수)' 한 줄을 적어 두어라 — 값을 지금 고르라는 것이 아니다

## [x] G-205 조리대 · 약제 작업대 (사람 결정 2026-10-12)
- 차선: 3d-start
- spec: spec/05_craft/facilities.md
- **물은 재료에서 뺀다** (G-203 과 같다)
- [x] 1. 조리대 — 기초 · 본격 · 고급 · 보존식 · 음료
  - 기준: spec 표의 음식이 **다 만들어진다** — 배고픔 · 기분 값은 표대로(임시) (테스트)
  - 기준: `@채소` · `@곡물` · `@과일` · `@고기` 태그로 **종류를 가리지 않고** 만들어진다 (테스트)
  - 기준: 통조림은 `@음식` 중 **1단 가공까지만** 받는다 (3단 한도)
  - 기준: 버터는 **만들지 않는다** (사람이 뺐다)
  - 기준: **기분과 스트레스가 한 축인지** 정해 코드에 하나로 둔다 — 음식은 기분 +, 술은 스트레스 − 로 적혀 있다.
    NPC 설계에는 스트레스 하나뿐이다 (spec/07_automation/npc-villagers.md). 어느 쪽이든 **한 수치**로 모으고 report 에 적는다
- [x] 2. 약제 작업대 — 약 · 마취탄
  - 기준: spec 표의 약이 다 만들어진다 (진통제 · 연고 · 화상 연고 · 마취제 · 독약 · 강한 독 · 해독제 ·
    기력 회복제 · 꿀약 · 해열제 · 소화제 · 붕대 · 약 붕대) (테스트)
  - 기준: 마취탄 세 종(만드라고라 · 벨라도나 · 투구꽃)이 **지금 자리에서 여기로 옮겨 온다** (테스트)
  - 기준: 약의 효과가 실제로 듣는다 — 적어도 **HP · 스트레스 · 독**은 (테스트)
  - 기준: (앞 단계 QA) 합친 뒤 조리대를 채소 끼움 없이 다시 봐야 한다 — 이 줄기에서는 @vegetable 이 비어 구운 채소 · 스튜 · 볶음밥 · 잔치 요리 · 절임 채소 다섯이 게임 안에서는 아직 못 만들어진다. test_cook_table.gd before_each 가 갈래가 비면 VEGETABLES 를 넣고, test_tag_new_materials.gd 는 빈 갈래 레시피를 건너뛴다. 3d-lane2 합친 직후 단계(G-204.4 앞 단계 QA 줄)에서 tags.json 에 vegetable 을 더할 때, test_cook_table.gd 의 VEGETABLES 끼움과 test_tag_new_materials.gd 의 _has_empty_tag 건너뛰기를 지우고 그대로 통과하는지 본다
  - 기준: (앞 단계 QA) test_cook_table 의 MENU 가 foods.json 을 그대로 베꼈다 — 사람 표가 없어 「표대로」의 기준이 테스트 상수뿐이다. 사람이 조리대 음식표를 주면 facilities.md · foods.json · cook_table.json · MENU 를 한꺼번에 바꿔야 한다. MENU 위 주석에 「사람 표가 오면 이 상수도 바꾼다」 한 줄을 더해 두어라
- [x] 3. (앞 단계 QA) 주민이 약을 먹는 곳이 게임에 없다
  - 기준: 주민이 약을 먹는 곳이 게임에 없다 — Villager.take_medicine 은 테스트에서만 불린다(eat · drink 도 같다). 스트레스 효과는 함수로는 듣지만 게임 안에서 주민이 스스로 약 · 음식을 먹는 길은 없다. spec/07_automation/npc-villagers.md 에 「주민이 언제 먹나」가 미정으로 적혀 있지 않으면 미정 줄에 한 줄 적어 두어라 (값을 지금 고르라는 것이 아님)
  - 기준: (앞 단계 QA) 독의 자투리 피해가 저장되지 않는다 — Medicine._poison_carry(1 HP 미만 몫)는 저장본에 안 담기는데 spec/01_settings/save.md 의 「안 담는다」 목록에는 없다(코드 주석에만 까닭이 있다). save.md 안 담는다 목록에 「독 자투리 피해(1 HP 미만) — 1초도 안 되는 몫」 한 줄을 더하라

## [x] G-206 제작대 — 무기 · 갑옷 · 도구 · 가구 (사람 결정 2026-10-12)
- 차선: 3d-start
- spec: spec/05_craft/facilities.md · 02_player/equipment.md · 08_combat/tools-as-weapons.md
- [x] 1. 자루를 쓰는 도구 · 금속 사슬
  - 기준: 곡괭이 · 도끼 · 망치 · 톱 · 칼이 **자루 + 주괴**로 만들어진다 (테스트)
  - 기준: 구리 · 청동 · 철 · 강철 무기가 만들어진다 (단검 · 창 · 검 · 도끼) (테스트)
  - 기준: **총 열여섯 자루는 그대로 둔다** — 네 계통 · 계통마다 네 단은 사람 결정이다 (2026-09-22, spec 의 표).
    사람 표의 사냥총 · 파이프 권총 · 돌격소총 · 경기관총 · 대물 저격총은 그 열여섯 안에 있는 이름이다
  - 기준: 활 · 화살이 만들어진다 (판자 + 밧줄 · 판자 + 깃털) (테스트)
  - 기준: 못 · 나사 · 렌즈 · 나침반은 **만들지 않는다** (사람이 뺐다. 렌즈는 **나중에** 스코프 · 망원경에 유리와 함께 쓴다)
- [x] 2. 갑옷 — 장비 칸이 모자라다
  - 기준: 가죽 · 청동 · 철 · 강철 갑옷 · 투구 · 각반 · 방패가 만들어진다 (테스트)
  - 기준: **입을 자리가 있다** — 지금 장비 칸은 신발 · 바지 · 상의 · 모자 · 귀걸이 · 목걸이 · 반지 · 가방 여덟이고
    **갑옷 칸이 없다**. 갑옷 · 투구 · 각반을 상의 · 모자 · 바지에 끼울지, 칸을 더할지 정해 **report 에 적고** 그대로 짓는다 (테스트)
  - 기준: 방어가 실제로 듣는다 — 입으면 받는 피해가 준다 (테스트)
- [x] 3. 가구 · 저장
  - 기준: 나무 통 · 상자 · 침대 · 의자 · 테이블 · 나무컵 · 사다리가 만들어진다 (테스트)
  - 기준: **맨손 상자는 그대로 둔다** (맨손 레시피는 셋 — 사람 결정 2026-09-21). 판자 상자를 더하려면 그 결정을 먼저 뒤집어야 하니 **건드리지 않는다**
  - 기준: 사다리로 오를 수 있다 (04_life/climbing.md 와 어긋나지 않는다) (테스트)
  - 기준: (앞 단계 QA) spec equipment.md 들여쓰기가 깨졌다 — spec/02_player/equipment.md 규칙 절에서 새 「- **방어** …」 줄을 「장비칸은 여덟」 줄의 하위 줄들 사이에 끼워 넣어, 그 뒤의 「가방 창의 인벤토리 칸 위에 있다 (사람 결정 2026-09-26)」 · 「장비칸은 저장본에 담긴다」 하위 줄이 이제 「방어」 아래에 매달린 것처럼 읽힌다. 「- **방어** …」 두 줄을 「장비칸은 저장본에 담긴다 …」 하위 줄 다음(「장비칸」 묶음이 끝난 뒤)으로 옮겨라. 사람 결정 줄 자체는 지워지지 않았다
- [x] 4. (앞 단계 QA) 사다리를 돌려 놓을 수 없다
  - 기준: 사다리를 돌려 놓을 수 없다 — game/build/furniture/furniture.gd 의 사다리 판은 늘 X 축을 따라 선다(report temporary 에 적힘). Z 축으로 뻗은 벽 · 위층 바닥 가장자리에 대려면 판 방향이 맞지 않는다. 건축물 돌리기(game/build/buildings.gd 의 rotate)와 같은 길로 놓을 때 방향을 고르게 하고, Furniture.ladder_of 의 local 판정이 돌린 판에서도 맞는지 보는 테스트(돌린 사다리 앞에서 W → y>1.5)를 더하라. spec/05_craft/facilities.md 가구 줄에 「방향은 돌리지 않는다(임시)」가 없으니 적어 두거나 고쳐라


## [x] G-904 루프가 찾은 것 — 스스로 갚는다
- 결정: **단계 4 는 사람이 손으로 했다** (2026-10-13). 차선 둘을 합치는 일에 매여 있었고 세션은 git 을 못 쓴다 —
  합치면서 같이 했다: `tags.json` 의 `pending` · `_pending` 을 지우고(보리 · 옥수수 · 채소 셋 · 약초 넷이 갈래에 들어갔다),
  `test_recipe_tags` 의 단언을 「pending 이 없다 + 그 재료가 갈래에 든다」로 바꿨다.
  채소 끼움(test_cook_table)과 `_has_empty_tag` 건너뛰기도 같이 지웠다. 단계 5 · 6 은 그대로 남았다
- 차선: 3d-start (루프가 가져갔다 2026-10-13 09:33)
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. G-201 앞부터 있던 「제련로」가 남았다 (G-201 단계 4)
  - 기준: G-201 앞부터 있던 「제련로」가 남았다 — 시설 이름이 facilities.md 와 다르다. G-201 이전부터 있던 주석·단언 문구에 「제련로」가 남아 있다: game/ui/hud/station_window.gd 13·27행, game/craft/stations/stations.json _recipes, game/core/debug/ui_shots.gd 545행, game/tests/ui/layout/test_window_layout.gd 358~390행, game/tests/ui/hud/test_station_window.gd 11~106행, game/tests/craft/test_from_empty_hands.gd 11·20·64·87·89·115·146·895·911~998행. 이번 기준 밖이고, 차선 1 이 쥔 파일과 부딪히지 않게 두 차선이 합친 뒤에 「용광로(smelter)」로 바꿔라. 글자만 바꾸면 된다
- [x] 2. 새 원자재의 얻을 길이 없다 (G-202 단계 1)
  - 기준: 새 원자재의 얻을 길이 없다 — 석탄 · 모래 · 구리/주석/금/은 광석이 섬 어디에도 안 나서 지금 게임에서는 강철괴 · 유리 · 주괴 넷을 만들 수 없다. 자원 배치는 사람이 정하지만(facilities.md 미정) 이 아이템들이 「얻을 길 없음」임을 미정 줄에 이름으로 적어 두어라 (막는 것은 아니다)
- [x] 3. spec 사냥 문서에 창자가 없다 (G-203 단계 4)
  - 기준: spec 사냥 문서에 창자가 없다 — spec/04_life/hunting.md 의 부위 산출 예(사슴 몸통 고기·가죽)에 이번에 더한 창자(임시)를 적어 두어라 (facilities.md 에만 적혀 있다)
- [x] 4. pending 목록을 합친 뒤 지울 길이 없다 (G-204 단계 4)
  - 기준: pending 목록을 합친 뒤 지울 길이 없다 — tags.json 의 "pending" · "_pending" 은 3d-lane2 가 합쳐지면 쓸모가 없어지지만(「다 들어온 뒤에는 지워도 된다」) 누가 지울지 정해진 곳이 없다. 3d-lane2 가 3d-start 에 합쳐진 뒤 tags.json 에서 pending · _pending 두 키를 지우고, test_recipe_tags.gd test_game_tag_file_lists_the_lane2_materials_as_pending 의 pending 단언을 「pending 이 없거나 비었다」로 바꿔라 (RecipeTags 의 pending 읽기 기능과 test_pending_name_joins_its_group_only_when_the_item_exists 는 남겨도 된다)
- [x] 5. pending 판정이 무게표만 본다 (G-204 단계 4)
  - 기준: pending 판정이 무게표만 본다 — RecipeTags.load_from 은 ItemWeights.table() 만으로 '있는 아이템'을 가르지만 test_tag_new_materials.gd 의 _exists 는 무게표와 ko.po ITEM_<이름> 둘을 본다. 무게만 먼저 들어온 이름이 갈래에 드는 틈이 있다 — 기준을 하나로 맞추거나(주석에 까닭을 적거나) 두 정의가 같은 함수를 쓰게 하라
- [x] 6. 사다리 판정 범위가 정사각이라 회전 판정이 안 걸린다 (G-206 단계 4)
  - 기준: 사다리 판정 범위가 정사각이라 회전 판정이 안 걸린다 — Furniture.ladder_of 의 범위가 |local.z|≤LADDER_TOUCH(0.6) · |local.x|≤size.x*0.5+0.2(0.6) 로 같아서, 90° 돌린 판에서는 판의 제 좌표(affine_inverse)로 재든 월드 좌표 차(at - ladder.global_position)로 재든 결과가 같다. 실제로 ladder_of 를 월드 좌표 차로 깨 봐도 test_furniture.gd 17개가 다 통과했다. 지금 동작은 맞지만 test_an_unturned_ladder_side_on_is_not_climbed 의 주석(「ladder_of 가 판 좌표로 잰다」)과 report 의 설명은 사실과 다르다 — 그 테스트가 지키는 것은 판의 몸(충돌)이 돌았는지다. 주석을 고치거나, 판 좌표 판정을 지키고 싶으면 ladder_of 를 직접 불러 돌린 사다리 기준으로 판 앞 0.5m · 옆 0.1m 와 판 옆 0.5m · 앞 0.1m 같이 판정이 갈리는 점(LADDER_TOUCH 와 옆 여유를 다르게 둘 때)을 단언하는 단위 테스트를 더하라

## [x] G-207 제작대 창이 끊긴다 · 맷돌이 비었다 (사람이 플레이해 보고 2026-10-13)
- 차선: 3d-start
- spec: spec/05_craft/crafting-stations.md · facilities.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- 사람이 플레이해 보고 짚었다: 「제작대에 렉 걸리듯이 **스크롤 내리면 뚝뚝 끊김**. 제작대 말고 다른 스크롤이 필요한
  시설에도 똑같이 적용되는 거면 이유를 찾고 **다 고칠 것**」 (2026-10-13)
- **까닭은 이미 재서 알아냈다** (2026-10-13):
  `station_window.gd:266 _process` 가 창이 보이는 동안 **매 프레임** `_refresh()` 를 부르고,
  그 안에서 **줄마다** `Stations.can_start()` → `plan()` → `room_chests()` 를 탄다.
  `room_chests` 는 매번 `get_nodes_in_group(건축물 전부)` + **`Rooms.find()`(방 홍수 채우기)**를 새로 돈다.
  `_refresh_uses()` 도 줄마다 `plan()` 을 또 부른다 — **제작대 레시피가 110줄이라 한 프레임에 방 탐색이 220번**이다
  - 창 코드의 주석은 「줄이 마흔 몇이라 세는 값이 싸다」고 적혀 있다 — 그 짐작이 깨졌다. 줄이 110 이 되고,
    같은 방 상자(G-?) 가 들어오며 「세는 값」 안에 방 탐색이 들어왔다
  - **시설 열한 곳이 같은 창을 쓴다** — 용광로 · 조리대 · 약제 … 전부 같은 길이라 한 곳을 고치면 다 고쳐진다
- [x] 1. 창이 매 프레임 방을 다시 재지 않는다
  - 기준: 창이 열려 있는 동안 `Rooms.find` · `get_nodes_in_group` 이 **한 프레임에 한 번을 넘지 않는다** (테스트로 센다)
  - 기준: **값이 바뀌면 그 프레임에 보인다** — 가방이 바뀌거나(`Inventory.changed`) 같은 방 상자가 바뀌거나
    (`Chest.changed`) 건축물이 서거나 부서지면 줄의 회색이 따라 바뀐다 (테스트)
  - 기준: 110 줄짜리 제작대를 열고 **한 프레임에 드는 시간을 재서** 앞뒤 숫자를 report 에 적는다
  - 기준: 보이는 것이 하나도 달라지지 않는다 — 회색 줄 · 쓰이는 재료 표시 · 남은 시간 · 버퍼가 그대로다 (테스트)
  - 참고: 주민 쪽에 같은 틀이 이미 있다 — `villagers.gd` 의 `_geom_dirty` · `_rooms_now`
- [x] 2. 긴 목록이 가볍게 흐른다
  - 기준: **안 보이는 줄은 세지 않는다** — 스크롤에 걸린 110 줄 가운데 화면에 든 것만 다시 본다 (테스트)
  - 기준: 사람이 스크롤을 내려 **끊기지 않는다** — 찍어서 확인하고, 한 프레임 시간을 report 에 적는다
  - 기준: 줄을 다시 세우지 않는다 (`_rebuild`) — 스크롤 자리가 튀지 않는다 (테스트)
  - 기준: (앞 단계 QA) 같은 프레임 상자 놓기 테스트가 수 비교를 안 지킨다 — station_window.gd _room_chests 의 `placed != _chests_placed` 를 지워도 test_a_chest_placed_in_the_room_shows_in_that_frame 를 포함해 7개가 다 통과했다. await 뒤 창의 _process 가 그 프레임에 아직 안 돌아 _chests_frame 이 앞 프레임이라 어차피 다시 잰다. 지키려면 테스트에서 _window()._refresh() 를 한 번 먼저 불러 이 프레임에 재게 한 다음 _chest(...) 를 놓고 다시 _refresh() 해 눌리는지 단언하라 (또는 수 비교가 쓸모없다고 보면 지우고 주석을 맞춰라)
- [x] 3. 맷돌이 비어 있다 — 밀가루 · 설탕 · 향신료
  - 기준: **맷돌에 레시피가 없다** (2026-10-13 계통도를 뽑다 알아챘다) — 시설은 서는데 만드는 것이 하나도 없다.
    묶음을 쪼갤 때 맷돌이 어느 묶음에도 안 들어갔다
  - 기준: 밀 → **밀가루** · 사탕수수 → **설탕** · 향신료 열매 → **향신료** 를 맷돌에서 만든다 (테스트). 개수는 임시다
  - 기준: 밀가루를 쓰는 음식이 **밀가루를 거친다** — 지금 밀빵은 밀 3 으로 바로 만들어진다 (spec 표는 밀가루 3)
  - 기준: 설탕 · 향신료를 쓰는 음식(케이크 · 초콜릿 · 잼 · 카레 · 로스트)이 spec 표대로 이어진다 (테스트)
  - 기준: 3단 한도를 넘지 않는다 (밀 → 밀가루 → 빵 = 2단, 빵을 쓰는 것이 3단)
  - 기준: (앞 단계 QA) _refresh 가 _drop_freed 를 두 번 부른다 — station_window.gd _refresh 첫 줄의 _drop_freed() 와 _stock() 안의 _drop_freed() 가 겹친다. _refresh 쪽 것을 지워라(_stock 이 늘 먼저 부른다). 막는 것은 아니다
  - 기준: (앞 단계 QA) rows() · row() 가 읽기만 해도 다시 센다 — 이름은 조회인데 _refresh_rows 를 타서 ui_shots 등 밖에서 부를 때마다 110줄 plan 을 돈다. 지금은 테스트 · 찍기에서만 부르지만, 게임 코드가 매 프레임 부르면 끊김이 돌아온다. rows() 문서 주석에 '게임 코드에서 매 프레임 부르지 말 것' 을 적거나, 보는 쪽 전용 이름(rows_fresh 등)으로 나눠라
- [x] 4. 조리용 화로가 비어 있다
  - 기준: `cook_stove` 에도 레시피가 없다 — 부엌 방은 조리대 + 화로 **둘**을 요구하는데(rooms.json) 화로는 할 일이 없다
  - 기준: 셋 중 하나를 골라 report 에 까닭을 적고 그대로 한다:
    (가) 사람 표대로 **조리대 하나로 합치고** 부엌 방의 요구에서 화로를 뺀다 ·
    (나) 화로에 **굽기 · 끓이기**를 나눠 준다 (조리대는 손질, 화로는 열) ·
    (다) 지금처럼 비워 두고 **왜 두는지** spec 에 적는다
  - 기준: 어느 쪽이든 부엌 방이 **지금처럼 서고**, 그 방에서 음식이 만들어진다 (테스트)
  - 기준: (앞 단계 QA) 맷돌 값을 테스트에 맞춰 낮췄다 — 빈손 한 바퀴(test_from_empty_hands)에 돌 일곱을 더 캐게 하자 짐이 무거워 5-2 물가 걷기가 빗나가 떨어졌고, 그래서 맷돌을 돌 20 → 5 · 40kg → 20kg 으로 내렸다(workbench.json · weights.json). 둘 다 임시 값이라 막는 것은 아니지만, 테스트의 걷기가 가방 무게 몇 kg 에 깨진다는 것은 그 테스트가 무르다는 뜻이다. test_from_empty_hands.gd 5-2 의 물가 걷기가 무게로 느려져도 목표에 닿게(도착 판정을 시간이 아니라 거리로, 또는 걷기 전에 짐 무게를 고려해 기다리는 프레임을 늘려) 고친 뒤, 맷돌 값을 다른 시설과 견줄 만한 값(돌 20)으로 되돌릴지 보라
  - 기준: (앞 단계 QA) 음식 재료 표가 spec 에 없다 — 기준은 「spec 표대로」라 했지만 spec/05_craft/facilities.md 에는 시설 표(맷돌 → 밀가루 · 설탕 · 향신료)만 있고 밀빵 밀가루 3 · 카레 · 로스트 · 초콜릿의 재료는 어디에도 없다(plan/list.md 기준 문장에만 「spec 표는 밀가루 3」). 구현은 지어내고 임시로 적었다. facilities.md 「미정」에 「음식별 재료 표 — 사람 표의 음식 줄이 spec 에 옮겨지지 않았다, 지금 줄은 G-205.1 · G-207.3 에 루프가 지은 임시」 한 줄을 두어 다음 단계 기준이 없는 표를 가리키지 않게 하라


## [x] G-208 무게를 없앤다 · 사다리와 기둥재를 뺀다 (사람 결정 2026-10-13)
- 차선: 3d-lane2
- **차선 둘이 같이 돈다** (사람 2026-10-13 「왜 ai 두 개로 안 돌려?」). 이 묶음은 G-207 과 **코드가 겹치지 않는다** —
  G-207 은 창 · 제작대 · 맷돌, 이쪽은 플레이어 · 가구 · 무게다. 다만 **번역(`i18n/*.po`)과 무게표가 양쪽에 걸린다**:
  G-207 은 줄을 **더하고**(밀가루 · 설탕 · 향신료) 이쪽은 **지운다**(사다리 · 기둥재) — 자리가 달라 대개 저절로 합쳐진다.
  고칠 때 **파일을 다시 읽고 그 자리만** 손대라. 부딪히면 루프가 멈추지 않고 사람에게 묻는다
- spec: spec/02_player/inventory-hotbar.md · 05_craft/facilities.md
- **사람 결정 2026-10-13**: 「무게 없애고 기둥재랑 사다리 다 빼. **사다리 쓸 데가 없을 것 같아**」
- 왜 무게를 없애나 (사람이 플레이해 보고 2026-10-13): 배고파서 느려진 뒤 **다 채워도 안 빨라지고 죽어야 돌아왔다**.
  까닭은 무게였다 — 배고픔 층은 0.55배, 짐 층은 최대 0.6배(−40%)라 **채워도 0.55 → 0.6 뿐**이고,
  죽으면 가방이 데스 상자로 빠져 1.0 으로 돌아왔다. 게다가 **무게를 보여 주는 데가 아무 데도 없다** (HUD 0건)
  - 막는 일은 이미 **칸**이 한다 (20칸 × 99개). 무게는 막지도 않고 속도만 깎아 같은 일을 두 번 한 셈이다
  - 수치도 깨졌다: 상한이 70kg 인데 **돌 한 칸(99개)이 148kg** — 캐고 다니면 거의 늘 −40% 였다
- **weights.json 은 지우지 않는다** — 그 파일이 「이 아이템이 게임에 있나」를 가리는 목록이기도 하다
  (`RecipeTags.load_from` 이 `ItemWeights.table()` 로 갈래에 들 이름을 가린다). 속도만 떼어 낸다
- [x] 1. 무게가 속도를 깎지 않는다
  - 기준: 짐이 아무리 무거워도 **걸음이 느려지지 않는다** (테스트). `Player.weight_factor` 를 속도 셈에서 뺀다
  - 기준: `ItemWeights` 의 `FREE_KG` · `SLOW_PER_KG` · `SLOW_MAX` · `speed_factor` 를 지운다.
    **`table()` · `of()` · `carried()` 는 남긴다** — 갈래 판정이 쓴다
  - 기준: 이동 속도의 층이 **배고픔 · 목마름 · 웅크림 · 비탈 · 버프**만 남는다 — 보이지 않는 층이 없다 (테스트)
  - 기준: spec/02_player/inventory-hotbar.md 의 무게 규칙 · 수치를 **사람 결정으로 고쳐 적는다**
    (「무게는 속도를 바꾸지 않는다 — 막는 것은 칸이다, 사람 결정 2026-10-13」). 낡은 줄은 지운다
  - 기준: 배고픔이 바닥났다가 **다 채우면 속도가 그대로 돌아온다** (테스트 — 사람이 겪은 그 자리다)
- [x] 2. 사다리와 기둥재를 뺀다
  - 기준: 사다리 · 기둥재를 **만들 수 없다** — `workbench.json` 의 ladder · `sawmill.json` 의 beam 을 지운다 (테스트)
  - 기준: `Furniture` 에서 사다리 종류 · `LADDER` · `ladder_of` 를 지우고, `Player` 가 사다리를 오르는 길도 지운다 (테스트)
  - 기준: **등반은 그대로다** — 절벽 오르기 · 갈고리총은 사다리와 다른 길이다 (04_life/climbing.md). 건드리지 않는다 (테스트)
  - 기준: 번역(ITEM_ladder · ITEM_beam) · 무게표의 두 줄도 같이 지운다 — 남은 이름이 없는지 grep 으로 확인해 report 에 적는다
  - 기준: 가구는 **의자 · 테이블 · 나무 통 · 침대**가 그대로 선다 (테스트)
  - 기준: spec/05_craft/facilities.md 의 가구 줄과 사다리 설명을 지우고, **없앤 것 목록에 사다리 · 기둥재를 더한다**
- [x] 3. 빠진 뒤에 끊긴 데가 없다
  - 기준: 빈손에서 집 · 침대 · 밥까지 **한 바퀴 검사가 통과한다** (테스트)
  - 기준: 제재소는 **판자 · 자루** 둘을 만든다 — 기둥재가 빠져도 제재소가 빈 시설이 되지 않는다 (테스트)
  - 기준: 전체 테스트 통과 개수가 **줄지 않는다** — 지운 검사(무게 속도 · 사다리)만큼은 report 에 숫자로 적는다
  - 기준: 저장본을 불러와도 깨지지 않는다 — 옛 저장본에 사다리 · 기둥재가 들어 있으면 **빠뜨리고 읽는다** (테스트)
  - 기준: (앞 단계 QA) test_animal_walk 가 시드 없이 흔들린다 — tests/life/hunting/test_animal_walk.gd test_nav_mesh_has_no_water_or_cliff 가 NavigationServer3D.map_get_random_point 로 400점을 시드 없이 뽑아, 앞 회차 기계 검사에서 (120.77, 21.06) 둘레 한 점으로 떨어지고 이번엔 통과했다. 점을 격자로 고르게 뽑거나 시드를 고정하고, 떨어진 그 자리(가장자리 물 · 절벽에 걸친 내비 삼각형)가 진짜 흠인지 먼저 확인하라
  - 기준: (앞 단계 QA) W 판 테스트가 사다리 코드를 못 잡는다 — test_furniture.gd test_w_against_a_tall_board_only_walks 는 LADDERS 무리에 안 든 맨 StaticBody 를 세우므로 옛 사다리 길이 player.gd 에 남아 있어도 통과한다. 지금은 test_player_has_no_ladder_path 가 소스로 대신 지키니 막는 것은 아니다 — 이 테스트는 지우거나 그 점을 주석에 적어라


## [x] G-209 제작 시설 — 업데이트 뒤 멈춤 · 대기열 · 결과 다섯 칸 · 검색 (사람이 플레이해 보고 2026-10-13)
- 차선: 3d-start
- spec: spec/05_craft/crafting-stations.md · facilities.md
- 결정: **「업데이트 뒤 멈춤」은 G-212 로 떼었다** (2026-10-13) — 옛 저장본으로 재현이 안 돼 사람에게 물었다.
  그 답을 기다리느라 결과 칸 · 대기열 · 검색까지 멈출 까닭이 없다 — 셋은 서로 매이지 않는다
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- [x] 1. 결과 칸을 다섯으로
  - 기준: 결과(출력 버퍼)가 **다섯 칸**이다 (지금 3 — `crafting_station.gd BUFFER_SLOTS`) (테스트)
  - 기준: 옛 저장본의 세 칸짜리 버퍼가 그대로 읽힌다 (테스트)
- [x] 2. 대기열 — 순서 · 옮기기 · 개수
  - 사람 결정 (2026-10-13): **① 우선순위를 정할 수 있고 ② 그 순서를 옮겨 바꿀 수 있고 ③ 개수를 정할 수 있다**.
    결과 칸이 차면 멈추고, 빈 칸이 생기면 이어 간다
  - 기준: 레시피를 눌러 **대기열에 넣고**, 줄마다 **개수**를 정한다 (테스트)
  - 기준: 대기열 줄을 **끌어 옮겨** 순서를 바꾼다 — 맨 위가 먼저 만들어진다 (테스트)
  - 기준: 재료가 모자란 줄은 **건너뛰고 다음 줄**을 만든다 — 막힌 줄은 까닭을 보인다 (테스트)
  - 기준: 대기열이 저장되고 불러오면 이어 간다 (테스트)
  - **도는 조건 — 사람이 그 시설에 붙어 있어야 돈다** (사람 결정 2026-10-13).
    「팰월드나 소울마스크 같은 NPC 가 자동화하는 게임들을 보면 **유저가 직접 붙들어 있어야** 하고 NPC 에게 보통 제작을
    맡긴다 — **NPC 의 중요성**과, 그 일 대신 **다른 일을 할 수 있는 분할 작업** 때문에. 처음엔 불편하더라도 NPC 자동화를 생각하면 좋다」.
    Claude 는 「같은 방이면 돈다」를 권했지만 사람이 더 조인 쪽을 골랐다 — **같은 방으로 느슨하게 하지 않는다**
  - 기준: 사람이 그 시설의 **손 닿는 거리 안에 서 있을 때만** 타이머가 흐른다 — 같은 방이어도 떨어져 있으면 멈춘다.
    떠나면 멈추고 돌아오면 이어 간다 (테스트)
  - 기준: 멈춘 동안 시설 창 · 시설 위에 **「멈춤 — 곁에 있어야 만든다」**가 보인다 — 왜 안 도는지 사람이 안다 (테스트)
  - 참고: NPC 가 대신 붙는 길은 **이 묶음에서 만들지 않는다** — 어느 직업이 제작을 맡을지는 사람이 정한다 (npc-villagers.md 직업 표에 아직 없다).
    다만 「붙어 있는 누군가」를 사람 하나로 박지 말고, **나중에 주민을 넣을 수 있는 자리**로 짠다 (함수 하나가 「지금 누가 붙어 있나」를 돌려준다)
  - 기준: spec/05_craft/crafting-stations.md 규칙에 **사람 결정으로 적는다** — 「사람이 그 시설에 붙어 있어야 만든다 (손 닿는 거리,
    같은 방이어도 떨어지면 멈춘다) · 까닭(팰월드 · 소울마스크, NPC 의 중요성 · 분할 작업) · 대기열(우선순위 · 옮기기 · 개수) · 결과 칸 다섯」
  - 기준: 그 조건이 **한 상수/함수 한 곳**에 있다 — 사람이 「자동으로 돌게」로 바꾸면 그 줄만 고친다
- [x] 3. 시설 창에 검색
  - 사람이 짚었다: 「**제작 관련 시설들 가독성 떨어짐. 검색 기능 추가**」
  - 기준: 시설 창 위에 **검색 칸**이 있고, 이름(번역된 것)에 글자가 들면 그 줄만 남는다 (테스트)
  - 기준: 검색해도 **스크롤 · 회색 줄 · 대기열**이 그대로 맞다 — G-207 의 가벼운 갱신을 깨지 않는다 (테스트)
  - 참고: **카테고리는 사람이 먼저 본다** (「카테고리 추가 — 나한테 검사」). 사람이 정하면 단계로 붙인다
  - 기준: (앞 단계 QA) 왕복 저장 검사의 대기열 두 줄이 비교되지 않는다 — game/tests/settings/save/test_round_trip.gd:511-512 에서 table 2 · chair 3 을 넣지만 불러온 뒤 비교하는 _station_work(:566)는 buffer · crafting · left · total 만 담아 대기열이 사라져도 통과한다. _station_work 의 사전에 queue = s.queue() 를 더해 저장 전후가 [{table,2},{chair,3}] 로 같은지 보게 하라 (기준 4 는 test_station_queue.gd 가 따로 지키므로 막는 것은 아니다)
  - 기준: (앞 단계 QA) 시설 거리 계산이 회전을 무시한다 — game/craft/stations/stations.gd _nearest_on 은 station.size() 를 세계 축 그대로 써서, 시설을 90° 돌려 놓을 수 있다면 길쭉한 시설의 바닥 상자가 틀려진다. 시설이 돌 수 있는지 보고, 돈다면 station.global_transform.affine_inverse() 로 at 을 로컬로 옮긴 뒤 clamp 하고 다시 세계로 옮기며, 돌린 시설의 긴 변 끝에 선 경우를 테스트로 넣어라
- [x] 4. (앞 단계 QA) 창 높이 단언이 _full_list_height 를 지키지 않는다
  - 기준: 창 높이 단언이 _full_list_height 를 지키지 않는다 — _fit 의 list 를 옛 _rows.get_combined_minimum_size().y 로 되돌려도 test_the_scroll_goes_back_to_the_top_and_the_window_does_not_jump 가 그대로 통과했다(_fit 은 _center.resized 에서만 불려 검색 때는 안 돌고, 「씨앗」은 줄이 많아 어차피 자리를 채운다). 기준 밖(창이 안 튀는 것은 덤)이라 막지 않는다. test_station_search.gd 에 「의자」처럼 한 줄만 남긴 채 w._fit() 을 부르고(또는 창 크기를 바꿔 resized 를 내고) 판 높이 · scroll 높이가 거르기 전과 같은지 보는 단언을 더하라

## [x] G-212 게임을 업데이트하면 제작 시설이 안 돈다 — 사람이 본 그대로 재현한다 (2026-10-13)
- 차선: 3d-start
- spec: spec/05_craft/crafting-stations.md · 01_settings/save.md
- G-209 단계 1 에서 뗐다. 옛 판(d219e9ed) 저장본으로는 **재현되지 않았다** — 저장 꼴이 같고 불러온 시설이 새로 놓은 것과 똑같이 돈다 (검사 다섯).
  사람이 본 그대로를 알아야 재현할 수 있다 — decisions.md 의 물음
- 결정: **사람이 답했다** (2026-10-13):
  ① **우클릭해도 창이 안 열렸다** (줄이 회색이거나 시간이 안 흐른 것이 아니다 — 창 자체가 안 뜬다)
  ② 업데이트(tools/play.sh)할 때 **Godot 을 켜 둔 채였던 것 같다**
  ③ 그 시설은 **방 안이 아니었다** (야외)
  - 「부수고 다시 놓으면 된다」와 ①을 같이 보면: **불러온 시설만 우클릭을 받지 못한다** — 놓을 때만 서는 것(조준 광선이 맞히는
    몸 · 충돌 층 · 무리 · 열기를 받는 쪽에 등록)이 불러올 때 안 서는 것을 먼저 의심한다
- **사람의 실제 저장본이 있다** — `~/Desktop/naru-saves/G-212/` (세계 `1790014507_e428c509.sav` · 캐릭터 `1789959457_2a520c0c.sav`,
  2026-10-13 19:41 저장). 루프가 만든 옛 저장본으로는 재현이 안 됐다 — **이 파일로 먼저 재현한다**.
  저장본을 저장소에 넣을 때는 테스트 픽스처로 복사하고, 사람 파일 자리(app_userdata)는 **건드리지 않는다**
- [x] 1. 게임을 업데이트하면 제작 시설이 안 돈다 — 재현부터
  - 사람이 겪었다: 「**게임 업데이트 하면 제작 관련 시설 작동 안 함. 부수고 다시 설치하면 정상 작동**」
  - 기준: **사람의 저장본**(`~/Desktop/naru-saves/G-212/`)을 지금 판으로 불러와, **야외에 선 시설을 우클릭으로 연다** —
    안 열리는 것을 검사로 먼저 재현한다 (테스트). 짐작으로 고치지 않는다
  - 기준: 재현되면 **불러온 시설과 새로 놓은 시설이 무엇이 다른지** 숫자와 이름으로 적는다 — 조준 광선이 무엇을 맞히나 ·
    충돌 층 · 무리 · 노드 꼴 · 시설 종류 이름
  - 기준: 그래도 재현이 안 되면 「Godot 을 켜 둔 채 업데이트」(②)를 재현한다 — 편집기 · 게임이 옛 스크립트를 쥔 채 플레이 폴더가
    바뀌면 무엇이 저장되나. 이 경우 고칠 곳은 `tools/play.sh` 일 수 있다 (켜 둔 Godot 을 알아채 알린다)
  - 기준: 불러온 시설이 **새로 놓은 것과 똑같이** 돈다 — 레시피 목록 · 시작 · 버퍼 · 수령 (테스트)
  - 기준: 무엇이 달랐는지(저장에 빠진 값 · 바뀐 이름 · 놓을 때만 채우는 값) **숫자와 이름으로** report 에 적는다
  - **옛 판 게임을 통째로 꺼내 Godot 으로 돌리지 마라** (2026-10-13 사람 화면에 크래시 리포트가 줄줄이 떴다) —
    `/tmp/naru_old` 에 옛 커밋을 꺼내 `godot --path /tmp/naru_old/game` 으로 열면 **매번 크래시**한다 (SIGABRT,
    터레인 확장을 다시 들이다 죽는다). 크래시마다 macOS 가 사람 화면에 리포트 창을 띄운다 — LOOP.md 2절 「사람 화면에 손대지 않기」에 어긋난다
  - 대신: **옛 저장본을 지금 판에서 글로 만든다** — `git show <옛 커밋>:game/settings/save/world_data.gd` 등으로 그때 저장 꼴을 읽고,
    그 꼴 그대로의 저장 파일(JSON · 딕셔너리)을 테스트 안에서 적어 **지금 판으로 불러온다**. 옛 판을 실행할 필요가 없다
- [x] 2. (앞 단계 QA) 기다리는 play.sh 가 겹쳐 쌓인다
  - 기준: 기다리는 play.sh 가 겹쳐 쌓인다 — tools/play.sh 는 godot_holding 검사를 잠금(mkdir "$LOCK") 앞에서 하므로, Godot 을 오래 켜 두는 동안 루프가 묶음마다 play.sh 를 부르면 여러 개가 함께 기다리고 저마다 「Godot 을 꺼 주세요」 알림을 한 번씩 낸다. 잠금을 먼저 잡고(또는 기다리는 표시 파일을 두고) 이미 기다리는 것이 있으면 새 것은 알리지 않고 끝나게 하고, test_play_sh_waits_for_godot.gd 에 두 번 부르면 알림이 한 줄인지 단언을 더하라
  - 기준: (앞 단계 QA) 「켜 둔 게임은 옛 것과 새 것이 섞이지 않는다」에 load() 길이 빠졌다 — report 는 preload 로 시작 때 다 들인다는 것에 기대지만, 실행 중 load() 로 처음 읽는 자원(예: menu_root.gd:27 의 load(GameRoot.ISLAND_PATH) 같은 것이 월드 들어갈 때 불리면)은 checkout 뒤 새 판 파일을 읽는다. game/ 에서 런타임 load( 를 훑어 시작 뒤에 처음 불리는 것이 있는지 적어 두고, 있으면 godot_holding.sh 머리글의 설명을 고쳐라 (play.sh 가 이제 기다리므로 막는 것은 아니다)

## [x] G-210 컵 · 음료 · 염전 · 상자 검색 · 주민 일 범위 · 이름 (사람이 플레이해 보고 2026-10-13)
- 차선: 3d-lane2
- spec: spec/02_player/hunger-thirst.md · 05_craft/facilities.md · 06_build/storage.md · 07_automation/npc-villagers.md
- 결정: 단계 4 의 「시설 창과 같은 모양」은 **G-213 으로 옮겼다** (2026-10-13) — 시설 창 검색은 차선 1 의 G-209 에서 이미 짜였다.
  두 쪽이 한 줄기에 모인 뒤에야 하나로 맞출 수 있다
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- **차선 1(G-209)과 코드가 안 겹친다** — 이쪽은 플레이어 · 컵 · 상자 창 · 주민 · 놓기 규칙이다. 번역 파일만 같이 걸린다 —
  고칠 때 파일을 다시 읽고 제 자리만 손대라
- [x] 1. 컵이 모금 수를 정하고, 다 마시면 사라진다
  - **사람 결정 2026-10-13** (2026-09-30 의 「나무컵 3 · 쇠컵 8 · 빈 컵은 겹친다」를 바꾼다):
    **컵이 마실 수 있는 횟수를 정한다 — 나무컵 3 · 돌컵 4 · 쇠컵 6.** 물이든 어떤 음료든 그 컵에 담으면 그 횟수다.
    **컵은 다 마시면 사라진다** (돌려받지 않는다). 기분 같은 스탯은 나중이다
  - 기준: **돌컵**을 새로 만든다 (돌로 — 재료는 임시) (테스트)
  - 기준: 물을 담으면 나무 3 · 돌 4 · 쇠 6 번 마신다 (테스트). 마지막 모금을 마시면 **컵이 사라진다** (테스트)
  - 기준: spec/02_player/hunger-thirst.md 의 컵 표 · 「빈 컵은 겹친다 · 다 마시면 빈 컵」 줄을 **사람 결정으로 고쳐 적고** 낡은 줄을 지운다
  - 기준: 옛 저장본의 빈 컵 · 반쯤 든 컵이 깨지지 않고 읽힌다 (테스트)
- [x] 2. 음료를 마실 수 있다
  - 사람이 짚었다: 「**지금 나와 있는 음료 마실 수 없음**」
  - 기준: 양조 · 증류 · 조리대의 음료(맥주 · 포도주 · 주스 · 따뜻한 우유 …)를 **컵에 담아** 마신다 — 모금 수는 컵이 정한다 (테스트)
  - 기준: 마시면 그 음료의 값(목마름 · 스트레스 등 — 지금 데이터대로, 임시)이 실제로 든다 (테스트)
  - 기준: 음료를 컵 없이 손에 들고 마실 수 있게 둘지 **컵을 꼭 거치게** 할지 — 사람 말대로 **컵을 거친다**. 컵에 담는 손짓을 report 에 적는다
  - 기준: (앞 단계 QA) cup.gd 머리 주석에 낡은 예가 남았다 — game/player/hunger_thirst/cup.gd:9 의 「{item = "iron_cup", count = 1, water = 8}」은 이제 쇠컵이 6모금이라 틀린 예다. water = 6 으로 고쳐라. 같은 파일 3줄의 「사람 결정 2026-09-30」도 2026-10-13 을 함께 적어라
  - 기준: (앞 단계 QA) spec 에서 「든 컵은 한 칸에 하나씩」의 사람 결정 표시가 빠졌다 — spec/02_player/hunger-thirst.md 18줄 「갓 만든 컵은 빈 채로 겹쳐 쌓이고, 음료(물 포함)가 든 컵은 한 칸에 하나씩이다」는 옛 줄의 「(사람 결정 2026-09-30)」을 떨궜다. 2026-10-13 결정이 바꾼 것은 모금 수 · 다 마신 뒤뿐이고 「든 컵은 한 칸에 하나」는 그대로 2026-09-30 사람 결정이니 그 표시를 줄 끝에 되살려라
- [x] 3. 염전은 바닷가에만 놓인다
  - 사람 결정 (2026-10-13): 「염전 → **바닷가에만 설치 가능**」 — 지금은 아무 데나 놓이고 바닷가에서만 소금이 난다
  - 기준: 바닷가가 아니면 **놓이지 않고**, 미리보기가 빨갛고 까닭이 보인다 (테스트). 바닷가 판정은 지금 소금 판정과 **같은 규칙 하나**를 쓴다
  - 기준: 옛 저장본에서 바닷가가 아닌 곳에 놓인 염전은 — 지우지 말고 그대로 두되 소금은 안 난다 (지금처럼). report 에 적는다
  - 기준: (앞 단계 QA) 음료 배고픔이 모금마다 통째로 드는 것이 spec 에 없다 — Drinking.sip_values 는 foods.json 의 한 잔 몫(우유 0.15)을 모금마다 넣어 쇠컵 하나로 우유 한 잔이 배고픔 0.9 를 채운다(밀빵 1.0 에 가깝다). report temporary 에는 적혔지만 spec/02_player/hunger-thirst.md 의 「음료 한 모금은 … 배고픔 몫도 든다 (임시)」 줄에는 「모금마다 한 잔 몫이 통째로 — 큰 컵일수록 한 잔이 더 채운다」가 빠졌다. 그 줄에 덧붙여 사람이 보게 하라
  - 기준: (앞 단계 QA) pour_held 가 빈 컵을 두 번 찾는다 — game/player/hunger_thirst/drinking.gd 의 pour_held 에서 Cup.empty_cup_at(player.inventory()) 을 if 에서 한 번, var c 에서 또 한 번 부른다. c 를 먼저 구해 c < 0 으로 묻게 한 번으로 줄여라
- [x] 4. 모든 상자에 검색
  - 사람이 짚었다: 「**모든 상자에 검색 기능 추가**」
  - 기준: 상자 창(보관 상자 · 데스 상자 · 개발용 상자 — 상자 창을 쓰는 것 전부)에 **검색 칸**이 있고, 이름에 글자가 든 칸만 밝게 남는다 (테스트)
  - 기준: 검색 중에도 칸 옮기기 · Shift+클릭 · 겹치기가 그대로 된다 (테스트)
  - 기준: 상자 쪽 검색 칸은 **공용 부품 하나(`ItemSearch`)**로 짠다 — 시설 창과 맞추는 일은 **G-213** 이 한다
    (2026-10-13: 두 차선이 같이 돌아 한쪽에서 맞출 수 없었다. 이 단계는 상자 쪽만 본다)
- [x] 5. 나무꾼 · 광부는 대상에 붙어서 일한다
  - 사람이 짚었다: 「나무꾼에서 나무 캐는 범위가 너무 크다 → **효율이 너무 좋음**. 광부도 마찬가지. **오브젝트랑 붙어서 해야 함**」
  - 기준: 나무꾼 · 광부가 **나무 · 바위 바로 곁(사람 손 닿는 거리와 같은 값)**까지 걸어가서야 친다 (테스트)
  - 기준: 멀리서 치는 길이 없다 — 지금의 범위 값이 어디 있는지 찾아 report 에 적고 그 값을 손 닿는 거리로 맞춘다
  - 기준: 그래서 한 그루를 다 베는 데 걸리는 시간이 **사람이 직접 하는 것과 비슷하다** — 앞뒤 숫자를 report 에 적는다
  - 기준: (앞 단계 QA) 검색 칸 낡은 주석 셋이 남았다 — 기준 3이 바뀌어(시설 창과 맞추는 것은 G-213) 코드·spec 주석은 고쳤는데 같은 말이 세 곳에 남았다: game/tests/ui/hud/test_chest_window_search.gd 6줄 「검색 칸은 제작 시설 창과 같은 것(ItemSearch) 하나다」와 39줄 단언 메시지 「제작 시설 창과 같은 ItemSearch」, game/i18n/ko.po 185줄 주석 「상자 창 · 제작 시설 창이 같이 쓴다」, game/i18n/en.po 185줄 「shared by chest and station windows」. 「상자 창 셋이 같이 쓴다 — 시설 창과 맞추는 것은 G-213」으로 고쳐라 (.po 는 차선 1과 같이 걸리니 그 줄만 손댄다)
- [x] 6. 이름 — 자루 → 손잡이
  - 사람 결정 (2026-10-13): 「**자루 → 손잡이**」. 번역(ko)만 바꾼다 — 아이템 이름(`handle`)은 그대로 둔다 (저장본이 깨지지 않게)
  - 기준: 게임 안 어디에도 「자루」가 그 뜻으로 안 남는다 — ko.po · spec · 계통도에 쓰인 말까지 grep 으로 확인해 report 에 적는다
  - 기준: (앞 단계 QA) 사람이 치는 빠르기를 0.6초로 가정했다 — test_villager_gather_close.gd 의 시간 비교는 사람도 HarvestWork.HIT_EVERY(0.6초)마다 한 대씩 친다고 본다. 사람의 좌클릭(Harvest.swing)에는 쿨다운이 없어 실제로는 더 빨리 칠 수 있다. 그 가정을 테스트 주석과 spec 의 「한 그루」 줄에 「사람 빠르기는 HIT_EVERY 로 가정 — 임시」라고 적어 두어라


## [x] G-211 제작자 — 시설의 재고 규칙을 보고 주민이 만든다 (사람 결정 2026-10-13)
- 차선: 3d-start
- spec: spec/07_automation/npc-villagers.md · 05_craft/crafting-stations.md
- **G-209 다음에 한다** — G-209 의 대기열 · 「붙어 있어야 돈다」 위에 얹힌다 (같은 차선이라 순서가 저절로 맞는다)
- **사람 결정 2026-10-13**:
  - 「**제작자**로 해서 따로 만들자」 — 새 직업이다 (지금 표: 나무꾼 · 광부 · 농부 · 연구원 · 관리인 · 청소부)
  - 「**상자 재고를 자동 제작하는 건 제작 시설 안에서** 하는 걸로」 — 재고 규칙은 시설 창에서 정한다. 따로 된 기계 · 상자 설정이 아니다
  - 「**제작자가 그걸 토대로 움직이는 거고**」 — 제작자는 시설의 재고 규칙을 보고 할 일을 찾는다
  - 까닭(같은 날 앞서): 사람은 시설에 **붙어 있어야** 만든다 — 제작은 NPC 에게 맡기는 일이라 NPC 가 중요해지고, 사람은 그동안 다른 일을 한다
- **재고는 그 시설과 같은 방 상자에서 센다** — 제작 재료를 이미 같은 방 상자에서 읽는다(crafting-stations.md). 다른 셈을 따로 두지 않는다
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- [x] 1. 시설 창에서 줄마다 「N개 유지」
  - 기준: 대기열 줄에 **「몇 개 만들기」와 「N개 유지」** 둘 중 하나를 고른다 (테스트)
  - 기준: 「N개 유지」 줄은 **같은 방 상자 + 그 시설 결과 칸**에 든 그 아이템이 N개보다 적으면 **할 일 있음**이 되고, N개가 되면 멈춘다 (테스트)
  - 기준: 사람이 붙어 있으면 사람 손으로도 그 규칙대로 돈다 — 대기열의 다른 줄과 같은 길이다 (테스트)
  - 기준: 시설 창에 지금 몇 개가 있고 몇 개를 유지하는지 보인다. 재료가 모자라 못 채우면 까닭이 보인다 (테스트)
  - 기준: 저장되고 불러오면 그대로다 (테스트)
- [x] 2. 제작자 직업
  - 기준: 주민 창에서 **제작자**를 고를 수 있다 (직업 표 · 번역 · 저장) (테스트)
  - 기준: 제작자는 **제 방(배치된 방)의 시설** 가운데 **할 일 있는 곳**(대기열이 있거나 「N개 유지」가 모자란 곳)으로 걸어가 **손 닿는 거리에 붙는다** (테스트)
  - 기준: 제작자가 붙어 있으면 **사람이 없어도 타이머가 흐른다** — G-209 의 「지금 누가 붙어 있나」에 주민이 든다 (테스트)
  - 기준: 다 된 것을 **같은 방 상자로 옮긴다** — 결과 칸 다섯이 차서 멈추지 않게 (테스트). 상자가 차면 까닭을 띄우고 기다린다
  - 기준: 할 일이 여럿이면 시설마다 **대기열 순서대로**, 시설끼리는 **가까운 곳부터** 한다 (테스트)
  - 기준: 할 일이 없으면 다른 주민처럼 쉰다 — 시설 앞에 계속 서 있지 않는다 (테스트)
  - 기준: 스트레스 · 특성이 다른 직업과 같이 듣는다 (70 부터 느리게 — npc-villagers.md) (테스트)
  - 기준: (앞 단계 QA) 유지 줄이 매 프레임 방을 다시 잰다 — Stations.tick_crafts 가 시설마다 has_work(s) 를 부르는데, 「N개 유지」 줄이 있으면 chests 없이 불려 room_chests(Rooms.find — 놓인 것 전부 훑기)를 프레임마다 돈다. 곁에 있고 놀고 있을 때도 advance_queue 가 또 한 번 잰다 (기준 밖, 막는 것은 아니다). game/craft/stations/stations.gd 의 tick_crafts 에서 한 시설당 한 번만 재 has_work · advance_queue 에 같은 chests 를 넘기거나, 창의 _room_chests 처럼 놓인 수가 바뀔 때만 다시 재는 캐시를 두고, chest_scans 로 「유지 줄 하나 · 60프레임 동안 방 재기가 몇 번 이하」를 단언하는 테스트를 더하라
- [x] 3. spec 에 적는다
  - 기준: spec/07_automation/npc-villagers.md 직업 표에 **제작자** 줄을 사람 결정으로 더한다 — 하는 일 · 쓰는 것
  - 기준: spec/05_craft/crafting-stations.md 에 「N개 유지」 규칙 · 재고는 같은 방 상자에서 센다 · 제작자가 그것을 보고 움직인다를 사람 결정으로 적는다
  - 기준: 찍어서 확인한다 — 제작자가 시설에 붙어 만들고 상자에 넣는 모습
  - 기준: (앞 단계 QA) 주민 사람 설계(G-160)가 spec 에 없다 — spec/07_automation/npc-villagers.md 6줄은 「사람됨(나이 · 특성 · 스트레스 · 관계 · 죽음)은 … (아래 규칙, 사람 결정 2026-09-28)」라고 하지만, 사람이 준 규칙(수면 30 부터 스트레스 오름 · 0 이면 대폭, 스트레스 100 에 부정 특성 발동, 50 · 80 경고, 70 부터 느리게, 부정 특성 넷 · 긍정 특성 여덟과 그 효과, 특성은 변하지 않는다)은 spec/ 어디에도 없다 (커밋 72774ced 에 있던 것이 이어지지 않았다 — 원문은 plan/list.md 1535~1550줄 G-160). 이번 기준 「70 부터 느리게 — npc-villagers.md」도 spec 이 아니라 목록에만 있다. 본문이 125줄 상한이라 spec/07_automation/ 에 villager-persona.md 같은 짝 파일을 만들어 그 줄들을 「사람 결정 2026-09-28」 표시와 함께 그대로 옮기고, npc-villagers.md 6줄의 「아래 규칙」을 그 파일로 가리키게 고쳐라. 이번 단계의 Villager.pace · Persona.STRESS_SLOW 도 거기서 가리키게 하라
- [x] 4. (앞 단계 QA) 제작자 방 바닥에 풀이 솟는다
  - 기준: 제작자 방 바닥에 풀이 솟는다 — /tmp/crafter_shot/villager_crafter.png · villager_crafter_done.png 에서 토대 위 방 안 바닥을 풀잎이 뚫고 올라와 있다(화면 아래쪽). 토대가 놓인 칸의 풀(잔디 인스턴스)을 걷지 않는 것으로 보인다. 토대를 놓을 때 그 발자국 안의 풀을 지우거나 숨기고, 토대 발자국 안에 풀 인스턴스가 0 인지 단언하는 테스트를 더하라
  - 기준: (앞 단계 QA) 두 그림이 거의 같아 「상자에 넣었다」가 눈으로 안 보인다 — villager_crafter 와 villager_crafter_done 은 같은 카메라 · 같은 구도라 차이가 주민 위치 조금뿐이고, 상자 안 의자는 로그로만 확인된다. shots.gd _set_up_crafter 의 done 장면에서 상자 내용(또는 제작대 결과 칸)을 화면 글로 띄우거나 카메라를 상자 쪽으로 돌려, 그림만 보고도 의자가 상자에 든 것을 알 수 있게 하라
- [x] 5. (앞 단계 QA) 풀 숨김 테스트가 MultiMesh 를 안 본다
  - 기준: 풀 숨김 테스트가 MultiMesh 를 안 본다 — VegetationView.visible_within 은 자료와 _hidden 표로 셀 뿐이라, cover() 의 mm.set_instance_transform(…scaled(ZERO)…) 줄을 지워도 4개 테스트가 다 통과한다(헤드리스 더미 렌더러가 get_instance_transform 에 늘 단위 변환을 돌려주는 것은 QA 가 Godot 4.7.2 로 확인했다 — 그래서 막는 것은 아니고 그림으로는 확인됨). 숨기기 · 되살리기를 한 곳(예: _apply(kind, chunk, n, xform))으로 모으고 테스트에서 그 함수가 부른 변환을 기록하게 하거나, 숨긴 인스턴스의 basis 를 따로 담아 두고 「숨긴 열쇠마다 마지막으로 넣은 basis 가 ZERO 크기」를 단언하라


## [x] G-213 검색 칸을 하나로 — 상자 창 · 시설 창 (2026-10-13)
- 차선: 3d-lane2
- **G-210 다음에 한다** — 새 묶음을 열 때 루프가 차선 1(G-209 의 시설 창 검색)을 먼저 합쳐 온다. 그래야 두 검색이 한 줄기에 있다
- 무엇이 갈렸나: 상자 창은 `ui/hud/item_search.gd ItemSearch` · 안내 「이름 검색」 · **칸을 흐린다**,
  시설 창은 `station_window` 안의 LineEdit · 안내 「이름으로 찾기」(`STATION_SEARCH`) · **줄을 숨긴다**
- **정한 것** (Claude 가 정했다 2026-10-13 — 화면 꼴의 작은 일이라 사람에게 묻지 않았다. 사람이 다르게 원하면 바꾼다):
  ① 공용 부품은 **`ItemSearch`** 하나 ② 상자는 **칸 흐리기**, 시설은 **줄 숨기기**를 그대로 둔다 — 상자 칸은 칸 번호가 손짓에 걸려
  숨기면 옮기기가 꼬이고, 시설은 줄이 110개라 숨겨야 찾는다 ③ 안내 글자는 **「이름으로 찾기」** 하나
- [x] 1. 두 창이 같은 검색 칸을 쓴다
  - 기준: 시설 창의 검색 칸을 `ItemSearch` 로 바꾸고 `STATION_SEARCH` 번역 키를 지운다 — 남은 곳이 없는지 grep 으로 확인해 report 에 적는다
  - 기준: 두 창의 검색 칸이 **같은 클래스 · 같은 안내 글자(「이름으로 찾기」)**인지 보는 테스트를 `game/tests/ui/hud/` 에 둔다 (테스트)
  - 기준: 상자는 칸 흐리기 · 시설은 줄 숨기기가 **그대로** 된다 — G-207 의 가벼운 갱신 · 스크롤을 깨지 않는다 (테스트)
  - 기준: 찍어서 확인한다 — 상자 창 · 시설 창에 검색 글자가 든 모습
- [x] 2. (앞 단계 QA) spec 검색 칸 줄이 시설 창 자리를 틀리게 말한다
  - 기준: spec 검색 칸 줄이 시설 창 자리를 틀리게 말한다 — spec/12_ui/hud.md 「검색 칸」 줄은 두 창 모두를 말하면서 「단추 줄 맨 앞에 서고」라고 한다. 시설 창에는 단추 줄이 없고 검색 칸이 제목 아래 · 줄 목록 위에 선다 (station_window.tscn, test_station_search::test_the_search_field_sits_above_the_rows). 「상자 창은 단추 줄 맨 앞, 시설 창은 줄 목록 위에 선다」로 고쳐라
  - 기준: (앞 단계 QA) shot.sh 가 끝에 종료 코드 2를 낸다 — report 에 따르면 harness/shot.sh --ui 가 8장을 다 찍은 뒤 godot 이 '"": No such file' 와 종료 코드 2를 냈다. 이 QA 세션 셸에서도 명령마다 같은 '"": No such file or directory (os error 2)' 가 찍혀, shot.sh 가 아니라 셸 환경(프로필이 빈 경로를 부르는 것) 탓일 수 있다. 어느 쪽인지 가려 shot.sh 탓이면 고쳐라 — 사진에는 영향이 없었다


## [x] G-905 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-start (루프가 가져갔다 2026-10-14 02:31)
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. LANES 를 기다리기 전에 읽는다 (G-212 단계 2)
  - 기준: LANES 를 기다리기 전에 읽는다 — tools/play.sh 는 LANES(step.py lanes)를 맨 위에서 한 번 계산하고 Godot 이 꺼질 때까지 몇 시간 기다릴 수 있다. 그 사이 뒤에 온 play.sh 는 exit 0 하므로 기다리는 동안 목록에 새로 붙은 차선은 빠진다. LANES 계산을 기다림 루프 뒤(잠금 잡은 뒤)로 옮겨라
- [x] 2. 죽은 표시 판정이 pid 재사용에 약하다 (G-212 단계 2)
  - 기준: 죽은 표시 판정이 pid 재사용에 약하다 — naru-play.waiting/pid 의 pid 가 죽은 뒤 다른 프로세스가 같은 pid 를 받으면 kill -0 이 참이라 뒤에 온 play.sh 가 영영 조용히 끝난다. pid 와 함께 시작 시각(ps -o lstart= -p)을 적어 대조하거나, 표시 나이(예: 하루)를 넘으면 버리게 하라
  - 기준: (앞 단계 QA) 부딪힘 알림이 PLAY_NOTIFY 를 안 따른다 — tools/play.sh 의 차선 부딪힘 줄은 "$ROOT/harness/notify.sh" 를 바로 부르는데 기다림 알림은 "$NOTIFY"(PLAY_NOTIFY 로 바꿀 수 있다)를 쓴다. 시험에서 PLAY_NOTIFY 로 알림을 막아도 부딪힘 때는 진짜 알림이 뜰 수 있다. 부딪힘 줄도 "$NOTIFY" 로 바꿔라 (이번 기준 밖)
- [x] 3. 차선 부딪힘 알림이 PLAY_NOTIFY 를 무시한다 (G-212 단계 2)
  - 기준: 차선 부딪힘 알림이 PLAY_NOTIFY 를 무시한다 — tools/play.sh 의 merge 실패 쪽은 "$ROOT/harness/notify.sh" 를 직접 불러 시험에서 진짜 알림이 뜰 수 있다. "$NOTIFY" 로 바꿔라

## [x] G-214 제작 창 · 창을 닫으면 한쪽으로 걷는다 · 음료는 컵을 재료로 (사람이 플레이해 보고 2026-10-14)
- 차선: 3d-start
- spec: spec/05_craft/crafting-stations.md · facilities.md · 02_player/hunger-thirst.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- [x] 1. 창을 닫으면 캐릭터가 한쪽으로만 걷는다 — 재현부터
  - 사람이 겪었다: 「창고를 쓸 때도, 제작 시설을 쓸 때도 **특정 트리거가 되면 창을 닫았을 때 한쪽으로만 움직이는** 버그가 있다.
    트리거를 못 찾겠다. **지금은 검색하면 한쪽으로 간다**. 이 밖에도 이런 버그가 있는데 찾아봐」
  - 기준: **검색 칸에 글자(특히 W · A · S · D 처럼 걷기 키와 겹치는 글자)를 친 뒤 창을 닫으면** 캐릭터가 저절로 걷는 것을 검사로 재현한다 (테스트)
  - 기준: 까닭을 찾아 report 에 적는다 — 짐작: 글 칸이 키를 먹어 **키를 뗀 신호가 게임에 안 닿아** 걷기가 눌린 채로 남는다
  - 기준: **같은 꼴을 다 찾는다** — 글 입력 칸(검색 · 방 이름 · 지도 표 · 캐릭터 · 세계 만들기)과 창 열기 · 닫기 · 조작 잠금이 걸린 모든 자리.
    「키를 누른 채 창을 열고, 창 안에서 떼고, 닫는다」 · 「글 칸에서 걷기 키 글자를 치고 닫는다」 · 「Esc 로 닫는다」를 창마다 검사로 돈다 (테스트)
  - 기준: 창을 닫거나 조작 잠금이 풀리는 순간 **눌려 있던 걷기 · 달리기 · 웅크리기가 실제 키 상태와 맞는다** — 한 곳에서 맞춘다 (테스트)
- [x] 2. 만들기 바가 처음부터 보인다
  - 사람이 짚었다: 「제작하는 거 밑에 **만들기 바가 처음부터 있어야** 플레이어가 설정할 수 있구나 알지 — 두 번째 클릭할 때부터 나오면 잘 모른다」
  - 기준: 시설 창을 열면 **만들기 바(개수 · 몇 개 만들기 / N개 유지 · 만들기 단추)가 처음부터** 보인다 (테스트)
  - 기준: 레시피를 고르기 전에는 바가 **흐리게 눌리지 않고** 「레시피를 고른다」 같은 안내가 보인다. 고르면 그 레시피로 바뀐다 (테스트)
  - 기준: 찍어서 확인한다 — 연 그대로의 시설 창
  - 기준: (앞 단계 QA) 짝 없는 .uid 쓰레기 파일이 남았다 — game/tests/core/input/test_zz_probe.gd.uid 가 test_zz_probe.gd 없이 남았다(git status 에 ?? 로 보인다). rm 으로 지워라
  - 기준: (앞 단계 QA) 세계 씨앗 칸 · 지도 Y 칸은 검사가 안 돈다 — test_held_keys.gd 는 세계 만들기의 이름 칸과 지도 field_x 만 친다. HeldKeys 는 LineEdit 면 다 잡으니 지금은 맞지만, world_create 의 SeedRow/Edit 과 map_window.field_y() 도 _type_lost 로 쳐 보는 줄을 매개변수에 더해 두면 칸이 LineEdit 이 아닌 것으로 바뀔 때 잡힌다
- [x] 3. 음료는 컵을 재료로 만든다 — 컵이 모금 수를 정한다
  - **사람 결정 2026-10-14** (G-210.2 의 「손에 든 음료를 빈 컵에 담는다」를 바꾼다):
    「마실 것을 만드는 **재료에 컵이 있어야** 한다. 과일 주스로 예를 들면 지금 아무 과일 ×2 인데 **아무 과일 ×2 + 컵**인 거다.
    그 컵이 **나무컵이면 3번 · 돌컵이면 4번 · 쇠컵이면 6번** 마신다」
  - 기준: 음료 레시피(조리대 음료 · 양조통 술 · 증류기 독주 · 알코올 빼고 마시는 것 전부)에 **컵 칸(`@컵` 갈래 — 나무 · 돌 · 쇠)**이 든다 (테스트)
  - 기준: 만들어진 것은 **그 음료가 든 그 컵**이다 — 넣은 컵이 나무면 3 · 돌 4 · 쇠 6모금이 찬 채로 결과 칸에 나온다 (테스트)
  - 기준: 시설 창에 컵 칸이 보이고, 어떤 컵이 쓰일지(가진 것 가운데) 보인다 (테스트)
  - 기준: **손에 든 음료를 빈 컵에 붓는 길(Cup.pour_selected · CUP_NEED_CUP)은 지운다** — 음료는 이제 컵에 든 채로만 있다.
    옛 저장본의 맨 음료(컵 없는 맥주 · 주스 …)는 깨지지 않고 읽히되 마실 수는 없다 — report 에 어떻게 다뤘는지 적는다 (테스트)
  - 기준: 물은 그대로다 — 빈 컵을 들고 민물을 우클릭하면 찬다
  - 기준: spec/02_player/hunger-thirst.md · 05_craft/facilities.md 를 **사람 결정으로 고쳐 적고** 낡은 「붓는다」 줄을 지운다
  - 기준: 3단 한도를 넘지 않는다 — 컵은 재료 한 칸일 뿐 단계를 늘리지 않는다 (레시피 깊이 검사)
- [x] 4. (앞 단계 QA) 주민의 drink 가 맨 음료만 받는다
  - 기준: 주민의 drink 가 맨 음료만 받는다 — automation/villagers/villager.gd:352 drink(item) 은 손에 든 맨 「beer」 같은 이름을 덜어 마시는데, 이제 술은 컵에 든 채로만 나온다(Cup.filled). 지금은 게임 코드가 부르지 않아(언제 마시나 미정) 막지 않지만, 주민이 마실 때가 오면 음료가 든 컵 칸을 찾아 한 잔으로 쓰도록 Cup.counts_as 로 세게 고치고, tests/craft/stations/test_cook_table.gd:214 의 v.hands.add({item="beer"}) 도 Cup.filled(Cup.WOODEN, "beer") 로 바꿔 단언하라
  - 기준: (앞 단계 QA) 낡은 주석 「컵에 담아 마신다」 — game/life/food/food_catalog.gd:11 의 drink 필드 설명이 G-210.2 시절 말 그대로다. 「컵에 든 채로 나와 컵에서 마신다 (G-214.3)」로 고쳐라

## [x] G-215 주민이 장애물을 돌아간다 · 대상에 더 붙는다 (사람이 플레이해 보고 2026-10-14)
- 차선: 3d-lane2
- spec: spec/07_automation/npc-villagers.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- 사람이 짚었다: 「NPC 문제가 **저번 농사 할 때랑 같은 문제**가 발생했는데 **다른 직업들도 똑같이 직선으로만** 움직여. 이동 거리를 계산하고
  가야 하는데 그냥 앞으로 쭉 가니까 **앞에 장애물이 있으면 머리를 박고 있어**」 · 「NPC **오브젝트와의 상호작용 거리를 더 가깝게**」
- **까닭은 이미 짚었다** (2026-10-14): 농부(`field_care.gd`) · 모으기(`gather_work.gd`) · 거닐기(`stroll.gd`)는 걸을 땅(`PathWalk`)을 타는데,
  **`cleaning_work.gd` · `crafting_work.gd` · `harvest_work.gd` · `mining_work.gd` 는 안 탄다** — 그 넷이 곧장 간다.
  농사 때(G-165 ~ G-168) 고친 길을 다른 직업이 안 물려받았다
- [x] 1. 모든 직업이 같은 걷기 하나를 탄다
  - 기준: 나무꾼 · 광부 · 청소부 · 제작자(와 앞으로 생길 직업)가 **농부와 같은 길 찾기**로 간다 — 직업마다 따로 걷지 않는다.
    걷기가 **한 곳**에 있고 직업은 「어디로」만 정한다 (테스트로 지킨다: 직업 파일이 스스로 위치를 옮기지 않는다)
  - 기준: 대상과 주민 사이에 **벽 · 횃불 · 상자 · 시설**이 있으면 **돌아서** 닿는다 — 직업마다 검사 하나씩 (테스트)
  - 기준: 길이 정말 없으면 박고 서 있지 않고 **「길이 막혔다」**를 띄우고 다른 일감으로 넘어간다 (테스트)
  - 기준: 찍어서 확인한다 — 장애물을 돌아 나무 · 바위로 가는 주민
- [x] 2. 대상에 더 붙어서 일한다
  - 지금 주민의 손은 2m (`villager.gd HAND`, 임시 — G-210.5 에서 몸통 겉으로 재게 했다). 사람이 **더 가깝게**를 원한다
  - 기준: 손 길이를 **걸을 땅이 허락하는 가장 가까운 자리**까지 줄인다 — 걸을 땅은 장애물 둘레를 깎아 둔다(`animal_nav.gd AGENT_RADIUS`
    · `WALL_PAD`). 그보다 짧으면 영영 못 닿는다. **재서 정하고** 앞뒤 값과 까닭을 report 에 적는다 (목표 1m 남짓)
  - 기준: 줄인 뒤에도 **모든 대상(나무 · 바위 · 밭 · 상자 · 시설 · 축사)에 닿는다** — 대상마다 검사 (테스트)
  - 기준: 값은 「임시」로 적는다 — 사람이 플레이해 보고 다시 정한다
  - 기준: (앞 단계 QA) 거닐기가 아직 제 걷기를 쥔다 — game/automation/villagers/stroll.gd 가 Villager.walker() 로 _walk 를 따로 두고 _walk.plan · _walk.step 을 직접 부른다. 직업이 아니라 이번 기준 밖이지만 「걷기는 한 곳」 원칙과 어긋난다. JobWork 의 _go · _stride 를 따로 떼어 둘이 같이 쓰게 하거나, stroll.gd 머리 주석에 왜 따로인지 적어라
  - 기준: (앞 단계 QA) 사람이 본 처박힘을 재현하지 못했다 — report 의 how 에 적힌 대로 네 직업은 이미 PathWalk 를 탔고, 사람의 섬(AsPlayedRoom)에서도 처박힘이 0.1초 아래였다. 이번 단계는 걷기를 모으고 길 없을 때 넘어가기를 더했을 뿐, 사람이 2026-10-14 본 증상의 원인은 아직 짚지 못했다. 다음 단계에서 사람이 쓰던 저장본(나무꾼 · 광부가 담긴 것)이나 실제 섬 지형(나무 밀집 · 절벽 · 상자 곁)으로 재현 테스트를 짜 원인을 찾아라. current.md 의 「까닭은 이미 짚었다」 줄은 코드와 달랐다는 것도 spec history 에 남아 있으니 함께 보라
- [x] 3. (앞 단계 QA) gather_work.gd 주석이 낡았다
  - 기준: gather_work.gd 주석이 낡았다 — game/automation/villagers/gather_work.gd:34 「Villager.hand, 처음 HAND 2m」를 1.2m(G-215.2)로 고쳐라
  - 기준: (앞 단계 QA) 손 검사가 직업 잣대를 다시 짠다 — test_villager_hand_close.gd 의 _resource_far · _station_far 가 GatherWork.reaches_from · CraftingWork.station_reach 를 부르지 않고 같은 셈을 테스트 안에서 다시 한다(높이 차 UP_REACH · slack 은 안 본다). 직업 쪽 잣대가 바뀌어도 테스트는 모른다. 각 선 자리 s 에서 GatherWork.new().reaches_from(v, s, handle, Villager.WAYPOINT) · CraftingWork.station_reach(v, s, st, Villager.WAYPOINT) 가 true 인지 함께 단언하라
- [x] 4. (앞 단계 QA) 손 검사에 고아 노드 2개가 생겼다
  - 기준: 손 검사에 고아 노드 2개가 생겼다 — test_villager_hand_close.gd 의 _set_up 이 Villager.new() · Harvest.new() 를 트리에 안 넣고 만들어, GUT 가 「2 orphans」를 보고한다(HEAD 판에는 없었다). 테스트마다 _set_up 을 부르니 after_all 의 free 전까지 고아로 센다. add_child(villager) · add_child(gather.harvest) 로 트리에 넣거나 autofree 로 감싸 orphans 0 이 되게 하라
  - 기준: (앞 단계 QA) slack 을 빼도 손 검사가 통과한다 — reaches_from · station_reach 의 '+ slack' 을 '+ 0.0' 으로 바꿔도 6개 다 통과했다. 선 자리가 slack 없이도 손 안이라 WAYPOINT 를 넘기는 뜻이 지켜지지 않는다. 기준 문장대로는 됐으니 막지는 않는다 — 지키려면 길 끝에서 WAYPOINT 만큼 더 멀리 선 자리를 하나 넣어 slack 0 이면 false, WAYPOINT 면 true 인지 단언하라


## [x] G-216 제작 창을 표로 · 음료가 든 컵은 음료로 보인다 (사람이 플레이해 보고 2026-10-14)
- 차선: 3d-start
- spec: spec/05_craft/crafting-stations.md · 02_player/hunger-thirst.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- [x] 1. 음료가 든 컵은 그 음료로 보인다
  - 사람이 짚었다: 「음료 만들면 **나무컵으로 나오는데** 해당 쥬스가 나와야지 왜 나무컵으로 나오는 거야」
  - **까닭은 이미 짚었다** (2026-10-14): 음료는 `{item = "wooden_cup", water = 3, drink = "fruit_juice"}` 칸으로 나오는데(`player/hunger_thirst/cup.gd:17~20`),
    칸을 그리는 `ui/hud/item_slot.gd:58` 이 `"ITEM_%s" % entry["item"]` 만 써서 「나무컵」으로 보인다. `drink` 를 보지 않는다
  - 기준: 음료가 든 컵 칸은 **그 음료 이름**(과일 주스 · 맥주 …)으로 보인다 — 가방 · 핫바 · 상자 · 시설 결과 칸 · 손에 든 것 · 줍는 알림 · 아이템 설명 모두 (테스트, 자리마다)
  - 기준: 남은 모금과 어느 컵인지가 함께 보인다 — 예: 「과일 주스」 + 2/3 · 작게 「나무컵」 (꼴은 임시)
  - 기준: 검색 칸에 「주스」를 치면 주스가 든 컵이 걸린다 (상자 창 · 시설 창 — ItemSearch 가 같은 이름을 본다) (테스트)
  - 기준: 칸 이름을 고르는 곳을 **한 곳**에 둔다 (Cup 이나 아이템 이름 함수 하나) — 자리마다 따로 `drink` 를 보지 않는다
  - 기준: 물이 든 컵 · 빈 컵은 그대로 컵 이름이다 (테스트)
  - 기준: 찍어서 확인한다 — 결과 칸 · 가방에 든 과일 주스
- [x] 2. 제작 창을 표로 — 칸에 숫자를 넣으면 만든다
  - **사람 결정 2026-10-14** (G-214.2 의 「줄을 골라 아래 만들기 바에서 정한다」를 바꾼다): 「제작 UI가 이상한데
    `이름 | 재료 | 만들기 | 유지` / `침대*1 | 목재*10 | -칸+ | -칸+` — **칸에 숫자 넣으면 자동으로 만들게**」
  - 기준: 시설 창의 레시피 목록이 **네 칸짜리 표**다 — 머리줄 「이름 · 재료 · 만들기 · 유지」, 줄마다 `나오는 것 ×n | 재료 ×n … | − [칸] + | − [칸] +` (테스트)
  - 기준: 「만들기」 칸에 숫자를 넣으면(치거나 − + 로) **단추 없이** 그만큼 만들기 시작한다 — 다 만든 만큼 칸의 숫자가 줄고 0 이면 멈춘다 (테스트)
  - 기준: 「유지」 칸에 숫자를 넣으면 그 수를 유지하는 규칙이 된다 — 지금의 「N개 유지」(제작자 주민이 보는 재고 규칙, G-211)와 **같은 값**이다 (테스트)
  - 기준: 칸은 숫자만 받는다 — W · A · S · D 같은 글자는 안 들어가고, 창을 닫으면 캐릭터가 걷지 않는다 (HeldKeys, G-214.1 과 같이) (테스트)
  - 기준: 재료가 모자라도 숫자는 넣을 수 있고, 재료가 모이면 이어 만든다 — 모자란 재료는 지금처럼 빨갛게 보인다 (테스트)
  - 기준: 아래 **만들기 바와 「레시피를 고른다」 안내는 지운다** — 줄을 누르는 고르기도 없앤다. 대기열 줄을 남기면 표의 숫자와 늘 같아야 한다 (테스트)
  - 기준: 옛 저장본의 대기열 · 유지 규칙이 표 칸에 그대로 읽힌다 (테스트)
  - 기준: spec/05_craft/crafting-stations.md 를 **사람 결정으로 고쳐 적고** 낡은 「만들기 바」 줄을 지운다
  - 기준: 찍어서 확인한다 — 제작대 창 (숫자를 넣은 줄 · 빈 줄 · 재료가 모자란 줄)
  - Claude 가 정했다(작은 꼴): 칸 너비 · 글자 크기 · 머리줄 꼴은 임시. 숫자는 치는 대로(Enter 없이) 반영한다
  - 기준: (앞 단계 QA) 줍는 알림·아이템 설명은 아직 없다 — 이번 기준에 적힌 두 자리가 게임에 없어서 테스트도 없다. 그 화면을 만들 때는 Cup.name_key(entry) 로 이름을 고르고, test_drink_cup_names.gd 에 그 자리를 하나 더하라 (지금 고칠 것은 없다)
- [x] 3. (앞 단계 QA) test_station_make_bar.gd 지움이 반쯤 스테이지됐다
  - 기준: test_station_make_bar.gd 지움이 반쯤 스테이지됐다 — git rm --cached 로 .gd 만 스테이지에, .gd.uid 는 작업 트리에서만 지워졌다. 커밋할 때 두 파일을 같이 git add -A 로 올려라 (해는 없다)

## [x] G-217 주민이 벽 · 상자를 돌아가지 않고 멈춘다 — 실제 섬에서 재현한다 (사람이 플레이해 보고 2026-10-14)
- 차선: 3d-lane2
- spec: spec/07_automation/npc-villagers.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- 결정: G-215 의 물음(처박힘을 본 자리)에 사람이 답했다 — 「**나무와 NPC 사이에 벽이라던가 상자 이런 방해물이 있으면 돌아가지 않고 멈춘다**」 (2026-10-14)
- [x] 1. 사람이 본 그대로 재현한다
  - G-215 의 `test_villager_jobs_detour.gd` 는 **평평한 시험 섬**(flat_shape.gd)에 판을 미리 깔고 굽는다 — 그래서 통과했다. 사람의 판과 다른 점을 하나씩 판으로 짠다
  - 기준: **실제 섬 땅**(게임이 쓰는 섬 · 높낮이 있는 땅, `AnimalNav.bake` 의 on_terrain)에서, 나무꾼과 나무 사이에 **벽 · 상자**를 둔 판을 짠다 (테스트)
  - 기준: 벽 · 상자를 **주민이 자리 잡은 뒤에** 플레이어가 짓는 길(실제 건축 · 놓기)로 놓는다 — 걷는 중에 놓는 판도 (테스트).
    다시 굽는 곳: `automation/villagers/villagers.gd:262~` 의 STALE 표 → `_bake_home` (216 줄)
  - 기준: 나무가 걸을 땅 끝 가까이에 있는 판도 — 걸을 땅은 거주지 방 가운데에서 `max(NAV_HALF, reach_m + NAV_MARGIN 5m)` 만 굽는다 (villagers.gd:216).
    돌아가는 길이 그 밖으로 나가야 하면 길이 없다고 서는지 본다 (테스트)
  - 기준: 사람 저장본(`game/tests/automation/villagers/fixtures/g215/`)에 나무꾼을 넣고 그 섬의 나무 · 상자 곁으로도 돈다 (테스트)
  - 기준: 서 있는 시간을 재서 report 에 판마다 적는다. 재현이 되면 까닭을 파일:줄로 적는다. **짐작으로 고치지 않는다**
  - 기준: 하나도 재현이 안 되면 needs_decision 으로 — 무엇을 해 봤는지 적는다 (사람에게 그때 저장본을 받는다)
- [x] 2. 돌아간다
  - 기준: 1단계에서 재현한 판이 모두 **돌아서** 나무에 닿는다 — 같은 걷기(JobWork._go)를 고친다. 직업 파일에 따로 걷기를 두지 않는다 (테스트)
  - 기준: 나무꾼만이 아니라 광부 · 청소부 · 제작자 · 농부도 같은 판(실제 섬 + 나중에 놓은 벽 · 상자)으로 돈다 (테스트)
  - 기준: 찍어서 확인한다 — 실제 섬에서 상자 · 벽을 돌아 나무로 가는 주민
  - 기준: spec/07_automation/npc-villagers.md 에 까닭과 고친 것을 적는다
  - 기준: (앞 단계 QA) 걸을 땅 끝 판 머리 주석의 x -385 가 틀리다 — test_villager_g217_real_island.gd 머리 주석·NAV_WEST 주석은 「서쪽 끝 x -385」라 하지만 ㄷ자 판 줄(z -222~-226)에서 걸을 땅은 x -383.5 에서 끝나고, 테스트가 찍는 「서쪽 끝」(-387.5, -228.75)은 북 팔 바깥 줄 값이다. NAV_WEST 단언(west.x > -388)은 그 판의 줄을 보지 않는다. map_get_closest_point 를 z = EDGE_TREE_AT.y(-225) 줄에서 재고, 그 x 가 남 팔 서쪽 끝(-384)보다 동쪽인지(입구가 걸을 땅 밖인지) 단언하도록 바꾸고 주석을 -383.5 로 고쳐라
  - 기준: (앞 단계 QA) 비탈 위 나무로 가다 땅에 2.3초 비빈다 — 걸을 땅 끝 판에서 뒤에 고른 나무 (-365.1, 31.6, -200.1)로 가다 (-341, 27.75, -195.3)에서 Terrain3D 에 2.3초 박혔다(report·로그). 벽·상자와 상관없는 다른 처박힘이다 — 그 자리에서 길 점과 몸 높이를 재는 판을 짜 재현하고 까닭을 파일:줄로 적어라
- [x] 3. (앞 단계 QA) 비탈 칸 처박힘 고치기가 남았다
  - 기준: 비탈 칸 처박힘 고치기가 남았다 — test_villager_g217_real_island.gd test_slope_tree_after_the_edge_rubs_on_terrain 은 「G-217.2 재현 — 고치면 뒤집는다」로 지금 흠을 단언한다. 주민 걸을 땅(villagers.gd:225·230 → animal_nav.gd:121~124)도 지형 충돌처럼 a–d 대각선으로 가르게(on_terrain) 해 칸 (341,414) 앞면이 걸을 땅에서 빠지거나 몸이 오르게 하고, 그 테스트를 「붙었다 · 비빈 <0.5초」로 뒤집어라
  - 기준: (앞 단계 QA) 광부 벽 판은 한 자리에만 서는 상수다 — test_villager_g217_every_job.gd _wall_row 는 광부가 z -200 북쪽 3m 안 · x -281~-272 에 들 때만 놓는다(WALL_PLATES 고정). 저장본이나 광부의 바위 고르기가 바뀌면 placed_at < 0 으로 떨어져 까닭이 흐려진다. 실패 메시지에 「광부가 벽 줄 곁을 지나지 않았다 — 고른 바위 위치」를 찍어 두면 좋다 (막는 것은 아니다)
- [x] 4. (앞 단계 QA) 주민 몸 비탈 50° 의 곁효과를 지키는 테스트가 없다
  - 기준: 주민 몸 비탈 50° 의 곁효과를 지키는 테스트가 없다 — Villager.FLOOR_MAX 를 45→50° 로 올려 주민 몸이 걸을 땅 밖의 46~50° 면(둔덕 · 절벽 어귀)에도 올라설 수 있다. 막는 것은 아니다(전체 통과). 걸을 땅 밖으로 밀려난 주민이 46~50° 면을 올라 걸을 땅이 없는 곳에 서는 일이 없는지, 또는 서도 PathWalk.plan 이 「길이 막혔다」로 다루는지 보는 판을 test_villager_g217_real_island.gd 나 test_villager_stairs.gd 곁에 하나 두면 좋다

## [x] G-218 길 다시 묻기 · 토대에 끼인 주민 · 걸을 땅을 섬 전체로 · 주민끼리 비켜 간다 (Claude 제안 · 사람이 좋다고 했다 2026-10-14)
- 차선: 3d-lane2
- spec: spec/07_automation/npc-villagers.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- [x] 1. 걷는 중에 길을 다시 묻는다 · 끼여도 곧장 포기하지 않는다
  - 기준: **걷는 중에 걸을 땅이 바뀌면 길을 다시 묻는다** — 지금은 길을 한 번만 묻는다(`core/path_walk.gd:52` plan). 상자 · 벽을 놓아 다시 구워도 옛 길로 가다 처박힌다.
    `NavigationServer3D.map_changed` 를 듣거나 걷는 동안 잠깐마다(임시 0.5 ~ 1초) 다시 묻는다 — PathWalk 한 곳에서 (테스트: 걷는 중 앞에 상자를 놓으면 돌아간다)
  - 기준: **끼이면 곧장 포기하지 않는다** — 지금은 STUCK 이면 그 대상을 바로 뺀다(`gather_work.gd:95` _blocked). 먼저 길을 다시 묻고, 몇 번(임시 2번) 더 끼일 때만 뺀다 (테스트)
  - 사람 결정 2026-10-14: 대상은 지금처럼 **직선 거리로 고른다** — 걸을 길이로 고르던 때 「집 안 벽 뒤에 밭이 있으면 계속 걸어갔다」. 길이로 바꾸지 않는다
- [x] 2. 토대에 끼인 주민을 위로 올린다
  - 사람이 봤다 (2026-10-14 15:00, 플레이 판 ba69d7dc): 「NPC가 이렇게 **토대에 끼일 때**가 있는 것 같은데 토대에 끼이면 **위로 올려 줄래**? 아직 다른 데 끼인 건 못 봤네」 —
    주민 하람(24살)이 방 바닥 토대 밑으로 빠져 **머리 꼭대기만** 토대 위로 보였다
  - 사람 저장본 · 사진: `~/Desktop/naru-saves/G-218.2-2026-10-14-1500/` (세계 1790014507_e428c509 · 캐릭터 1789959457_2a520c0c · stuck.png). 테스트 판으로 쓸 때는 game/tests/…/fixtures 로 복사한다 — 사람 파일 자리(app_userdata)는 건드리지 않는다
  - 기준: **재현부터** — 저장본을 읽어 하람이 토대 밑에 있는지 · 어떻게 거기 들어가나를 판으로 짠다. 후보: 주민이 선 자리에 토대를 놓았다 · 불러올 때 주민이 토대보다 먼저 서서 땅까지 떨어졌다 · 걷다가 토대 가장자리(턱)로 파고들었다. 맞는 것을 파일:줄로 report 에 적는다 (테스트)
  - 기준: 주민 몸이 토대(와 놓은 바닥판) 안 · 밑에 끼이면 **그 토대 위로 올린다** — 놓을 때 · 불러올 때 · 걷는 중 모두 (테스트, 경우마다)
  - 기준: 올린 뒤 주민이 곧 다시 일을 한다 — 걸을 땅 위에 선다 (테스트)
  - 기준: 위가 막혀 올릴 수 없으면(벽 · 다른 토대) 가장 가까운 빈 걸을 땅으로 옮긴다 (테스트)
  - 기준: 찍어서 확인한다 — 사람 저장본을 열어 토대 위에 선 하람
  - 기준: (앞 단계 QA) g217_every_job 주석이 낡았다 — game/tests/automation/villagers/test_villager_g217_every_job.gd 13 · 16 · 179 줄 주석과 249 줄 단언 메시지가 아직 「_stride 의 다시 묻기 · JobWork._stride」라 적는다. 다시 묻기는 이제 PathWalk.step 에 있다 — 그 넷을 PathWalk.step 으로 고쳐라
- [x] 3. 걸을 땅을 타일로 — 섬 전체를 덮고 바뀐 타일만 다시 굽는다
  - 지금은 거주지 방 가운데에서 `max(NAV_HALF, reach_m + NAV_MARGIN 5m)` 만 굽는다(`automation/villagers/villagers.gd:216`) — 일 범위 가장자리 대상은 돌아갈 땅이 없고, 무엇을 놓을 때마다 그 땅 전체를 다시 굽는다
  - 기준: 주민이 걷는 땅이 **섬 전체**를 타일로 덮는다 — 일 범위 끝의 나무도 벽 바깥으로 돌아서 닿는다 (테스트)
  - 기준: 무엇을 놓거나 헐면 **그 자리를 덮는 타일만** 다시 굽는다 — 다른 타일은 그대로다 (테스트: 굽힌 타일 수)
  - 기준: 굽는 데 드는 시간을 재서 report 에 앞뒤로 적는다 — 놓을 때 화면이 끊기지 않는다 (한 프레임 안, 아니면 나눠서 굽는다)
  - 기준: 짐승의 땅(기본 층)과 주민의 땅(FOLK_LAYERS)은 지금처럼 섞이지 않는다 (테스트)
  - 기준: 옛 저장본이 그대로 읽히고 주민이 곧 걷는다 (테스트)
  - 기준: (앞 단계 QA) 놓을 때 판이 _on_placed 갈래를 따로 지키지 않는다 — test_placed_on_a_standing_villager_lifts_them 은 놓은 뒤 20 물리 프레임을 기다리는데 Villager.READY_FRAMES 가 15라, Villagers._on_placed 의 unstick_soon() 을 지워도 박자(_on_beat)가 잡아 통과할 것이다. 놓은 뒤 기다림을 READY_FRAMES 보다 짧게(예: 3 프레임) 하거나 놓기 직전 박자를 맞춰, 곧바로 올렸는지 단언하라
  - 기준: (앞 단계 QA) 위가 막힌 판이 벽만 본다 — 기준의 「다른 토대」로 막힌 경우(끼인 판 바로 위에 또 다른 토대가 겹쳐 선 자리)는 test_blocked_above_… 에 없다. 위층 토대로 막은 판을 하나 더 넣어 nearest_free 로 빠지는지 단언하라
- [x] 4. 주민끼리 비켜 간다
  - 기준: 마주 오는 두 주민 · 한 상자 앞의 두 주민이 서로 비벼 끼이지 않고 비켜 간다 — Godot 의 회피(NavigationAgent3D avoidance · RVO) 또는 같은 꼴 (테스트)
  - 기준: 걷기는 여전히 PathWalk 한 곳 — 직업 파일은 「어디로」만 정한다 (테스트)
  - 기준: 찍어서 확인한다 — 좁은 문을 엇갈려 지나는 두 주민
  - 기준: spec/07_automation/npc-villagers.md 에 적는다
  - 기준: (앞 단계 QA) project.godot 내비 설정 바꾼 까닭이 없다 — game/project.godot 에 [navigation] 3d/merge_rasterizer_cell_scale=0.01 을 새로 넣었는데 코드 · spec 어디에도 왜 넣었는지 · 짐승의 땅에도 걸리는지가 적혀 있지 않다(grep 해도 project.godot 한 줄뿐). 전역 지도 설정이라 짐승 길찾기에도 걸린다. FolkGround 머리 주석이나 spec/07_automation/npc-villagers.md 의 FolkGround 줄에 까닭(조각 변 꼭짓점을 잇기 위한 것인지)과 「임시」 여부를 적어라
  - 기준: (앞 단계 QA) G-217 테스트의 시작 자리를 옮겼다 — test_villager_g217_every_job.gd 의 OUT_AT 을 남동쪽 비탈 (-269,-188) 에서 (-266,-204) 평지로 옮겨 통과시켰다. 줄 놓기 자리 찾기 문제라 주석에 까닭은 적었지만, 비탈 아래에서 출발해 일을 시작하는 경우는 이제 아무 테스트도 안 본다. 옛 자리에서도 일을 시작하는지(줄 놓기 없이 r.ok 만) 보는 판을 하나 더 두는 것을 고려하라
- [x] 5. (앞 단계 QA) 「같은 쪽이면 앞사람보다 빨리 안 걷는다」를 보는 테스트가 없다
  - 기준: 「같은 쪽이면 앞사람보다 빨리 안 걷는다」를 보는 테스트가 없다 — spec 과 PathWalk 머리 주석이 이 갈래(_give_way 의 마지막 `if best <= AVOID_NEAR: return h * minf(...)`)를 약속하지만 test_villager_g218_give_way.gd 에는 마주 오기 · 문 · 상자만 있다. 트인 땅에서 두 주민을 1m 간격으로 같은 쪽 한 자리 근처(서로 다른 끝점)로 걷게 하고, 뒤 주민의 비빈 시간 < PRESS_LIMIT · 두 몸 가장 가까운 거리 > 몸 지름을 보는 판을 더하라
  - 기준: (앞 단계 QA) 선 몸을 돌아갈 옆이 막혔을 때의 갈래를 보는 테스트가 없다 — _give_way 의 `if not _clear(body, away, true): return Vector3.ZERO` (문구멍에 일하느라 선 주민) 는 끼임 시계로만 끝난다. 문 한가운데에 주민 하나를 세워 두고(걷지 않음) 다른 주민이 문을 지나려 할 때 밀어 비비지 않고(pressed < PRESS_LIMIT) STUCK 으로 끝나는지 보는 판을 더하라
- [x] 6. (앞 단계 QA) 같은 쪽 판의 pressed 단언은 그 갈래를 못 잡는다
  - 기준: 같은 쪽 판의 pressed 단언은 그 갈래를 못 잡는다 — 같은 쪽 갈래를 `return v` 로 지워도 wb.pressed < PRESS_LIMIT 는 그대로 통과했다(거리 단언 둘만 떨어졌다). 판 전체는 거리 단언으로 지켜지니 막는 것은 아니다. test_villager_g218_give_way.gd 의 그 판 주석에 「이 갈래를 지키는 것은 거리 단언 — pressed 는 덤」이라 적어 두거나, 앞 주민을 아예 세운 채(목적지 바로 앞) 따라붙게 하는 판으로 pressed 도 걸리게 하라
- [x] 7. (앞 단계 QA) 주석이 잰 회차를 잘못 적었다
  - 기준: 주석이 잰 회차를 잘못 적었다 — test_villager_g218_give_way.gd:161 의 「(G-218.6 QA 가 재 봤다)」는 틀렸다. `return v` 로 지워 본 것은 단계 5 를 채점한 QA(G-218.5 QA)다. 「(G-218.5 QA 가 재 봤다)」로 고쳐라 (막는 것은 아니다)

## [x] G-219 배고픔은 체력을 깎고 목마름은 -30% — 바닥이어도 크게 느려지지 않는다 (사람 결정 2026-10-14)
- 차선: 3d-start
- spec: spec/02_player/hunger-thirst.md · 08_combat/damage-death.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- **사람 결정 2026-10-14** (2026-09-23 의 「하나라도 바닥이면 걷기 속도 · 둘 다 바닥이면 -30%」를 바꾼다):
  「이동 속도가 엄청 느려지면 스트레스 유발하니까. **배고픔이 0 이 되면 체력이 줄어들게** 만들고 **목마름은 0 이 되면 이동 속도 -30%** 로 하자.
  체력이 줄어드는 비율은 잘 모르겠네. 나중에 체력에 대한 건 추가할 거라 일단 알아서 만들어 줘」
- 지금 코드: `player/hunger_thirst/hunger_thirst.gd:89` speed_factor — 하나 바닥 WALK_FACTOR 0.55 · 둘 다 0.55 × 0.7. 게이지 색(`ui/hud/need_gauge.gd:16` 초록 > 50% · 주황 > 20% · 빨강)은 속도와 상관없다 — 그대로 둔다
- [x] 1. 목마름 바닥 -30% · 배고픔은 속도를 안 건드린다
  - 기준: 배고픔은 **속도에 손대지 않는다** — 배고픔이 바닥이어도 기본 속도(달리기) 그대로다 (테스트)
  - 기준: 목마름이 **바닥(0)이면 기본 속도에서 -30%** (배율 0.7, 임시). 조금이라도 남았으면 그대로다 (테스트)
  - 기준: 둘 다 바닥이어도 속도는 -30% 하나뿐이다 — 더 겹쳐 느려지지 않는다 (테스트)
  - 기준: WALK_FACTOR · BOTH_EMPTY_SLOWDOWN 처럼 안 쓰게 된 값과 그것을 부르는 곳(테스트 · 디버그 상자 dev_box.gd)을 지운다 — `grep` 으로 저장소 전체를 훑는다
  - 기준: 버프 음식은 지금처럼 그 위에 곱해진다 (테스트)
- [x] 2. 배고픔이 바닥이면 체력이 준다
  - 기준: 배고픔이 바닥인 동안 체력이 **천천히** 준다 — Claude 가 정했다(임시): **5초에 1** (체력 100 이면 가득에서 8분 남짓). 값은 hunger_thirst.gd 한 곳에, 「임시」로 (테스트)
  - 기준: **배고픔만으로는 죽지 않는다** — 체력 1 에서 멈춘다 (2026-09-23 「굶어 죽지 않는다」는 사람이 거두지 않았다. Claude 가 정했다(임시) — 사람이 체력을 더할 때 다시 정한다) (테스트)
  - 기준: 먹어서 배고픔이 조금이라도 차면 곧 멈춘다. 깎인 체력은 먹는다고 돌아오지 않는다 — 지금 체력이 돌아오는 길을 그대로 쓴다 (테스트)
  - 기준: 깎이는 것이 보인다 — 체력 바가 줄고, 배고픔 게이지 곁이나 알림 줄에 「배가 고파 체력이 준다」 같은 말이 뜬다 (번역 키 · 꼴은 임시) (테스트)
  - 기준: 껐다 켜도 이어진다 — 저장본의 배고픔 0 · 체력이 그대로 읽히고 다시 깎이기 시작한다 (테스트)
  - 기준: spec/02_player/hunger-thirst.md 를 **사람 결정으로 고쳐 적고** 낡은 「하나라도 바닥이면 걷기 속도」 · 「둘 다 바닥이면 -30%」 줄을 지운다. damage-death.md 에 배고픔이 체력을 깎는 것을 한 줄 더한다
  - 기준: 찍어서 확인한다 — 배고픔이 바닥인 HUD (체력 바 · 알림)
  - 기준: (앞 단계 QA) hunger-thirst.md 목적·규칙이 낡았다 — spec/02_player/hunger-thirst.md 4행 「배고픔 · 목마름이 이동 속도를 정한다」와 12행 「굶어 죽거나 말라 죽지 않는다 — 느려지기만 한다」는 배고픔이 이제 속도를 안 건드리므로 맞지 않다. 이 묶음의 「배고픔 0 → 체력 감소」 단계에서 함께 고쳐라: 목적은 「목마름이 속도를, 배고픔이 체력을」로, 「느려지기만 한다」는 배고픔 쪽 결과(체력)로 바꾼다. hunger_thirst.gd 머리 주석 「바닥이어도 죽지 않는다」도 그 단계에서 체력 규칙과 맞춘다
- [x] 3. (앞 단계 QA) 체력 1 에서도 「체력이 준다」 알림이 남는다
  - 기준: 체력 1 에서도 「체력이 준다」 알림이 남는다 — hunger_thirst.gd _update_alert 의 want 가 is_hungry() and health != null 뿐이라, 체력이 STARVE_FLOOR 에 닿아 더 안 깎이는 동안에도 「배가 고파 체력이 준다」가 뜨고 is_starving() 이 참이다 (죽은 동안에도). 막는 것은 아니다 — 굶는다는 알림으로 남겨 둘지, 문구를 「배가 고프다」 쪽으로 다듬을지 정해 test_starving.gd 에 체력 1 일 때의 알림 단언을 하나 더해 두면 좋다
- [x] 4. (앞 단계 QA) 차선끼리 섬 캐시를 같이 써 서로 지운다
  - 기준: 차선끼리 섬 캐시를 같이 써 서로 지운다 — test_round_trip.gd 의 BAKE_ONLY 는 증상만 뺐다. 원인은 user://island_cache 를 모든 체크아웃(차선)이 같이 쓰고 IslandCache.prune 이 다른 차선의 판을 지우는 것이라, 다른 테스트도 같이 돌 때 구울지 읽을지가 바뀌어 흔들릴 수 있다. world/terrain/island_cache.gd 에서 캐시 경로를 체크아웃마다 가르거나(예: 프로젝트 경로 해시를 폴더 이름에 붙임) prune 이 자기 차선 판만 지우게 하고, 두 경로가 달라지는 것을 test_island_cache.gd 에 단언으로 더하라
- [x] 5. (앞 단계 QA) prune 테스트가 옛 코드에서도 통과한다
  - 기준: prune 테스트가 옛 코드에서도 통과한다 — test_island_cache.gd 의 test_prune_leaves_another_checkout_alone 은 prune 이 원래부터 받은 폴더만 보므로 G-219.4 전 코드에서도 통과해 아무것도 지키지 않는다(기준의 단언은 test_cache_dir_differs_per_checkout 이 맡아 통과에는 문제없다). 지우거나, IslandCache.save(_shape) 를 기본 DIR 로 부른 뒤 dir_for(다른 경로) 폴더의 판이 남는지 보는 식으로 실제 DIR 갈림을 거치게 고쳐라
  - 기준: (앞 단계 QA) 공용 폴더의 옛 저장본이 영영 남는다 — user://island_cache 바로 아래의 옛 .bin(섬마다 수십 MB)은 이제 어느 prune 도 보지 않는다. 모든 차선이 새 코드로 넘어간 뒤 지울 길을 만들어라: 예) IslandCache 에서 ROOT 바로 아래의 VERSION 이 낮거나 파일(폴더 아닌) 저장본을 지우는 정리 함수와 그 테스트(임시 폴더로)
- [x] 6. (앞 단계 QA) 실패하면 진짜 user:// 에 폴더가 남는다
  - 기준: 실패하면 진짜 user:// 에 폴더가 남는다 — test_island_cache.gd 의 test_save_leaves_another_checkout_alone 은 진짜 user://island_cache 아래 다른 차선 폴더를 만들고 맨 끝에서만 지운다. dir_for 가 깨져 그 폴더가 이 차선 DIR 과 같아지면 내 판이 들어 있어 remove_absolute(other) 가 실패해 폴더가 남는다(QA 가 깨 봤을 때 island_cache/same 이 남아 손으로 지웠다). 막는 것은 아니다 — after_each 나 테스트 끝에서 other 안의 파일을 모두 지운 뒤 폴더를 지우게 고쳐라

## [x] G-906 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-lane2 (Claude 가 고쳤다 2026-10-14 — G-218 잔일이라 고칠 코드가 차선 2 에만 있다. 차선 1 이 잘못 가져갔었다)
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. 지붕(놓은 바닥판)에 끼인 판이 없다 (G-218 단계 2)
  - 기준: 지붕(놓은 바닥판)에 끼인 판이 없다 — BuildPiece.is_floor(build_piece.gd:124)는 foundation · roof 둘을 보고 spec 도 「토대 · 지붕」이라 적지만, test_villager_g218_floor_stuck.gd 는 토대만 쓴다. 지붕 판 속에 몸을 둔 경우를 하나 넣어 그 윗면으로 올라서는지 단언하라
- [x] 2. 처음 덮기의 한 조각이 한 프레임을 넘는다 (G-218 단계 3)
  - 기준: 처음 덮기의 한 조각이 한 프레임을 넘는다 — report 에 사람 섬에서 가장 느린 한 조각 17.6ms(>16.7ms)가 있었다. bake_some 은 적어도 한 조각은 굽으니 불러온 직후 그 프레임은 끊긴다. 놓을 때 기준과는 상관없지만, 가파르거나 물 많은 조각의 굽는 시간을 줄이거나(TILE 을 줄이거나 물 추리기 더) 테스트에 slowest_us 상한을 걸어 두라

## [x] G-220 시설 대기열을 오른쪽으로 · 만드는 중인 것은 대기열에 남긴다 · 장비 이름 상의 · 하의 (사람이 플레이해 보고 2026-10-14)
- 차선: 3d-start
- spec: spec/05_craft/crafting-stations.md · 02_player/equipment.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- [x] 1. 대기열을 창 오른쪽으로 뺀다
  - 사람이 짚었다: 「제작하는 시설들 밑에 **대기열 있는 거 아래에 있으니** 좀 보기 이상해서 **오른쪽으로 따로 빼 줘**」
  - 기준: 시설 창의 대기열(만드는 중 · 기다리는 줄)이 레시피 표 **오른쪽 따로 된 칸**에 선다 — 표 아래에 없다 (테스트)
  - 기준: 창이 화면 안 · 핫바 위에 들어간다 (tests/ui/layout 이 잰다) · 대기열이 길면 그 칸만 스크롤된다 (테스트)
  - 기준: 출력 버퍼 · 남은 시간 막대도 어울리는 자리에 — 꼴은 Claude 가 정한다(임시)
  - 기준: 찍어서 확인한다 — 대기열에 셋이 든 제작대 창
- [x] 2. 만드는 중인 것은 대기열에 남는다
  - 사람이 짚었다: 「하나 만들고 나서 **0 으로 만들어도 취소가 안 됨**. 제작 중인 하나는 무조건 만드는 거 같은데 이렇게 해도 좋은데 적어도 **버그처럼 보이지 않게 대기열 하나는 유지**가 되게 만들어야 됨. 지금은 **이름만 없어짐**」
  - 기준: **재현부터** — 만들기 칸에 2 를 넣어 하나가 만들어지는 중에 0 으로 바꾸면 대기열 줄의 이름이 사라지는 것을 테스트로 잡는다
  - 기준: 만드는 중인 하나는 그대로 끝까지 만든다 (사람이 좋다고 했다). 그동안 대기열에 **그 한 줄이 이름 · 남은 시간과 함께 남는다** — 「만드는 중」이 보인다 (테스트)
  - 기준: 그 줄의 숫자는 1(만드는 중)로 보이고, 표의 만들기 칸도 그에 맞는다 — 0 을 넣었는데 1 로 남는 까닭이 보이게 (말 · 꼴은 임시) (테스트)
  - 기준: 다 만들면 그 줄이 사라진다. 유지 규칙이 걸려 있으면 유지 쪽 규칙대로다 (테스트)
  - 기준: (앞 단계 QA) spec 이 아직 대기열을 「아래」라고 한다 — spec/05_craft/crafting-stations.md 37줄 「아래 대기열 줄은 남는다 (차례 바꾸기)」가 G-220.1 뒤로 낡았다. 「오른쪽 칸의 대기열 줄은 남는다」로 고치고, 같은 절에 「대기열은 표 오른쪽 따로 된 칸 (사람이 짚었다 2026-10-14, G-220.1) — 길면 그 칸만 스크롤」 한 줄과 미정에 「오른쪽 칸 너비 · 차례 (임시 — Claude 가 정했다)」를 적어라
  - 기준: (앞 단계 QA) 막힌 까닭이 따로 된 줄처럼 보인다 — 대기열 줄이 HFlowContainer 라 좁은 칸에서 「재료가 모자라 건너뛴다」가 아랫줄로 흐르는데, 줄 사이 구분이 없어 workbench_ko.png 에서 「유지 테이블 …」 아래의 그 글자가 넷째 대기열 줄처럼 보인다. 흐른 까닭 글자를 들여 쓰거나(왼쪽 여백) 줄마다 배경 · 구분선을 주고, 줄 높이가 둘로 늘 때 그 줄 rect 안에 Why 가 드는지 단언하는 테스트를 더하라
- [x] 3. 장비 이름 — 갑옷은 상의 · 각반은 하의
  - 사람 결정 2026-10-14: 「장비를 **~상의 ~하의**로 변경」
  - 기준: 상의 칸에 끼우는 `*_armor` 는 「가죽 상의 · 청동 상의 · 철 상의 · 강철 상의」, 하의 칸의 `*_leggings` 는 「가죽 하의 · 청동 하의 · 철 하의 · 강철 하의」 (game/i18n/ko.po · en.po 도 Top · Bottoms 로) (테스트)
  - 기준: 아이템 키(leather_armor …)는 그대로 둔다 — 저장본이 깨지지 않는다. 이름만 바꾼다 (테스트)
  - 기준: 투구 · 방패 · 방어 수치는 그대로다
  - 기준: spec/02_player/equipment.md 의 「갑옷 · 각반」을 「상의 · 하의」로 고쳐 적는다. 계통도에 이름이 나오는 곳도 줄기 이름을 따른다
- [x] 4. (앞 단계 QA) 「하의은」 조사가 틀렸다
  - 기준: 「하의은」 조사가 틀렸다 — game/craft/recipes/workbench.json 의 _temporary 「상의 · 투구 · 하의은 가죽」과 game/tests/player/equipment/test_armor.gd 5줄 「상의 · 투구 · 하의은 상의 · 모자 · 바지칸」을 「하의는」으로 고쳐라 (각반은 → 하의은 으로 기계적으로 바꾼 흔적)

## [x] G-221 멀티 — 같은 게임에 있으면 한 팀 · 서버장이 길드장 (사람 결정 2026-10-14)
- 차선: 3d-start
- spec: spec/11_multiplayer/ (listen-server · character-world · permissions) · 00_core/roadmap.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- **사람 결정 2026-10-14**: 「이제 **멀티로 변경해야 할 차례**. 파티나 이런 거 필요 없이 이 게임은 **같은 게임 안에 있으면 팀**. 일단 **서버장이 길드장**이 되는 거고 **서버장을 제외한 나머지는 권한 박탈**. **UI 따로 만들 것**」
- 이미 정해진 것 (spec/11_multiplayer): 호스트 겸용 리슨 서버 · 내 섬 정원 12 (13번째 거부) · 캐릭터와 월드를 따로 두고 캐릭터를 데려간다 · 방문자 기본값 「못 건드림」 · 친구 co-op 은 방 코드 + 중개/릴레이(방식 미정)
- **사람 결정 2026-10-14**: 「**스팀에 일단은 출시하는 게 목표**」 — 접속은 **Steam 로비 + 방 코드**로 간다 (Steam 중계망이 NAT · 릴레이를 맡는다 — 우리 서버 없음).
  파트너 등록 전에는 Steam 시험용 앱 ID **480**(Spacewar)으로 시험한다 — 사람이 친구들과 붙여 본다
- 짜는 순서: 루프 테스트는 이 맥 안 headless 둘을 **ENet** 으로 붙여 돈다(Steam 은 계정 하나로 한 맥에서 둘을 못 붙인다). 그래서 **접속은 한 곳에** 두고 ENet · Steam 을 갈아 끼운다
- 테스트는 headless Godot 둘(서버 · 참가)을 이 맥 안에서 붙여 돈다. **사람 화면에 창을 띄우지 않는다** (LOOP.md 2절)
- [x] 1. 설계 — 무엇이 어디서 돌고 무엇을 맞추나
  - 기준: 상태를 바꾸는 모든 곳을 훑어 표로 적는다 — 플레이어 · 가방 · 장비 · 채집(나무 · 바위 · 약초) · 땅에 떨어진 것 · 건축 · 놓기 · 상자 · 제작 시설(대기열 · 버퍼) · 농사 · 동물 · 주민 · 시간 · 배고픔 · 체력 · 전투 · 데스 상자 · 저장. 자리마다 「서버가 정한다 / 각자 정한다」 · 맞추는 길(RPC · 동기화 · 생성) · 고칠 파일을 적는다
  - 기준: **서버가 정한다**가 기본이다 (리슨 서버 — 서버장 기계가 세계를 돌린다). 참가자는 입력 · 바라는 일만 보내고 서버가 해 보고 결과를 돌려준다
  - 기준: 캐릭터는 각자 기계에 남는다(character-world.md — 캐릭터를 데려간다). 세계는 서버장 기계에 저장된다
  - 기준: 결과는 spec/11_multiplayer/ 에 새 문서(netcode.md 등, 150줄 아래)로. roadmap.md 의 「빼는 것: 멀티」를 사람 결정 2026-10-14 로 고쳐 적는다
  - 기준: 이 단계는 코드를 바꾸지 않는다 — 다음 단계들을 이 표대로 나눠 목록에 붙일 것을 report 에 적는다
- [x] 2. 서버 열기 · 참가
  - 기준: 메뉴의 월드 고르기에 「서버 열기」, 메인 메뉴에 「참가 (IP)」 — 캐릭터를 고르고 들어간다 (테스트)
  - 기준: 둘이 붙고 · 한쪽이 끊기면 다른 쪽이 멈추지 않는다 — 참가자가 나가면 그 몸이 사라지고, 서버장이 나가면 참가자는 메뉴로 돌아가 「서버가 닫혔다」를 본다 (테스트)
  - 기준: 13번째 접속은 거부된다 (listen-server.md 수용 기준) (테스트)
  - 기준: 혼자 하기는 지금 그대로다 — 서버 없이 도는 판이 깨지지 않는다 (전체 테스트)
  - 기준: **접속은 한 곳**(예: game/net/ 의 접속 자리)에만 있다 — ENetMultiplayerPeer 를 부르는 곳이 거기 하나다. 위의 동기화 · 권한 코드는 어떤 접속인지 모른다 (테스트: 다른 곳에서 ENet 을 부르면 떨어진다)
  - 기준: (앞 단계 QA) 권한 단계에서 「도구함」 줄과 맞춰 볼 것 — spec/06_build/building.md 102줄 「도구함(건축 권한) · 부패는 나중에 — 멀티용이다 (사람 결정 2026-09-24)」가 이번 「서버장이 길드장 · 권한 UI」와 겹친다. 사람 결정 줄이라 지우지 말고, 권한 단계(report 의 14번)에서 permissions.md 에 「도구함은 권한 UI 와 어떤 관계인가」를 한 줄 적어 이어라
- [x] 3. Steam 로비 · 방 코드 (앱 ID 480)
  - 기준: GodotSteam(GDExtension, 쓰는 Godot 판에 맞는 것)을 넣고 앞 단계의 접속 자리에 Steam 접속(SteamMultiplayerPeer)을 더한다 — ENet 은 시험용으로 남긴다. 라이선스를 THIRD_PARTY.md 에 적는다
  - 기준: 서버장이 서버를 열면 Steam 로비가 생기고 **짧은 방 코드**(임시 6자, 헷갈리는 0/O · 1/I 빼고)가 화면에 뜬다. 참가자가 코드를 치면 그 로비를 찾아 들어온다 — 로비 찾기는 「나루 로비」 표시로 거른다(480 은 다른 개발자 로비와 같이 쓴다) (테스트: Steam 을 흉내 낸 가짜로 코드 만들기 · 거르기 · 찾기)
  - 기준: Steam 친구 목록의 「게임 참가」 · 초대로도 들어온다
  - 기준: Steam 이 안 켜져 있으면 「Steam 을 켜 주세요」를 띄우고 혼자 하기는 그대로 된다 (테스트)
  - 기준: 루프는 진짜 Steam 접속을 시험하지 않는다 — 사람의 Steam 앱을 띄우거나 로그인하지 않는다. 진짜 접속은 사람이 친구와 확인한다 (report 에 「사람이 볼 것」으로 적는다)
  - 기준: spec/11_multiplayer/listen-server.md 의 「네트워크 방식(ENet / Steam) · 릴레이 미정」을 사람 결정 2026-10-14 로 고쳐 적는다
  - 기준: **친구에게 줄 윈도우 빌드를 뽑는다** (사람: 친구들은 윈도우다, 2026-10-14) — `game/export_presets.cfg` 에 Windows Desktop(x86_64) 판을 두고, 한 줄 명령(예: `tools/export_windows.sh`)으로 `~/Desktop/naru-builds/<날짜>-<커밋>/` 에 실행 파일 · .pck · GDExtension dll(Terrain3D · GodotSteam) · `steam_api64.dll` · `steam_appid.txt`(480) 를 담아 zip 한 개로 낸다. 빌드는 headless 로 뽑는다 — 사람 화면에 창을 띄우지 않는다
  - 기준: Godot 4.7.2 내보내기 템플릿이 없으면 공식 릴리스(github.com/godotengine/godot 의 4.7.2-stable export_templates.tpz)에서 받아 맞는 자리에 풀고 체크섬을 확인한다. 받지 못하면 needs_decision 으로 — 사람이 에디터의 「내보내기 템플릿 관리」로 넣는다
  - 기준: 뽑은 zip 에 위 파일이 다 들었는지 · .pck 가 열리는지 검사한다 (테스트). 윈도우에서 실제로 켜지는지는 사람이 친구와 본다 — report 에 「사람이 볼 것」으로 적는다
  - 기준: (앞 단계 QA) 참가자 몸 세우는 자리 1.5m 에 「임시」가 없다 — game/core/game_root.gd _seat_visitor 의 `Vector3(1.5, 0.0, 0.0)` 는 report temporary 에는 적혔지만 코드 주석에 「임시」 표시가 없다. 그 줄에 「서버장 옆 1.5m — 임시, 몸 동기화 단계 전까지」 주석을 달아라
- [x] 4. 서로 보인다 — 몸 · 움직임 · 든 것 · 입은 것
  - 기준: 참가자와 서버장이 서로의 몸이 걷고 · 뛰고 · 웅크리는 것을 본다. 이름표가 뜬다 (테스트)
  - 기준: 든 것 · 입은 것이 남에게도 보인다 (테스트)
  - 기준: 마우스 · 카메라는 각자 제 것만 — 남의 몸이 내 입력을 받지 않는다 (guard · 테스트)
  - 기준: (앞 단계 QA) SteamApi 의 진짜 신호 잇기는 테스트 밖이다 — core/net/steam_api.gd start() 안에서 Steam 신호(lobby_created result==1 · lobby_joined response==1/4)를 우리 신호로 바꾸는 람다는 가짜가 start() 를 덮어써 한 번도 돌지 않는다. 이름 · 인자 수는 QA 가 ClassDB 로 맞는 것을 봤지만 값 해석(ENTER_OK=1 · ENTER_FULL=4 · LOBBY_PUBLIC=2 · WORLDWIDE=3)이 GodotSteam 상수와 같은지는 지켜지지 않는다. headless 에서도 Engine.get_singleton("Steam") 이 있으니(초기화 없이) tests/core/net 에 `ClassDB.class_get_integer_constant("Steam", "LOBBY_TYPE_PUBLIC") == SteamApi.LOBBY_PUBLIC` 같은 단언을 네 상수에 대해 더하고, 신호 변환은 static 함수로 빼 단위 테스트하라
  - 기준: (앞 단계 QA) SteamApi.set_lobby_data 가 부를 때마다 setLobbyJoinable 도 부른다 — core/net/steam_api.gd: 키 하나 적을 때마다 같은 호출이 반복된다. 로비가 섰을 때 한 번(SteamRoom._on_created 또는 create_lobby 답) 부르게 옮겨라 (동작 차이는 없다)
- [x] 5. 세계를 맞춘다 — 1단계 표대로 나눠 단계를 더한다
  - 기준: 1단계 표의 자리마다 단계를 이 묶음에 더한다 — 시험은 ENet 으로, 사람은 Steam 으로 친구와 본다 (채집 · 떨어진 것 · 건축 · 상자 · 시설 · 농사 · 동물 · 주민 · 시간 · 전투 · 데스 상자 …) — 한 단계 = 한 무리
  - 기준: 자리마다 「한쪽이 바꾸면 다른 쪽이 곧 같은 것을 본다」 · 「둘이 한꺼번에 같은 것을 집으면 하나만 얻는다」를 테스트로
  - 기준: 데스 상자는 죽은 본인만 연다 (spec/08_combat/damage-death.md — 혼자일 때 드러나지 않던 것)
  - 기준: (앞 단계 QA) MAX_KEYS 에 「임시」 표시가 없다 — game/core/net/net_bodies.gd:18-20 의 MAX_KEYS := 16 은 report temporary 에 임시로 적혔지만 코드 주석에는 「임시」가 없다. PERIOD 처럼 주석 첫머리에 「— 임시.」를 달아라
  - 기준: (앞 단계 QA) Player 손 자리를 베껴 둔 상수 — game/multiplayer/visitor_body.gd HAND_AT 는 player.tscn 의 Body/Hand 자리를 숫자로 베꼈다(주석에 「같다」고만). player.tscn 이 바뀌면 조용히 어긋난다. Player 의 Hand 노드 자리를 읽어 쓰거나, 두 값이 같음을 단언하는 테스트를 tests/multiplayer 에 더하라
- [x] 6. 캐릭터 사본 · 가방 · 장비 · 손 만들기 — 서버가 쥐고 주인에게 돌려준다
  - 기준: 참가자가 들어올 때 건넨 캐릭터 사본(CharacterData)을 서버가 쥐고, 가방 칸 옮기기 · 나누기 · 버리기 · 핫바 고르기 · 입기 · 벗기 · 손으로 만들기를 참가자가 바라면 서버가 그 사본에 해 보고 바뀐 가방 · 장비를 주인에게 돌려준다 (netcode.md 표 「가방 · 장비 · 손으로 만들기」) (테스트, ENet 둘)
  - 기준: 바라는 일은 **한 문**(예: core/net/ 의 요청 자리 하나)을 지난다 — 내 기계는 조준까지, 서버가 `do_*(who, …)` 로 실행한다. 혼자일 때도 같은 길을 탄다. 길드 단계가 이 문에 권한을 건다 (테스트: 혼자 하기가 그대로다)
  - 기준: 한쪽이 바꾸면 다른 쪽이 곧 같은 것을 본다 — 참가자 가방이 바뀌면 주인 화면 가방 · 남에게 보이는 든 것 · 입은 것이 같다 (테스트) · **같은 칸 버리기 · 옮기기가 겹쳐 와도 한 번만 일어난다** — 떨어진 것과 가방 개수의 합이 늘지 않는다 (테스트)
  - 기준: 자동 저장 · 나갈 때 서버가 사본을 주인에게 보내고 주인이 **자기 기계에** 저장한다. 서버장이 끊기면 참가자는 마지막으로 받은 캐릭터를 저장한다 (character-world.md 수용 기준) (테스트)
- [x] 7. 시간 — 서버 시계 하나
  - 기준: 서버 시계가 날짜 · 시각을 주기로 보내고 참가자 시계는 받으면 맞춘다. 날 바뀜 일(재생 · 자람 · 시설 등)은 서버만 돌린다 (netcode.md 표 「시간」) (테스트, ENet 둘)
  - 기준: 한쪽에서 시간이 흐르면 다른 쪽이 곧 같은 날 · 같은 시각(허용 오차 안)을 본다 — 주기는 임시로 정하고 코드 · report 에 「임시」 (테스트)
  - 기준: (앞 단계 QA) 한 줄에 주석 둘이 겹쳤다 — game/ui/hud/slot_drop.gd quick_move 의 `if slot != "" and not gate.remote(): # 참가자면 막힌 까닭은 서버가 돌려준다 # 장비인데 막혔다 — …` 에 옛 주석 뒤에 새 주석이 이어 붙었다. 하나로 합쳐(「장비인데 막혔다 — 까닭을 한 줄 띄운다. 참가자면 서버가 돌려준다」) 적어라
  - 기준: (앞 단계 QA) 답이 오기 전에 이어서 하는 일이 엇갈린다 — 참가자는 본 칸(seen)을 자기 화면의 옛 값으로 보내므로, 한 왕복 안에 1→3 옮기고 곧 3→5 를 끌면 둘째가 서버에서 거부된다(조용히 제자리). 또 place_one/place_all 은 seen 없이 가서, 자리 바꾸기 놓기를 답 전에 두 번 하면 두 번 바뀌어 제자리로 돌아간다. 이번 기준(같은 칸 버리기 · 옮기기)은 지켰다. 상자 안 · 길드 단계에서 문을 다시 만질 때 PLACE_* 에도 seen(손 · 칸)을 붙이고, 거부되면 참가자 화면이 서버 답으로 되돌아가는 것을 보이는 테스트(test_bag_on_server 에 place_all 겹치기 두 번 → 한 번만)를 더하라
- [x] 8. 채집 · 땅에 떨어진 것 — 나무 · 바위 · 약초 · 재생 · 줍기
  - 기준: 나무 · 바위 치기 · 약초 캐기(스킬체크는 각자, 결과만)를 서버가 실행하고 남은 내구 · 없앤 표시 · 캔 자리를 모두에게 맞춘다. 재생은 날이 바뀔 때 서버만 (netcode.md 표) (테스트, ENet 둘)
  - 기준: 부서지면 떨어진 것이 서버에서 생성되어 모두에게 같은 자리 · 종류 · 개수로 선다. 밟으면 서버가 누구 가방에 넣을지 정한다 (테스트)
  - 기준: 한쪽이 바꾸면 다른 쪽이 곧 같은 것을 본다 (테스트) · **둘이 한꺼번에 같은 떨어진 것 · 같은 약초를 집으면 하나만 얻는다** — 개수의 합이 늘지 않는다 (테스트)
- [x] 9. 건축 · 놓기 — 토대 · 벽 · 문 · 지붕 · 계단 · 방 이름 · 상자 · 침대 · 가구 · 횃불 · 앵커 · 선착장
  - 기준: 놓기 · 치기 · 돌리기 · 올리기 · 문 여닫기 · 방 이름 바꾸기를 서버가 실행하고 조각 생성 · 무너짐 · 생성 해제를 모두에게 맞춘다. 미리보기는 각자 (netcode.md 표 「건축 · 방 이름 · 놓기」) (테스트, ENet 둘)
  - 기준: 한쪽이 바꾸면 다른 쪽이 곧 같은 것을 본다 (테스트) · **둘이 한꺼번에 같은 칸에 놓으면 하나만 선다**, 같은 조각을 둘이 헐면 재료는 한 번만 돌아온다 (테스트)
  - 기준: 나중에 들어온 참가자도 이미 지어진 것을 그대로 본다 (테스트)
  - 기준: (앞 단계 QA) 늘 참인 단언 한 줄 — test_harvest_on_server.gd test_two_guests_on_one_pile_… 의 assert_eq(host.inventory.count_of("stone") + 2, 5, …) 는 바로 위 줄(서버장 3)과 같은 것을 본다. 상수 2 대신 _kit_of(host, full) 에서 이 회차에 늘어난 몫을 세어(remove 뒤 97 → 99 이므로 count_of - (STACK_MAX - 2)) 합을 단언하거나 stone_of.call() 처럼 세 가방을 합쳐 보라
  - 기준: (앞 단계 QA) 테스트 사이 빈 줄 하나 — test_harvest_on_server.gd 의 test_the_guest_skill_check_… 끝과 test_two_reaping_one_herb_at_once_get_only_one 사이가 한 줄만 띄워졌다. 다른 함수들처럼 두 줄로
- [x] 10. 상자 안 — 여럿이 같이 연다
  - 기준: 열기 · 칸 옮기기 · 정렬 · 채우기를 서버가 실행하고 연 사람 **모두**에게 내용을 보낸다 — `_open` 하나를 연 사람마다로 바꾼다 (netcode.md 「창은 여럿이 같이 연다」) (테스트, ENet 둘)
  - 기준: 한쪽이 바꾸면 같이 연 다른 쪽 창이 곧 같은 것을 본다 (테스트) · **둘이 한꺼번에 같은 칸을 집으면 하나만 얻는다** (테스트)
  - 기준: (앞 단계 QA) 방 이름 바꾸기에 거리 · 길이 검사가 없다 — core/net/net_world.gd _build_for 의 &"room" 갈래는 권한(Acts.BUILD)만 보고 어느 방이든 아무 길이의 이름으로 바꾼다. 다른 갈래처럼 그 방의 조각(room id → 토대 자리)에 _near(id, …, PIECE_GIRTH) 를 보거나, 방 이름 창의 최대 길이를 서버에서도 잘라라(테스트: 멀리 선 참가자의 rename 이 서버 room_names 를 안 바꾼다)
  - 기준: (앞 단계 QA) 서 있는 것이 바뀌면 그 놓는 곳을 통째로 restore 한다 — net_world._placed 가 상자 하나가 바뀌어도 참가자 기계의 상자를 전부 지우고 다시 세운다. 「상자 안」 단계에서 참가자가 연 상자 창 · 노드 참조가 끊기지 않는지 그 단계 기준에 넣어 확인하라
- [x] 11. 제작 시설 · 대기열 · 버퍼 · 목장
  - 기준: 대기열 · 개수 · 꺼내기 · 먹이 · 가축 넣기를 서버가 실행하고, 시설 틱(Stations._process)은 서버만 돈다 → 연 사람에게 대기열 · 진행 · 버퍼 (netcode.md 표 「제작 시설 · 목장」) (테스트, ENet 둘)
  - 기준: 한쪽이 바꾸면 다른 쪽이 곧 같은 것을 본다 (테스트) · **둘이 한꺼번에 같은 버퍼를 꺼내면 하나만 얻는다** (테스트)
  - 기준: (앞 단계 QA) 연 사람에게 보내는 _send_chest 를 지켜 주는 테스트가 없다 — net_world.gd _send_chest 첫 줄에 return 을 넣어도 test_chest_on_server 5개가 모두 통과했다. 상자 안이 놓는 곳 목록(standing, 0.1초마다 Chests.sync)으로도 모든 참가자에게 가서 그 길이 대신 맞춰 주기 때문이다. 둘 중 하나로 정리하라: (가) _send_chest 가 맡는다면 standing 맞추기를 끈 채(예: 테스트에서 _placed 의 chests 갈래를 막거나 PLACED_PERIOD 를 크게) 연 사람만 곧 받는지, 열지 않은 참가자는 _chest_items 를 안 받는지 단언하는 테스트를 더하라. (나) standing 이 맡는다면 _send_chest · SENDS_META 를 지우고 netcode.md 「창은 여럿이 같이 연다」 줄을 그 길로 고쳐라
- [x] 12. 농사 — 밭 · 심기 · 물 · 비료 · 수확
  - 기준: 밭 만들기 · 심기 · 물 · 비료 · 수확을 서버가 실행하고 밭 · 작물 칸을 모두에게 맞춘다. 날이 바뀌면 서버만 자란다 (netcode.md 표 「농사」) (테스트, ENet 둘)
  - 기준: 한쪽이 바꾸면 다른 쪽이 곧 같은 것을 본다 (테스트) · **둘이 한꺼번에 같은 작물을 거두면 하나만 얻는다** (테스트)
  - 기준: (앞 단계 QA) 시설 목록이 0.1초마다 통째로 간다 — 만드는 중 · 가축이 든 시설은 craft_left · 무리 남은 초가 늘 바뀌어 net_world._placed 가 stations 의 shared() 전체를 PLACED_PERIOD(0.1초, 임시)마다 모두에게 다시 보낸다(구현 report 도 짚었다). 시설이 많아지면 진행 · 남은 초만 따로 작게 보내거나 남은 초를 0.5초 단위로 반올림해 비교하는 길로 줄여라. 테스트: 시설 하나가 만드는 중일 때 1초 동안 보낸 stations 목록 횟수가 정한 상한 이하임을 단언
- [x] 13. 짐승 · 사냥 · 해체 · 포획
  - 기준: 짐승 몸은 서버에서 생성되고 자리가 모두에게 맞춰진다. AI · 맞기 · 마취 · 도망은 서버만, 무작위는 서버에서만 굴린다. 해체 · 줍기 · 포획은 요청으로 (netcode.md 표 「짐승」) (테스트, ENet 둘)
  - 기준: 한쪽이 바꾸면 다른 쪽이 곧 같은 것을 본다 (테스트) · **둘이 한꺼번에 같은 사체를 해체하면 한 번만 나온다** (테스트)
  - 기준: (앞 단계 QA) 참가자 물뿌리개 채우기가 거절돼도 채운 클릭으로 친다 — crops.gd fill_held 의 remote 갈래는 서버 답을 기다리지 않고 _filled 를 올려 true 를 돌려준다. 서버가 거리 · 권한으로 거절하면 그 우클릭은 마시기로도 안 가고 한 줄도 없다(서버 _build 는 told 가 비면 아무것도 안 보낸다). 거절 때도 까닭 한 줄(예: 권한 없음)을 _farm_said 로 보내거나, 적어도 주석에 이 맞바꿈을 적어라
  - 기준: (앞 단계 QA) net_world._farm_said 가 Crops._say 비공개 함수를 부른다 — 밑줄 함수를 밖에서 부르지 않게 Crops 에 공개 say(key, args) 를 두거나 show_ground 처럼 이름을 공개로 바꿔라
- [x] 14. 낚시
  - 기준: 떡밥 · 던지기는 요청, 떡밥 영역은 모두에게 맞춘다. 물면 그 사람에게만 게이지가 뜨고 결과만 보낸다. 물고기는 서버가 굴린다 (netcode.md 표 「낚시」) (테스트, ENet 둘)
  - 기준: 한쪽이 바꾸면 다른 쪽이 곧 같은 것을 본다 (테스트) · **둘이 같은 떡밥 영역에서 한 마리를 두고 겨루면 하나만 얻는다** (테스트)
  - 기준: (앞 단계 QA) 사냥 권한 거절은 말없이 삼켜진다 — net_world._hunt 는 _kit_for(id, Acts.HUNT) 가 null 이면 그냥 return 해서, 권한 없는 참가자가 도축 단추·우클릭을 눌러도 아무 한 줄이 없다(농사는 이번에 NET_NOT_ALLOWED 를 보내게 고쳤다). _build 의 농사 갈래처럼 gate.kit_of(id) != null 이고 권한만 없으면 _session().acts().give(id, SAY_NOT_ALLOWED) 나 바란 사람에게 한 줄을 보내고, test_a_far_or_unpermitted_guest_is_refused 에 참가자 쪽 알림을 단언으로 더하라
  - 기준: (앞 단계 QA) 짐승 훑기가 매번 그룹 전체를 돈다 — _animal_by_id 는 _animals_here() 로 Animal.ALL 전부를 훑고 바람마다 부른다. 지금 섬의 짐승 수로는 문제 아니나, 서버 쪽에도 번호 → 짐승 사전을 두면 _poll_animals · _hunt 가 같이 쓸 수 있다(막는 것은 아니다)
- [x] 15. 주민
  - 기준: 주민 몸은 서버에서 생성되고 자리가 맞춰진다. 일 · 손 · 수면은 서버만. 직업 · 거주지 · 반경 · 떠나보내기는 요청이고, 주민 창은 서버 표를 받아 그린다 (netcode.md 표 「주민」) (테스트, ENet 둘)
  - 기준: 한쪽이 바꾸면 다른 쪽이 곧 같은 것을 본다 (테스트) · **둘이 한꺼번에 같은 주민에게 다른 직업을 주면 하나만 남는다** — 두 창이 같은 답을 본다 (테스트)
  - 기준: (앞 단계 QA) 낚시 거절 테스트의 영역 밖 자리 식이 꼬였다 — test_fish_on_server.gd test_a_miss_says_why… 의 cast_at(ZONE_AT + Vector3(0,0,-RADIUS-1) + Vector3(0,0,2*RADIUS+2)) 는 결국 ZONE_AT + Vector3(0,0,RADIUS+1) 이다. 읽기 쉽게 그 한 식으로 줄여라 (막는 것은 아니다)
- [x] 16. 몸 상태 · 전투 — 배고픔 · 목마름 · 버프 · 독 · 체력 · 갑옷 · 낙하 · 등반 · 휘두르기 · 총
  - 기준: 배고픔 · 목마름 · 버프 · 독은 서버가 사람마다 틱하고 주인에게 보낸다. 먹기 · 마시기 · 약은 요청 (netcode.md 표) (테스트, ENet 둘)
  - 기준: 낙하 피해는 주인이 알리고 서버가 깎는다 — 서버장 · 참가자 누가 떨어져도 그 체력은 서버 사본에서 줄고 주인에게 돌아간다 (netcode.md 표 「플레이어 몸」) (테스트, ENet 둘)
  - 기준: 남에게 붙은 자리 · 갈고리줄이 보인다 — 등반 · 기력 · 갈고리줄은 각자 정하고 붙은 자리를 몸 소식에 싣는다. 한쪽이 붙으면 다른 쪽이 곧 같은 자리를 본다 (netcode.md 표 「등반 · 기력 · 갈고리줄」 — 앵커 박기는 9단계) (테스트, ENet 둘)
  - 기준: 맞기는 서버에서만 깎는다. 휘두르기 · 쏘기는 요청(조준 시작점 · 방향) → 서버가 맞히고 탄알을 날리고 탄 · 재장전은 서버가 사본에. 조준 확대 · 반동 · 탄 자국은 각자 (테스트)
  - 기준: 한쪽이 바꾸면 다른 쪽이 곧 같은 것을 본다 — 짐승 체력 · 탄알 · 남의 몸이 맞은 것 (테스트) · **둘이 한꺼번에 같은 먹을 것을 먹으면 하나만 줄어든다** (테스트)
  - 기준: (앞 단계 QA) 참가자 화면에 주민 알림 목록이 없다 — Villagers._sync_alerts 가 remote 면 돌아가 버려, 상자가 차서 멈춤 · 좁은 방 알림(AlertLog)은 서버장 화면에만 뜬다(netcode.md 에 「아직」으로 적혀 있음). 그림이 받는 stalled 와 서버가 잰 cramped 를 모아 참가자 섬도 AlertLog 를 맞추게 하고, 참가자 기계에 멈춘 주민 알림 줄이 서는지 보는 ENet 테스트를 test_folk_on_server.gd 에 더하라
  - 기준: (앞 단계 QA) 쓰지 않는 NetWorld.folk() — net_world.gd 의 folk()는 「테스트가 들여다본다」고 적혔지만 부르는 곳이 없다. 지우거나 테스트에서 쓰게 하라
- [x] 17. 죽음 · 데스 상자
  - 기준: 서버가 죽은 사람의 가방을 비워 데스 상자를 생성하고 주인에게 빈 가방 · 되살 자리를 보낸다. 수명 틱은 서버만 (netcode.md 표 「죽음」) (테스트, ENet 둘)
  - 기준: **데스 상자는 죽은 본인만 연다** — 참가자 A 의 상자를 서버장 · 참가자 B 가 열려 하면 서버가 거부한다. 지금 death.gd 의 can_open 은 한 기계 안 character_id 만 본다 — 여는 사람의 캐릭터 id 를 서버가 보고 정한다 (spec/08_combat/damage-death.md — 혼자일 때 드러나지 않던 것) (테스트)
  - 기준: 한쪽이 바꾸면 다른 쪽이 곧 같은 것을 본다 — 상자가 서고 · 줄고 · 사라진다 (테스트) · **주인이 두 기계에서 한꺼번에 같은 칸을 꺼내도 하나만 얻는다** (테스트)
  - 기준: (앞 단계 QA) 쏘기 바람의 방향을 단언하지 않는다 — test_body_on_server.gd test_swings_and_shots_are_asked_and_the_server_hits 의 쏘기 쪽은 (asked.back()[1] as Array).slice(0, 1) 로 시작점만 본다. 휘두르기처럼 asked.back()[1][1] 이 me.aim_direction()(또는 Attack 이 쓴 그 방향)과 같은지 단언을 하나 더하라. 지금은 짐승이 맞은 것으로 간접적으로만 확인된다
  - 기준: (앞 단계 QA) 알림 목록 단언이 다른 기계 줄로도 참이 된다 — test_folk_on_server.gd 의 assert_true(mir.alert_log().texts().size() >= 2) 는 주석대로 한 godot 안에서 AlertLog.GROUP 을 두 기계가 같이 써 서버장 줄만으로도 참이 될 수 있다. guest_folk.alerts() 단언이 실제로 지키고 있으니 이 줄은 지우거나, 미르 AlertLog 에만 든 줄을 가려 보는 단언으로 바꿔라
- [x] 18. 지도 밝힘 · 섬 열기 · 배 · 월드 설정 · 저장
  - 기준: 서버가 모든 몸 둘레를 밝혀 섬 지도에 합치고 모두에게 맞춘다 — 한 팀이 같이 밝힌다 (테스트, ENet 둘)
  - 기준: 해금 아이템 넣기는 요청. 배는 서버장만 타고 모두 같이 건너간다 (임시, netcode.md 미정). 월드 설정은 서버장 기계에서만 바꾸고 다른 이에게 맞춘다 (테스트)
  - 기준: 한쪽이 바꾸면 다른 쪽이 곧 같은 것을 본다 — 밝힌 지도 · 열린 섬 · 월드 설정 (테스트) · **둘이 한꺼번에 같은 해금 아이템 자리에 넣으면 한 번만 들어가고 나머지는 가방에 남는다** (테스트)
  - 기준: 세계는 서버장 기계에만 저장되고, 서버장이 다시 열면 참가자가 바꾼 세계(6~17단계의 자리)가 그대로다 (테스트)
  - 기준: 사람이 Steam 으로 친구와 볼 것을 report 에 적는다 — 6~18단계를 한 판에서 (사람이 볼 것)
  - 기준: (앞 단계 QA) 죽은 자리를 모르면 가방이 안 비는 길 — net_world.gd die_for 는 where.call(id) 가 Vector3 가 아니면(서버장 화면에 그 사람 몸이 아직 안 섰을 때) 상자도 안 놓고 가방도 안 비운 채 되살 자리만 돌려준다. 페널티가 켜져 있어도 아무것도 잃지 않는다. 드문 길이지만 자리를 모를 때는 사본의 마지막 자리나 spawn 에 상자를 놓도록 하고, test_death_on_server.gd 에 몸 소식 전에 죽는 경우를 하나 더하라
- [x] 19. 길드 — 서버장이 길드장 · 나머지는 권한이 없다
  - 기준: 같은 서버에 들어온 사람은 모두 **한 팀**이다 — 파티를 맺는 길이 없다. 서로 때려도 피해가 없다 (테스트)
  - 기준: 서버장이 **길드장**이다. 서버장을 뺀 사람은 **처음에 권한이 하나도 없다** (permissions.md 「못 건드림」) (테스트)
  - 기준: 권한 단위는 1단계 표에서 뽑는다 — Claude 가 정한다(임시): 건축 · 헐기 · 상자 · 제작 시설 · 밭 · 채집 · 주민 부리기 · 동물 · 줍기 정도. 권한이 없으면 해 보려 할 때 「권한이 없다」가 뜨고 **서버가 막는다** — 참가자 쪽에서만 막지 않는다 (테스트, 단위마다)
  - 기준: **길드 창을 따로** 만든다 — 키 하나로 연다. 들어온 사람 목록 · 각자의 권한을 켜고 끈다. 길드장만 바꿀 수 있고 다른 사람은 보기만 한다 (테스트)
  - 기준: 준 권한은 세계 저장본에 캐릭터별로 남는다 — 다시 들어와도 그대로다 (테스트)
  - 기준: 찍어서 확인한다 — 길드 창 (길드장 눈 · 참가자 눈)
  - 기준: spec/11_multiplayer/permissions.md 의 「권한 단위 미정」을 이 단위로 고쳐 적는다 (임시 표시)
  - 기준: (앞 단계 QA) 저장 테스트가 6~17단계 일부만 본다 — test_world_on_server.gd test_the_world_is_saved_only_on_the_host_and_reopens_as_the_guest_left_it 는 짓기(9)·캐기(8)·데스 상자(17)·섬/지도(18)만 참가자가 바꾼다. 같은 길(서버장 섬 상태 → IslandState)이라 기준은 넘겼지만, 참가자가 상자에 넣은 것(10)·심은 밭(12)·시설 대기열(11)·주민 직업(15) 중 몇 가지를 같은 테스트(또는 world_island 에 손발을 더한 새 테스트)에 넣어 다시 연 뒤 그대로인지 단언하면 「6~17단계」가 말 그대로 지켜진다
  - 기준: (앞 단계 QA) 지도를 통째로 0.1초마다 실을 수 있다 — net_world.gd _placed_now_of 의 MAP 은 바뀔 때마다 MapMemory.to_dict() 전체를 보낸다. 진짜 섬(칸 수가 큰 지도)에서 여럿이 걸으면 칸이 바뀔 때마다 지도 한 장이 간다. 지금 기준 밖이지만, 밝힌 칸만 보내거나 보내는 주기를 늦추는 것을 netcode.md 미정에 한 줄 적거나 고쳐 두면 좋다
- [x] 20. (앞 단계 QA) 길드 창 머리 칸이 언어를 바꿔도 안 바뀐다
  - 기준: 길드 창 머리 칸이 언어를 바꿔도 안 바뀐다 — ui/hud/guild_window.gd 150행이 머리 칸을 세울 때 tr(Guild.key_of(u)).replace(" ", "\n") 로 글자를 박아 둔다(자동 번역이 아니라 이미 번역한 문자열). 게임 중 설정에서 언어를 바꾸면 길드 창 머리만 옛 언어로 남는다. NOTIFICATION_TRANSLATION_CHANGED 에서 머리 글자를 다시 적거나, 다른 창이 언어 바뀜을 다루는 방식을 따르고, 언어를 en→ko 로 바꾼 뒤 머리 칸이 「제작\n시설」 이 되는지 보는 테스트를 더하라
  - 기준: (앞 단계 QA) 참가자 눈의 잠긴 칸이 잠겨 보이지 않는다 — guild_guest_ko.png 에서 참가자 창의 칸(disabled)이 길드장 창의 누를 수 있는 칸과 똑같은 테두리 · 밝기라 눌러지는 것처럼 보인다. 위의 안내 한 줄만이 보기 전용임을 알린다. ui_theme 의 CheckBox disabled 모양을 흐리게(예: 테두리 · 점 알파 0.5) 주거나 GuildWindow 에서 보는 눈일 때 modulate 를 낮춰라
- [x] 21. (앞 단계 QA) 참가자 첫 자리를 서버가 원점으로 친다
  - 기준: 참가자 첫 자리를 서버가 원점으로 친다 — 게임 코드는 그대로 두고 test_build_on_server.gd _join 이 VisitorBody.placed() 를 기다리게만 바꿔 전체 실패를 풀었다. 참가자가 들어온 뒤 몸 소식이 처음 오기 전(0.1초 남짓)에는 GameRoot._where_is 가 VisitorBody.target 처음 값 (0,0,0) 을 그 사람 자리로 주어서, 손 닿는 거리 검사가 원점을 기준으로 한다(원점 곁의 바람은 잘못 받고, 먼 곳의 바람은 잘못 거부한다). report 에 따르면 _where_is 가 placed 전에 null 을 주게 하면 원점 곁에서 곧바로 바라는 멀티 테스트 셋(상자 · 세계 · 채집)이 떨어진다. 고치는 길: 접속 때(참가자가 들어올 때) 참가자가 첫 자리를 함께 보내 VisitorBody 를 그 자리에 놓거나, placed 전에 온 바람은 거부하고 그 세 테스트도 placed 를 기다리게 하라. 그다음 placed 전 바람이 원점 기준으로 받아지지 않는지 보는 테스트를 더하라
  - 기준: (앞 단계 QA) 잠긴 칸 알파 「임시」 표시가 코드에 없다 — 테두리 알파 0.45 · 점 알파 0.5 는 report.temporary 에만 「임시」로 적혔다. guild_window.gd 의 MARK 주석(26~28행)이나 CELL 주석 곁에 「GuildCell disabled 테두리 0.45 · 점 0.5 — **임시**, 눈으로 잡았다 (naru_theme.tres)」 한 줄을 더하라

## [x] G-907 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-start
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. 스킬체크 테스트가 private 를 만진다 (G-221 단계 8)
  - 기준: 스킬체크 테스트가 private 를 만진다 — test_the_guest_skill_check_… 가 g._herb 를 직접 넣는다(조준점 대신). Gathering 에 테스트용 open(herb) 같은 길이 있으면 그것을 쓰고, 없으면 기존 gathering 테스트처럼 조준으로 여는 길을 쓰는 편이 Gathering 속이 바뀌어도 깨지지 않는다

## [x] G-222 멀티 시험에서 본 것 — 참가자 배고픔 0 · 외형 · 권한 칸 · 게임 중 서버 열기 · Steam 끄기 충돌 (사람이 친구와 해 보고 2026-10-15)
- 차선: 3d-start
- spec: spec/11_multiplayer/ (netcode · permissions · listen-server) · spec/01_settings/world-settings.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- 시험한 판: 서버장 윈도우(내보낸 zip, e8219982) · 참가자 맥(`NARU_STEAM=1 godot --path ~/Desktop/naru-play/game`). 맥 참가 기록: `~/Desktop/naru-saves/mac-join.log`
- 테스트는 지금처럼 headless 둘을 ENet 으로 · Steam 은 가짜(fake_steam.gd)로. **사람의 Steam 을 켜거나 로그인하지 않는다**
- [>] 1. 참가자가 들어오면 배고픔 · 목마름 · 체력이 바닥이다 → G-222-1 로 떼었다
  - 사람이 본 것 (2026-10-15): 「일단 들어오면 배고픔이랑 목마름이 0임. 서버장은 정상인데 접속하는 사람의 배고픔이랑 hp는 바닥임」
  - 기준: **재현부터** — 배고픔 · 목마름 · 체력이 찬 캐릭터(저장본)로 참가하는 테스트를 먼저 만들어 떨어지는 것을 본다. 그 뒤 고친다
  - 기준: 참가자는 제 캐릭터 저장본의 배고픔 · 목마름 · 체력 그대로 들어온다 — 서버장 화면의 그 사람 사본 · 참가자 화면 둘 다 (테스트). 나갔다 다시 들어와도 그 값이다
  - 짚어 볼 곳 (짐작이다 — 재현으로 가린다): 참가자가 건네는 캐릭터 사본(net_session.gd `_carried` — `CharacterData.to_dict`)에 몸 상태가 담기나 · 서버 사본(net_vitals.gd)이 새 몸으로 시작하나
- [x] 2. 캐릭터 외형이 세계에 안 보인다 — 나는 주황 · 남은 흰색
  - 사람이 본 것 (2026-10-15): 「캐릭터가 설정해둔거랑 다르게 나와 있음. 맥에서 나를 보면 주황 상대는 흰색 / 윈도우에선 … 똑같이 나를 보면 주황 상대는 흰색. 만약 이게 구별하기 위함이면 상관은 없지만 **나중에 에셋을 가져와서 커스터마이징 했는데 제대로 안된다면 문제가 됨**」
  - 까닭(이 세션이 잼): 고른 외형(`CharacterData.appearance`)은 가방 창 미리 보기(ui/hud/bag.gd:140 `set_look`)에만 입힌다 — 세계의 내 몸 · 남의 몸(multiplayer/visitor_body.gd)은 외형을 모른다
  - 기준: 세계의 내 몸과 남의 몸 모두 그 캐릭터가 고른 외형으로 그린다 — 입히는 곳은 미리 보기와 **같은 한 곳**(appearance.json · character_preview 의 apply). 외형은 몸 소식(VisitorBody.state_of)이나 들어올 때 건네는 사본에 실어 간다 (테스트: 외형이 다른 둘이 붙으면 서로 고른 대로 보인다)
  - 기준: 나중에 외형 에셋을 바꿔 끼워도 이 길 하나로 바뀐다 — 몸 그림이 외형을 따로 읽는 곳이 없다
- [x] 3. 권한 칸 — 헐기를 건축에 합치고 섬 열기 칸을 뺀다
  - **사람 결정 2026-10-15**: 「헐기: 건축이랑 합치고 이름은 건축으로」 · 「섬열기는 자유롭게 다같이 가도 되고 혼자 가도 되고 서버 몇명 남아서 본섬을 지켜도 되고 이런 식으로 할거라서 그냥 이건 상관 없을거 같은데」
  - 기준: 길드 창의 칸에서 「헐기」 · 「섬 열기」가 없다. 「건축」이 놓기 · 돌리기 · 올리기 · 문 · 방 이름 · 치기 · 내리기를 다 연다. 섬 열기(해금 아이템 넣기)는 권한을 묻지 않는다 (테스트)
  - 기준: 저장본에 「헐기」 · 「섬 열기」로 켜 둔 것이 있어도 읽힌다 — 헐기를 켜 둔 사람은 건축으로 친다 (테스트)
  - 기준: permissions.md 의 「권한 단위」를 사람 결정 2026-10-15 로 고친다. netcode.md 「미정 — 서로 다른 섬에 흩어질 때」에 위 사람 말을 적는다 (흩어져 따로 가는 것 자체는 이 단계에서 짓지 않는다 — 큰 일이라 사람과 따로 묶음으로 잡는다)
  - 기준: (앞 단계 QA) netcode.md 남에게 보이는 것에 외형이 없다 — spec/11_multiplayer/netcode.md:141 「남에게 보이는 것」 줄에 「고른 외형(몸 소식 a, CharacterModel 로 입는다)」을 더해라. 코드(visitor_body.gd 머리 주석)는 고쳤는데 spec 이 따라가지 않았다
  - 기준: (앞 단계 QA) bag.gd 가 외형 파일을 따로 읽는다 — ui/hud/bag.gd:141 set_look 이 AppearanceOptions.load_from() 을 스스로 불러 apply 한다. CharacterModel.wear 와 같은 일이니 look().model().wear(appearance) 로 바꾸고 test_character_model.gd 의 ALLOWED 에서 bag.gd 를 빼라 (막는 것은 아니다 — 같은 CharacterModel.apply 로 가므로 그림은 한 곳이다)
- [x] 4. 혼자 하던 게임에서 서버를 연다 — ESC → 월드 설정 → 멀티 서버
  - **사람 결정 2026-10-15**: 「멀티 서버를 싱글서버 내에서도 열 수 있게 esc-> 월드 설정에 멀티서버로 변경하면 열 수 있게」
  - 기준: 혼자 하는 중 ESC 의 월드 설정에 「멀티 서버」 켜기가 있다 — 켜면 메뉴로 나가지 않고 그 자리에서 서버가 열리고(Steam 판은 방 코드가 뜬다), 끄면 참가자에게 「서버가 닫혔다」가 가고 혼자 하던 대로 이어진다 (테스트: ENet 판으로 켜고 · 붙고 · 끄기)
  - 기준: 켠 채 저장하고 다시 들어오면 어떻게 되는지(그대로 연다 / 혼자로 연다)는 정하지 않았다 — 혼자로 연다(임시)로 하고 spec 에 적는다
  - 기준: (앞 단계 QA) 권한 단위 목록의 「임시」 표시가 빠졌다 — 사람은 헐기·섬 열기 두 칸만 정했는데 permissions.md:12 「권한 단위」와 game/core/net/guild.gd:15 주석에서 「임시」를 지웠다(나머지 아홉 칸은 여전히 Claude 가 1단계 표에서 뽑은 것). permissions.md:12 를 「권한 단위 (임시 — 헐기·섬 열기 말고는 Claude 가 1단계 표에서 뽑았다, G-221.19)」로, guild.gd UNITS 주석에 「(**임시** — 나머지 칸은 Claude 가 뽑았다)」를 되살려라. 미정 줄(permissions.md:32)은 이미 「사람이 더 쪼개거나 합치면 고친다」라 그대로 둬도 된다
- [x] 5. 참가자 화면의 주민이 살짝씩 끊긴다
  - 사람이 본 것 (2026-10-15): 「npc움직임이 살짝씩 끊김」 — 서버장 화면인지 참가자 화면인지는 아직 모른다
  - 기준: 참가자 화면의 주민 그림(Villager.remote)은 서버 모습(0.1초마다, net_world.gd)을 받아 그 사이를 **이어서** 움직인다 — 남의 몸(visitor_body)처럼 (테스트: 모습 두 번 사이 프레임에서 자리가 한 번에 뛰지 않는다)
  - 기준: 서버장(혼자) 화면의 주민도 프레임마다 자리를 재어 뚝 끊기는 데가 있나 본다 — 있으면 고치고 없으면 report 에 「서버장 화면은 안 끊긴다」로 적는다
  - 기준: (앞 단계 QA) test_body_on_server 가 한 번 떨어졌다 — 흔들리는 테스트로 보인다. QA 가 tests/multiplayer 폴더를 두 번 돌렸는데 첫 판(game_root.gd 의 _shut_server 에서 set_room("") 한 줄만 바꾼 상태, 이 테스트는 그 함수를 안 지난다)에서 「미르가 서버장이 붙은 자리를 본다」가 떨어졌고, 되돌린 뒤 둘째 판과 전체 판에서는 통과했다. game/tests/multiplayer/test_body_on_server.gd 의 그 단언이 고정 프레임 · 시간을 기다리는지 보고, Sides.wait_until 로 조건을 기다리게 바꿔라
  - 기준: (앞 단계 QA) 저장 단언이 지어낸 키를 본다 — test_server_from_game.gd 의 `to_dict().has("server")` 는 아무도 쓰지 않는 키 이름이라 늘 참이다. 그 테스트의 뜻은 다음 단언(다시 들어오면 server_open() false)이 지키니 이 줄은 지우거나, 저장한 월드 dict 가 켜기 전 dict 와 키 목록이 같다는 단언으로 바꿔라
- [x] 6. Steam — 끌 때 충돌 · 「접속하는 중」이 끝나지 않는다
  - 사람이 본 것 (2026-10-15): 참가한 맥에서 창을 X 로 끄면 macOS 충돌 보고(EXC_BAD_ACCESS, `recursive_mutex::lock` ← libgodotsteam). 그 전 한 번은 방을 찾은 뒤 「접속하는 중…」만 계속 떴다(재현 못 함 — 다음 시도에서는 붙었다)
  - 이 세션이 잰 것: 진짜 Steam 으로 **참가자가 붙은 뒤 끄면 매번** 충돌한다(peer.close() 를 불러도). 서버만 연 판 · 붙지 않은 판은 끌 때 멀쩡하다. 붙는 데는 3~8초 걸렸다
  - 기준: 끄는 길(창 X · 메뉴의 끝내기 · 판에서 나가기)이 모두 한 곳을 지난다 — Steam 판이면 접속을 닫고 · `multiplayer_peer` 를 비우고 · 로비에서 나가고 · `steamShutdown` 을 부른 **뒤** 엔진을 끈다 (창 X 는 `auto_accept_quit` 를 끄고 닫기 요청을 받아서). 차례는 가짜 Steam 으로 테스트한다 — 충돌이 사라졌는지는 사람이 본다
  - **사람 결정 2026-10-15**: 「세이브만 제대로 되면 에러 저거 안나왔으면 좋겠는데 … x로 끄더라도 바로 꺼지는게 아닌 꺼지는것 처럼 보이는데 세이브나 안전하게 세이브 되도록 꺼지게」
  - 지금 길(이 세션이 읽음): 창 X → game_root.gd:241 → `_quit_game`(947) — 참가자 사본을 기다리고(최대 1.5초) · 저장 · 접속 거두기 · 끄기. **저장은 충돌 전에 끝난다** — 충돌은 그 뒤 엔진이 내려갈 때 난다
  - 기준: X 를 누르면 **창이 곧바로 사라진다**(숨기기 — 꺼진 것처럼 보인다). 그 뒤 뒤에서 저장 → Steam 정리(위 차례) → 끈다. 숨긴 동안에도 저장이 끝나기 전에는 끄지 않는다 (테스트: 닫기 알림 뒤 창이 숨고 · 저장본이 쓰이고 · 그다음 끈다)
  - 기준: 메뉴 화면(게임 밖)에서 X 로 꺼도 Steam 정리를 지난다 — 지금은 메인 화면이 auto_accept_quit 로 그냥 꺼진다(game_root.gd:229)
  - 기준: 끄는 데 오래 걸려도(사본 기다리기 1.5초 + 저장) 정해 둔 시간(임시 5초)이 넘으면 저장을 마친 것까지로 끈다 — 숨긴 채 영영 남지 않는다
  - 기준: Steam 으로 붙을 때도 포기 시간이 있다 — 로비에 들어간 뒤 일정 시간(임시 15초) 안에 월드를 못 받으면 「접속하지 못했다」로 돌아온다 (테스트: 가짜 Steam 이 답하지 않을 때)
  - 기준: Steam 에 붙을 때 `initRelayNetworkAccess` 를 한 번 불러 중계망을 미리 잰다 (첫 접속이 빨라진다)
  - 기준: (앞 단계 QA) netcode.md 주민 줄이 옛 방식을 적는다 — spec/11_multiplayer/netcode.md:65 「받은 자리로 다가간다」를 「받을 때 보이던 자리에서 새 모습까지 FOLLOW_TIME(임시 0.15초) 동안 고르게 잇는다 · 6m 넘으면 곧바로 옮긴다 (G-222.5)」로 고쳐라. 파일이 이미 150줄이라 한 줄 안에서 바꿔야 한다
  - 기준: (앞 단계 QA) 서버장 주민이 길 꺾일 때 한 번에 돈다 — report 가 찾았다: PathWalk.step 의 look_at 으로 보는 쪽이 꺾이는 점에서 한 프레임에 돈다. 사람이 본 「살짝씩 끊김」이 이것일 수도 있다. 보는 쪽을 몇 프레임에 걸쳐 돌게(lerp_angle) 하고, 꺾이는 점에서 한 틱 yaw 변화가 정한 각 아래인지 재는 테스트를 붙여라

## [x] G-222-1 참가자가 들어오면 배고픔 · 목마름 · 체력이 바닥이다 (G-222 단계 1 에서 떼었다 — 버그 아님, 닫음)
- 결정: **버그가 아니라 닫는다** (사람 결정 2026-10-15 「그럴 가능성이 높겠다 그럼 저거 결정대기 아예 없애줘」) — 참가한 맥 캐릭터 저장본(ㅁㄴㅇ, 1789959457_2a520c0c.sav, 사본 ~/Desktop/naru-saves/G-222-1-2026-10-15/)이 이미 배고픔 0 · 목마름 0 · 체력 1 로 저장돼 있었다 — 혼자 판에서 굶은 몸이 그대로 실려 간 것. 하다 만 브랜치 g/G-222-1 은 합치지 않는다
- 차선: 3d-start
- 떼어 온 곳: G-222 단계 1 — 나머지 단계는 G-222 가 이어 한다. 하다 만 것은 브랜치 g/G-222-1
- spec: spec/11_multiplayer/ (netcode · permissions · listen-server) · spec/01_settings/world-settings.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- 시험한 판: 서버장 윈도우(내보낸 zip, e8219982) · 참가자 맥(`NARU_STEAM=1 godot --path ~/Desktop/naru-play/game`). 맥 참가 기록: `~/Desktop/naru-saves/mac-join.log`
- 테스트는 지금처럼 headless 둘을 ENet 으로 · Steam 은 가짜(fake_steam.gd)로. **사람의 Steam 을 켜거나 로그인하지 않는다**
- [x] 1. 참가자가 들어오면 배고픔 · 목마름 · 체력이 바닥이다
  - 사람이 본 것 (2026-10-15): 「일단 들어오면 배고픔이랑 목마름이 0임. 서버장은 정상인데 접속하는 사람의 배고픔이랑 hp는 바닥임」
  - 기준: **재현부터** — 배고픔 · 목마름 · 체력이 찬 캐릭터(저장본)로 참가하는 테스트를 먼저 만들어 떨어지는 것을 본다. 그 뒤 고친다
  - 기준: 참가자는 제 캐릭터 저장본의 배고픔 · 목마름 · 체력 그대로 들어온다 — 서버장 화면의 그 사람 사본 · 참가자 화면 둘 다 (테스트). 나갔다 다시 들어와도 그 값이다
  - 짚어 볼 곳 (짐작이다 — 재현으로 가린다): 참가자가 건네는 캐릭터 사본(net_session.gd `_carried` — `CharacterData.to_dict`)에 몸 상태가 담기나 · 서버 사본(net_vitals.gd)이 새 몸으로 시작하나

## [x] G-223 윈도우에서 몸 상태 칸이 몰린다 · 월드 만들 때 시드 빼기 · 대기열의 「만들기」 빼기 (사람이 친구와 해 보고 2026-10-15)
- 차선: 3d-lane2
- [>] 1. 오른쪽 아래 배고픔 · 목마름 · 체력이 윈도우에서 몰려 보인다 → G-223-1 로 떼었다
  - 사람이 본 것 (2026-10-15): 「윈도우에서 오른쪽 아래 배고픔 목마름 hp 위치가 몰려 있게 보임」 — 맥에서는 괜찮다. 윈도우 PC 는 RTX 3070 · 화면 크기 · 배율은 모른다
  - 기준: **재현부터** — 창 크기(1280×720 · 1920×1080 · 2560×1440)와 화면 배율(100 · 125 · 150%, content_scale_factor)을 바꿔 가며 몸 상태 칸(ui/hud/needs_gauges.gd · need_gauge.gd · 체력)을 찍어 겹치거나 붙는 것을 먼저 본다 (ui_shots_kit). project.godot 은 1280×720 · stretch canvas_items
  - 기준: 어느 크기 · 배율에서도 세 칸이 같은 간격으로 떨어져 오른쪽 아래에 선다 — 칸끼리 겹치지 않는다 (테스트: 크기마다 칸의 사각형이 서로 안 겹친다)
  - 기준: 재현이 안 되면 그 사진들을 report 에 남기고 결정 대기로 올려 사람에게 윈도우 화면 사진 · 해상도 · 배율을 묻는다
- [x] 2. 월드 만들기에서 시드를 뺀다
  - **사람 결정 2026-10-15**: 「맵을 만들 때, 시드 어차피 없으니 시드 만드는 것도 빼」
  - 기준: 월드 만들기 화면(ui/menu/world_create.gd · SeedRow)에 시드 칸이 없다. 월드는 지금처럼 만들어진다 — 저장본의 `world_seed` 를 읽는 곳이 있으면 그대로 두고 값만 화면에서 안 받는다 (테스트) — 새 월드의 시드는 지금 빈 칸일 때처럼 무작위(world_create.gd `seed_from`). 시드는 자원 배치를 정하므로(game_root.gd 「이 월드의 시드가 자원 배치를 정한다」) 없애지 않는다
  - 기준: spec(world-settings 등)에 시드 칸이 적혀 있으면 사람 결정 2026-10-15 로 고친다
- [x] 3. 제작 시설 대기열 줄의 「만들기」를 뺀다
  - **사람 결정 2026-10-15**: 「제작 시설에서 오른쪽에 만들기 구운고기 이런식으로 되어 있는데 만들기 빼」
  - 기준: 대기열 줄(ui/hud/station_window.gd:432 · :461 의 `Mode` 칸, STATION_QUEUE_MAKE)에 「만들기」 글이 없다 — 이름 · 개수만. 유지 대기열(STATION_QUEUE_KEEP)은 무엇이 다른지 보여야 하니 그 글은 남긴다 (임시) (테스트 · 화면 사진)

## [x] G-223-1 윈도우에서 게임 창 아래가 작업 표시줄에 가려 몸 상태 막대 · 핫바가 잘린다 (G-223 단계 1 에서 떼었다)
- 차선: 3d-lane2
- 떼어 온 곳: G-223 단계 1 — 나머지 단계는 G-223 가 이어 한다. 하다 만 것은 브랜치 g/G-223-1
- 사람이 준 것 (2026-10-15): 윈도우 화면 사진 `~/Desktop/naru-saves/G-223-1-2026-10-15/windows-esc.png` (2560×1440, 창 모드) — 「몸 배율이 이상한게 아니라 **색상이 이상하다**는거였어」. 어떤 색인지는 사람에게 물었다
- 이 세션이 사진에서 본 것: 게임 창 아래가 **작업 표시줄 밑으로 들어가** 배고픔 · 목마름 글만 보이고 막대는 가려졌다 (왼쪽 아래 빨간 실선 하나만 보인다). 핫바 칸도 아래가 잘렸다 — 창이 작업 영역보다 크다
- 결정 (사람 답 2026-10-15): 「사진은 윈도우인데 싱글이었고 그냥 **윈도우에서 나오는 ui가 문제**인거 같은데」 · 「비율이 이상하다는게 아니라」 — 배율 · 해상도 문제가 아니다.
  「색이 이상하다」는 멀티에서 나는 주황 · 남은 흰색인 것 — 그것은 G-222 단계 2(외형)가 맡는다. 이 묶음은 **창이 작업 표시줄 밑으로 들어가는 것**만 본다
- [x] 1. 윈도우에서 창이 작업 영역 안에 선다 — 몸 상태 막대 · 핫바가 작업 표시줄에 안 가린다
  - 기준: 창을 세우거나 최대화하거나 크기를 바꿀 때 창의 바깥 사각형이 그 화면의 **작업 영역**(`DisplayServer.screen_get_usable_rect` — 작업 표시줄을 뺀 곳) 안에 든다. 처음 켤 때 창이 작업 영역보다 크면 줄여서 가운데에 둔다 (테스트: 작업 영역을 흉내 낸 값으로 맞추는 계산 — 2560×1440 화면 · 아래 48px 작업 표시줄 · 배율 100/125/150%)
  - 기준: 몸 상태 막대(ui/hud/needs_gauges.gd · 체력) · 핫바 · 시계 등 화면 가장자리에 붙은 HUD 는 보이는 창 안쪽에 선다 — 창 아래 가장자리에서 떨어진 간격이 맥과 같다 (화면 사진: 1280×720 · 2560×1440)
  - 기준: 맥 · 리눅스에서 지금과 달라지지 않는다. 고친 판을 윈도우에서 실제로 보는 것은 사람이 한다 — report 에 「윈도우에서 최대화 · 창 모드로 아래 막대가 보이나」를 확인할 것으로 적는다
  - 사람이 준 사진: `~/Desktop/naru-saves/G-223-1-2026-10-15/windows-esc.png` — 창 오른쪽 위 단추가 「이전 크기로」(최대화된 상태)인데 아래가 작업 표시줄 밑이다

## [x] G-224 제작 시설 대기열 줄 · 「연료」 · 핫바 휠 · 가득 차면 안 먹기 · 몸 상태 칸 겹침 (사람이 친구와 해 보고 2026-10-16)
- 차선: 3d-lane2
- spec: spec/05_craft/crafting-stations.md · spec/02_player/hunger-thirst.md · spec/12_ui/hud.md
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- [x] 1. 대기열 줄을 「용광로 − 칸 + ×」 로
  - **사람 결정 2026-10-16**: 「유지 용광로 지금 2 / 유지2 이런식으로 되어 있는데 너무 조잡해 … 유지 용광로 지금 2/유지2 이것도 이상해 그냥 용광로 - 칸 + x 이렇게 만들어」
  - 지금(이 세션이 잼): ui/hud/station_window.gd `_queue_line()` 457-488 이 Mode(「유지」) · Item · × · Count · Doing · −/+/× 버튼 · Why 를 줄에 늘어놓고, `_refresh_queue()` 425-436 이 유지 줄의 Count 를 STATION_QUEUE_HAVE(「지금 %d / 유지 %d」)로 · Mode 를 「유지」로 쓴다 — 「유지」가 두 번 나온다
  - 기준: 대기열 한 줄 = **물건 이름 · − · 수 칸 · + · ×** 그것뿐 (테스트: 줄의 보이는 자식이 이 다섯이다). 「유지」 글자 · 「지금 %d / 유지 %d」 · 「×N」 은 줄에 없다
  - 기준: 수 칸은 숫자를 바로 넣을 수도 있다(임시 — − · + 와 같은 change_queue_count 길). 「만드는 중 n초」 · 막힌 까닭(Why)은 줄 안 글자가 아니라 마우스를 올리면 뜨는 풀이로 옮긴다(임시)
  - **사람 결정 2026-10-16**: 「그 목록에서 유지랑 지금의 글자를 빼달라는거야. 유지라는 기능을 빼달라는게 아니라」 — 「N개 유지」 기능 · 다 채워도 줄이 남는 것은 그대로다. 이 단계는 줄의 글자 · 모양만 바꾸고 유지의 동작(crafting_station.gd · stations.gd)은 건드리지 않는다
- [x] 2. 「아무 연료」 → 「연료」
  - **사람 결정 2026-10-16**: 「아무연료 말고 그냥 연료 라고만 하면 될 것 같아」
  - 기준: i18n/ko.po:1157 `TAG_fuel` 을 「연료」로. en.po 는 그 뜻 그대로(Fuel). 다른 TAG_ 라벨에 「아무」가 붙은 것이 있으면 같이 뺀다 (테스트: tests/craft/recipes/test_smelter_recipes.gd:238 을 맞춘다)
  - 기준: (앞 단계 QA) 쓰지 않게 된 _set_text 가 남았다 — game/ui/hud/station_window.gd:437 static func _set_text 를 이제 아무도 부르지 않는다(grep 결과 정의뿐). 지워라
  - 기준: (앞 단계 QA) 쓰지 않는 테마 갈래 QueueWhy · QueueDoing — game/ui/theme/naru_theme.tres:87-90 의 QueueWhy(들여 쓰기 stylebox queue_why) · QueueDoing(초록 글자)은 Why · Doing 라벨이 사라져 쓰는 곳이 없다. 두 갈래와 SubResource("queue_why")를 지우고, 이 갈래를 보는 테스트가 있으면 함께 정리하라
- [x] 3. 핫바 1~0 을 마우스 휠로도 고른다
  - **사람 결정 2026-10-16**: 「1234567890 이거 마우스 휠로도 움직일 수 있게」
  - 지금: 휠은 R 을 누른 탄 고르기(combat/tools_as_weapons/attack.gd:168-171) · 기초 높이(build/buildings.gd:176)에만 쓴다 — core/input/input_actions.gd:30-31 「핫바는 휠을 쓰지 않는다」
  - 기준: 그 둘이 휠을 쥐지 않았을 때 휠 아래 = 다음 칸 · 위 = 앞 칸(임시), 10 ↔ 1 은 돌아간다. 탄 고르기 · 기초 높이 중에는 핫바가 안 움직인다 · 창(가방 · 시설 창 등)이 열려 있을 때도 안 움직인다 (테스트). input_actions.gd 주석과 spec 을 고친다
  - 기준: (앞 단계 QA) 주석에 옛 갈래 이름 「아무 …」가 남았다 — 화면 글자를 가리키는 주석이 옛 라벨 그대로다. game/tests/craft/stations/test_drink_cups.gd:191 의 메시지 「갈래 이름(아무 빈 컵)으로 적힌다」 → 「(빈 컵)」, game/tests/ui/hud/test_station_window_frame_cost.gd:22 · :197 「아무 곡물」 → 「곡물」, game/tests/ui/hud/test_station_tag_inputs.gd:5 「아무 곡물 1」 → 「곡물 1」, game/tests/craft/recipes/test_smelter_recipes.gd:211 「아무 연료가 아니라」 → 「연료 갈래가 아니라」. 사람 말을 옮긴 인용(test_drink_cups.gd:4 · :99, spec)과 '어느 곡물이든' 뜻으로 쓴 _temporary 설명은 그대로 둬도 된다
- [x] 4. 배고픔 · 목마름이 100% 면 먹지도 마시지도 않는다
  - **사람 결정 2026-10-16**: 「음식 및 음료는 100% 상태에선 먹어지지 않게」
  - 지금: player/hunger_thirst/hunger_thirst.gd:128-141 eat · drink 는 1.0 으로 자르기만 한다 — 가득이어도 food.gd `eat_from()` 60-68 이 물건을 먼저 bag.take 하고, drinking.gd `_sip()` 74-84 · 물가 마시기(96) · 컵(107)도 그대로 쓴다. 서버 길(core/net/net_vitals.gd:322-324 · 358-359 · 366-372)도 같다
  - 기준: 그 음식이 채우는 몸 상태(배고픔 / 목마름)가 **모두** 100% 면 먹지 않고 물건 · 컵 물이 줄지 않는다 — 하나라도 덜 찼으면 먹는다(임시). 혼자 판 · 서버 판(참가자의 몸은 서버 사본) 둘 다 (테스트). 「배가 부르다」 같은 짧은 알림(임시)
- [x] 5. 몸 상태 칸(배고픔 · 목마름 · 체력)이 아직도 겹친다 — 윈도우
  - 사람이 본 것 (2026-10-16): 「아직도 왼쪽 아래 배고픔 목마름 Hp바가 겹쳐져 있음」 — G-223-1(창을 작업 표시줄 위로 맞추기, core/window_fit.gd) 판 뒤에도
  - 자리(이 세션이 잼): ui/hud/needs_gauges.tscn:6-12 Needs 는 아래 기준 offset_top −160 · bottom −122 (배고픔 고리 0-38 · 글 44-116 · 목마름 고리 124-162 · 글 168-240), ui/hud/health_bar.tscn:5-11 Health 는 −116 · −100. 둘이 따로 아래에 붙어 있고 글 너비가 고정(72)이다
  - 기준: **재현부터** — 윈도우 배율(content_scale_factor 1.25 · 1.5) · 창 크기 · 글이 긴 경우(「배고픔 100%」 · 영어)를 바꿔 ui_shots 로 찍어 사각형이 겹치는 것을 먼저 본다. 사람 윈도우 사진은 사람에게 따로 받는다(받으면 ~/Desktop/naru-saves/G-224-5-* 에 둔다)
  - 사람 사진 (2026-10-16, 친구 윈도우 2560×1440 · 창 **최대화** · 판은 **36c217cc**(G-223-1 창 맞추기가 든 판 — 사람이 version.txt 로 확인)): ~/Desktop/naru-saves/G-224-5-2026-10-16/windows-hud.png
    - 본 것(이 세션): 게임 화면 아래 끝이 작업 표시줄 **위**에서 끝난다(최대화 창이라 표시줄에 안 가린다 · 좌우는 검은 띠). 그런데 그 화면 **안에서** 배고픔 · 목마름 고리의 아래가 잘리고, 체력 막대는 왼쪽 아래 구석에 빨간 조각만 보인다 — 몸 상태 칸이 **화면 아래 끝 밖으로** 그려진다. G-223-1 의 「작업 표시줄에 가린다」만으로는 설명이 안 된다
    - 기준: 이 사진의 비율(가로가 더 넓어 좌우 검은 띠 · stretch canvas_items keep)로 ui_shots 를 찍어 칸이 화면 밖으로 나가는 것을 먼저 본다 — 윈도우 글꼴 · 배율에서 칸 높이가 커져 아래 기준(offset_bottom)에서 밖으로 밀리나 본다
  - 기준: 셋을 **한 컨테이너**(VBox/HBox)에 넣어 글이 길어져도 서로 밀어낸다 — 어느 배율 · 크기에서도 사각형이 서로 안 겹친다 (테스트: tests/ui/hud/test_body_gauges_layout.gd 를 배율 · 긴 글로 넓힌다)
- [x] 6. (앞 단계 QA) 뽑은 판(pck) 자체를 재는 검사가 루프에 없다
  - 기준: 뽑은 판(pck) 자체를 재는 검사가 루프에 없다 — 이번 원인은 tools/export_windows.sh 로 뽑은 판에서만 보였고, test_scene_roots_survive_export.gd 는 .tscn 글 모양만 본다. 다른 덮어쓰기(예: 프리셋이 아닌 다른 속성)가 같은 식으로 끼면 또 맥에서는 안 보인다. ~/Desktop/naru-saves/G-224-5-2026-10-16/repro/probe7.gd 를 tools/ 로 옮겨, 머리 없이 뽑은 .pck 와 글 씬의 HUD 사각형을 견주는 검사(다르면 실패)를 export 뒤에 한 번 돌게 하라 (막는 것은 아니다)

## [x] G-225 선착장은 정해진 한 곳 · 모일 사람만 모여 출발 · 섬을 다녀오면 느려진다 · 서버장이 끄면 참가자가 튕긴다 (사람이 친구와 해 보고 2026-10-16)
- 차선: 3d-start
- spec: spec/11_multiplayer/netcode.md · spec/03_world/frontier-islands.md (선착장 · 배)
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- 테스트는 headless 둘(셋)을 ENet 으로 · Steam 은 가짜(fake_steam.gd)로. **사람의 Steam 을 켜거나 로그인하지 않는다**
- [x] 1. 서버장이 끄면 참가자(맥)가 튕긴다
  - 사람이 본 것 (2026-10-16): 「맥이 참가자 윈도우가 서버입장에서 윈도우가 바로 끄면 맥에서 아까 같은 에러가 나와」 — G-222.6(5b0501d3, 끌 때 안전하게) 이 든 판(bd4852a3)에서
  - 증거: ~/Desktop/naru-saves/G-224-crash-2026-10-16/Godot-2026-10-16-000322.ips — EXC_BAD_ACCESS(0x1d8) · `std::recursive_mutex::lock` ← Godot ← **libgodotsteam** ← Godot ← `-[NSApplication terminate:]`. 앱이 끝나는 길(종료 알림)에서 godotsteam 이 이미 풀린 것을 잡는다
  - 지금(이 세션이 잼): 서버가 사라지면 net_session.gd `_on_server_gone` 411 → `_end` 416-422 → leave(deferred, peer.close · multiplayer_peer=null · leaveLobby) → game_root.gd `_leave_to_main_menu` 941-957. steamShutdown 은 그 뒤 AppQuit.finish(core/app_quit.gd:27-32) 에서만. 로비를 떠난 것을 받는 Steam 신호(lobby_chat_update)는 안 잇는다(steam_api.gd:59-62)
  - 기준: **재현부터** — 가짜 Steam 으로 서버장이 사라진 뒤 참가자가 메뉴로 나가고 · 끄는 길(창 X · Cmd+Q = NSApplication terminate · 메뉴 끝내기) 셋을 테스트로 밟고, 그 길에서 SteamMultiplayerPeer · 로비 · 콜백이 steamShutdown 보다 **먼저** 다 풀리는지(남은 참조 0) 본다. Cmd+Q 가 AppQuit 길을 타는지 본다
  - 기준: 서버장이 끄든 튕기든 참가자는 「서버가 닫혔다」를 보고 메뉴로 나가고, 그 뒤 어떤 길로 꺼도 충돌 보고서가 생기지 않는다 — 고친 판은 사람이 맥에서 다시 본다 (사람 확인)
- [x] 2. 집 섬 선착장은 만들지 않고 정해진 한 곳에 있다
  - **사람 결정 2026-10-16**: 「선착장을 만드는거 말고 한곳에 지정해야 할 것 같음」
  - 지금: 집 섬 선착장은 작업대 레시피(craft/recipes/workbench.json:765-769)로 만들어 world/frontier_islands/dock_builder.gd 로 물가에 놓는다(기둥을 치면 부서진다, 172). 개척 섬 선착장은 지도에 박혀 있다(islands.json `dock_prebuilt`)
  - 기준: 집 섬에도 개척 섬처럼 선착장이 **처음부터 한 곳**에 박혀 있다(못 부순다). 자리는 사람이 아직 안 정했다 — 처음 나는 자리 가까운 물가(임시)로 하고 spec 에 「임시」로 적는다
  - 기준: 선착장 레시피 · 놓기를 뺀다. 옛 저장본에 놓아 둔 선착장 · 가방의 선착장 물건이 있어도 읽힌다 — 놓인 것은 지우고 재료를 그 자리 상자/가방으로 돌려준다(임시) (테스트)
- [x] 3. 선착장 창 — 모일 사람만 모여 출발한다
  - **사람 결정 2026-10-16**: 「선착장에 들어가면 서버에 모든 사람이 텔레포트 하는데 이렇게 말고 모일 사람들이 모이고 출발 하면 출발 시키게」 · 「선착장에 우클릭하면 바로 와지는데 ui창이 열리게 하고 ui창에 참가한 사람만 이동할 수 있도록」 · 앞서 2026-10-15 「섬열기는 자유롭게 다같이 가도 되고 혼자 가도 되고 서버 몇명 남아서 본섬을 지켜도 되고」
  - 지금: dock.gd `_unhandled_input` ~148-155 우클릭 → board() → game_root.gd `sail_to` 405-418(참가자는 HOST_SAILS_NOTICE 만) → net_world.gd `sail_all` 1080-1087 이 **모든 사람**에게 _sail → `_cross` 439-468 이 옛 섬을 풀고 새 섬을 짓는다. 서버는 섬 하나만 돌린다 (netcode.md:113 · 148 「임시」)
  - 기준: 우클릭하면 바로 건너가지 않고 **선착장 창**이 열린다 — 갈 섬 · 「참가」/「빠지기」 · 참가한 사람 목록 · 「출발」. 서버장 · 참가자 누구나 참가하고 출발을 누를 수 있다(임시 — 누가 출발을 누르나는 사람이 안 정했다). 같은 선착장의 창을 연 사람끼리 목록이 같이 바뀐다 (테스트: 셋이 붙어 둘만 참가)
  - 기준: 출발하면 **참가한 사람만** 그 섬으로 간다 — 아무도 끌려가지 않는다. 섬 여럿(사람이 서로 다른 섬에 있다)의 짜임 — 섬마다 누가 있나 · 사람이 없는 섬의 몹 · 주민 · 시설 — 을 netcode.md 「섬 여럿」에 적고 단계로 나눈다. **이 단계에서는** 남는 사람이 있으면 배가 떠나지 않고 누른 사람이 까닭을 본다(임시 막음 CREW_SPLIT_NOTICE — 5~7단계가 걷는다). 섬을 열 권한은 묻지 않는다(G-222.3)
  - 쪼갬 (루프 2026-10-16): 「안 한 사람은 제 섬에 남아 그대로 논다」는 서버가 섬 여럿을 같이 돌려야 해서 한 회차에 못 짓는다 — 5 · 6 · 7단계로 옮겼다 (앞 회차 QA 의 제안)
  - 기준: 돌아올 때도 같은 창으로 (테스트 — 다 타면 같이 돌아온다). netcode.md:113 · 148 을 이 결정으로 고친다. 「돌아올 때 참가한 사람만(남는 사람은 개척 섬에)」은 7단계
  - 기준: (앞 단계 QA) netcode.md 에 DockBuilder 가 남았다 — spec/11_multiplayer/netcode.md 41줄 「놓는 곳(Buildings · Stations · Chests · Torches · Beds · Furnitures · ClimbGear · DockBuilder)」에서 DockBuilder 를 지워라. dock_builder.gd 는 이번 단계에서 없어졌고 같은 파일 99줄 표는 이미 고쳤다 (파일이 150줄 꽉 찼으니 줄을 늘리지 말 것)
- [>] 4. 다른 섬에 다녀오면 느려진다 → G-225-4 로 떼었다
  - 사람이 본 것 (2026-10-16): 「선착장에 와서 다른 섬으로 오니 이동속도가 느려지는 것 같음. 그리고 다시 본섬에 돌아와도 느린느낌 배고픔이랑 목마름 100%임에도」 — 서버장인지 참가자인지는 모른다
  - 짚어 볼 곳 (짐작이다 — 재현으로 가린다): player.gd:226 속도 = base_speed(목마름 0 이면 ×0.7, hunger_thirst.gd:153) · speed_multiplier(버프) · crouch_factor · slope_factor. 섬을 건널 때 player_state.gd remember/restore(12-61)가 웅크림 · 버프를 되살린다. 참가자는 net_vitals.gd `_vitals` 469-483 이 서버 사본으로 덮는다 · `_cross` 가 섬을 지을 때마다 `_fit_body`(game_root.gd:1239-1302)가 다시 붙인다(두 번 붙어 두 번 깎이나)
  - 기준: **재현부터** — 평지에서 걸은 속도를 재고, 섬을 갔다 와서(서버장 · 참가자 둘 다, 3단계 전 길이든 뒤 길이든) 같은 자리에서 다시 잰다. 느려지면 까닭을 고치고 테스트로 남긴다. 재현이 안 되면 report 에 잰 숫자를 남기고 사람에게 서버장 · 참가자 어느 쪽이었나 묻는다
  - 기준: (앞 단계 QA) sail_to 가 아직 모두를 끌고 간다 — game/core/game_root.gd sail_to(421~) 는 사람 길에서는 빠졌지만(Dock.boarded → open_dock_window) 여전히 net.members() 전부를 _sail_crew 에 넘긴다. 테스트(test_world_on_server · test_locked_island · test_autosave)만 쓰는 길이라 막는 것은 아니지만 「아무도 끌려가지 않는다」와 어긋난 공개 함수다. G-225.7 에서 막음을 걷을 때 sail_to 를 테스트 도우미로 이름을 바꾸거나 crew 를 인자로 받게 하고, 주석에 「사람 길 아님」을 적어라
  - 기준: (앞 단계 QA) 낡은 주석 둘 — game/core/game_root.gd 844줄 _depart 주석 「(sail_to → NetWorld.sail_crew)」는 실제로는 _sail_crew 를 부른다 → 「_sail_crew → NetWorld.sail_crew」로 고쳐라. game/ui/hud/notice.gd 6줄 「(core/game_root.gd 의 sail_to)」는 잠긴 섬 알림이 이제 open_dock_window 에서 뜨므로 그 이름으로 고쳐라
- [x] 5. 섬 여럿 ② — 서버가 섬을 사람 따라 세우고 내린다
  - 3단계에서 쪼갰다. 짜임은 spec/11_multiplayer/netcode.md 「섬 여럿」(루프가 정한 임시)
  - 지금: 서버장 기계는 섬 하나(GameRoot._island)만 돌리고 `_cross` 가 옛 섬을 풀고 새 섬을 짓는다. 3단계 막음(GameRoot `CREW_SPLIT_NOTICE`)이 남는 사람이 있으면 배를 안 띄운다
  - 기준: 서버가 누가 어느 섬에 있나(peer → 섬 id, 캐릭터 places 와 같은 값)를 쥐고, 사람이 선 섬을 다 세워 돌린다 — 섬마다 제 3D 세계(own_world_3d). 서버장 화면은 제가 선 섬만 그린다 (테스트: 서버장이 HOME 에 있는 채로 SULFUR 를 세우고 두 섬의 물리가 섞이지 않는다)
  - 기준: 아무도 없는 섬은 내린다 — IslandState 로 월드에 담고 짐승 · 주민 · 시설 틱은 그동안 멈춘다. 다시 오면 담은 대로 선다. 저장하면 선 섬 모두가 담긴다 (테스트)
- [x] 6. 섬 여럿 ③ — 맞추기 · 바람을 섬마다 가른다
  - 기준: 맞추기(놓는 곳 · 짐승 · 주민 · 떨어진 것 · 지도 · 몸 소식)는 그 섬에 선 사람에게만 간다. 바람(치기 · 놓기 · 줍기 …)은 바란 사람이 선 섬에 한다. 시계 · 날 · 연 섬 · 길드 · 배 명단은 모두 같다 (테스트: 두 섬에 갈린 둘이 서로의 몸 · 짐승을 받지 않고, 각자 제 섬에서 친 것이 제 섬에만 남는다)
  - 기준: (앞 단계 QA) _lower_afar 가 _afar 가 null 이면 터진다 — game/core/game_root.gd _lower_afar 의 `_afar.get_node_or_null(id)` 는 지금 부르는 곳이 모두 _afar 가 있을 때뿐이라 괜찮지만, 앞에 `if _afar == null: return` 을 두어 다음 단계에서 부르는 곳이 늘어도 안전하게 하라
  - 기준: (앞 단계 QA) 참가자 명단 값이 캐릭터 places 에 되써지지 않는다 — set_island_of 는 서버 _places 만 바꾸고 참가자 캐릭터 places 의 island 는 그대로다. 지금은 참가자가 서버장과 다른 섬에 설 길이 막혀(CREW_SPLIT_NOTICE) 어긋날 일이 없지만, G-225.7 에서 막음을 걷을 때 참가자 places 와 island_of 가 같은지 보는 단언을 test_dock_crew.gd 에 넣어라
- [x] 7. 섬 여럿 ④ — 「다 타야 떠난다」 막음을 걷는다
  - 기준: 출발하면 **참가한 사람만** 그 섬으로 간다 — 안 한 사람은 제 섬에 남아 그대로 논다. GameRoot `CREW_SPLIT_NOTICE` 와 그 번역 키를 걷는다
  - 기준: tests/multiplayer/test_dock_crew.gd 에 「셋 중 둘이 타고 출발 → 둘은 SULFUR, 솔은 HOME 에 남아 움직이고 치기가 된다」 테스트
  - 기준: 돌아올 때도 참가한 사람만 — 개척 섬에 둘이 있을 때 하나만 참가해 출발하면 그 사람만 HOME 으로 오고 다른 사람은 SULFUR 에 남는다 (test_dock_crew.gd)
  - 기준: netcode.md 표 「섬 열기 · 배」의 「남는 사람이 있으면 아직 안 떠난다(임시)」를 지운다
  - 기준: (앞 단계 QA) test_islands_split 의 _split 이 서버 속을 직접 부른다 — host.set_island_of · everywhere().sail_crew · _world_for_guest() 로 미르만 건너게 한다. G-225.7 에서 「다 타야」 막음을 걷으면 이 테스트의 _split 을 선착장 창(사람이 여는 길)으로 바꿔 진짜 길로 갈리게 하라
  - 기준: (앞 단계 QA) spec netcode.md 가 딱 150줄이다 — 한 줄만 더 붙어도 넘는다. G-225.7 에서 「섬 여럿」 단계 줄(①~④)을 붙일 때 다른 곳을 줄이거나 나누어라

## [x] G-225-4 다른 섬에 다녀오면 느려진다 (G-225 단계 4 에서 떼었다)
- 결정: **사람 답 2026-10-16** — 「1. 둘다 였어. 2. 걸음만인거 같아. 3. 아니야 그냥 계속 돌아 다닐때 체감상 느렸어」 · 「너가 쟀을 때도 시험 섬은 5.40 m/s, 진짜 섬은 5.21 이라며. 숫자가 눈으로 보이는데 왜 나한테 물어보는거야 ?」 — **잰 숫자에 이미 차이가 보이면 묻지 말고 그것부터 판다**
- 차선: 3d-start
- 떼어 온 곳: G-225 단계 4 — 나머지 단계는 G-225 가 이어 한다. 하다 만 것은 브랜치 g/G-225-4
- spec: spec/11_multiplayer/netcode.md · spec/03_world/frontier-islands.md (선착장 · 배)
- **자가 피드백**: 찾은 것을 단계로 계속 붙인다
- 테스트는 headless 둘(셋)을 ENet 으로 · Steam 은 가짜(fake_steam.gd)로. **사람의 Steam 을 켜거나 로그인하지 않는다**
- [x] 1. 다른 섬에 다녀오면 느려진다
  - 사람이 본 것 (2026-10-16): 「선착장에 와서 다른 섬으로 오니 이동속도가 느려지는 것 같음. 그리고 다시 본섬에 돌아와도 느린느낌 배고픔이랑 목마름 100%임에도」 — 서버장인지 참가자인지는 모른다
  - 짚어 볼 곳 (짐작이다 — 재현으로 가린다): player.gd:226 속도 = base_speed(목마름 0 이면 ×0.7, hunger_thirst.gd:153) · speed_multiplier(버프) · crouch_factor · slope_factor. 섬을 건널 때 player_state.gd remember/restore(12-61)가 웅크림 · 버프를 되살린다. 참가자는 net_vitals.gd `_vitals` 469-483 이 서버 사본으로 덮는다 · `_cross` 가 섬을 지을 때마다 `_fit_body`(game_root.gd:1239-1302)가 다시 붙인다(두 번 붙어 두 번 깎이나)
  - 기준: **재현부터** — 평지에서 걸은 속도를 재고, 섬을 갔다 와서(서버장 · 참가자 둘 다, 3단계 전 길이든 뒤 길이든) 같은 자리에서 다시 잰다. 느려지면 까닭을 고치고 테스트로 남긴다. 재현이 안 되면 report 에 잰 숫자를 남기고 사람에게 서버장 · 참가자 어느 쪽이었나 묻는다
  - 기준: (사람 답 뒤 — 이 세션) 지난 재기는 「같은 섬에서 전후」만 봤다. 사람은 **돌아다니는 동안 내내** 느렸다고 했다 — 그래서 아래 둘을 판다
    - ① 진짜 섬 5.21 m/s vs 시험 섬 5.40 m/s (−3.5%) — 평지라 고른 자리에서도 빠진다. 그 3.5% 가 어디서 오나(slope_factor · 잔 턱 relief · 땅에 걸리는 미끄럼 · can_walk_to 로 0 이 되는 프레임) 프레임마다 갈라 적는다. 진짜 섬 · 유황 섬을 **사람이 걷는 길**(스폰 → 숲 → 마을 → 선착장, 언덕 포함)로 걸려 평균 속도를 잰다 — 그 길에서 config.move_speed 보다 얼마나 덜 가나
    - ② **벽시계 속도** — 게임 안 속도(velocity)가 아니라 **실제 1초에 간 거리**를 잰다. 섬을 건넌 뒤 서버가 섬 여럿을 같이 돌리면(G-225.3) 물리 한 걸음이 16ms 를 넘어 물리 시간이 실제보다 늦게 흐를 수 있다 — 그러면 velocity 는 같아도 사람 눈엔 느리다. headless 가 아니라 창 있는 판(shot.sh 처럼 포커스 안 뺏는 창)에서 서버장 + 참가자, 건너기 전 · 유황섬 · 돌아온 뒤 각각 물리 시간 · 프레임 시간 · 벽시계 거리/초를 잰다. 옛 섬이 풀리지 않고 남는가(노드 수 · 물리 바디 수 전후)도 본다
    - 느려지는 것이 보이면 고치고 테스트로 남긴다. 보이지 않으면 잰 숫자를 report 에 남기고 **사람에게 다시 묻지 않는다** — 다음 사람 시험 판에서 F3(FPS)와 같이 보게 「사람 확인」으로 남긴다
  - 기준: (앞 단계 QA) sail_to 가 아직 모두를 끌고 간다 — game/core/game_root.gd sail_to(421~) 는 사람 길에서는 빠졌지만(Dock.boarded → open_dock_window) 여전히 net.members() 전부를 _sail_crew 에 넘긴다. 테스트(test_world_on_server · test_locked_island · test_autosave)만 쓰는 길이라 막는 것은 아니지만 「아무도 끌려가지 않는다」와 어긋난 공개 함수다. G-225.7 에서 막음을 걷을 때 sail_to 를 테스트 도우미로 이름을 바꾸거나 crew 를 인자로 받게 하고, 주석에 「사람 길 아님」을 적어라
  - 기준: (앞 단계 QA) 낡은 주석 둘 — game/core/game_root.gd 844줄 _depart 주석 「(sail_to → NetWorld.sail_crew)」는 실제로는 _sail_crew 를 부른다 → 「_sail_crew → NetWorld.sail_crew」로 고쳐라. game/ui/hud/notice.gd 6줄 「(core/game_root.gd 의 sail_to)」는 잠긴 섬 알림이 이제 open_dock_window 에서 뜨므로 그 이름으로 고쳐라
- [x] 2. (앞 단계 QA) 되걷기 가지만 따로 지키는 테스트가 없다
  - 기준: 되걷기 가지만 따로 지키는 테스트가 없다 — player.gd:225 의 `if climb > 0.0 and (is_on_wall() or not is_on_floor()):` 되걷기를 `if false:` 로 꺼도 movement_controls 12개가 다 통과했다(_climb_along_floor 의 is_on_wall 막음이 먼저 걸러서). 둘을 같이 꺼야 test_slope_real_island 가 196/300 으로 깨진다. 되걷기가 정말 필요한 경우(이번 걸음에 처음 면에 닿아 is_on_wall 이 아직 false 인 걸음)를 짚는 단언을 test_slope_real_island 에 더하거나, 필요 없으면 되걷기를 걷고 주석을 하나로 모아라
- [x] 3. (앞 단계 QA) 주석 오타 '잼'
  - 기준: 주석 오타 '잼' — game/player/movement_controls/player.gd 의 _climb_along_floor 주석 '(본섬 0.8 cm · 유황 섬 0.05 cm, G-225-4.2 잼)' 의 '잼' 을 '잰 값' 으로 고쳐라

## [x] G-908 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-lane2
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. 번역 키 STATION_QUEUE_HAVE 가 짝 없이 남았다 (G-224 단계 1)
  - 기준: 번역 키 STATION_QUEUE_HAVE 가 짝 없이 남았다 — 「지금 %d / 유지 %d」 글자를 쓰는 곳이 없어졌는데 game/i18n/ko.po:1207 · en.po:1207 에 남아 있다. 두 po 에서 지워라 (test_station_keep_window 의 번역 검사 목록에서는 이미 뺐다)

## [x] G-226 최적화 — 섬 만들기 · 그리기 · 한 프레임 (동작은 그대로)
- 차선: 3d-lane2
- **사람 결정 2026-10-16**: 「맵생성이나 멀티플레이나 모든 부분에서 최적화 해줄 수 있어 ? 기능적인거 많이 건들이는거 말고」 — 멀티 쪽은 G-227(차선 3d-start, G-225 뒤)
- **건드리지 않는 곳**: core/net/ · multiplayer/ · core/game_root.gd · world/frontier_islands/ — G-225 · G-227 이 쥔다. 여기서 찾은 것은 report 에 적어 G-227 에 단계로 붙인다
- **동작 · 보기는 그대로** — 보이는 거리 · 그림자 · 풀 밀도 같은 화질 값을 낮추는 것은 최적화가 아니다(사람이 안 정했다 — 낮추고 싶으면 제안으로 report 에). 같은 시드는 같은 섬 · 같은 자원 자리를 낸다
- **자가 피드백**: 잰 숫자에서 큰 것부터 단계로 붙인다. 한 단계 = 한 곳 · 전후 숫자
- 웹에서 찾은 기법 (이 세션이 2026-10-16 찾고 코드와 맞대 봄 — 사람 요청 「최신 최적화 기법을 웹에서 찾아보고 적용 할만한 것들을 가져와보자」). **잰 숫자로 효과가 있는 것만** 남긴다
  - Terrain3D 가리개 굽기 — 지형으로 Occluder3D 를 굽는다(LOD 4 가 기본 · 영역마다 몇 초 · 지형이 바뀌면 다시). Godot 기본 굽기는 Terrain3D 를 못 본다 (terrain3d.readthedocs.io/en/latest/docs/occlusion_culling.html). 우리 섬은 실행 중에 만들어지므로 섬을 만들 때 굽고 island_cache 에 같이 둔다 — project.godot 에 occlusion culling 켜기
  - Terrain3D 충돌 — 지금 `Terrain3DCollision.FULL_GAME`(world/terrain/test_island.gd:188). Terrain3D 1.0 의 Dynamic 은 사람 둘레만 충돌을 만든다(메모리 큰 절감). **서버는 사람 · 몹 · 주민이 섬 곳곳에 있으니 FULL 을 둔다** — 참가자 화면만 Dynamic 이 되나 잰다(참가자도 제 몸은 제가 움직이므로 제 둘레만 있으면 된다)
  - 횃불 빛(world/lighting/torch_light.gd) — 빛 · 그림자의 distance fade(멀면 그림자 먼저 끄고 빛도 끈다). 놓은 횃불이 많을수록 크다. 가까이서 보이는 빛 · 그림자는 그대로
  - 주민 · 몹 애니메이션 — VisibleOnScreenEnabler3D / Notifier3D 로 화면 밖이면 애니메이션 멈추기, 멀면 덜 자주 (Godot 문서 「Optimizing 3D performance」)
  - 자동 인스턴싱 — Forward+ 는 **같은 메시 · 같은 재질**의 MeshInstance3D 를 묶어 그린다. 블록 · 시설 · 상자가 재질을 하나씩 복사하면(material.duplicate · 색만 다른 새 재질) 묶이지 않는다 — 같은 것은 재질을 나눠 쓰고, 색은 instance uniform 으로
  - 섬 만들기를 WorkerThreadPool 로 — 높이 · 자원 · 나무 자리 계산은 스레드에서, 노드 만들기 · 붙이기는 메인에서(call_deferred) 나눠 한다. 노이즈는 한 번 이미지/배열로 뽑아 다시 쓴다
  - 하지 않을 것: Terrain3D auto_shader 끄기 · 그림자 품질 낮추기 — 빨라지지만 보이는 것이 바뀐다(사람이 안 정했다)
- [x] 1. 먼저 잰다 — 어디가 느린가
  - 기준: 숫자를 **spec/00_core/performance.md**(새로)에 표로 적는다 — 이 맥 · 날짜 · 커밋. 고치기 전 기준선이다
    - 섬 만들기: 새 월드(시드 셋) 만들기 · 저장본 열기 · island_cache 가 있을 때/없을 때 — 단계별(지형 · 물 · 자원 · 나무 · 풀 · 콜리전 · 내비) 걸린 시간 (headless, Time.get_ticks_usec)
    - 한 프레임: harness/shot.sh 의 자리별 프레임 시간 + Performance 모니터(그리기 호출 · 물체 수 · 정점 · 물리 바디) — 집 섬 넓은 자리 · 숲 · 마을(주민 많을 때) · 개척 섬
    - 스크립트: _process/_physics_process 를 도는 노드 수와 그 합 시간 (가장 비싼 열)
    - 메모리 · 저장본 크기 · 저장 걸린 시간
  - 기준: 사람 창을 띄우지 않는다 — headless 와 harness/shot.sh 만 (LOOP.md 2절)
- [x] 2. 안 보이는 것은 그리지도 · 꾸미지도 않는다
  - **사람 요청 2026-10-16**: 「안보이는 부분은 렌더나 연산을 안한다거나 그런것들도 잇잖아 그런건 되어 있어 ?」
  - 지금(이 세션이 잼, 3d-start): 화면 밖은 Godot 가 저절로 안 그린다(frustum). 나무 · 풀 · 바위 · 자원은 칸마다 MultiMesh + 멀면 안 그림(world/terrain/vegetation_view.gd · resource_view.gd 의 visibility_range) — **되어 있다**. 그 밖은 없다: project.godot 에 occlusion culling 이 꺼져 있다(OccluderInstance3D 0 개 — 산 · 언덕 · 벽 뒤도 그린다) · 건물 · 시설 · 상자 · 주민 · 몹 · 배 같은 하나하나 놓인 것에 visibility_range 가 없다 · 화면 밖 · 먼 것의 _process(움직임 꾸미기 · 애니메이션 · 불빛)를 멈추는 곳이 없다(VisibleOnScreenEnabler 0 · _process 를 도는 스크립트 48 파일)
  - 기준: 1단계에서 잰 자리로 켜 보고 나아지는 것만 남긴다 — ① occlusion culling(지형 · 큰 건물 · 벽을 가리개로, Terrain3D 가 가리개를 굽는 길이 있으면 그것) ② 놓인 것의 visibility_range(지금 나무가 쓰는 거리와 같은 꼴 — **보이는 거리는 사람 눈에 차이가 없게**, 멀리서 사라지는 게 보이면 안 된다) ③ 화면 밖 · 먼 것은 **보기만** 멈춘다
  - 기준: **게임 일은 안 보여도 계속한다** — 주민 일 · 시설 제작 · 작물 · 몹 · 배고픔은 보이든 말든 똑같이 흐른다(테스트: 카메라를 돌려 안 보이게 해도 제작 · 주민 일이 같은 시간에 끝난다). 멈추는 것은 그림 · 애니메이션 · 소리 · 꾸밈뿐
- [x] 3. 잰 것에서 가장 비싼 것을 줄인다 (단계를 더 붙인다)
  - 기준: 고친 곳마다 같은 자리에서 다시 재 performance.md 에 전후를 적는다 — 나아지지 않으면 되돌린다
  - 기준: 같은 시드 → 같은 섬을 테스트로 묶는다(지형 높이 · 자원 · 나무 자리의 해시가 고치기 전과 같다). 전체 테스트 · 사진(shot.sh) 이 전과 같다
  - 기준: 흔한 것부터 본다(짐작 — 재서 가린다): 섬 만들기에서 한 프레임에 몰린 일 → 나눠 하기/스레드 · 같은 계산 되풀이 → island_cache · 나무 · 풀 · 바위 → MultiMesh · 멀리 있는 것 → visibility range(보이는 거리는 그대로) · 매 프레임 도는 스크립트 → 타이머/필요할 때만
  - 기준: (앞 단계 QA) 섬 위 건물 조각 검사가 상수와만 견준다 — tests/core/test_far_cull.gd test_on_the_island_built_pieces_are_not_shorter_than_trees 는 assert_gte(…, FarCull.BUILDING) 이라 BUILDING 을 0 으로 낮춰도 그대로 통과한다(QA 가 깨 봄). 지금은 옆 단위 검사가 BUILDING == ResourceView.RANGE["pine"] 을 묶어 지키니 막히지는 않는다. 상수 대신 ResourceView.RANGE["pine"] 과 견주게 바꿔라
  - 기준: (앞 단계 QA) 작물이 자랄 때 다시 재는 검사가 없다 — life/farming/crops.gd:723 의 FarCull.refresh(plant_node) 를 지워도 떨어지는 검사가 없다. tests/core/test_far_cull.gd 에 섬에 작물을 심고 단계를 올린 뒤 plant_node.visibility_range_end 가 새 크기의 FarCull.range_for 와 같은지 단언을 더하라

## [x] G-227 최적화 — 멀티 · 주민 · 몹 (동작은 그대로)
- 차선: 3d-start
- 순서: G-225 뒤 — 섬 여럿을 같이 돌리는 것이 들어간 다음에 잰다
- **사람 결정 2026-10-16**: 「맵생성이나 멀티플레이나 모든 부분에서 최적화 해줄 수 있어 ? 기능적인거 많이 건들이는거 말고」
- **동작은 그대로** — 보내는 내용 · 순서 · 권한 · 저장 형식은 바꾸지 않는다. 남의 몸 · 주민이 덜 부드러워지면 안 된다(G-222.5)
- 테스트는 headless 셋을 ENet 으로 · Steam 은 가짜(fake_steam.gd). **사람의 Steam 을 켜거나 로그인하지 않는다**
- **자가 피드백**: 잰 숫자에서 큰 것부터 단계로 붙인다. G-226 이 report 에 남긴 것도 여기 단계로
- 웹에서 찾은 기법 (이 세션이 2026-10-16 찾음 — 잰 숫자로 효과가 있는 것만 남긴다)
  - 보내는 수 줄이기 — 위치 · 방향을 그대로(Vector3 float 셋) 보내지 말고 줄인 꼴(섬 안 좌표는 16비트 · 방향은 각도 하나 등)로. 지금 몸 · 주민 모습은 0.1초마다(net_world.gd) 이어 그리기가 있으니 빈도는 그대로 두고 크기를 먼저 줄인다
  - 사람마다 보이는 것만 — MultiplayerSynchronizer 의 visibility(set_visibility_for · public_visibility=false) 처럼 사람마다 받을 것을 고른다. 우리는 RPC 로 보내므로 같은 생각을 rpc_id 대상 고르기로 — 다른 섬 · 먼 것은 빼거나 덜 자주. 커뮤니티 글에는 visibility 가 대역폭보다 숨김용이라는 말도 있다 — **반드시 잰다**
  - 안 바뀐 것은 안 보낸다(델타) — 멈춘 주민 · 몹 · 시설은 바뀔 때만
- [x] 1. 먼저 잰다
  - 기준: spec/00_core/performance.md 에 「멀티」 표를 더한다 — 서버장 + 참가자 둘(셋이면 셋)로 5분: 사람당 초당 보내고 받는 바이트 · RPC 수(종류별 위 10) · 서버 한 프레임 시간 · 들어올 때 건네는 월드 크기 · 걸린 시간 · 섬을 건널 때 걸린 시간
  - 기준: 주민 · 몹 · 시설 · 작물이 서버에서 한 프레임에 쓰는 시간 (주민 0 · 10 · 30 일 때)
- [x] 2. 잰 것에서 가장 비싼 것을 줄인다 (단계를 더 붙인다)
  - 기준: 고친 곳마다 전후 숫자를 적는다 — 나아지지 않으면 되돌린다. 전체 테스트가 전과 같다
  - 기준: 흔한 것부터 본다(짐작 — 재서 가린다): 바뀐 것만 보내기 · 안 움직이는 것은 안 보내기 · 같은 프레임 소식 묶기 · 멀리 있는/다른 섬의 것 덜 자주 · 신뢰 안 해도 되는 것(몸 모습)은 unreliable · 주민 길 찾기 · 판단을 매 프레임 말고 띄엄띄엄
  - 기준: 보내는 횟수를 줄여도 남의 몸 · 주민 이어 그리기(G-222.5 테스트)가 통과한다
  - 기준: **안 보이는 것은 덜 보낸다**(사람 요청 2026-10-16 「안보이는 부분은 렌더나 연산을 안한다거나」) — 지금은 섬의 모든 주민 · 몹 · 남의 몸 소식을 모두에게 보낸다(짐작 — 1단계에서 잰다). 다른 섬에 있는 사람에게는 그 섬 것을 안 보내고, 먼 것은 덜 자주 보낸다(거리 · 빈도 임시). 가까이 오면 바로 제자리로 보인다 (테스트)
  - 기준: (앞 단계 QA) 참가자가 받는 RPC 수가 표에 없다 — performance.md 「사람당」 표는 RPC 보냄만 있다. counting_multiplayer.gd 는 보낸 쪽만 센다. 다음 재기 때 받는 쪽 셈(서버 RPC 를 받는 사람마다 나눈 값)을 「RPC 받음」 열로 더하거나, 서버 보냄을 사람 수로 나눈 값임을 표 아래 한 줄로 적어라
  - 기준: (앞 단계 QA) ENet 기다림 60~120초에 「임시」 표시가 없다 — net_measure_probe.gd 의 set_timeout(32, 60000, 120000) 두 곳은 주석에 「재기용」만 있다(report 는 temporary 로 올렸다). STATIONS · PLATES 처럼 「임시 — 재기용」으로 적어라. 들어올 때 22초 멈춤을 고치는 단계에서 이 늘림을 걷을 수 있는지 다시 재라
- [x] 3. (앞 단계 QA) 몸 첫 소식을 잃으면 이름·외형이 1초 빈다
  - 기준: 몸 첫 소식을 잃으면 이름·외형이 1초 빈다 — net_bodies.gd _send_to_members 는 처음 보는 몸에 전부를 한 번 싣고 곧바로 _told[id][who]=true 로 적는데, _snapshot 은 unreliable 이라 그 한 번을 잃을 수 있다. 그러면 다음 소식은 n·a·e 가 빠진 것이고 받는 쪽 _filled(state, null) 은 그대로 넘겨 VisitorBody.apply 가 입은 것({})·기본 외형으로 그린다 — STEADY_EVERY(1초)까지. 먼 몸이면 {} 를 받아 _states[who]={} (자리 없는 몸)가 된다. (실제 망 손실에서만, 테스트 루프백에선 안 난다.) 고치는 법: 처음 보는 몸은 STEADY_EVERY 번 동안 계속 전부를 싣거나(_told 를 true 대신 남은 횟수로), 받는 쪽 _snapshot 에서 앞선 것이 없는 번호에 빈 소식/steady 없는 소식이 오면 건너뛰게 하라. test_send_less 에 「첫 _snapshot 하나를 버린 뒤에도 이름표 · 외형이 다음 소식에 서 있다」 단언을 더하라
  - 기준: (앞 단계 QA) performance.md 사람당 표에 창이 섞였다 — 「사람당 초당 바이트 · RPC (5분 평균)」 표의 보냄·받음은 G-227.1 의 5분·고치기 전 숫자인데 새 「RPC 받음」 열은 G-227.2 의 60초 창 숫자다(전인지 후인지도 안 적혔다). 다음 재기 때 표 전체를 같은 창 · 같은 시점(고친 뒤) 숫자로 갈거나, 「RPC 받음」 열이 어느 재기(전/나)인지 표 아래 줄에 적어라

## [x] G-909 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-lane2
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. 손에 든 것 바꿔 들기 검사가 바꿔 들지 않는다 (G-226 단계 2)
  - 기준: 손에 든 것 바꿔 들기 검사가 바꿔 들지 않는다 — test_on_the_island_the_held_item_is_remeasured_when_switched 는 wood 하나만 들고 _ours 만 본다. 크기가 다른 두 아이템을 차례로 들어 거리가 각 크기(또는 몸 거리 이상)로 바뀌는지 단언하라

## [x] G-228 짐승 — 흔적 · 해체 시간 · 생포한 짐승 나이 · 멀리서 끊김 · 참가자 들어올 때 튐
- 차선: 3d-lane2
- **사람 요청 2026-10-16** (원문 그대로)
  - 「동물 발자국 시스템 만들기. 동물의 똥이나 발자국 같은걸 동물 스폰기준으로 랜덤하게 생성. 어떤 동물이 서식하는지 알 수 있음.」
  - 「동물 해체 하는데 딜레이 추가 임시 3초」
  - 「npc에 나이 있는지 확인하고 없으면 추가. 동물은 생포한 동물에게만 나이 추가.」
  - 「멀티서버에서 참가자 기준으로 동물 움직이는게 뚝뚝 끊겨보임. 근데 이게 가까이 있으면 부드럽게 보임. 아마 어제 최적화 때문에 그러는거 같은데. 이렇게 하지말고 스폰으로 아예 결정 짓는게 나을듯 함. 끊기는건 너무 이상하게 보임」
  - 「멀티서버에서 참가자 기준으로 스폰 될 때, 서버장 입장에서 보면 주변에서 스폰 됐다가 다시 전에 위치로 이동 되는게 보임」
- 테스트는 headless 셋을 ENet 으로 · Steam 은 가짜(fake_steam.gd). **사람의 Steam 을 켜거나 로그인하지 않는다**
- [x] 1. 짐승 흔적 (발자국 · 똥)
  - 기준: 짐승 스폰 자리(island_animals.gd)를 기준으로 그 둘레에 흔적을 랜덤하게 깐다 — 개수 · 반경 · 다시 생기는 주기는 **임시**
  - 기준: 흔적을 보면(조준점을 대면 — 다른 대상 이름표와 같은 길) **어떤 짐승의 흔적인지** 안다 (「사슴 발자국」 처럼, ko · en 번역 키)
  - 기준: 짐승 종류마다 흔적이 다르게 보인다(모양 · 크기 · 색 — 임시 그림이면 된다). 흔적은 그 섬의 스폰에 서식하는 짐승만 낸다 (테스트)
  - 기준: 멀티에서 서버장 · 참가자가 같은 자리에 같은 흔적을 본다 (테스트)
  - 기준: spec/04_life 의 사냥 문서에 「흔적」 절을 사람 원문과 함께 더한다
- [x] 2. 해체에 시간 (임시 3초)
  - 기준: 사체를 해체하면 **3초(임시, 사람 결정 2026-10-16)** 가 지나야 나온다 — butchery.gd. 그동안 진행이 보이고 · 멀어지거나 손을 떼면(누르고 있는 꼴이면) 끊긴다
  - 기준: 참가자가 해체해도 같은 3초 — 서버가 시간을 지킨다 (테스트)
  - 기준: (앞 단계 QA) 흔적 「참가자가 굴리지 않음」 테스트가 약하다 — test_traces_on_server.gd 의 test_a_guest_does_not_roll_traces_of_its_own · test_traces_laid_anew… 는 앞의 wait_until(_same_as_host) 결과를 단언하지 않는다. 들어올 때 흔적이 안 가도(빈 목록끼리) 두 테스트는 통과했다. 두 테스트의 첫 wait_until 을 assert_true 로 감싸고, before 가 비지 않았는지(assert_false(before.is_empty())) 단언하라
- [x] 3. 나이 — 주민은 이미 있다 · 생포한 짐승만 더한다
  - 이 세션이 2026-10-16 봤다: 주민 나이는 **이미 있다** — Villagers.AGE_MIN~AGE_MAX · villager_window.gd 「24살」 표시 · 멀티 net_world.gd:1006. 주민은 손대지 않는다
  - 기준: **생포한 짐승에게만** 나이를 단다 (풀린 짐승 · 사체에는 없다). 생포할 때 정해지고 · 칸(captive_* 아이템) · 상자 · 축사 · 저장 · 멀티로 따라간다 (테스트)
  - 기준: 생포한 짐승을 보는 곳(칸 설명 · 축사)에 나이가 보인다. 나이 범위 · 나이가 드는 빠르기는 **임시**. 나이로 무엇이 달라지는지는 사람이 안 정했다 — 만들지 않는다
  - 기준: (앞 단계 QA) 해체 막대를 그림으로 본 적이 없다 — 구현 세션이 tools/shot.sh 의 자리(PLACES)에 도축 패널이 없어 막대가 화면에 어떻게 보이는지 찍지 못했다고 했다. 테스트는 보임 · 값만 확인한다. shot.sh 에 주검 옆에서 단추를 누르고 있는 자리를 하나 더해 막대가 단추 아래 제대로 그려지는지 찍어 두어라
- [x] 4. 참가자 화면의 먼 짐승이 뚝뚝 끊긴다
  - 까닭(이 세션이 2026-10-16 찾음): G-227.2 의 먼 것 덜 보내기 — net_world.gd:121 `FAR := 40.0` · :123 `FAR_EVERY := 5` 라 40 m 밖 짐승은 0.5초에 한 번 자리가 간다. 받는 쪽 animal.gd:649 `_follow` 는 받은 자리로 lerp 만 해서, 다다르면 서고 다음 소식에 또 움직인다 → 끊김
  - 기준: 사람 말대로 **거리로 덜 보내기를 하지 않는다** — 짐승 FAR_EVERY 를 걷어 거리와 상관없이 같은 빈도로 보낸다. 받을 사람은 「스폰으로 아예 결정」 — 그 짐승이 사는 섬(스폰)에 있는 사람에게만 보낸다(이미 다른 섬은 안 보내면 그대로). 주민(같은 FAR) · 남의 몸(net_bodies.gd:28 FAR · :30 FAR_EVERY)도 같은 끊김이 나면 같이 걷는다 — Claude 가 정함: 사람이 「끊기는건 너무 이상하게 보임」
  - 기준: 테스트 — 참가자에서 60 m 밖 걷는 짐승의 그림 자리가 매 프레임 움직인다(0.5초 서 있는 구간이 없다). 걷은 뒤 대역폭을 performance.md 멀티 표에 다시 적는다
- [x] 5. 참가자가 들어올 때 서버장 화면에서 엉뚱한 자리에 떴다가 제자리로 옮겨진다
  - 기준: **재현부터** — 서버장 화면에서 참가자 몸이 처음 보이는 자리를 잰다. 서버장 둘레(스폰 · 기본 자리)에 먼저 떴다가 참가자 저장 자리로 옮겨지면 그대로 테스트로 만든다
  - 기준: 참가자 몸은 **제 자리(저장된 자리 · 처음이면 스폰)가 정해진 뒤에야** 서버장 화면에 나타난다 — 첫 프레임부터 그 자리 (테스트)
  - 기준: (앞 단계 QA) performance.md 에 사라진 테스트 설명이 남았다 — spec/00_core/performance.md:103~104 「40m 는 이 재기에서 짐승에만 걸렸다 … 먼 몸은 테스트로만 확인했다 (tests/multiplayer/test_send_less.gd — 먼 것은 1초에 4번 아래, 가까이 오면 0.3초 안에 제자리)」는 G-228.4 에서 지운 테스트를 가리킨다. 지금 테스트(먼 것도 0.1초마다 · 60 m 밖 짐승이 서지 않는다)로 고치거나 지난 기록이라고 밝혀 적어라
- [x] 6. (앞 단계 QA) netcode.md 가 150줄 턱밑이다
  - 기준: netcode.md 가 150줄 턱밑이다 — spec/11_multiplayer/netcode.md 가 148줄이다. 다음에 한 줄만 더해도 넘으니, 표나 지난 기록 몇 줄을 옆 md(예: spec/11_multiplayer/ 아래 새 파일)로 나눠 두어라
  - 기준: (앞 단계 QA) 흔들리는 테스트를 테스트 쪽에서 눌렀다 — test_body_on_server.gd:374 는 기다리는 사이 서버장이 벽에서 떨어지면 다시 붙이고, test_from_empty_hands.gd:1748 은 겨누는 동안 사슴 물리를 끈다. 둘 다 보는 것(소식 · 산출)은 그대로라 받아들였지만, 서버장이 왜 벽에서 떨어지는지(여럿이 돌 때 물리 틱이 몰리면 붙잡기가 풀리는지)는 확인하지 않았다. climbing.gd 의 붙잡기가 한 프레임 긴 delta 에 풀리는지 보는 테스트를 하나 짜 두면 좋다

## [x] G-229 총 — 숨 고르기 · 흔들림으로 보이는 퍼짐 · 1인칭 아이언사이트
- 차선: 3d-start
- **사람 요청 2026-10-16** (원문 그대로)
  - 「총기 탄퍼짐에 대해 좀 더 명확히 할 필요가 있음. 지금 아마도 있는게 움직였다가 멈추고 쏘면 탄퍼짐이 심해지고 점점 괜찮아지는건데 직관적으로 보기 어려움. '숨 고르기'같은 것도 있으면 좋을 것 같음. 움직이고 조준하면 어느정도 흔들리고 있는지에 따라 지금 탄퍼짐이 어느정도다 라는게 인지가 되도록. 요즘 나오는 게임들이 이런식으로 많이 함. 조준을 하고 가만히 몇초동안 있으면 점점 숨이 멎고 그대로 쏴지는거. 탄착이랑 그런건 그대로 있고.」
  - 「아이언 스코프는 1인칭으로 아이언스코프가 보이게 해야지」
- 이 세션이 2026-10-16 봤다: 지금 퍼짐은 attack.gd:391 `spread_degrees` — 총 퍼짐 × 조준 0.5 · 걸음 1.5 · 웅크림 0.6 을 **그 순간에만** 곱한다(멈추면 곧바로 줄어 「점점 괜찮아지는」 것은 없다 — 사람이 느낀 것은 반동 회복일 수 있다). 화면에 퍼짐이 안 보인다
- **탄착은 그대로** — 탄속 · 낙차 · 유효 거리 · 반동 · 서버가 알을 굴리는 길(net_vitals.gd:435 `_shoot`, attack.gd:489)은 바꾸지 않는다
- [x] 1. 숨 고르기 — 조준하고 가만히 있으면 점점 흔들림이 준다
  - 기준: 퍼짐을 「그 순간 곱하기」에서 **흔들림 값**으로 바꾼다 — 움직이면 커지고, 멈추고 조준하면 몇 초에 걸쳐 줄어 가장 작아진다(숨이 멎음). 걸리는 시간 · 가장 작은 값 · 숨을 얼마나 참나는 **임시**
  - 기준: 그 흔들림이 **조준점(총구가 가리키는 곳)이 실제로 흔들리는 것**으로 보인다 — 흔들림이 클수록 크게. 쏜 알은 그 순간 흔들린 쪽 둘레로 간다. 보이는 흔들림과 퍼짐이 어긋나지 않는다 (테스트: 흔들림 0 근처에서 쏘면 조준점 둘레로 모인다)
  - 기준: 참가자도 같다 — 서버가 받는 spread 상한(net_vitals.gd:446)이 새 흔들림 값 최댓값을 막지 않는지 본다 (테스트)
  - 기준: spec/08_combat/tools-as-weapons.md 에 「숨 고르기」를 사람 원문과 함께 적는다
- [x] 2. 조준하면 1인칭 아이언사이트
  - 기준: 총을 들고 우클릭 조준하면 카메라가 **1인칭**으로 넘어가 총의 가늠쇠 · 가늠자가 보인다(지금은 3인칭에서 FOV 만 좁힌다 — aiming.gd:58). 떼면 3인칭으로 돌아온다
  - 기준: 1인칭에서 내 몸 · 머리가 화면을 가리지 않는다. 총 그림이 없는 총은 임시 모양이면 된다
  - 기준: 1단계 흔들림이 1인칭에서 가늠쇠가 흔들리는 것으로 보인다
  - 기준: spec/02_player/movement-controls.md 의 「3인칭」 줄에 「조준만 1인칭 (사람 결정 2026-10-16)」을 더한다
  - 기준: (앞 단계 QA) 숨이 차거나 걸으면 조준점이 한 프레임에 튄다 — attack.gd _breathe 가 BREATH_SETTLE+BREATH_HOLD 에서 _still 을 0 으로 바로 되돌려 breath 가 0.15→1 로 한 번에 뛰고, sway_offset = sway_path × 현재 퍼짐이라 조준점이 한 프레임에 최대 (1-0.15)×0.8×조준 퍼짐(돌격소총 약 0.85°, 걸으면 약 1.5°) 튄다. 흔들림 크기(또는 _still)를 초당 몇 배로 부드럽게 따라가게 하고(예: 숨이 차면 _still 을 0.5초쯤에 걸쳐 내림), test_breath.gd 에 숨이 차는 순간 연속 두 프레임의 player.sway() 차이가 작은지(예: 0.1° 미만) 보는 테스트를 더하라
  - 기준: (앞 단계 QA) 허리 사격 퍼짐은 여전히 화면에 안 보인다 — 조준하지 않을 때는 시선을 흔들지 않아(3인칭 출렁임을 피하려는 판단, spec 에 적혀 있음) 걸으며 허리 사격한 퍼짐(가장 큰 값)이 보이지 않는다. 사람 원문은 「조준하면」이라 이번 기준엔 맞지만, 다음 단계(아이언사이트 · HUD)에서 허리 사격 퍼짐을 조준점 크기 등으로 보일지 spec 「미정」에 한 줄 적어 두어라
- [x] 3. (앞 단계 QA) 1인칭 · 흔들림 크기가 save.md 안 담는다 목록에 없다
  - 기준: 1인칭 · 흔들림 크기가 save.md 안 담는다 목록에 없다 — player.gd _first_person 과 attack.gd _sway_size 는 저장하지 않는데 까닭이 코드 주석에만 있다. spec/01_settings/save.md 「안 담는다」 줄(40행 근처, 「조준 흔들림의 시계(숨 고르기)」 옆)에 「조준 중의 1인칭 · 보이는 흔들림 크기 — 우클릭을 누르는 동안의 것이라 껐다 켜면 3인칭 · 0 에서 시작한다」를 더하라
  - 기준: (앞 단계 QA) 쓰지 않는 물러남 길이 남았다 — player.gd set_camera_fov 의 「좁힌 시야만큼 물러나기」(G-151.2)는 이제 조준이 1인칭이라 부르는 곳이 없고 spec 에도 「쓰는 곳이 없다」고 적혔다. 다시 쓸 계획이 없으면 그 가지와 주석을 걷어내고, 남길 거면 테스트가 그 가지를 지키는지 확인하라 (test_shoulder_camera 의 물러남 테스트는 이번에 지워졌다)

## [x] G-230 제작 창 — 바 대신 타이머 · 검색칸 옆 분류 · 대기열 타이머
- 차선: 3d-start
- 순서: G-229 뒤
- **사람 요청 2026-10-16** (원문 그대로): 「제작하는게 맨 위에 바가 있고 줄어드는게 보이는데 그냥 목록 옆에 타이머만. 그리고 이름으로 찾기를 목록넘지 않게 하고 사진처럼 검색칸 옆에 카테고리 넣고 아래에는 기존 그대로 그리고 오른쪽 대기열도 저렇게 해서 바는 빼고 해당 대기열에 타이머만 보이게」
- 사람이 말한 사진은 아직 못 받았다 — 받으면 ~/Desktop/naru-saves/G-230-2026-10-16/ 에 둔다. 있으면 그 꼴을 따른다
- [x] 1. 제작 창 꼴
  - 기준: 맨 위의 **진행 바와 타이머를 둘 다 없앤다** — 맨 위에는 아무 진행 표시도 없다 · 목록 줄 옆에도 타이머를 달지 않는다
    (사람 고침 2026-10-16: 「맨위에 그냥 바랑 타이머 다 없애게 하고 대기열에 있는게 목재면 목재 옆에 타이머만 나오게」)
  - 기준: 「이름으로 찾기」 칸이 목록 너비를 넘지 않는다. **검색칸 옆에 분류(카테고리)** 를 놓는다. 그 아래 목록은 지금 그대로
  - 기준: 남은 시간은 **오른쪽 대기열에만** — 대기열 줄의 이름 옆에 타이머만 보인다(대기열에 목재가 있으면 「목재」 옆에 타이머). 바는 뺀다 (G-225 의 「용광로 − 칸 + ×」 줄은 그대로)
  - 기준: 1280×720 · 1920×1080 · 한국어 · 영어 화면 찍기(ui_shots)로 넘치거나 겹치는 곳이 없다

## [x] G-912 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-lane2
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. 서버장이 벽에서 떨어지는 까닭은 아직 모른다 (G-228 단계 6)
  - 기준: 서버장이 벽에서 떨어지는 까닭은 아직 모른다 — test_grip_long_frame.gd 로 climbing.gd 의 붙잡기(긴 delta · 몰린 틱)는 아님이 드러났다. test_body_on_server.gd:374 근처의 「다시 붙이기」를 잠시 빼고 떨어질 때 is_climbing 이 false 가 되는 자리(release 를 부르는 곳 — 몸 소식 받기 · 다른 몸과의 부딪힘 · restore_hold 덮어쓰기)에 로그를 달아 찾고, 찾은 원인을 고친 뒤 테스트 쪽 누름을 걷어라
- [x] 2. (앞 단계 QA) body_island.gd 머리 주석이 낡았다
  - 기준: body_island.gd 머리 주석이 낡았다 — game/tests/multiplayer/body_island.gd:11 의 「절벽 — 지형 층(Climbing.TERRAIN_LAYER)의 곧게 선 벽」이 이제 층 1|지형 층이라 틀린다. 「몸이 막히는 층 1 과 지형 층(1 | Climbing.TERRAIN_LAYER)의 곧게 선 벽」으로 고쳐라
- [x] 1. 새 테스트가 옆 테스트의 머리 주석을 가로챘다 (G-912 단계 2)
  - 기준: 새 테스트가 옆 테스트의 머리 주석을 가로챘다 — game/tests/multiplayer/test_islands_split.gd 에서 test_guest_drops_bodies_of_the_island_left_behind 를 test_each_strike_stays_on_its_own_island 의 머리 주석(「## 각자 제 섬에서 친 것은 제 섬에만 남는다 …」 두 줄)과 그 func 사이에 끼워 넣었다. 그래서 그 두 줄이 이제 새 테스트 위에 붙어 엉뚱한 설명이 되었고, test_each_strike 는 주석을 잃었으며, 새 테스트가 「# ── 바람은 바란 사람이 선 섬에 ──」 칸 아래에 들어갔다. 새 테스트(와 그 주석 세 줄)를 test_split_two_do_not_receive_each_others_bodies_or_beasts 바로 뒤, 「바람은 바란 사람이 선 섬에」 칸 머리 앞으로 옮기고, 「각자 제 섬에서 친 것은…」 두 줄은 test_each_strike_stays_on_its_own_island 바로 위에 남겨라

## [x] G-231 장비 모듈 — 무기 · 장비 제작대 · 모듈 탭 · 세트 모듈 · 스코프 배율
- 차선: 3d-start
- 순서: G-230 뒤 (attack.gd · aiming.gd · iron_sight.gd 를 G-229 와 같이 만진다)
- spec: spec/02_player/equipment.md, spec/08_combat/tools-as-weapons.md → 새 spec/02_player/equipment-modules.md
- **사람 설계 2026-10-17** (원문 그대로)
  - 「모든 장비나 무기에는 '파츠' ? 혹은 모듈 ? 개념을 붙이면 좋을 것 같다.」
  - 「방어구 같은 경우는 기본적인 틀은 어떤 광석으로 만드느냐에 따라 방어력+체력 같은 기본적인 스텟이랑 한칸은 세트효과용 모듈칸 하나 그리고 방어구에 붙일 수 잇는 모듈칸은 방어구를 어떤 광석으로 만드느냐에 따라 늘어나는 느낌. 예를 들어, 철주괴로 만든 방어구: 체력 +50 방어력 +30 / 세트 모듈: (사슴 세트) 2세트: 체력+10, 3세트: 이동속도 +20 / 모듈1: (북극곰의 냉기) 방한 +30 / 모듈2: (낙타의 온기) 방열 +30」
  - 「무기 같은 경우는 기본 총기스타일을 구성하는것만 가져오고 스코프 1, 모듈 1 이렇게 붙이면 될 듯함. 여기서 모듈은 딥락갤의 오버클록 모티브로 하고 싶음. 패시브 모듈 or 액티브 모듈로 나뉜다. 패시브 모듈 같은 경우는 단순 스텟을 늘리고 액티브 모듈은 총기의 작동 방식이 달라질 수 있다.」 · 「스코프: (2m 스코프) 2m 줌 탄퍼짐 -10」
  - 「액티브모듈에 대해선 나중에 추가하는걸로 하고 패시브 모듈로만 만들어 놓을거야. 일단 자리는 액티브 모듈 자리 하나 둘거고.」
  - 「액티브모듈은 넣는 칸만 하나 만들어놔. 무기 제작대랑 장비 제작대로 따로 장비들은 빼주고 해당 제작대에 뭐라고 하지 크롬의 탭같이 한 페이지에는 제작하는거 두번째 페이지에는 모듈 넣는걸로 해서 만들어줘. 제작하는건 기존꺼대로 하면 되고 모듈 넣는건 내 인벤에서 드래그해서 넣고 거기에 모듈도 인벤토리에서 넣을 수 잇게 하면 될 것 같아.」 (2026-10-17)
  - 「모듈을 뺄땐 돌려 받는거지」 · 스코프는 **배율**(2배 · 3배 · 6배)이다 — 「그럼 배율이 맞아」 · 「배율 좋은거 같고」
- 이 세션이 2026-10-17 봤다
  - 장비칸은 `{item, count}` 딕셔너리다. **equipment.gd:54 가 `SlotMove.plain(entry)` 를 요구한다** — item · count 말고 다른 열쇠가 있으면 장비칸에 안 들어간다(slot_move.gd:71). 모듈을 안은 방어구가 끼워지게 고쳐야 한다. 잡은 짐승(captives.gd)이 이미 칸에 제 상태를 안는 선례다 — 겹쳐 쌓이지 않는다
  - 방어는 armor.gd:24 `DEFENSE`(가죽 5 · 청동 8 · 철 11 · 강철 14, %) · :31 `MAX_DEFENSE` 80 — **이 계산은 이번에 바꾸지 않는다**. 최대 체력은 health.gd:15 `DEFAULT_MAX` 100 · :17 `max_health`
  - 제작대 레시피는 game/craft/recipes/workbench.json 하나에 있고 `category` 로 갈린다 — weapon(활 · 화살 · 근접 무기 16 · 총 16 · 총알 4) · armor(상의 · 모자 · 하의 · 방패 × 가죽 · 청동 · 철 · 강철 = 16). 시설 목록은 game/craft/stations/stations.json · 창은 StationWindow(tests/ui/hud/test_station_window.gd)
  - 조준 시야는 aiming.gd:60 `camera_fov / ZOOM` — 배율을 여기에 얹는다
- **이번에 만들지 않는 것**: 액티브 모듈의 작동(록온 등 — 칸만 둔다) · 방한 · 방열 모듈(온도가 없다 — 날씨와 같이, weather-season.md) · 장신구 세트 · 모듈의 레시피 · 얻는 길(사람이 정한다). 모듈은 **개발용 상자(dev_box.gd)에만** 넣는다
- [x] 1. 무기 제작대 · 장비 제작대 — 레시피를 옮기고 창에 탭 둘
  - 기준: 시설 둘을 더한다 — **무기 제작대**(weapon_bench) · **장비 제작대**(armor_bench). 제작대(workbench)에서 만들고 놓는다(재료 · 크기 **임시**). stations.json · 번역 키 ko · en
  - 기준: 제작대에서 빼서 옮긴다 — 무기 제작대: **총 16 · 근접 무기 16 · 활** / 장비 제작대: **상의 · 모자 · 하의 · 방패 16 · 배낭 · 스파이크**(장비칸에 끼우는 것 — 「왜 배낭이랑 스파이크도 넣어야지」 사람 2026-10-17). 제작대 목록에서는 사라진다 (테스트). **총알 넷 · 화살은 제작대에 남긴다** — 「총알은 제작대에서 만든다」(사람 결정 2026-09-22)
  - 기준: 두 시설의 창 위에 **크롬 탭 같은 탭 둘** — 「제작」 · 「모듈」. 「제작」은 지금 제작 창 그대로(G-230 꼴 · 대기열 · 버퍼). 다른 시설 창에는 탭이 없다
  - 기준: 「모듈」 탭은 이 단계에서 빈 자리만 — 다음 단계가 채운다. 탭을 바꿔도 대기열 · 타이머가 그대로다 (테스트)
  - 기준: spec/05_craft/facilities.md 표의 제작대 줄에서 무기 · 갑옷을 빼고 두 시설을 더한다(「시설 열둘」 머리에 「+ 무기 · 장비 제작대 (사람 결정 2026-10-17)」). 1280×720 · 1920×1080 · ko · en ui_shots
- [x] 2. 방어구에 모듈 칸 — 끼우고 빼면 돌려받는다 · 광석이 체력도 준다
  - 기준: 상의 · 모자 · 하의(armor.gd PIECES 의 방패 뺀 셋)마다 **세트 모듈 칸 1** + **일반 모듈 칸**이 있고, 일반 칸 수는 금속으로 늘어난다 — 가죽 0 · 청동 1 · 철 1 · 강철 2 (**임시**). 칸 수는 한 곳의 표다
  - 기준: 모듈은 아이템이다. 칸에 끼우면 가방에서 빠지고, **빼면 가방으로 돌아온다**(사람 결정 2026-10-17). 가방이 차 있으면 빠지지 않는다 — 잃지 않게 (테스트)
  - 기준: 모듈을 안은 방어구도 장비칸에 끼워지고 벗겨진다 · 겹쳐 쌓이지 않는다 · 상자 · 데스 상자 · 저장 · 멀티(참가자)로 모듈이 그대로 따라간다 (테스트 — test_save_roundtrip 류 왕복 검사에 넣는다)
  - 기준: 배낭 · 스파이크는 장비 제작대에서 만들지만 **모듈 칸이 없다** — 「모듈」 탭 자리에 안 놓인다 (테스트)
  - 기준: 모듈은 **장비 제작대의 「모듈」 탭에서만** 바꾼다 — 탭에 장비 자리 하나가 있고, **내 인벤토리에서 방어구를 끌어 놓으면** 그 방어구의 모듈 칸이 펼쳐진다. 모듈도 **인벤토리에서 끌어** 칸에 끼우고, 칸에서 인벤토리로 끌면 빠진다. 방어구를 인벤토리로 끌어 오면 모듈을 안은 채 돌아온다 (사람 결정 2026-10-17)
  - 기준: 창을 닫거나 멀어지면 자리에 둔 방어구는 인벤토리로 돌아온다 — 잃지 않는다(인벤토리가 차 있으면 닫기 전에 알린다든지 땅에 떨군다든지, 지금 있는 넘침 규칙을 따른다) · 여럿이 같은 시설을 열어도 남의 자리를 건드리지 않는다 (테스트)
  - 기준: 1280×720 · 1920×1080 · ko · en ui_shots 로 넘치거나 겹치지 않는다
  - 기준: 방어구가 **체력도 준다** — 입은 벌의 체력을 더해 최대 체력(health.gd max_health)이 는다. 가죽 20 · 청동 35 · 철 50 · 강철 65 (**철 50 만 사람 예시, 나머지 임시**). 벗으면 줄고, 남은 체력이 새 최대를 넘으면 깎는다. HUD 체력 막대가 따라간다 · 참가자도 같다 (테스트)
  - 기준: 시험용 일반 모듈 하나 — 「체력 모듈」(체력 +10, **임시** — 사람이 모듈 목록을 줄 때까지 칸이 도는지 보는 시험용). 개발용 상자에 있다
- [x] 3. 세트 모듈 — 같은 세트를 n 부위에 끼우면 효과
  - 기준: 세트 모듈은 **세트 칸에만** 들어가고 일반 칸에는 안 들어간다(거꾸로도) (테스트)
  - 기준: 입은 방어구의 세트 칸에 **같은 세트 모듈이 몇 개냐**로 효과가 붙는다 — 금속은 상관없다(철 상의 + 가죽 모자도 사슴 둘이면 2세트). 2세트 효과는 3세트에서도 남는다 (테스트)
  - 기준: 「사슴 세트」 — 2세트 체력 +10 · 3세트 이동속도 +20% (사람 예시 2026-10-17 · **%는 임시** — 사람은 「+20」만 적었다). 이동속도는 배고픔 · 목마름 · 비탈 배율(hunger-thirst.md · terrain.md)에 곱한다
  - 기준: 가방 창(입은 것)과 장비 제작대 「모듈」 탭에 지금 붙은 세트 효과가 보인다(「사슴 2/3 — 체력 +10」처럼) · 번역 키 ko · en
  - 기준: spec/02_player/equipment.md 「세트 효과 — 부위를 모으면」 줄을 「세트 칸에 같은 세트 모듈을 n 부위」로 고치고 새 파일로 잇는다
  - 기준: (앞 단계 QA) 자리 따로 테스트가 늘 참이다 — test_armor_modules.gd::test_each_person_has_their_own_spot 은 Kit 객체 둘을 새로 만들어 a.bench 에 넣고 b.bench 가 비었는지만 본다(서로 다른 객체라 늘 참). 실제 보장은 test_modules_on_server.gd 가 하니 이 테스트는 지우거나, Acts 하나에 두 사람(who=1 · who=2 kit)을 두고 do_mod_hold(2, …) 뒤 kit_of(1).bench 가 그대로인지 보도록 바꿔라
  - 기준: (앞 단계 QA) 시설이 부서져 닫힐 때의 테스트가 없다 — test_module_page.gd 는 stations.close() 만 부른다. 기준은 통과(Stations.hit_station 도 같은 close()→open_changed 길)지만, 자리에 방어구를 둔 채 그 장비 제작대를 부숴(stations.hit_station 을 HITS 번) 창이 닫히고 방어구가 모듈을 안은 채 가방으로 돌아오는지 보는 테스트를 test_module_page.gd 에 더하라. 죽을 때(창이 열린 채 맞아 죽음) 자리의 방어구가 어디로 가는지도 같이 보면 좋다
- [x] 4. 총에 칸 — 스코프 1 · 패시브 모듈 1 · 액티브 모듈 1(자리만)
  - 기준: 모든 총(tools-as-weapons.md 표의 열여섯)에 **스코프 칸 1 · 패시브 모듈 칸 1 · 액티브 모듈 칸 1** — 2단계와 같은 끼우기 · 빼기 · 돌려받기 · 저장 · 멀티 · 상자 길을 쓴다(따로 짜지 않는다). 총에 든 남은 탄과 같이 따라간다 (테스트)
  - 기준: **액티브 칸은 넣는 칸만 하나 있다** — 「액티브모듈은 넣는 칸만 하나 만들어놔」(사람 결정 2026-10-17). 끼울 액티브 모듈은 아직 없다. 패시브 · 스코프는 그 칸에 안 들어간다 (테스트)
  - 기준: 시험용 패시브 모듈 하나 — 「반동 완화」(반동 −20%, **임시** — 사람이 목록을 줄 때까지 시험용). 개발용 상자에 있다
  - 기준: 칸은 **무기 제작대의 「모듈」 탭**에서 바꾼다 — 2단계와 같은 탭 · 같은 끌어 놓기(따로 짜지 않는다). 근접 무기 · 활은 칸이 없어 자리에 안 놓인다
  - 기준: (앞 단계 QA) 죽음 테스트가 진짜 죽음을 안 탄다 — test_module_page.gd::test_dying_with_the_window_open_keeps_the_armor_out_of_the_death_chest 는 game.inventory.clear() 로 Death._drop_chest 를 흉내 내고 stations.close() 를 직접 부른다. 진짜로 죽을 때 창이 닫히는지 · 닫히는 차례(가방 비우기 전/후)에 따라 자리의 방어구가 데스 상자로 가는지 가방으로 가는지는 확인되지 않는다. 창이 열린 채 player.health().take(큰 값) 으로 실제 Death 를 태워 (1) 창이 닫히고 (2) 데스 상자 안에 steel_helmet 이 있는지/없는지 · 다시 선 뒤 가방에 있는지를 단언하는 테스트로 바꿔라. 바라는 쪽(데스 상자로 가느냐 곁에 남느냐)이 spec 에 없으면 equipment-modules.md 에 지금 동작을 적어 둔다
- [x] 5. 스코프 — 배율로 줌 · 배율마다 쓰임이 다르다
  - 기준: 스코프 셋 — **2배 · 3배 · 6배**(사람 결정 2026-10-17 — 「2배 3배 6배 이런식으로」). 스코프를 낀 총으로 조준하면 1인칭에서 가늠쇠 대신 **스코프 조준선**이 보이고, 시야각은 `tan(새 FOV/2) = tan(기본 FOV/2) / 배율` 로 좁힌다(aiming.gd:60 의 ZOOM 자리). 안 끼면 지금 아이언사이트 그대로
  - 기준: **높은 배율이 무조건 낫지 않다** — 저배율은 퍼짐을 더 줄이고 조준에 드는 시간이 짧다, 고배율은 멀리 보이지만 퍼짐 보정이 작고 조준에 드는 시간이 길다. 2배 스코프 퍼짐 −10%(사람 예시), 3배 · 6배 값과 조준 시간은 **임시**. 흔들림(숨 고르기)은 그대로 — 좁은 시야라 크게 보이는 것이 맞다
  - 기준: 테스트 — 2 · 3 · 6배를 낀 채 조준하면 시야각이 위 식대로다 · 스코프를 빼면 아이언사이트로 돌아온다 · 서버가 받는 퍼짐 상한(net_vitals.gd:446 근처)이 가장 큰 값을 막지 않는다
  - 기준: 새 파일 spec/02_player/equipment-modules.md 에 이 묶음의 규칙(칸 · 돌려받기 · 세트 · 총 칸 · 배율 · 시험용 모듈은 임시 · 만들지 않은 것)을 사람 원문과 함께 적는다. spec/README.md 목차 · tools-as-weapons.md 미정의 「총에 파츠를 붙이고 강화한다 — 나중에」를 이 파일로 잇는다(강화는 그대로 미정). 150줄 안
  - 기준: (앞 단계 QA) 서버 문이 시설 갈래를 안 본다 — core/net/acts.gd:398 의 MOD_HOLD 검사는 ArmorModules.has_slots 만 봐서, 참가자가 문으로 직접 바라면 장비 제작대 창에서도 총을(무기 제작대에서 방어구를) 자리에 올릴 수 있다. 막는 것은 UI(ModulePage.ArmorSpot._can_drop_data 의 for_bench)뿐이다. 칸 규칙(fits)은 그대로 지켜져 해는 작지만, Kit 에 지금 연 시설의 modules 갈래를 두고 MOD_HOLD 에서 ArmorModules.for_bench(item, what) 로 거절하게 하고, Acts 로 MOD_HOLD 를 장비 제작대 갈래에서 revolver 로 바라 false 인지 보는 테스트를 더하라
  - 기준: (앞 단계 QA) ArmorModules 이름이 낡았다 — 총의 칸도 이제 armor_modules.gd 의 ArmorModules 가 맡는데 이름 · fits(armor_item, …) 인자 이름이 방어구만 말한다. 이름을 EquipModules 등으로 바꾸거나 최소한 fits 의 armor_item 인자를 item 으로 고치고 클래스 머리 주석 첫 줄에 「방어구 · 총」을 적어라
- [x] 6. (앞 단계 QA) 창을 닫아도 bench_what 이 남는다
  - 기준: 창을 닫아도 bench_what 이 남는다 — ModulePage._tell_gate 가 kind == "" 이면 문에 알리지 않아, 시설 창을 닫은 뒤에도 Kit.bench_what 이 armor/weapon 그대로다. 그래서 창이 닫힌 채 문으로 MOD_HOLD 를 바라면 자리에 놓인다(spec 「안 열었으면 아무것도 안 놓인다」와 어긋남). 창이 닫힐 때(station_window 닫기 · GameRoot._put_bench_back 근처) Acts.MOD_BENCH [""] 를 보내거나 do_mod_return 에서 kit.bench_what = "" 로 비우고, 닫은 뒤 MOD_HOLD 가 false 인지 보는 테스트를 더하라
  - 기준: (앞 단계 QA) ui_shots.gd 가 Aiming._process 를 직접 부른다 — _shoulder 의 aiming._process(2.0) 은 밑줄 붙은 내부 함수를 찍기 코드가 부른다. Aiming 에 finish_now() 같은 공개 함수(조준 시간을 다 흘린다)를 두고 ui_shots.gd · test_scope.gd 가 그것을 부르게 하라

## [x] G-914 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-start
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. 개발용 상자에 체력 모듈이 드는지 보는 테스트가 없다 (G-231 단계 2)
  - 기준: 개발용 상자에 체력 모듈이 드는지 보는 테스트가 없다 — dev_box 테스트(tests/core/debug/ 쪽 contents 검사)에 DevBox.contents() 에 health_module 이 있는지 단언 한 줄을 더하라

## [x] G-232 주민 한 장 — 스텟과 직업 레벨을 숙련도 하나로 합친다
- 차선: 3d-start
- 순서: G-231 뒤 (i18n · 주민 창을 같이 만진다)
- spec: spec/07_automation/villager-persona.md, npc-villagers.md
- **사람 결정 2026-10-17** — 주민 스탯 목록 「숙련도: 채광 벌목 낚시 사냥 요리 등반 제작 의학 예술 연구 조련 사교 / 체력 / 배고픔 / 목마름 / 버프 / 방어력 공격력 / 나이 / 스트레스」에서
  - 「등반은 빼고 농사 넣는게 좋은거 같아. 농사 건축 넣어야 돼. 수면시간이 있는거랑 특성도 그대로 넣고」
  - 스텟 5개(힘 · 민첩 · 체력 · 지식 · 손재주)는 숙련도와 겹친다 → 「그게 맞다」(숙련도 하나로) · 「전체적으로 합칠것들은 합칠 수 있도록 해서 너가 만들어줄래 ? 5개에 국한되지 말고 단어를 변경해도 되고 추가해도 돼」
- **이 세션이 짠 한 장** (사람이 맡겼다 2026-10-17)
  - **그 사람**: 이름 · 나이 · 특성(긍정 하나 · 부정 하나)
  - **상태** (오르내린다): 체력(HP) · 배고픔 · 목마름 · 수면 · 스트레스 · 버프
  - **숙련도 13**: 농사 · 벌목 · 채광 · 제작 · 요리 · 의학 · 연구 · 건축 · 사냥 · 낚시 · 조련 · 예술 · 사교 — 직업 레벨과 스텟이 하던 일을 하나가 맡는다(하면 오르고 → 그 일이 빨라진다)
  - **공격력 · 방어력은 숙련도가 아니다** — 입은 것 · 든 것에서 나오는 계산값이다(장비 모듈 G-231 과 같은 길, 「무기 격차를 작게」). 주민이 무기 · 방어구를 드는 것은 습격이 들어올 때다 — 이번에 만들지 않는다
  - 스텟이 하던 일이 가는 곳: 힘·지식·손재주 → 그 숙련도 · 이동 속도 → 「날렵함」 특성(+ 배고픔 · 목마름) · HP → 「튼튼함」 특성 · **회피 · 낙하 피해 감소는 없앤다**(사람: 코어키퍼의 회피 90% 가 밸런스를 무너뜨렸다 · 주민은 절벽을 안 탄다)
  - **청소부 → 관리인으로 합친다** — 청소 · 정리 · 수리가 한 직업이다(지금은 축사 똥 치우기만, 정리 · 수리는 그것이 생길 때)
- 이 세션이 2026-10-17 봤다: 스텟은 persona.gd:13 `STATS` · :92 `flat_stats` · :100 `roll_stats` · villager.gd 저장(:456 to_dict · :641 맞추기 목록) · villager_window.gd:28 `STATS_KEY` · :203 `_build_stats` — **값과 저장뿐이고 쓰는 곳이 없다**. 직업 목록은 jobs.gd:31 `LIST` 한 곳 · 일 빠르기는 villager.gd:447 `pace()` 한 곳
- [x] 1. 스텟 다섯을 숙련도 열셋으로
  - 기준: persona.gd 의 STATS 를 **SKILLS 열셋**으로 바꾼다(위 「숙련도 13」 차례 · 번역 키 VILLAGER_SKILL_* ko · en). 레벨 0 ~ 100 · 들어올 때 하나하나 0 ~ 10 무작위(**임시**)
  - 기준: 옛 저장본의 stats 는 **버리고** 숙련도는 새로 굴린다 — 불러오다 깨지지 않는다 (테스트: stats 를 안은 옛 주민 저장본을 읽는다)
  - 기준: 주민 창에 스텟 칸 대신 숙련도 열셋이 보인다 · 참가자 화면에도 같다(맞추기 목록) · 1280×720 · 1920×1080 · ko · en ui_shots 로 넘치지 않는다
- [x] 2. 숙련도가 오르고 일이 빨라진다
  - 기준: jobs.gd LIST 의 줄마다 **그 직업이 올리는 숙련도**를 적는다(농부→농사 · 나무꾼→벌목 · 광부→채광 · 관리인→없음). 제작자는 붙은 시설로 가른다 — 조리대 · 훈연기 · 양조통 · 증류기→요리 · 약제 작업대→의학 · 연구대→연구 · 그 밖→제작. 이 표는 한 곳이다
  - 기준: 그 일을 한 번 마칠 때마다(한 그루 · 한 덩이 · 한 번 물 주기 · 하나 만들기) 그 숙련도에 경험이 붙고 차면 레벨이 오른다. 오르는 빠르기는 **임시**. 저장 · 멀티로 따라간다 (테스트)
  - 기준: **5레벨마다 그 일이 5% 빨라진다**(「5레벨마다 +n%」 사람 결정 2026-09-28 · 5% 는 **임시**) — villager.gd pace() 가 맡은 직업의 숙련도를 곱한다(스트레스 느려짐과 같이). 레벨 0 과 50 의 같은 일 시간이 다르다 (테스트)
  - 기준: 「천부적인 재능」(persona.gd TRAITS gifted, 「그 직업 경험치 증가」) — 들어올 때 숙련도 하나가 정해지고 그 숙련도 경험이 1.5배(**임시**). 주민 창에 어느 숙련도인지 보인다
  - 기준: 레벨로 **새로 열리는 일**(직업 레벨이 하던 「쓸 수 있는 것이 늘어난다」)은 이번에 만들지 않는다 — 열 것이 아직 없다
  - 기준: (앞 단계 QA) 주민 창 · 입력 주석에 「스텟」이 남았다 — game/ui/hud/villager_window.gd:6 「스텟 다섯(레벨)」 → 「숙련도 열셋(레벨)」, game/core/input/input_actions.gd:47 「스텟 · 특성」 → 「숙련도 · 특성」, game/core/debug/ui_shots.gd:119 「스텟 · 특성」 → 「숙련도 · 특성」, game/tests/ui/hud/test_villager_window.gd:2 「스텟 다섯」 · :50 「스텟 · 특성」 → 숙련도로 고쳐라 (막는 것은 아니다)
  - 기준: (앞 단계 QA) spec 의 「스텟」 줄이 낡았다 — spec/01_settings/input.md:22 V 줄의 「스텟」 → 「숙련도」, spec/07_automation/villager-persona.md:1 제목 「… 특성 · 스텟」 → 「… 특성 · 숙련도」. spec/02_player/level-skills.md:22 「주민은 힘 스텟이 채광 속도를 정한다 (사람 결정 2026-09-28)」와 villager-persona.md:26~40 의 스텟 절은 사람 결정 줄이라 지우지 말고, 옆에 「→ 2026-10-17 숙련도로 합쳤다(채광 숙련도)」 한 줄을 붙여 낡은 것임을 표시하라
- [x] 3. 관리인 하나로 · 신체 특성 둘 · 체력
  - 기준: 청소부(jobs.gd JANITOR, id `janitor` 그대로 — 저장본이 깨지지 않는다)의 이름을 **관리인**으로 바꾼다. 하는 일은 지금 그대로(축사 똥)
  - 기준: 긍정 특성에 **튼튼함**(최대 체력 +20%) · **날렵함**(걸음 +15%) 을 더한다(크기 **임시**). 날렵함은 pace 가 아니라 걸음에만 듣는다 (테스트)
  - 기준: 주민에게 **체력(HP)** — 최대 100(**임시**) · 저장 · 주민 창에 보인다. 아직 다치는 길은 없다(습격 · 다침이 생길 때). 튼튼함이 최대를 올린다 (테스트)
  - 기준: (앞 단계 QA) villager-persona.md 목록 들여쓰기가 깨졌다 — spec/07_automation/villager-persona.md:14 에 넣은 「맡은 일의 숙련도가 빠르게 하는 것도 같은 곳에서 곱한다」가 두 칸 들여쓰기라 「지금 게임:」 하위 항목(네 칸) 사이에 끼어 그 아래 「얼마나 느린가(0.5 배)는 임시」 줄이 엉뚱한 줄에 붙는다. 그 줄을 네 칸으로 들여 앞 줄(:13) 뒤에 잇거나 :15 다음으로 옮겨라
  - 기준: (앞 단계 QA) 재능 정하기를 글자로만 본다 — test_villager_skills test_a_newcomer_with_the_gift_gets_one_skill 은 from_dict 길만 실제로 돌리고 Villagers.settle 은 파일에 「Persona.roll_gift」 글자가 있는지만 본다. Villagers 를 하나 만들어 settle 을 여러 번 불러 gifted 특성이 있는 이는 Persona.SKILLS 안의 gift, 없는 이는 &"" 인지 단언하는 테스트로 바꿔라 (막는 것은 아니다 — 지금 코드는 맞다)
- [x] 4. spec 을 한 장으로
  - 기준: villager-persona.md 「직업 레벨 · 스텟」 절을 위 「이 세션이 짠 한 장」으로 고쳐 쓴다 — 사람 원문 · 날짜를 붙이고, 스텟 다섯이 숙련도로 합쳐졌다는 것과 회피 · 낙하 감소를 없앤 까닭을 한 줄씩. 150줄 안
  - 기준: npc-villagers.md 직업 표 — 청소부 · 관리인 두 줄을 「관리인(청소 · 정리 · 수리)」 한 줄로 · 「비료는 아직 제작대에서」 줄이 지금(약제 작업대에도 있다)과 맞는지 고친다
  - 기준: (앞 단계 QA) reach 테스트의 걸음 표시가 틀린 배를 찍는다 — test_villager_reach.gd:469 _plant_from_outside_the_wall 의 stride 를 give_job(:479) 전에 재서 pace() 에 농사 숙련도가 안 들어가, stride_1_05 · _1_10 테스트의 실패 글에 「걸음 1.00 배」가 찍힌다(QA 가 깨 보니 1.05 판이 1.00 으로 보였다). stride 계산을 give_job 뒤로 옮기고, 판마다 기대 배(1.05 · 1.10 · 1.15)와 walk_speed()/WALK_SPEED 가 같은지 assert_almost_eq 로 단언하라 — 지금은 숙련도 → 걸음 배가 바뀌어도 그 테스트가 원하는 걸음으로 걷는지 아무도 안 본다
  - 기준: (앞 단계 QA) PathWalk.BACK_ON 에 「임시」 표시가 없다 — report temporary 에는 임시라고 했지만 game/core/path_walk.gd 의 const BACK_ON := 0.05 주석에 **임시** 가 없다. 주석 끝에 「**임시**.」를 붙여라
- [x] 5. (앞 단계 QA) 비료 레시피 둘이 하나처럼 읽힌다
  - 기준: 비료 레시피 둘이 하나처럼 읽힌다 — spec/07_automation/npc-villagers.md 농부 줄의 「(apothecary_bench — 석회 2 · 똥 2)」는 한 레시피에 석회 2와 똥 2가 함께 드는 것처럼 읽힌다. 실제로는 apothecary_bench.json:6-7 의 레시피 둘(fertilizer = 석회 2 / fertilizer_from_manure = 똥 2)이다. 「석회 2 또는 똥 2」로 고쳐라

## [x] G-233 주민의 상태가 움직인다 — 수면 · 스트레스 · 배고픔 · 목마름
- 차선: 3d-start
- 순서: G-232 뒤
- spec: spec/07_automation/villager-persona.md 「수면 · 스트레스」
- **사람 결정 2026-09-28** (villager-persona.md): 「수면: 최대 100. 깨어 있으면 내려간다. 30부터 스트레스가 오른다. 0 이면 스트레스가 대폭 오른다」 · 「스트레스: 50 과 80 에 경고. 경고는 사라지지 않고 오른쪽 위에 로그로 계속 뜬다」 · 「70 부터는 느리게 일한다」(이미 있다 — villager.gd:447)
- **사람 결정 2026-10-17**: 주민 상태에 배고픔 · 목마름 (G-232 머리)
- 이 세션이 2026-10-17 봤다: 수면 · 스트레스는 값과 저장뿐이다(villager.gd:109 · :110). eat · drink · take_medicine 은 함수로만 듣고(villager.gd:403 · :425) 주민이 스스로 먹는 길이 없다 — 「주민이 언제 먹나」가 npc-villagers.md 미정이다
- **이번에 만들지 않는 것**: 스트레스 100 의 부정 특성 발동(부수기 · 불피우기 · 가출 · 폭식이 무엇을 하는지 정해지지 않았다) · 음식 버프가 주민에게 듣는 것
- [x] 1. 수면 — 깨어 있으면 줄고 밤에 침대에서 잔다
  - 기준: 깨어 있으면 수면이 준다(빠르기 **임시** — 하루 40분 가운데 깨어 있는 동안 100 → 30 남짓). **밤이 되면** 일을 멈추고 거주지 방의 제 침대로 가서 잔다 — 자는 동안 수면이 오른다 · 아침에 일어나 일로 돌아간다. 침대가 없으면 선 자리에서 잔다(덜 오른다, **임시**)
  - 기준: 수면 30 아래에서 스트레스가 오르고, 0 이면 크게 오른다(사람 결정 2026-09-28 · 크기 **임시**) (테스트)
  - 기준: 저장 · 멀티(참가자 화면의 자는 주민) · 주민 창의 수면 값이 따라간다 (테스트)
- [x] 2. 스트레스 경고
  - 기준: 스트레스가 50 · 80 을 넘으면 **오른쪽 위 로그에 경고**가 뜨고 **사라지지 않는다**(사람 결정 2026-09-28) — 「<이름> 스트레스 50」. 같은 경고를 같은 주민에게 거듭 내지 않는다(내려갔다 다시 넘으면 다시). 번역 키 ko · en (테스트)
  - 기준: 술 · 음식 · 약의 스트레스 내림(G-205.1)이 이제 실제로 쓰인다 — 아래 3단계에서 먹으면 내려간다
  - 기준: (앞 단계 QA) BED_NEAR 주석과 보고의 거리가 어긋난다 — villager.gd 의 BED_NEAR 는 1.6(침대 가운데에서 수평)인데 report 의 임시 목록은 「침대 곁 닿는 거리 — 0.5m 남짓」이라 적었다. 주석 「0.5m 남짓 깎인 띠 끝」이 무엇을 재는지(가운데에서 1.6 = 반폭 + 깎인 띠 + 몸) 한 줄로 풀어 적어 두어라 (막는 것은 아니다)
- [x] 3. 배고픔 · 목마름 — 방 상자에서 꺼내 먹는다
  - 기준: 주민에게 배고픔 · 목마름(최대 100). 시간이 지나면 준다(플레이어와 같은 빠르기, hunger-thirst.md · **임시**). 저장 · 주민 창 · 멀티 (테스트)
  - 기준: **언제 먹나 (이 세션이 정함 · 임시)**: 30 아래로 내려가면 하던 일을 마치고 거주지 방 상자로 가서 먹을 것 · 마실 것(음식 · 물 · 술)을 하나 꺼내 Villager.eat · drink 로 먹는다 — 플레이어가 먹는 것과 같은 표(foods.json · drinks)를 쓴다. 상자에 없으면 그냥 일로 돌아간다
  - 기준: 0 이 되면 스트레스가 오른다(**임시** — 굶어 죽지는 않는다). 대식가 · 소식가(persona.gd TRAITS)가 줄어드는 빠르기를 바꾼다(2배 · 절반, **임시**) (테스트)
  - 기준: villager-persona.md · npc-villagers.md 미정의 「주민이 언제 먹나」를 위 규칙으로 옮겨 적는다(「이 세션이 정함 · 임시」 표시)
  - 기준: (앞 단계 QA) 두 농부 물주기 테스트가 흔들린다 — 앞 시도에서 test_villager_watering.gd 의 test_two_farmers_in_one_room_never_hold_the_same_field_and_nothing_doubles_or_vanishes 가 394·398·400 줄에서 떨어졌다가 이번엔 통과했다. 구현 세션은 「시간 흔들림」으로 보고 넘겼다. 거두기가 없는 판이라 씨앗 굴림(G-236.1)과 무관하다 — 기다리는 끝이 빠듯한지, 두 농부가 같은 밭을 두고 엇갈려 시간을 버리는지 찾아 원인을 고치고, 같은 시드로 여러 번 돌려 안 떨어지는 것을 확인하라
  - 기준: (앞 단계 QA) 경고 줄의 이름이 올릴 때의 언어로 굳는다 — villager.gd _watch_stress 가 tr(name_key) 를 인자로 넘겨, 언어를 바꾸면 틀만 바뀌고 이름은 앞 언어로 남는다(주석에 적혀 있다). 다른 알림이 이름을 키로 넘겨 다시 옮기는 길이 있으면 그 길(Notice 인자 번역)을 따르게 하라
- [x] 4. (앞 단계 QA) 알림 글 인자를 모두 번역 키로 본다
  - 기준: 알림 글 인자를 모두 번역 키로 본다 — Notice.text_of 가 String 인자를 전부 TranslationServer.translate 에 넣는다. 사람이 지은 방 이름이 우연히 번역 키(예: 「VILLAGERS_HP」)와 같으면 엉뚱한 글로 바뀐다. 막는 것은 아니다. 고치려면 키를 StringName 으로 넘길 때만 옮기거나(_watch_stress 에서 StringName(name_key)), 「VILLAGER_NAME_」처럼 알려진 앞머리일 때만 옮기게 하고, test_villager_stress_warn.gd 에 키 모양의 방 이름이 그대로 남는지 보는 단언을 더하라

## [x] G-234 채광 — 바위가 갈라지고 희귀 광물이 나온다
- 차선: 3d-lane2
- 순서: 차선2 첫 묶음 (차선 1 의 G-232 · G-233 과 나란히 — 사람 2026-10-18 「너가 알아서 분량을 맞춰서 분할 해야지」)
- spec: spec/04_life/mining.md
- **사람 결정 2026-10-18** — 「한번에 캐지는게 아닌, 바위에 체력이 있고 n% 달 때마다 갈라지게(나중에 에셋으로 표현해도 됨) 일정 확률로 희귀 광물이 나옴」 · 「희귀 광물이라 해서 다음 등급의 광물을 주는게 아닌 다른 광물을 주는 의의」 · 「희귀 광물이 나올 확률이 3%다 하면 3%에서 뭐 석탄 철 유황이 희귀 광물 종류다 하면 석탄 50% 철 30% 유황 20% 이런식으로」
  - 이 세션이 다듬고 사람이 받았다(「너가 다듬어준 그대로 해도 좋은거 같아」): 굴리는 때는 **갈라질 때마다**(때릴 때마다가 아니다 — 빠른 곡괭이가 확률까지 더 받지 않게) · 희귀가 나오면 반짝임 · 소리를 확실히 · **새 아이템은 늘리지 않는다**(지금 있는 광석으로만 표를 짠다, 새 것은 쓸 곳이 생길 때) · 짝은 실제 지질을 따른다 · 숙련도가 희귀 확률을 올린다
- 이 세션이 2026-10-18 봤다: 바위 · 광물 · 나무는 이미 내구도가 있다 — life/harvest/harvest.gd:27 `DROPS`(rock 150 · 광물 200 · 나무 100) · :42 GOOD_HIT 25 / POOR_HIT 5 · `strike()` :104 → `_break()` :168 → `_drop()` :182 · 신호 `struck(handle, left)` 를 듣는 곳이 없다(list.md 「다음」의 「치는 티가 안 난다」) · 저장은 `dents()` :130 · 멀티는 서버가 친다(net_world.gd `_on_struck`) · 주민 광부도 같은 길(mining_work.gd)
- [x] 1. 갈라진다 — 치는 티가 난다
  - 기준: 내구도가 **25%씩 깎일 때마다 한 번 갈라진다**(네 단계, 25% 는 **임시** — harvest.gd 한 곳). 갈라질 때 그 바위가 눈에 띄게 바뀐다(금 간 무늬 · 조금 작아짐 · 돌 부스러기 — 에셋은 나중, 지금은 코드로 그린 것이면 된다) · 소리 (테스트: 갈라짐 단계가 내구도를 따라간다)
  - 기준: 치기만 하고 안 갈라지는 대에도 흔들림 · 작은 부스러기가 있다 — 나무도 같은 길이라 흔들린다(「맨손 20대 · 도끼 4대를 치는 동안 표시가 없다」 사람 2026-09-21)
  - 기준: 갈라진 단계는 저장 · 다시 불러오기 · 참가자 화면에 따라간다(지금의 dents 로 계산된다) (테스트)
- [x] 2. 갈라질 때 희귀 광물을 굴린다
  - 기준: 갈라질 때마다 **희귀 확률 3%**(사람이 든 예 · **임시**)를 굴리고, 붙으면 그 바위의 표에서 무게로 하나를 고른다. 표는 **한 곳**(예: life/harvest/rare_drops.json) — 바위 종류마다 `{아이템: 무게}`. 서버가 굴린다
  - 기준: 표 (**임시** — 실제로 같이 나는 짝, 지금 있는 아이템만):
    - 돌(rock): 석탄 50 · 철광석 30 · 유황광석 20 (사람이 든 예 그대로) — 첫 섬에서 철이 아주 드물게 나오는 길이다(「첫 섬은 돌이 대부분이고 철은 아주 드물다」 사람 2026-09-22)
    - 석탄: 철광석 60 · 유황광석 40 (석탄층의 황철석)
    - 구리광석: 은광석 60 · 금광석 40 (구리 광산의 부산물)
    - 은광석: 금광석 60 · 구리광석 40 · 금광석: 은광석 100 (자연금의 은)
    - 모래: 금광석 100 (사금) · 유황광석: 철광석 100 (황철석) · 철광석: 유황광석 100
    - 주석광석 · 석회석: 없음 — 짝(텅스텐 · 화석)이 아직 아이템이 아니다. 생기면 넣는다고 표에 한 줄
  - 기준: 희귀가 나오면 **반짝이는 빛 · 따로 나는 소리 · 한 줄 알림**(「희귀 광물 — 철광석」, 번역 키 ko · en) — 바닥에 떨어지는 것은 평소 드롭과 같다 (테스트: 확률을 1 로 두면 갈라질 때마다 표의 것이 떨어진다 · 0 이면 안 나온다)
  - 기준: 희귀 확률은 **굴리는 쪽이 넘기는 덤을 더하는 꼴**로 둔다(기본 덤 0) — 숙련도가 붙을 자리. **채광 숙련도로 덤을 주는 것(5레벨마다 +0.5%p, 임시)은 G-232(차선 1) 가 이 줄기에 들어온 뒤**다: persona.gd 에 SKILLS 가 있으면 주민 광부에 붙이고, 없으면 붙이지 말고 spec 미정에 「숙련도 덤 — G-232 뒤」 한 줄
  - 기준: (앞 단계 QA) spec/04_life/mining.md 에 갈라짐이 안 적혔다 — 사람 결정 2026-10-18 의 「n% 마다 갈라짐」과 지금 값(25% 임시 · 갈라짐 셋 + 넷째 마디 부서짐 · 갈라짐은 남은 내구도에서 셈해 따로 저장하지 않음 · 나무는 흔들리기만)을 spec 에 몇 줄로 적어라. 다음 단계(희귀 광물)를 짤 때 spec 만 읽고도 갈라짐 규칙을 알 수 있게
- [x] 3. spec
  - 기준: mining.md 에 갈라짐 · 희귀 굴림 · 표 · 숙련도를 사람 원문 · 날짜와 함께 적는다. 미정 「내구도 · 드롭 수량」 줄을 지금과 맞춘다. 광산 방(mine-room.md)은 아직 없으니 「생기면 같은 표를 쓴다」 한 줄

## [x] G-235 벌목 — 약점을 치면 크리티컬 · 희귀 드롭
- 차선: 3d-lane2
- 순서: G-234 뒤 (같은 harvest.gd)
- spec: spec/04_life/logging.md
- **사람 결정 2026-10-18** — 「벌목도 비슷하게 하는데 벌목은 일정 확률로 크리티컬이 터져서 데미지가 조금 더 많이 들어가도록 하는거」
  - 이 세션이 다듬고 사람이 받았다(2026-10-18): 무작위 크리티컬은 플레이어가 못 느낀다 → **나무에 약점 표시가 뜨고 그곳을 맞히면 크리티컬** · 벌목에도 **희귀 드롭**(씨앗 — farming.md 미정 「씨앗을 어디서 얻나」를 같이 메운다)
- 이 세션이 2026-10-18 봤다: 나무는 harvest.gd `DROPS` pine · broadleaf(내구도 100 → wood ×4) · attack.gd 에 크리티컬이 없다 · 꿀(honey)은 레시피에 쓰이는데 **나는 곳이 없다** · 송진 · 새 둥지 아이템은 없다(만들지 않는다) · 씨앗은 제작대에서 나무 1 → 2 뿐(workbench.json)
- [x] 1. 약점 — 맞히면 크리티컬
  - 기준: 도끼를 든 플레이어가 나무를 처음 치면 줄기의 한 자리(높이 · 방향 무작위)에 **약점 표시**가 뜬다. 조준점이 약점 둘레(반지름 **임시**)를 맞히면 **크리티컬 — 피해 2배(임시)** · 다른 소리 · 튀는 조각. 크리티컬이 나면 약점이 다른 자리로 옮긴다
  - 기준: 약점은 치는 사람 화면에만 보여도 된다. 피해는 서버가 정한다(맞힌 자리를 서버가 검사) (테스트: 약점을 맞힌 대와 빗맞힌 대의 깎인 내구도가 다르다)
  - 기준: 주민 나무꾼은 조준이 없다 — 크리티컬 확률(기본 5%, **임시**)로 굴린다. 벌목 숙련도 덤(5레벨마다 +2%p, 임시)은 G-232 가 이 줄기에 있으면 붙이고, 없으면 spec 미정에 한 줄
- [x] 2. 갈라질 때 희귀 드롭
  - 기준: G-234 의 갈라짐 · 희귀 굴림을 나무도 쓴다 — 같은 표 파일에 pine · broadleaf 줄: **씨앗 70 · 꿀 30**(**임시**). 씨앗은 crops.json 의 씨앗 가운데 무작위 하나(**임시** — 밀 · 보리처럼 흔한 것에 무게를 더 준다) (테스트)
  - 기준: 제작대의 씨앗 레시피는 그대로 둔다(**임시** — 없앨지는 사람이 플레이해 보고)
  - 기준: (앞 단계 QA) swing 의 조준 자리 넘김을 보는 테스트가 없다 — Harvest.swing() 이 Aim 광선의 hit["position"] 을 strike/ask 에 넘기는 길은 테스트마다 strike(..., at) 를 직접 불러 건너뛴다. 플레이어를 나무 앞에 세워 약점 쪽을 보게 하고 swing() 한 대가 GOOD_HIT×CRIT_MULT 를 깎는지, 반대쪽을 보면 GOOD_HIT 인지 보는 테스트를 tests/life/harvest/test_tree_weak_spot.gd 에 더하라
- [x] 3. spec
  - 기준: logging.md 에 약점 · 크리티컬 · 희귀 드롭을 사람 원문 · 날짜와 함께. farming.md 미정 「씨앗을 어디서 얻나」에 「나무 희귀 드롭 · 수확(G-236)」 한 줄

## [x] G-236 농사 — 거두면 씨앗이 돌아온다
- 차선: 3d-lane2
- 순서: G-235 뒤
- spec: spec/04_life/farming.md
- **사람 결정 2026-10-18** — 「지금 그대로 해도 됨. 피드백」 → 이 세션의 피드백을 사람이 받았다: 거두면 씨앗이 확률로 돌아온다 · **물을 안 준 날은 안 자랄 뿐 죽지 않는다**
- 이 세션이 2026-10-18 봤다: crops.gd `harvest()` :478 은 작물만 준다(씨앗 없음) · 안 준 밭은 그날 안 자랄 뿐이다(crops.gd:9-10 · :454) — 이미 받은 규칙과 같다
- [x] 1. 씨앗이 돌아온다
  - 기준: 거두면 그 작물의 씨앗이 **50% 로 하나**(**임시**) 같이 나온다 — 가방이 차면 바닥에(지금 `_give` 와 같다). 주민 농부가 거둘 때도 같다(방 상자로) (테스트)
  - 기준: 농사 숙련도 덤(5레벨마다 +5%p, **임시**)은 G-232(차선 1) 가 이 줄기에 있으면 붙이고, 없으면 spec 미정에 「숙련도 덤 — G-232 뒤」 한 줄
  - 기준: farming.md 에 씨앗이 돌아오는 것 · 「물을 안 준 날은 안 자랄 뿐 죽지 않는다 (사람 결정 2026-10-18)」를 적고 미정 「물 마름 규칙」을 지운다

## [x] G-237 낚시 — 낚싯대 · 찌 · 떡밥 · 물고기 종류
- 차선: 3d-lane2
- 순서: G-236 뒤 (모듈은 G-231 의 길)
- spec: spec/04_life/fishing.md
- **사람 결정 2026-10-18** — 「낚시는 현재 미니게임 괜찮음. 낚시는 낚시대랑 찌, 떡밥의 종류를 세분화 해야됨」 · 「낚시대: 튼튼하다를 목적으로 %게이지가 - 되는걸 완화해준다」 · 「찌: 이것도 총과 같이 모듈 개념. 낚시대에 넣으며, 이건 물고기 바가 커짐」 · 「떡밥: 이건 물고기가 여러개 나옴」
  - 이 세션이 다듬고 사람이 받았다(2026-10-18): 셋 다 「쉽게 · 많이」라 「다르게」가 없다 → **떡밥 종류 · 물 종류(민물 · 바다)가 어떤 물고기가 오나를 정하고**, **희귀한 물고기일수록 미니게임에서 거칠게 움직인다**. 낚싯대 = 버팀 · 찌 = 맞추기 · 떡밥 = 무엇이 얼마나 · 물고기 = 난이도
- 이 세션이 2026-10-18 봤다: fishing.gd:41 BAIT `bait` · :42 ROD `fishing_rod` · :45 `BAIT_GRADES {bait=[3,5]}` · :50 `FISH_KINDS ["fish"]` · fishing_gauge.gd `BAR_WIDTH 0.26`(주석: 찌가 넓힐 자리) · `DRAIN_RATE 0.1` · `FISH_SPEED 0.3` · `swim()` :81 은 일정한 빠르기로 무작위 때에 방향만 바꾼다 · 전부 상수라 낚싯대마다 바꾸려면 인스턴스 값이 된다 · 모듈은 armor_modules.gd(`GUN_SLOTS` · `MODULES` · `for_bench` · `slot_kind`) + 무기 제작대 모듈 탭(station_catalog `MODULES_WEAPON`) · 민물 · 바닷물은 물뿌리개가 이미 가른다(farming.md)
- [x] 1. 물고기 종류와 움직임
  - 기준: 물고기 종류 (**임시 — 사람이 목록을 주면 바꾼다**, fishing.md 「종류 목록은 사람이 나중에 알려 준다」): 민물 셋(붕어 · 잉어 · 메기) · 바다 셋(고등어 · 농어 · 도미) — 종마다 **흔함 / 드묾 / 희귀** 한 단계. 표는 한 곳(예: life/fishing/fish.json)
  - 기준: 종마다 미니게임 움직임이 다르다 — 빠르기 · 방향 바꾸는 간격 · **확 튀기**(잠깐 빨라짐) · **멈칫**(잠깐 섬). 흔한 것은 지금과 비슷하고 희귀할수록 거칠다(값 **임시**) (테스트: 희귀 종의 한 판 움직임이 흔한 종보다 크다)
  - 기준: 던진 자리가 민물이면 민물 종 · 바다면 바다 종만 문다 (테스트)
  - 기준: 옛 아이템 `fish` 를 쓰던 레시피는 **모든 물고기를 받는 태그**(tags.json, 예: `@fish`)로 바꾼다 · 옛 저장본의 `fish` 는 깨지지 않는다(남기거나 한 종으로 옮긴다 — old_items.gd)
- [x] 2. 떡밥 종류 — 마리 수와 무엇이 오나
  - 기준: 떡밥 세 단(**임시**): 떡밥(지금 것, 3~5마리 — 사람 결정 2026-09-28 · 흔한 종 쪽) · 둘째(4~6마리 · 드문 종 무게 ↑) · 셋째(5~8마리 · 희귀 종 무게 ↑). BAIT_GRADES 를 늘리고 종마다 떡밥별 무게를 표에 둔다. 둘째 · 셋째 재료는 지금 있는 것으로(예: 생선 · 꿀 — **임시**) (테스트)
  - 기준: (앞 단계 QA) 쓰지 않는 is_darting() — game/life/fishing/fishing_gauge.gd 의 is_darting() 을 부르는 곳이 코드 · 테스트 어디에도 없다. 테스트의 darted 세기를 g.is_darting() 으로 바꿔 쓰거나(그러면 dart_boost=1.0 이어도 튀기를 셀 수 있다) 함수를 지워라
- [x] 3. 낚싯대 등급 — 덜 빠진다
  - 기준: 낚싯대 세 단(**임시**): 나무(지금 `fishing_rod`) · 청동 · 철 — 게이지 빠지는 빠르기 ×1 · ×0.75 · ×0.5(**임시**). FishingGauge 가 든 낚싯대에서 값을 받는다. 재료 · 만드는 곳은 지금 낚싯대 레시피 곁에(**임시**) (테스트)
- [x] 4. 찌 — 낚싯대의 모듈
  - 기준: 낚싯대에 **찌 칸 하나** — G-231 모듈과 같은 길(armor_modules.gd 에 낚싯대 칸 · 찌 종류)로 **무기 제작대 「모듈」 탭**에서 인벤토리에서 끌어 넣는다 · 빼면 돌려받는다(사람 2026-10-17 「모듈을 뺄땐 돌려 받는거지」) · 찌를 안은 낚싯대는 겹치지 않는다
  - 기준: 찌 두 단(**임시**): 나무 찌 · 깃털 찌(feather 는 있다) — 초록 바 너비 +20% · +40%(**임시**) (테스트: 찌를 끼운 낚싯대의 판에서 바가 넓다 · 빼면 돌아온다)
  - 기준: 저장 · 멀티(참가자가 끼운 찌가 그 사람 판에 듣는다) · ui_shots 로 모듈 탭에 낚싯대가 넘치지 않는다 (테스트)
  - 기준: (앞 단계 QA) 영어 번역은 테스트가 안 본다 — test_rod_grades.gd test_higher_rods_are_items_made_beside_the_wooden_rod 는 ko 로케일에서만 tr 을 본다. en.po 에 ITEM_bronze_fishing_rod · ITEM_iron_fishing_rod 는 들어 있다(이번 것은 괜찮다). 비슷한 테스트를 짤 때 en 로케일에서도 tr 이 키 그대로가 아닌지 단언하면 좋다
- [x] 5. spec
  - 기준: fishing.md 규칙을 위로 고쳐 쓴다 — 사람 원문 · 날짜 · 「찌는 나중에」 줄을 지금으로. 미정 「찌가 무엇을 얼마나」 · 「떡밥 등급이 몇 단」을 지운다. 물고기 목록이 임시라는 줄은 남긴다 · 낚시 숙련도가 무엇을 올릴지는 미정에 한 줄
  - 기준: (앞 단계 QA) 무기 제작대 빈 자리 이름표가 「총」뿐이다 — 낚싯대도 놓이는데 module_page.gd:217 이 무기 제작대 빈 자리에 STATION_MODULE_WEAPON(「총」/"Gun")을 그린다. 「총 · 낚싯대」("Gun / Rod") 같은 글로 바꾸거나 새 키를 두고 ko.po · en.po 둘 다 넣고, test_module_page 에서 빈 자리 이름표를 단언하라
  - 기준: (앞 단계 QA) fishing.md 낡은 줄 — 「미끼는 없다. 찌를 바꿀 수 있다 — 찌는 나중에 부착」 spec/04_life/fishing.md:28 은 떡밥(G-237.2) · 찌(G-237.4)가 생긴 지금과 어긋난다. 사람 결정 줄인지 확인하고, 아니면 「찌는 낚싯대 모듈 칸에 끼운다(위)」로 고쳐라 — 사람 결정 줄이면 지우지 말고 옆에 지금 사정을 덧붙여라
- [x] 6. (앞 단계 QA) 흔들리는 일꾼 테스트를 기다림만 늘려 덮었다
  - 기준: 흔들리는 일꾼 테스트를 기다림만 늘려 덮었다 — test_villager_hands_room.gd _wait_for 를 2×→4×PATIENCE, test_villager_watering.gd 둘 일꾼 테스트를 PATIENCE→2×PATIENCE 로 늘렸다. 이번 전체 실행은 3262개 다 통과했고 wait_until 은 되면 곧바로 돌아오니 지금은 막는 것이 아니지만, G-237.3 부터 실패가 보이기 시작했다는 보고가 있다. 일꾼이 마지막 짐을 상자로 나르는 데 실제로 얼마나 걸리는지(시뮬레이션 초)를 재어 테스트 주석에 적고, 늘어난 까닭이 일꾼 길찾기 · 짐 나르기의 느려짐이 아닌지 확인하라 — 느려진 것이면 그쪽을 고치고 기다림은 되돌린다

## [x] G-238 목장 — 우리 · 야생과 온순함 · 목줄 · 굶으면 죽는다
- 차선: 3d-lane2
- 순서: 차선2 — 차선2 가 G-234 ~ G-237 을 끝내고 쉬고 있어 옮겼다(2026-10-18). G-232 는 이미 합쳐졌다 · 차선 1 의 G-233(주민 상태)과 villager.gd 가 겹칠 수 있다 — 조련사(단계 6)는 G-233 이 이 줄기에 들어온 뒤에 하면 덜 부딪힌다
- spec: spec/04_life/ranching-breeding.md, hunting.md, spec/06_build/rooms.md
- **사람 결정 2026-10-18** (원문): 「사냥을 해서 마취된 동물을 케이지에 넣음. 아이템에 (마취)타이머가 돌아가고 타이머가 끝나면 도망감. 그래서 플레이어는 미리 동물 방을 만들어야함. 그냥 땅에 놔도 되지만, 그럼 거기서 길러야됨. 처음엔 '야생'이라는 상태가 되며 사료를 먹거나 사람이랑 상호작용을 하면 '온순함'이 오름. 온순함이 아닌 0%~99% 에선 야생상태이고 100%가 되면 온순함 상태로 변경됨. 야생 상태는 잡실이거나 외부에 있으면 1분뒤에 무조건 도망침. 도망이라는건 다시 잡을 수 없는 상태인 그냥 없어지는거라 생각하면 됨. 이걸 막을 방법은 목줄로 목줄만 있는게 아니라 땅에 박고 목줄을 연결하면됨. 방은 사료통, 물통이 있으면 ~방(대기중) … 동물을 넣으면 해당 동물 방 예를 들면 사슴방 이런 식으로 변경되게. … 사슴을 키우면 녹용을 지속적으로 얻을 수 있음」
  - 사람이 답했다(2026-10-18): **축사는 우리로 합친다**(축사 설치물을 없앤다) · **야생이어도 제 우리 안이면 도망가지 않는다** · **적대 동물(곰)도 키운다 — 야생이면 다가가 먹이 · 쓰다듬기를 하면 공격한다** · **사료통이 비면 죽는다 — 비자마자 서서히 줄어 사흘 뒤에 죽는다**
  - 이 세션이 다듬고 사람이 받았다: 방 이름 **「빈 우리」→「사슴 우리」** · 도망까지 남은 1분은 **동물 머리 위 초읽기** · **쓰다듬기는 하루 한 번만** 오른다(사료는 꾸준히) · **조련사 주민**이 온순함을 올린다(조련 숙련도) · 녹용은 실제로도 해마다 새로 자라는 뿔이다
- 이 세션이 2026-10-18 봤다: 축사는 설치물(stations.json:23 `barn` · herd `{feed:@grain, heads:8, feed_max:20}`) — 로직은 life/ranching/herd.gd(먹이 · 산출 · 같은 종 둘 → 새끼) · stations.gd:748 `put_animal` · 가축 여덟 산출은 livestock.json · 잡은 짐승은 가방 칸(captives.gd:121 `captive_<kind>` · sedation · cage) — 케이지 없이 0 이 되면 `_escape` :197 로 풀려 달아난다 · **땅에 놓는 길이 없다** · **온순함 · 사료통 · 물통 · 목줄 · 말뚝이 없다** · 방 종류는 데이터(build/rooms.json `needs` · room_catalog.gd:93 `kind_of`) · 방은 바닥 · 지붕 · 벽이 다 있어야 방이다(rooms.gd:39) · 조련사 직업은 없다(jobs.gd:31) · 들판에 나는 짐승은 사슴뿐(가축 여덟은 아직 들판에 안 난다 — 이번에 만들지 않는다)
- **이번에 만들지 않는 것**: 가축 여덟이 들판에 나는 자리 · 이로치 · 펫 · 탈것
- [x] 1. 사료통 · 물통 · 우리 방
  - 기준: 설치물 **사료통**(곡물 `@grain` 을 넣는다 · 한도 **임시**) · **물통**(물뿌리개로 채운다 · 한도 **임시**). 만드는 곳 · 재료 **임시**
  - 기준: rooms.json 에 **우리**(`pen`, needs 사료통 1 · 물통 1). 동물이 없으면 이름이 **「빈 우리」**, 동물이 들면 **「<그 동물> 우리」**(사슴 우리 — 여럿이면 가장 많은 종 · **임시**). 방 이름 표시(rooms_named 등)에 그대로 보인다 · 번역 키 ko · en (테스트)
- [x] 2. 잡은 동물을 놓는다 — 야생과 온순함
  - 기준: 가방의 잡은 짐승(captive_*)을 **땅에 놓는다**(놓기 단추 · 케이지째든 아니든). 놓으면 살아 있는 개체가 되어 그 자리 둘레를 걷는다 — 우리 안이면 우리 안만 걷는다(벽 · 문을 넘지 않는다)
  - 기준: 개체마다 **온순함 0~100** · 0~99 는 **야생**, 100 이면 **온순함** 상태(사람 결정). 놓은 짐승은 0 에서 시작(**임시**). 나이(born)는 이어진다 · 저장 · 멀티 (테스트)
  - 기준: 개체를 겨누면 종 · 상태(야생 n% / 온순함) · 배고픔이 보인다
- [x] 3. 도망 — 1분 · 말뚝과 목줄
  - 기준: **야생이 잡실(misc)이나 방 밖에 있으면 1분 뒤 사라진다** — 다시 잡을 수 없다(사람 결정). 머리 위에 남은 초가 보인다 · 사라질 때 한 줄 알림. **제 우리(pen) 안이면 안 간다**(사람 답 2026-10-18) · 온순함 100 은 어디서도 안 간다 (테스트)
  - 기준: 설치물 **말뚝**(땅에 박는다) · 아이템 **목줄** — 목줄을 든 채 짐승과 말뚝을 차례로 누르면 묶인다. 묶인 짐승은 말뚝 둘레(반지름 **임시**)만 걷고 도망 초읽기가 없다 · 풀 수 있다 · 저장 · 멀티. 재료 **임시** (테스트)
- [x] 4. 온순함이 오른다 — 사료 · 손길 · 적대 짐승
  - 기준: 사료통에서 먹을 때마다 온순함이 오른다(꾸준히, 크기 **임시**) — 묶인 짐승은 플레이어가 손으로 먹이를 준다(곡물을 들고 누른다)
  - 기준: **쓰다듬기**(맨손으로 누른다) — 개체마다 **하루 한 번**만 오른다(이 세션 다듬음 · 사람 받음). 두 번째는 「오늘은 이미 했다」 한 줄 (테스트)
  - 기준: **적대 짐승(곰)이 야생이면** 먹이 주기 · 쓰다듬기를 하려 다가간 사람을 **공격한다**(사람 답 2026-10-18 · 사냥의 공격 길을 쓴다 · 묶였으면 줄이 닿는 데까지). 온순함 100 이면 공격하지 않는다 (테스트)
  - 기준: (앞 단계 QA) 참가자 화면의 달아남 알림을 안 본다 — test_hunt_on_server.gd 의 test_a_wild_one_running_off_vanishes_for_the_guest_too 는 그림이 사라지는 것만 보고 net_world._animals_seen 이 띄우는 Notice(RANCH_ESCAPED)는 단언하지 않는다. 미르 섬의 Notice 가 「사슴이(가) 달아났다」인지 단언을 하나 더하라. 또 이 판정은 남은 초 ≤ ESCAPE_GONE(1초)에 기대어 다른 까닭으로 1초 남짓에 사라진 개체에도 뜰 수 있다 (혼자 판 기준은 통과했다)
  - 기준: (앞 단계 QA) 남의 파일 비공개 함수를 부른다 — net_world.gd 가 Tether._name(beast) 를 부른다. 밑줄 함수는 그 파일 안에서만 쓰는 약속이니 Tether 에 공개 함수(예: species_name)로 바꾸거나 Ranch 쪽 이름 함수를 써라
- [x] 5. 산출 · 번식 · 굶어 죽음 — 축사를 우리로 합친다
  - 기준: **온순한** 짐승이 우리 안에 있고 사료통 · 물통이 차 있으면 산출이 바닥(또는 우리 상자)에 나온다 — 가축 여덟은 지금 livestock.json 산출 그대로 · **사슴은 녹용**(새 아이템 `deer_antler`, 주기 **임시**) · 곰은 없음(**임시**). 같은 종 온순한 둘 + 먹이 → 새끼(herd.gd 규칙을 옮긴다 · 새끼는 온순함 100 으로 난다, **임시**) (테스트)
  - 기준: **사료통이 비면 그 우리 짐승의 배고픔이 그때부터 서서히 줄어 사흘(게임 날) 뒤 0 — 0 이면 죽는다**(사람 답 2026-10-18). 비어 있는 동안 산출이 멈추고 머리 위에 굶는 표시 · 하루 남으면 한 줄 경고 · 다시 채우면 배고픔이 차오른다(빠르기 **임시**). 죽으면 주검이 남는다(사냥의 주검 · 해체 길). 묶인 짐승도 손으로 안 먹이면 같다 · 물통은 비면 산출만 멈춘다(**임시**) (테스트)
  - 기준: **축사 설치물을 없앤다**(사람 답) — 레시피에서 빼고, 옛 저장본의 축사는 깨지지 않게: 놓인 자리에 든 가축을 온순함 100 으로 풀어 놓고 먹이 · 축사 재료를 돌려준다(old_items.gd · 저장 검사). 축사를 쓰던 테스트 · ui_shots(barn) · 관리인 똥 치우기는 우리로 옮긴다 (테스트)
  - 기준: (앞 단계 QA) Animal.provoke 를 지켜 주는 테스트가 없다 — provoke 첫 줄에 return 을 넣어도 test_taming.gd 13개가 다 통과했다(곰이 가까워 _look 으로 어차피 본다). 「보이지 않아도 손길을 내민 사람을 쫓는다」를 지키려면 test_taming.gd 에 플레이어를 곰 시야 밖(뒤쪽 · 눈 각도 밖)에 두고 taming.press 뒤 bear._prey == player 또는 물림을 단언하는 테스트를 더하라. 아니면 provoke 가 필요 없다면 지워라
- [x] 6. 조련사 주민
  - 기준: jobs.gd LIST 에 **조련사**(올리는 숙련도 조련) — 맡은 우리의 사료통 · 물통을 방 상자에서 채우고, 짐승마다 하루 한 번 쓰다듬는다. 조련 숙련도 5레벨마다 온순함 오름 +10%(**임시**) · 번역 키 ko · en (테스트)
- [x] 7. spec
  - 기준: ranching-breeding.md 를 위 흐름으로 고쳐 쓴다 — 사람 원문 · 날짜 · 사람의 네 답. 「축사」 줄은 「우리로 합쳤다 (사람 결정 2026-10-18)」. hunting.md 의 케이지 · 도망 줄과 맞추고, rooms.md 에 우리 방. 150줄 안
  - 기준: (앞 단계 QA) 방 밖 짐승 단언이 순서에 기대 있다 — test_villager_tamer.gd 의 test_the_tamer_pets_every_beast_in_the_pen_once_a_day 에서 TendingWork._beasts 의 Rooms.holds(room, …) 조건을 지워도 「방 밖 짐승은 맡은 우리가 아니다」 단언은 통과했다(밖 짐승이 멀어 맨 나중에 가기 때문). 지금은 뒤따르는 「오늘 할 일을 마쳤다」 단언이 우연히 잡아 준다. 방 밖 짐승만 하나 두고(우리 안 짐승 없이) 조련사를 몇 초 돌린 뒤 outside.tameness == START_TAMENESS 와 why_still == WHY_NOTHING_TO_DO 를 단언하는 테스트를 따로 더하라
- [x] 8. (앞 단계 QA) 새 테스트의 온순함 단언은 아무것도 지키지 않는다
  - 기준: 새 테스트의 온순함 단언은 아무것도 지키지 않는다 — QA 가 tending_work.gd:201 에서 Rooms.holds 조건을 지웠을 때 test_a_beast_outside_the_pen_alone_is_never_petted 는 why_still 단언에서만 떨어졌고, tameness == START_TAMENESS · petted_day != today 단언은 그대로 통과했다. 조련사가 20초 안에 밖 짐승에 닿아 쓰다듬지 못한다는 뜻이라 주석 「걸음 1.5 m/s 로 밖 짐승(18 m)에 닿고도 남는다」가 틀렸다. 조건을 지웠을 때 tameness 단언도 떨어지도록 기다리는 시간을 늘리거나(예: 60초) 밖 짐승을 방 문 바로 앞에 두고, 일부러 깨 봐서 두 단언이 다 떨어지는지 확인하라

## [x] G-915 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-start
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. 관리인 바꿈에서 조사가 틀렸다 (G-232 단계 3)
  - 기준: 관리인 바꿈에서 조사가 틀렸다 — game/automation/villagers/villagers.gd:136 「관리인가 똥을 치울」→「관리인이 똥을 치울」, game/automation/villagers/crafting_work.gd:86 「제작자와 관리인(축사)가 같이 쓴다」→「…관리인(축사)이 같이 쓴다」로 고쳐라

## [x] G-239 내 섬 야생 동물 열한 종 — 크기 · 먹이 · 도축 산출
- 차선: 3d-start · 시작 2026-10-18 18:53
- 순서: 차선 1 첫 묶음 (차선 2 의 G-242 와 나란히 · G-243 · G-244 가 이 묶음 뒤)
- spec: spec/04_life/hunting.md, spec/04_life/ranching-breeding.md
- **사람 결정 2026-10-18** (원문 요약): 내 섬에 만들 동물 — 「멧돼지: 가죽, 고기 얻는 용으로만 가죽은 하급 가죽, 고기는 5개 · 꿩: 깃, 고기 1개 · 매: 새장으로 사육 고기1개 · 오소리: 오소리 기름, 고기. 기름은 약으로 고기는 1개 · 담비: 담비 가죽 1개 → 고급 모피, 고기1개 · 거위: 경비 동물. 깃털 5개, 고급 침대 재료. 고기1개 · 꿀벌 · 올빼미: 밤에 사냥, 고기1개 · 앙고라토끼: 털을 주기적으로 깎아줘야 함, 베틀에 넣어 천 / 고기 1개 · 호저: 가시가 엄청 자람 → 고급 찌, 고기1개 · 사슴: 녹용2개, 고기3개 · 고슴도치: 고슴도치가죽 → 약재, 고기 1개 · 곰: 그냥 큼 기를 수 없음. 고기 20개, 곰발바닥 → 요리」
  - 「동물마다 대형동물 중형동물 소형동물을 정해서」 · 「각 동물마다 어느 동물을 먹는지를 적어두자. 매는 중형, 소형 올빼미는 소형. 동족을 공격하지 않는다」
  - 이 세션이 다듬고 사람이 받았다: 크기 · 먹이표(아래) · 고슴도치 가죽은 실제 한약재(위피)
- 이 세션이 2026-10-18 봤다: 야생은 사슴 · 곰 둘뿐(life/hunting/animal_kinds.gd:9 · 부위 → 산출 `KINDS`) · 나는 자리는 island_animals.gd:55 `bp.deer_dens` · 가축 여덟은 같은 표에 있다 · 아이템 등록은 player/inventory_hotbar/weights.json · **곰 길들이기는 G-238 에서 만들었다**(test_taming.gd) — 사람이 이번에 「기를 수 없음」으로 바꿨다
- **이번에 만들지 않는 것**: 야생끼리 실제로 사냥하기(먹이표는 데이터만 — 곰 · 오소리의 벌통 습격은 G-241) · 늑대(사람이 아직 안 넣었다) · 부정 특성(사람이 따로 말한다)
- [x] 1. 열한 종을 들판에 낸다
  - 기준: 새 종 — 멧돼지(boar) · 꿩(pheasant) · 매(hawk) · 오소리(badger) · 담비(marten) · 거위(goose) · 올빼미(owl) · 앙고라토끼(angora_rabbit) · 호저(porcupine) · 고슴도치(hedgehog). 사슴 · 곰은 있다. 꿀벌은 짐승이 아니다(G-241)
  - 기준: 종마다 내 섬에 나는 자리(설계도의 집 자리를 종마다 — 자리 · 마릿수 **임시**) · 몸 크기 · 색이 다른 코드 모양(에셋은 나중) · 마취 · 케이지 · 도축 길은 사슴과 같다. 매 · 올빼미는 날아다닌다(낮게 · 임시) — 올빼미는 밤에만 나온다. 매는 잡기 어렵게(더 빠르고 멀리서 달아난다 · 수치 **임시**) (테스트)
  - 기준: 성향 — 곰은 지금처럼 선제 공격 · 멧돼지는 맞으면 덤빈다(**임시**) · 나머지는 달아난다. 거위는 달아나며 운다 · 저장 · 멀티 (테스트)
- [x] 2. 크기와 먹이표를 데이터로
  - 기준: 종마다 **크기**(대 · 중 · 소)와 **노리는 크기** · 먹는 것을 한 곳(animal_kinds.gd 또는 json)에 — 대: 곰 · 멧돼지 · 사슴 · 소 · 물소 · 말 / 중: 매 · 올빼미 · 오소리 · 담비 · 거위 · 호저 · 양 · 염소 · 돼지 / 소: 꿩 · 앙고라토끼 · 고슴도치 · 닭 · 오리
  - 기준: 사냥하는 종 — **매는 중형 · 소형**, **올빼미는 소형**(사람 결정) · **같은 종은 노리지 않는다** · 우리 안이거나 온순함 100 인 짐승(내 가축)은 노리지 않는다. 함수 하나로 묻는다(예: `AnimalKinds.can_hunt(hunter, prey_beast)`) (테스트: 매→꿩 참 · 매→매 거짓 · 매→곰 거짓 · 올빼미→오소리 거짓 · 매→내 거위 거짓)
  - 참고(먹이 — spec 에 적는다): 곰 열매 · 물고기 · 꿀 · 사슴 / 멧돼지 뿌리 · 도토리 / 사슴 풀 / 오소리 지렁이 · 열매 · 야생 벌집 / 담비 꿩 · 토끼 · 알 / 거위 풀 · 곡식 / 호저 나무껍질 / 꿩 씨앗 · 곤충 / 앙고라토끼 풀 / 고슴도치 곤충 · 지렁이
  - 기준: (앞 단계 QA) _brightness 의 doc 주석이 떨어져 나갔다 — game/core/debug/shots.gd 에서 _set_up_wild_kinds 를 「## 그림의 평균 밝기 (0~1) …」 두 줄과 func _brightness 사이에 끼워 넣어, 그 주석이 _set_up_wild_kinds 위에 붙었다. 「## 그림의 평균 밝기 …」 두 줄을 func _brightness 바로 위로 옮겨라
  - 기준: (앞 단계 QA) 올빼미 시계 연결을 지키는 테스트가 없다 — test_island.gd _build_clock 의 `animals.clock = clock` 한 줄을 지워도 떨어질 테스트가 없다(test_wild_kinds 는 IslandAnimals 에 시계를 손으로 넣는다). 실제 섬(test_island_animals.gd)에서 _island.clock.set_clock(day, 22.0) 뒤 _island.animals.tick_night() 하면 올빼미가 서는지(animals().any(kind == OWL)) 보는 단언을 더하라
- [x] 3. 도축 산출 — 사람이 정한 수
  - 기준: 고기 — 멧돼지 5 · 꿩 1 · 매 1 · 오소리 1 · 담비 1 · 거위 1 · 올빼미 1 · 앙고라토끼 1 · 호저 1 · 고슴도치 1 · **사슴 3**(지금 4 에서) · **곰 20**(지금 6 에서). 뼈 · 내장 등은 크기 따라 **임시**
  - 기준: 새 아이템 — **하급 가죽**(멧돼지) · **깃털**(꿩 — 있는 feather) · **오소리 기름** · **담비 가죽**(고급 모피, 1) · **거위 깃털**(5) · **고슴도치 가죽** · **곰발바닥**. 사슴 머리는 **녹용 2**(G-238 의 deer_antler). 이름 · 무게 · 번역 키 ko · en (테스트: 종마다 도축 산출표)
  - 기준: 쓰임 — 오소리 기름은 화상 연고(apothecary_bench burn_ointment)의 기름 자리에 넣을 수 있다 · 거위 깃털은 고급 침대 재료(침대 레시피 하나 · 재료 **임시**) · 곰발바닥은 요리대 요리 하나(**임시**) · 고슴도치 가죽 · 담비 가죽의 레시피는 사람이 정할 때(spec 미정 한 줄)
- [x] 4. 곰은 기를 수 없다
  - 기준: 잡은 곰은 **땅에 놓을 수 없다**(놓기를 누르면 「곰은 기를 수 없다」 한 줄) — 마취 · 케이지 · 도축은 그대로. 곰은 온순함이 오르지 않는다. G-238 의 곰 길들이기 테스트는 이 규칙으로 고친다 (테스트)
  - 기준: 옛 저장본에 놓여 있던 곰은 들판의 야생 곰이 된다(저장 검사) (테스트)
  - 기준: (앞 단계 QA) spec 산출표 머리줄이 「개수 전부 임시」로 낡았다 — spec/04_life/hunting.md 의 「도축 산출표 (G-204.3 …) — 개수 전부 임시」 줄은 이제 고기 수 · 녹용 2 · 담비 가죽 1 · 거위 깃털 5 가 사람 결정이다. 「개수는 아래 사람 결정 줄 밖의 것만 임시」로 고쳐라 (표 아래 사람 결정 줄이 있어 막는 것은 아니다)
- [x] 5. spec
  - 기준: hunting.md 에 열한 종 · 크기 · 먹이표 · 도축 산출표 · 사람 원문 날짜 · 「곰은 기를 수 없다(사람 2026-10-18, G-238 의 곰 키우기를 바꿨다)」 · ranching-breeding.md 의 곰 줄을 맞춘다. 150줄 안
  - 기준: (앞 단계 QA) 들판으로 옮긴 곰이 restore 두 번에 겹친다 — game/life/ranching/ranch.gd restore 는 「두 번 불러도 겹치지 않게」 Ranch 자식만 치우는데, _to_field 는 곰을 scope()(섬) 아래에 세우므로 같은 저장본으로 restore 를 두 번 부르면 들판 곰이 둘 선다. 지금 게임 길에서 두 번 부르는지는 확인 못 했다(옛 저장본에서 한 번뿐이라 막는 것은 아니다). _to_field 로 세운 곰에 표(meta 나 그룹)를 달아 restore 머리에서 함께 치우고, test_ranch_place.gd 에 restore 를 두 번 불러도 곰이 하나인지 보는 단언을 더하라
- [x] 6. (앞 단계 QA) 앙고라토끼 표기가 둘로 갈렸다
  - 기준: 앙고라토끼 표기가 둘로 갈렸다 — game/i18n/ko.po 691행 ITEM_captive_angora_rabbit 는 칸 줄바꿈을 피하려 「잡은 앙고라 토끼」로 띄었지만 2001행 짐승 이름은 「앙고라토끼」 그대로다. 한쪽으로 맞춰라(이름도 「앙고라 토끼」로 띄우거나, 칸 맞춤 쪽을 다른 방법으로). test_item_slot_fit 이 계속 통과하는지 확인
  - 기준: (앞 단계 QA) give_way 테스트가 부하에 흔들린다 — test.log 의 앞선 전체 실행에서 game/tests/automation/villagers/test_villager_g218_give_way.gd 가 6개 동시 실행 중 2.53초 > 0.6초로 한 번 떨어졌다(마지막 전체 실행은 3400/3400 통과). 시간 문턱을 벽시계가 아니라 물리 프레임 수로 재거나 여유를 두어 동시 실행에서도 흔들리지 않게 하라

## [x] G-240 가위 — 앙고라토끼 털 · 호저 가시
- 차선: 3d-start · 시작 2026-10-18 21:44
- 순서: G-239 뒤
- spec: spec/04_life/ranching-breeding.md, spec/05_craft/recipes.md
- **사람 결정 2026-10-18**: 「털깎기 시설을 넣을바엔 가위로 하나씩 하는게 플레이어 입장에선 안귀찮지」 — 털깎기 시설은 안 만든다 · 가위 하나로
  - 앙고라토끼 「복슬복슬한 털을 주기적으로 깎아줘야 함. 털을 베틀에 넣어서 천으로」 · 호저 「가시가 엄청 자람 → 호저 가시 → 제작대에서 가공하면 고급 찌」
- 이 세션이 2026-10-18 봤다: 베틀은 설치물(stations.json loom) · 찌는 무기 작업대 모듈 탭(G-237 — 나무 +20% · 깃털 +40%)
- [x] 1. 가위로 깎는다
  - 기준: 아이템 **가위**(작업대 · 재료 **임시**). 들고 앙고라토끼 · 호저를 누르면 털(**앙고라 털**) · 가시(**호저 가시**)가 나온다 — 개수 **임시**. 깎고 나면 다시 자랄 때까지(주기 **임시**) 못 깎고, 자라는 정도가 겨누면 보인다 · 모양도 바뀐다(털이 줄어든다)
  - 기준: **온순함 100 이거나 묶인 짐승만** 깎인다 — 야생은 「가만히 있지 않는다」 한 줄 · 저장 · 멀티 (테스트)
- [x] 2. 털 → 천 · 가시 → 고급 찌
  - 기준: 베틀에서 앙고라 털 → 천(있는 아이템 · 수 **임시**) · 작업대에서 호저 가시 → **고급 찌**(무기 작업대 모듈 탭의 찌 칸 · 입질 +60% **임시**) (테스트)
  - 기준: 조련사 주민이 맡은 우리의 다 자란 짐승을 가위로 깎는다(가위가 방 상자에 있으면) (테스트)
  - 기준: (앞 단계 QA) weights.json _temporary 문장이 꼬였다 — game/player/inventory_hotbar/weights.json 의 _temporary 에서 「곰발바닥 찜(G-239.3)도 임시다.」 뒤에 가위 문장을 끼워 넣어, 원래 곰발바닥 쪽 설명이던 「— 가죽 · 깃털 · 기름에 견주어 잡았다」가 가위 문장 꼬리에 붙었다. 「…곰발바닥 찜(G-239.3)도 임시다 — 가죽 · 깃털 · 기름에 견주어 잡았다. 가위 · 앙고라 털 · 호저 가시(G-240.1)는 칼 · 양털에 견주어 잡은 임시 값이다.」 순으로 고쳐라
  - 기준: (앞 단계 QA) 서버가 가위를 '들었나' 대신 '가방에 있나'로 본다 — net_world.gd _shear_animal 은 kit.inventory.count_of(Shearing.ITEM) > 0 만 본다. 손에 다른 것을 들고 가방 어딘가에 가위만 있어도 서버는 깎아 준다(참가자 쪽 Shearing.press 가 holds 로 막으니 정상 길에선 안 난다). 사본 가방의 고른 칸(held_item 에 해당하는 것)으로 보거나, Taming 의 서버 검사와 같은 꼴인지 맞춰 주석으로 까닭을 적어라
- [x] 3. spec
  - 기준: ranching-breeding.md 에 가위 · 「털깎기 시설은 안 만든다(사람 2026-10-18)」 · 찌 줄(fishing.md)에 고급 찌
  - 기준: (앞 단계 QA) test_shear_goods 가 로케일을 안 돌려놓는다 — game/tests/life/ranching/test_shear_goods.gd 마지막에 TranslationServer.set_locale("ko") 한 뒤 되돌리지 않는다. 파일마다 따로 돌아 지금은 괜찮지만, 앞 로케일을 받아 두고 끝에 되돌리거나 after_each 에서 되돌려라
  - 기준: (앞 단계 QA) 새 서버 테스트가 낡은 wait_frames 를 쓴다 — game/tests/multiplayer/test_hunt_on_server.gd 의 test_the_server_shears_only_with_scissors_in_hand 의 await wait_frames(30) 이 GUT DEPRECATED 경고를 낸다. wait_physics_frames(30) 으로 바꿔라

## [x] G-241 꿀벌 — 야생 벌집 · 여왕벌 · 벌통 창 · 곰과 오소리의 습격
- 차선: 3d-start · 시작 2026-10-18 23:18
- 순서: G-240 뒤
- spec: spec/04_life/ranching-breeding.md, spec/05_craft/facilities.md
- **사람 결정 2026-10-18** (원문): 「꿀벌을 보이게 만들진 않고 그냥 개체수로만 나오게 함. 야생 꿀벌집을 털면 꿀이랑 밀랍이 나오고 낮은 확률로 여왕벌이 나옴. 밀랍으로 벌집을 만들 수 있음. 벌집에 여왕벌을 넣으면 알아서 개체수가 늘어남. 벌집도 ui를 따로 만들어서 개체수가 보여지고 꿀도 계속해서 생산되는 빈칸들을 만듦 … 여왕벌의 생사도. 개체수가 100%에 가까워질수록 꿀의 생산 속도는 빨라지지만 … 여왕벌이 죽는다면 개체수는 바로 0」
  - 사람이 받았다(「그게 좋겠다」): **온도는 뺀다** · **개체수는 100% 까지만 늘고 넘지 않는다** · 위협은 바깥에서 — **곰 · 오소리가 벌통을 턴다**(실제로도 그렇다) → 개체수가 확 줄고 낮은 확률로 여왕벌이 죽는다 · 그래서 벌통 곁에 경비 시설(G-244)을 둘 이유가 생긴다
- 이 세션이 2026-10-18 봤다: 벌집(beehive)은 이미 설치물 — stations.json:28 `yields beeswax 60초 · room farm`(농사장 방 안에서만 밀랍 — 사람 결정 2026-10-12, 그대로 둔다) · **꿀(honey)은 레시피에만 있고 나는 곳이 없다**(벌꿀 미끼 · 미드 · 화상 연고 · 기력 강장제)
- [x] 1. 야생 벌집 — 꿀 · 밀랍 · 여왕벌
  - 기준: 내 섬 나무에 **야생 벌집**이 난다(자리 · 수 **임시** · 재생은 resources-regrowth 길). 털면(때려 부수면) 꿀 · 밀랍(수 **임시**)과 **낮은 확률로 여왕벌**(아이템 · 5% **임시**). 털 때 벌에 쏘인다(작은 피해 **임시**) · 저장 · 멀티 (테스트)
- [x] 2. 벌통에 여왕벌 — 개체수 · 꿀 칸
  - 기준: 벌집 설치물에 여왕벌을 넣으면 **개체수가 0% 에서 100% 까지 오른다**(빠르기 **임시**) · 100% 를 넘지 않는다. 여왕벌이 없으면 개체수 0 · 꿀이 안 난다
  - 기준: **꿀은 빈칸에 쌓인다**(칸 수 **임시**) · 생산 빠르기는 개체수에 비례(100% 가까울수록 빠르다 · 식 **임시**) · 칸이 차면 멈춘다 · 꺼내 간다. 밀랍은 지금 규칙(농사장 방) 그대로 (테스트)
  - 기준: **벌통 창** — 개체수 % · 여왕벌 살았나 · 꿀 칸 · 여왕벌 넣는 칸. ui_shots 에 자리 하나 · 번역 키 ko · en · 저장 · 멀티 (테스트)
  - 기준: (앞 단계 QA) 재생 표에 야생 벌집 줄이 없다 — spec/03_world/resources-regrowth.md 의 재생 표(약초 줄 아래)에 「야생 벌집 | O — 게임 시간 며칠 (임시 5일, game/life/gathering/wild_hives.gd REGROW_DAYS) | 턴 자리에 다시 매달린다 · 베인 나무에는 안 난다」 한 줄을 더해라. 이 묶음 4단계(spec)의 기준에 같이 넣으면 된다
  - 기준: (앞 단계 QA) 참가자가 쏘여도 「벌에 쏘였다!」 알림이 안 뜬다 — 서버가 대신 칠 때(net_world.gd _strike_hive) 체력은 깎이지만 HIVE_STUNG 한 줄은 혼자 판(_sting_player)에서만 낸다. 참가자 기계에서 ask 를 보낼 때(WildHives.swing 의 ask 길) 또는 서버가 쏘임을 알릴 때 그 사람 화면에 Notice 를 띄우고, test_harvest_on_server 에 참가자 알림을 보는 단언을 더해라
- [x] 3. 곰과 오소리가 벌통을 턴다
  - 기준: 들판의 곰 · 오소리가 벌통 가까이(반경 **임시**) 오면 벌통으로 가서 턴다 — 개체수 −50% · 꿀 칸이 빈다 · **여왕벌이 죽을 확률 10%**(죽으면 개체수 바로 0 · 사람 결정) — 수는 다 **임시**. 털리면 한 줄 알림 · 털린 벌통은 한동안(**임시**) 다시 안 노린다
  - 기준: 털기 전에 쫓거나 잡으면 안 털린다 · G-244 경비 시설이 이 줄기에 있으면 경비 동물이 우는 대상에 든다(없으면 spec 미정 한 줄) (테스트)
- [x] 4. 습격하는 동물은 한 곳의 표로
  - 기준: 벌통을 터는 종은 **표 하나**(예: AnimalKinds 의 RAIDERS 또는 json)에서 읽는다 — 지금은 곰 · 오소리(**임시**). 표에 종을 넣고 빼는 것만으로 습격하는 동물이 바뀐다(코드를 안 고친다) · 사람 결정 2026-10-19 「곰이나 오소리 같은 습격들은 내가 따로 분류 할거라서 일단 습격만 만들어줘」 (테스트: 표에 사슴을 넣으면 사슴이 털고, 곰을 빼면 곰은 안 턴다)
  - 기준: (앞 단계 QA) 털기 길을 실제 걸음으로 본 테스트가 없다 — test_hive_raid.gd 는 길찾기 지도가 없는 땅이라 짐승을 손으로 REACH 안에 옮겨 닿게 한다. walk_to 로 정말 벌통 2m 안까지 걸어가는지(지은 섬 · 벌통이 설치물 몸통이라 길을 막는 경우)는 아무도 보지 않는다. 길찾기가 있는 섬 테스트(tests/automation/villagers 의 실제 섬 받침처럼)에서 오소리를 벌통 10m 곁에 놓고 시간을 흘려 colony.honey == 0 이 되는지 보는 테스트를 더해라 — 못 닿으면 REACH 를 벌통 몸 크기에 맞춰라
- [x] 5. spec
  - 기준: ranching-breeding.md 에 꿀벌 흐름 · 사람 원문 · 「온도는 뺐다 · 100% 상한 · 습격(사람 2026-10-18)」 · facilities.md 벌집 줄을 맞춘다
- [x] 6. (앞 단계 QA) 습격 GIVE_UP 60초가 spec 수치에 없다
  - 기준: 습격 GIVE_UP 60초가 spec 수치에 없다 — game/life/ranching/hive_raid.gd:32 GIVE_UP := 60.0 (노리다 못 닿으면 놓는 시간으로 보인다)이 spec/04_life/ranching-breeding.md 수치의 꿀벌 줄에 안 적혔다. 「습격: … 쫓기면 120초」 뒤에 「· 못 닿으면 60초에 놓는다」를 더하고 코드 주석에 임시 표시가 있는지 본다 (막는 것은 아니다)

## [x] G-242 주민 장비 — 갑옷 · 무기 · 탄약 · 주민 설정 창
- 차선: 3d-lane2 · 시작 2026-10-18 18:53
- 순서: 차선 2 첫 묶음 (차선 1 의 G-239 와 나란히 — 주민 파일만 만진다)
- spec: spec/07_automation/npc-villagers.md, spec/08_combat
- **사람 결정 2026-10-18** (원문): 「주민들도 일 하다가 공격하는걸로 생각했어. 이럼 npc에 장비 넣는거랑 무기 넣는거, 총알 넣는것도 있어야겠네. 그리고 npc들이 탄약같은건 알아서 챙길 수 있게 설정도 되면 좋겠다. npc용 ui를 하나 만들어야 될 것 같아. 일단 만들어보고 내가 설정 해볼게」
- 이 세션이 2026-10-18 봤다: 주민 코드는 automation/villagers/(villager.gd · jobs.gd · hands.gd 든 것) · **주민에게 장비 · 무기 칸이 없다** · 플레이어 갑옷 · 총 · 탄은 player/equipment
- [x] 1. 주민에게 장비 칸
  - 기준: 주민마다 **갑옷 칸**(플레이어와 같은 부위) · **무기 칸 하나** · **탄약 칸**. 입힌 갑옷의 방어력 · 체력이 주민에게 붙는다 · 무기 · 갑옷이 주민 모양에 보인다(코드 모양이면 된다) · 저장 · 멀티 (테스트)
- [x] 2. 주민 설정 창
  - 기준: 주민을 누르면 여는 **주민 창**에 장비 탭 — 가방에서 끌어 넣고 뺀다. 지금 있는 주민 정보(직업 · 숙련도 · 상태)가 있는 창이면 그 창에 탭을 더한다 · ui_shots 자리 · 번역 키 ko · en (테스트)
  - 기준: 설정 — **탄약 알아서 챙기기**(켬/끔 · 기본 켬): 탄이 n 발 아래(**임시**)면 거주지 방 상자에서 맞는 탄을 가져온다 · **싸움에 나서기**(켬/끔 · 기본 켬) (테스트)
- [x] 3. 주민이 싸운다
  - 기준: 「싸움에 나서기」가 켜진 주민은 자기나 곁(반경 **임시**)의 사람 · 주민 · 가축을 공격하는 적대 짐승을 보면 하던 일을 멈추고 무기로 싸운다 — 총이면 탄을 쓴다 · 탄이 없으면 근접 · 무기가 없으면 피한다. 짐승이 죽거나 멀어지면 하던 일로 돌아간다 · 다치면 체력이 준다(지금의 주민 체력) (테스트)
  - 기준: (앞 단계 QA) 우클릭이 다른 상호작용과 겹치는지 테스트가 없다 — Villagers._unhandled_input 이 INTERACT 를 주민에 닿으면 삼킨다. 주민 뒤·곁의 상자 · 문 · 짐승 쓰다듬기가 같은 우클릭을 먼저 받는지(입력 순서) 보는 테스트가 없다. 기준 밖이라 지적만 — 주민을 겨눈 우클릭이 상자를 열지 않고, 주민이 아닌 곳의 우클릭이 그대로 지나가는지 단언하는 테스트를 더해 두면 좋다
- [x] 4. spec
  - 기준: npc-villagers.md 에 장비 · 설정 창 · 싸움 · 사람 원문 날짜. 기본값은 「사람이 해 보고 정한다」
  - 기준: (앞 단계 QA) 체력 0 이면 안 나선다는 단언이 hp 검사를 안 지킨다 — villager.gd _mind_fight 의 둘째 줄에서 `hp <= 0.0 or` 를 빼도 test_at_zero_hp_the_beast_stops_and_the_villager_does_not_fight 가 그대로 통과했다(짐승이 죽은 몸을 안 쫓아 spot 이 애초에 아무것도 못 찾기 때문). 체력 0 인 주민 곁에서 짐승이 다른 주민(mate, join_fights=false)을 물게 하고 assert_false(v.fighting()) 를 거는 경우를 더해라. 기준 밖(쓰러짐 규칙 없음)이라 지적만
  - 기준: (앞 단계 QA) 사람을 지키는 길은 단위 단언뿐이다 — test_it_guards_people 는 VillagerFight.guards(person) 만 본다. Player.GROUP 에 든 몸을 짐승이 provoke 로 쫓게 하고 곁의 주민이 fighting() 이 되는지 보는 경우를 더해 두면 좋다 (막는 것은 아니다 — spot 의 길은 주민 · 가축과 같다)

## [x] G-243 새장 · 사냥새 · 망원경 — 매와 올빼미가 진짜로 사냥한다
- 차선: 3d-lane2 · 시작 2026-10-18 20:48
- 순서: G-239 뒤 · G-242 뒤
- spec: spec/04_life/hunting.md, spec/04_life/ranching-breeding.md
- **사람 결정 2026-10-18** (원문): 「새장은 새 사육하는 곳으로 설치물이며, 안에 먹이랑 물을 넣을 수 있음. 새는 새장 뿐만 아니라 새장을 기준으로 날아다니기도 하고 잠을 자거나 밥,물을 먹을 땐 새장으로 돌아와서 자거나 밥,물을 먹음. 휴식을 취할 때도 새장으로 돌아옴. 새를 호출하면 새장으로 돌아오며, 1분 있다가 알아서 다시 날아다님. 만약 피로도가 50%이상일 경우 그냥 거기서 쉬었다가 나감. 새는 기본적으로 피로도가 80%이상일 때, 새장으로 돌아와서 쉼. 새장에 있는 상태에서 새장을 캐면 해당 새가 있는 상태로 나옴」
  - 「매 같은 경우는 키우는 것도 잡는것도 어렵게 만들거라 중형동물로 취급하고 중형동물부터 소형동물까지 범위 내에 동물을 잡아오는 방식 … 사냥을 진짜로 하게 하는거지. 지금 도축 시스템도 있으니까 플레이어가 지정하면 새가 날아가서 공격을 하는거야. 지정하는건 망원경을 제작해서 망원경은 배율로 가까이부터 멀리 마우스휠로 조절이 가능하고 총 쏘듯이 좌클릭을 누르면 공격명령이 되는거야. 해당 동물이 선제 공격이라면 싸우는거고 아니면 일방적인 공격이 되겠지. 새가 담긴 새장을 여러개 두고 여러마리 풀어서 사냥하게 해도 돼」 · 올빼미 「밤에 사냥이 가능하며, 매와 똑같이 새장을 사용함」
  - 망원경 세부는 사람이 이 세션에 맡겼다(「너가 선호하는데로」): 휠 1~8배 · 좌클릭 공격 · 우클릭 불러들이기 · 겨눈 짐승 테두리 초록(잡을 수 있음) / 빨강(못 잡음) · 반경 안 풀린 새가 다 간다 · 잡으면 새가 그 자리에서 주검을 지키고 사람이 가서 도축 · 호저를 노린 새는 가시에 다친다 · 고슴도치는 몸을 말아 잘 안 들어간다
- 이 세션이 2026-10-18 봤다: 스코프 배율 계산이 있다(armor_modules.gd:248 `zoom_of` · :253 `scoped_fov`) · 잡은 짐승은 가방 칸(captives.gd) · 도축은 그 자리(butchery.gd)
- [x] 1. 새장 — 매 · 올빼미를 기른다
  - 기준: 설치물 **새장**(작업대 · 재료 **임시**) — 잡은 매 · 올빼미(가방의 captive)를 넣는다 · 한 마리 · 먹이 · 물 칸. 새는 새장 둘레(반경 **임시**)를 날아다니고, 먹을 때 · 잘 때(올빼미는 낮 · 매는 밤) · **피로도 80% 이상**이면 새장으로 돌아와 쉰다(사람 결정)
  - 기준: **부르기**(새장을 누르면) — 새장으로 돌아와 1분 뒤 다시 날아간다 · 그때 **피로도 50% 이상이면 쉬었다가 나간다**(사람 결정) · **새가 든 새장을 캐면 새가 든 채로 아이템이 된다** · 먹이 · 물이 비면 G-238 처럼 굶는다(사흘) · 새장 창(피로도 · 배고픔) · 저장 · 멀티 (테스트)
- [x] 2. 망원경 — 겨누고 명령한다
  - 기준: 아이템 **망원경**(작업대 · 재료 **임시**). 들고 조준하면 마우스휠로 **1~8배**(scoped_fov 를 쓴다). 겨눈 짐승에 테두리 — `AnimalKinds.can_hunt` 로 **초록/빨강**(G-239) · 반경 안 풀린 새 수가 보인다 (테스트)
  - 기준: **좌클릭 = 공격 명령** — 풀린 새 가운데 그 짐승을 잡을 수 있는 새가 다 날아간다(올빼미는 밤에만) · **우클릭 = 불러들이기**. 못 잡는 짐승을 누르면 한 줄 · 번역 키 ko · en (테스트)
  - 기준: (앞 단계 QA) spec 에 새장이 한 줄도 없다 — spec/04_life/ranching-breeding.md(또는 hunting.md)에 current.md 의 사람 결정 2026-10-18 원문(새장 · 80% 귀가 · 부르기 1분 · 50% 쉬기 · 새가 든 채 캐기)과 임시 값(반경 15 m · 피로 8분/3분 · RESTED 20% · 먹으러 120초 · 칸 20 · 재료 목재8+밧줄2), 그리고 「가방 · 상자 속 새장은 시간이 흐르지 않는다」를 적어라. grep -rn 새장 spec 이 지금 비어 있다
  - 기준: (앞 단계 QA) 혼자 하는 판의 손에 든 새장 놓기 테스트가 없다 — Stations.place_held 의 _unpack_cage 는 멀티(place_for)로만 확인됐고 test_bird_cage.gd 는 _unpack_cage 를 곧바로 부른다. 가방 칸에 {item=bird_cage, cage=...} 를 넣고 손에 든 채 place_held() 로 놓아 새가 든 채 서는지 보는 테스트를 더하라
- [x] 3. 새가 사냥한다
  - 기준: 날아간 새가 짐승을 공격한다 — 선제 공격 · 맞으면 덤비는 짐승은 새와 싸우고(새도 다치고 죽을 수 있다), 아니면 일방적으로. **호저는 공격한 새를 다치게 한다** · **고슴도치는 몸을 말아 피해가 준다**(수 **임시**). 사냥할 때마다 피로도가 오른다 (테스트)
  - 기준: 잡으면 새가 **주검 곁에 앉아 지킨다** — 머리 위 표시 · 한 줄 알림. 사람이 가서 도축(지금의 도축 길)하면 새는 새장으로 돌아간다 · 시간이 지나면(**임시**) 그냥 돌아간다 · 멀티 (테스트)
- [x] 4. spec
  - 기준: hunting.md(사냥새 · 망원경) · ranching-breeding.md(새장 · 피로도) · 사람 원문 날짜 · 이 세션이 고른 망원경 세부를 「사람이 맡겼다」로
  - 기준: (앞 단계 QA) test_bird_hunt.gd _send 주석이 낡았다 — 「덮칠 때까지 흘린다 — 덮친 수를 돌려준다」라 적혀 있지만 함수는 send_bird 만 부르고 void 다. 주석을 「이 짐승에게 새를 보낸다」로 고쳐라
- [x] 5. (앞 단계 QA) 옮기며 같이 하는 판 망원경 줄이 빠졌다
  - 기준: 옮기며 같이 하는 판 망원경 줄이 빠졌다 — 옛 ranching-breeding.md 망원경 문단의 「같이 하는 판은 손이 닿지 않아도 서버가 받는다」와 「들면 곧 1인칭 — 우클릭이 불러들이기라 누르는 조준이 아니다」의 까닭이 hunting.md 「사냥새 · 망원경」 세부에 안 옮겨졌다(150줄이라 줄였다). 막는 것은 아니다 — hunting.md 세부 줄 끝 (`life/hunting/telescope.gd`) 앞에 「같이 하는 판은 서버가 받는다」를 붙이고, 줄 수는 이웃 줄을 합쳐 150 안에 맞춰라

## [x] G-244 경비 시설 — 거위가 울고 주민이 싸운다
- 차선: 3d-lane2 · 시작 2026-10-19 00:34
- 순서: G-243 뒤
- spec: spec/04_life/ranching-breeding.md, spec/06_build
- **사람 결정 2026-10-18** (원문): 「경비시설(이름 못정함): 설치물로 반경을 정할 수 있음. 설치물에서 반경은 원을 그려서 볼 수 있음. 경비시설은 따로 ui를 만들고 거기에 팀을 배정해서 팀단위로 지정해 경비를 돌게함. 경비시설도 새장과 마찬가지인 사육시설이라 안에 밥, 물을 넣어줘야함」 · 「동물용으로 사육용으로 만들거라서 주민은 못해. 다른 동물들도 경비가 되는 동물이면 넣을거라 확장성을 둔거고 거위는 일단 울어서 알리기만 하는걸로 … 울으면 거위는 앞에서 울다가 어그로 끌리면 적대 동물은 거위를 공격하려 달려들고 거위는 파닥거리며 도망가는걸 생각했어. 그중에 뭐 적대 동물이 죽이면 거위는 죽는거고 … 경비시설에 늑대 같은걸 넣어서 늑대나 공격성이 있는 동물을 넣으면 바로 달려가서 공격하고 주민들도 일 하다가 공격하는걸로」
  - 이름은 사람이 못 정했다 — 임시 이름 **경비 우리**(번역 키 하나라 바꾸기 쉽게)
- [x] 1. 경비 우리 — 반경 · 팀 · 먹이
  - 기준: 설치물 **경비 우리**(작업대 · 재료 **임시**) · 먹이 · 물 칸(비면 G-238 처럼 굶는다). **반경**은 창에서 정한다(최소 · 최대 **임시**) — 놓을 때 · 창을 열 때 · 겨눌 때 바닥에 원이 보인다
  - 기준: **경비 우리 창** — 넣은 동물 목록 · **팀**(동물을 팀으로 묶어 우리 하나에 배정) · 반경. **동물만** 넣는다(주민 안 됨 · 사람 결정) · 경비가 되는 동물인지는 종 데이터의 역할(**알림**: 거위 · **공격**: 아직 없음 — 늑대 등은 사람이 넣을 때)로 · ui_shots 자리 · 저장 · 멀티 (테스트)
  - 기준: 배정된 동물은 반경 안을 돌며 경비하고(길들임 100 이어야 · 아니면 「길들여야 한다」) 먹을 때 우리로 돌아온다 (테스트)
- [x] 2. 거위가 울고 어그로를 끈다
  - 기준: 적대 짐승(곰 · 맞고 덤비는 중의 멧돼지 · G-241 의 벌통을 노리는 곰 · 오소리)이 반경에 들면 **거위가 그 앞에서 운다** — 소리 · 한 줄 알림 · 짐승의 노림이 거위로 바뀐다(어그로) · 거위는 파닥이며 달아난다 · 잡히면 죽는다(주검 · 도축 길) (테스트)
  - 기준: 역할이 **공격**인 동물은 반경에 든 적대 짐승에게 곧장 달려가 싸운다 — 지금은 테스트용 종으로만 확인한다(늑대는 사람이 넣을 때) (테스트)
  - 기준: (앞 단계 QA) ui_shots 의 시험 땅 만들기가 겹친다 — game/core/debug/ui_shots.gd 의 _guard_pen · _guard_pen_held 가 200×1×200 StaticBody3D 땅 만드는 코드를 그대로 두 번 적었다(다른 자리에도 같은 꼴이 있으면 함께). _shot_ground() 같은 도움 함수 하나로 묶어라 (막는 것은 아니다)
  - 기준: (앞 단계 QA) 팀 번호는 아직 하는 일이 없다 — Animal.guard_team 은 저장 · 동기화 · 창 표시만 되고 경비 행동은 팀과 상관없다. spec/04_life/ranching-breeding.md 에 「팀은 지금 묶기만 한다 — 팀마다 무엇을 하나는 다음 단계」로 적혀 있으니, 그 다음 단계가 이 묶음 목록에 실제로 있는지 확인하고 없으면 spec 「미정」 줄로 옮겨 적어라
- [x] 3. 울음을 들은 주민이 싸운다
  - 기준: 거위가 울면 반경 둘레(**임시**)의 주민 가운데 「싸움에 나서기」가 켜진 주민(G-242)이 하던 일을 멈추고 그 짐승에게 가서 싸운다 · 끝나면 돌아간다 (테스트)
  - 기준: (앞 단계 QA) _detour_house 의 머리 주석이 밀려났다 — game/core/debug/ui_shots.gd 에서 _shot_ground 를 _detour_house 바로 위에 끼우면서 원래 _detour_house 의 주석 「## 판 plate 부터 size 판만큼 토대 · 둘레 벽 · 지붕을 놓아 막은 건물 (문 없음) — 바닥 높이를 돌려준다.」가 _shot_ground 주석 위에 남았고 _detour_house 는 주석이 없다. 그 한 줄을 func _detour_house 바로 위로 옮겨라
  - 기준: (앞 단계 QA) 오소리 · 성난 멧돼지가 거위를 쫓는 끝까지는 테스트가 없다 — test_guard_watch.gd 는 오소리 · 멧돼지를 threatens() 로만 보고, 울기 · 어그로 · 물려 죽기까지 실제로 도는 것은 곰 하나다. 오소리는 적대가 아니라 _foe/_rage 길(fierce 가 성으로 켜진다)을 타므로 길이 다르다. QA 탐침(오소리 4.2 m · 거위 체력 3 · 6초)으로는 울고 죽었다. 같은 꼴로 test_a_badger_is_drawn_by_the_goose 를 더해 cry_count>0 · badger.foe()==goose · goose.is_carcass() 를 단언해라
- [x] 4. spec
  - 기준: ranching-breeding.md(또는 새 파일)에 경비 우리 · 역할 · 사람 원문 날짜 · 「이름은 사람이 정한다」

## [x] G-916 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-start · 시작 2026-10-19 01:57
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. weights.json 의 새 captive_* 줄이 차례에서 벗어났다 (G-239 단계 1)
  - 기준: weights.json 의 새 captive_* 줄이 차례에서 벗어났다 — captive_angora_rabbit … captive_porcupine 열 줄이 captive_bear 앞에 몰려 있어 이름 차례(captive_badger · captive_bear · captive_boar …)가 깨졌다. 다른 줄처럼 이름 차례로 끼워 넣어라 (막는 것은 아니다)
- [x] 2. 날렵한 주민이 문짝에 2.5초 처박힌다 (G-239 단계 6)
  - 기준: 날렵한 주민이 문짝에 2.5초 처박힌다 — 구현 세션이 찾았다: 날렵함(NIMBLE ×1.15) 주민이 G-218 양보 장면에서 문을 엇갈리면 열린 문짝에 2.53초 비빈다. give_way 테스트는 특성을 비워 피했을 뿐 게임 쪽 흠은 그대로다. test_villager_g218_give_way.gd 에 v.traits = [Persona.NIMBLE] 를 박은 판을 하나 더 두고(비빈 시간 < 0.6초 단언) 문 엇갈림/양보 코드를 걸음 빠르기에 상관없이 비키게 고쳐라
- [x] 3. 벌집 치기에 휘두르기 간격이 없다 (G-241 단계 1)
  - 기준: 벌집 치기에 휘두르기 간격이 없다 — WildHives.swing 은 좌클릭마다 곧바로 한 대라 연타로 순식간에 턴다(평타 · 캐기는 도구 휘두르기 빠르기를 따른다, tests/combat/tools_as_weapons/test_fire_rate.gd). 같은 간격을 걸지 사람이 볼 일이 아니면 Attack 의 휘두르기 간격을 재사용해 막고 테스트로 확인해라
- [x] 4. (앞 단계 QA) 벌집 간격이 주민 상수를 빌린다
  - 기준: 벌집 간격이 주민 상수를 빌린다 — life/gathering/wild_hives.gd 의 SWING_EVERY 가 automation/villagers/harvest_work.gd 의 HIT_EVERY 를 가리켜 생활 쪽이 주민 자동화에 기댄다. 사람 휘두르기 간격이 정해지면 Attack(또는 도구 데이터) 쪽 한 곳에 두고 벌집 · 주민이 그것을 읽게 옮겨라
- [x] 1. 주민 테스트 주석이 HIT_EVERY 를 사람 빠르기라 부른다 (G-916 단계 4)
  - 기준: 주민 테스트 주석이 HIT_EVERY 를 사람 빠르기라 부른다 — tests/automation/villagers/test_villager_gather_close.gd:158 · 168 · 181 이 사람 쪽 셈에 HarvestWork.HIT_EVERY 를 쓴다(값은 같아 막지 않음). 사람 몫(person 계산 · 158줄 주석)은 Attack.SWING_EVERY 로 바꾸고 주민 몫(181줄)만 HarvestWork.HIT_EVERY 로 두어라

## [x] G-245 동물이 나오는 기믹 — 바위 속 굴 · 나무 위 담비 · 올빼미 나무 · 디스폰
- 차선: 3d-lane2 (3d-start 몫이었는데 시작 전이라 가져왔다) · 시작 2026-10-19 02:17
- 순서: G-241 뒤
- spec: spec/04_life/hunting.md, spec/04_life/mining.md
- **사람 결정 2026-10-18** (원문): 「오소리: 굴에 살고 있음 ← 맵에 구현하기 힘드니 바위를 캐면 낮은 확률로 구멍이 나옴. 굴에 물을 부울 수 있고 그럼 오소리 가족 엄마1 아빠1 새끼1 이렇게 나옴. 빠르게 도망은 감 … 도망가다가 한대 맞으면 공격」 · 「담비: 3마리 같이 다님(성별 랜덤). 아무대나 돌아다니며 도망치다가 나무를 발견하면 나무 위로 재빠르게 도망감. 나무를 베면 떨어진다는 가정으로 다시 나옴」 · 「올빼미: 혼자다님. 나무에 랜덤으로 올빼미가 나올지 말지 결정이 됨 … 낮에 나무를 베면 나옴. 밤에는 사냥하고 나무에 다시 돌아옴. 그 나무를 한대라도 치면 올빼미는 도망침. 배회 하다가 플레이어가 나무 영역 밖으로 나가면 다시 돌아옴」 · 「앙고라토끼: 바위를 캐다가 낮은 확률로 5마리의 토끼가 나옴. 엄마 아빠 새끼 새끼 새끼. 무조건 도망감」 · 「호저: 혼자. 바위를 캐다가 낮은 확률로 나옴」
  - 사람이 답했다(2026-10-18): **오소리는 가족이 다 같이 덤빈다** · **담비가 나무에 오르면 무조건 나무를 베어야 한다 — 떨어져 기절** · **올빼미는 나무가 집** · **모든 동물이 똑같이, 눈에 안 보이면 디스폰**
  - 사람이 답했다(2026-10-19): **굴은 바위를 깰 때 0.5% · 무엇이 나올지는 무작위 · 굴은 다 물을 부어야 나온다** · 멧돼지 · 꿩 · 매 · 거위는 **자연 스폰**(지금처럼 집 자리) · 습격하는 동물은 사람이 따로 분류한다(G-241 은 습격 장치만)
  - 이 세션이 다듬고 사람이 받았다: 동물은 **바위가 다 부서질 때 한 번** 굴린다(G-234 희귀 광물은 갈라질 때마다 — 겹치지 않게) · **플레이어가 캘 때만**(광부 주민이 캔 바위에선 안 나온다) · 호저는 가시를 세우고 달그락 경고 → 뒤로 돌진(실제 습성 · 가시를 쏘지는 못한다) · 디스폰은 **나온 지 게임 하루가 지났고 모든 플레이어에게서 멀 때**(바로 없애면 뒤만 돌아도 사라진다 · 거리로만 본다) · 올빼미 나무 밑에 **펠릿**(찾는 실마리)
- 이 세션이 2026-10-18 봤다: 들판 짐승은 집 자리(island_animals.gd · G-239 의 wild_dens)에 나고 하루가 넘어가면 빈 집을 채운다(:13 · :121 next_day) · 바위 부수기는 life/harvest/harvest.gd `_break()` · 주민 광부는 mining_work.gd 같은 길
- **이번에 만들지 않는 것**: 멧돼지 · 꿩 · 매 · 거위의 습성(사람이 따로 말한다) · 야생끼리 실제 사냥
- [x] 1. 바위 속 굴 — 오소리 · 앙고라토끼 · 호저
  - 기준: 플레이어가 바위를 **다 부술 때** **0.5%** 로 굴린다(사람 결정 2026-10-19 「정말 낮은 확률 0.5%」) — 붙으면 **오소리 · 토끼 · 호저 가운데 무작위로 하나**(사람 「랜덤하게 굴이 나오게」 · 바위 자리로 가르지 않는다 · 셋의 무게는 같게 **임시**). 주민이 캔 바위는 안 굴린다 · 서버가 굴린다 · 오소리 · 토끼 · 호저는 **집 자리에서 더는 나지 않는다**(G-239 를 바꾼다) (테스트)
  - 기준: **굴은 셋 다 같다** — 붙으면 바위 자리에 **굴**이 남고, **물뿌리개로 물을 부어야 나온다**(사람 결정 2026-10-19 「오소리 뿐만 아니라 굴은 다 물을 뿌려야 나오는거야」). 무엇이 든 굴인지는 물을 붓기 전엔 모른다(**임시**) · 물을 안 부은 굴은 하루 뒤(**임시**) 메워진다 · 저장 · 멀티 (테스트)
  - 기준: **오소리** — 물을 부으면 **엄마 · 아빠 · 새끼** 셋이 나와 빠르게 달아난다 · **한 마리라도 맞으면 셋이 다 덤빈다**(사람 답) (테스트)
  - 기준: **앙고라토끼** — 물을 부으면 **엄마 · 아빠 · 새끼 셋** 다섯이 튀어나와 흩어져 달아난다 · 무조건 달아난다(덤비지 않는다) (테스트)
  - 기준: **호저** — 물을 부으면 혼자 나온다 · 건드리지 않으면 천천히 걷는다 · 다가가면(반경 **임시**) 가시를 세우고 달그락 소리로 경고 · 더 다가가면 **뒤로 돌진**해 찌른다(피해 **임시**) (테스트)
  - 기준: 새끼는 G-238 의 나이로 — 작고 도축 산출이 적고(**임시**) 다 자라기 전엔 번식하지 않는다 · 저장 · 멀티 (테스트)
- [x] 2. 담비 — 나무 위로 달아난다
  - 기준: 담비는 **셋이 같이** 다닌다(성별 무작위 · 사람 결정) · 쫓기면 달아나다 가까운 나무(반경 **임시**)를 찾으면 **셋이 같은 나무로** 재빨리 오른다 (테스트)
  - 기준: 나무 위 담비는 **총 · 근접 · 마취 · 사냥새 무엇으로도 안 맞는다** — **나무를 베어야 한다**(사람 답). 나무가 쓰러지면(지금은 사라진다 — 그 순간 밑동에 놓인다) 떨어져 **기절**(몇 초 **임시** · 머리 위 표시) — 그동안 마취 · 잡기 · 공격이 된다 · 깨면 다시 달아난다 · 멀티 (테스트)
- [x] 3. 올빼미 나무 — 나무가 집
  - 기준: 나무가 날 때 낮은 확률(**임시**)로 **올빼미 나무**가 된다 — 밑에 **펠릿**이 보인다(줍지 못하는 표시 — 쓰임은 사람이 정할 때). G-239 의 「올빼미는 밤에만 집 자리에 난다」를 이것으로 바꾼다 (테스트)
  - 기준: **낮** — 올빼미는 그 나무 안에서 잔다. 그 나무를 **한 대라도 치면** 날아올라 달아나 둘레를 돌아다니고, 플레이어가 나무 둘레(반경 **임시**) 밖으로 나가면 집으로 돌아간다 (테스트)
  - 기준: **밤** — 집을 비우고 돌아다니다(사냥은 이번에 안 만든다) 새벽에 돌아온다 · 집 나무가 베여 없어지면 떠돌다 디스폰된다(단계 4) · 저장 · 멀티 (테스트)
  - 기준: (앞 단계 QA) 새가 쫓던 중 나무에 오르면 포기하는 길이 테스트가 없다 — craft/stations/stations.gd _follow_prey 에 `or prey.treed()` 를 더해 새장 새가 give_up 하게 했는데, 이 줄을 지켜 주는 테스트가 없다(피해는 take_hit 이 0 으로 막아 기준은 지켜진다). tests/life/hunting/test_bird_hunt.gd 에 '매를 땅 위 담비에게 보낸 뒤 담비를 나무에 올리면(_perch 또는 hear 로 오르게) tick_cages 다음에 cage.hunting() 이 거짓이고 담비 체력이 그대로' 를 단언하는 테스트를 하나 더해라
- [x] 4. 디스폰 — 모든 동물이 똑같이
  - 기준: 내 것이 아닌 짐승은 **나온 지 게임 하루가 지났고 모든 플레이어에게서 멀 때**(거리로만 본다 — 60 m **임시** · 멀티에서 카메라 방향을 서버로 모으지 않으려고) 조용히 없어진다 — 모든 종이 같다(사람 결정). 눈앞에서는 없어지지 않는다 (테스트)
  - 기준: 안 없어지는 것 — 마취된 것 · 가방 · 케이지 · 목줄 · 우리 · 온순함이 0 보다 큰 것 · 사냥새가 지키는 주검 (테스트)
  - 기준: 들판 짐승이 없어진 집 자리는 지금의 재생(next_day)으로 다시 찬다 · 올빼미는 집 나무가 남았으면 그 나무에 다시 난다 · 저장 · 멀티 (테스트)
  - 기준: (앞 단계 QA) 옛 저장본의 집 번호가 하나 당겨진다 — island_blueprint.gd wild_dens 에서 owl 을 지워 뒤의 hedgehog 집 번호가 하나 앞으로 왔다. 옛 저장본의 「오늘 비운 집」 번호가 하루 동안 다른 집을 가리킬 수 있다(하루 뒤 저절로 맞아 막는 것은 아니다). spec/01_settings/save.md 의 옛 저장본 줄이나 hunting.md 에 한 줄 적어 두거나, 집 번호 대신 종+자리로 담도록 바꿔라
  - 기준: (앞 단계 QA) 밤에 나와 있는 올빼미를 치는 길에 테스트가 없다 — island_animals.gd _on_struck 의 `elif _lives_at(i): flee_from(...)` 가지를 꺼도 test_owl_trees.gd 가 지키지 않는다. 22시에 나온 올빼미 곁에서 harvest.strike 한 뒤 mode()==Animal.FLEE 를 단언하는 테스트를 더해라
- [x] 5. spec
  - 기준: hunting.md 에 기믹 · 디스폰 · 사람 원문 · 날짜 · 이 세션이 다듬은 것(바위 자리 · 한 번 굴림 · 플레이어만 · 호저 경고 · 게임 하루 + 안 보일 때 · 펠릿) · mining.md 에 바위에서 동물이 나온다 한 줄
  - 기준: (앞 단계 QA) hunting.md 가 딱 150줄이다 — 이번에 디스폰 · 집 번호 설명을 기존 줄 끝에 길게 이어 붙여 줄 수를 맞췄다(:42 · :45 가 한 줄에 여러 문장). 다음에 한 줄만 더해도 넘는다. 디스폰 · 재생 단락을 spec/04_life/ 아래 따로 파일(예: animal-spawn.md)로 떼고 hunting.md 에서 가리키게 해 줄을 비워 두어라
  - 기준: (앞 단계 QA) Stations.guarded 가 남의 _prey 를 들여다본다 — game/craft/stations/stations.gd guarded() 가 다른 Stations 노드의 비공개 사전 here._prey 를 읽는다. Stations 에 prey_of(s) 같은 공개 함수를 두고 그것을 쓰게 바꿔라 (동작은 같다)
- [x] 6. (앞 단계 QA) spec 가리킴이 낡았다 (흔적 · 디스폰)
  - 기준: spec 가리킴이 낡았다 (흔적 · 디스폰) — 흔적 · 디스폰 · 재생 단락이 animal-spawn.md 로 옮겨졌는데 아직 hunting.md 「흔적」 · 「디스폰」을 가리키는 곳이 남았다: game/life/hunting/animal_traces.gd:3 · game/life/hunting/animal_trace.gd:3 · game/tests/multiplayer/test_traces_on_server.gd:3 · game/i18n/ko.po:699(「흔적」), game/tests/multiplayer/test_despawn_on_server.gd:2(「디스폰」), game/tests/settings/save/test_round_trip.gd:64 · game/tests/craft/test_from_empty_hands.gd:1490(「다시 난다」). 주석의 spec/04_life/hunting.md 를 spec/04_life/animal-spawn.md 로 바꿔라. grep -rn 'hunting.md' game | grep '흔적\|디스폰\|다시 난다' 결과가 비어야 한다

## [x] G-917 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-lane2 · 시작 2026-10-19 06:38
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. 올빼미 밤 배회 테스트가 흔들린다 (G-245 단계 6)
  - 기준: 올빼미 밤 배회 테스트가 흔들린다 — 앞 전체 실행에서 game/tests/life/hunting/test_owl_trees.gd:183(밤에 10초 안에 4m 넘게 돌아다닌다)이 떨어졌고 이번 전체 실행에선 통과했다. godot 6개를 같이 돌릴 때 실제 시간 10초 대기가 모자라는 것으로 보인다. 실제 시간을 기다리지 말고 물리 프레임 수(또는 Engine.time_scale · 직접 _physics_process(delta) 부르기)로 게임 시간을 밀어 같은 단언을 하게 고쳐라 — 부하와 상관없이 같은 결과가 나와야 한다
- [x] 2. (앞 단계 QA) 올빼미 테스트에 실제 시간 대기가 셋 남았다
  - 기준: 올빼미 테스트에 실제 시간 대기가 셋 남았다 — game/tests/life/hunting/test_owl_trees.gd:155 · :156 · :169 가 아직 wait_until(…, 2.0/6.0/6.0) 으로 실제 시간을 기다린다. 부하 때 :183 과 같은 까닭으로 흔들릴 수 있다(막는 것은 아니다 — 이번 기준은 :183 하나). 같은 파일의 _frames_until(until, seconds) 로 바꿔 게임 시간으로 기다리게 하라 — 단언 문장 · 거리 · 초는 그대로 두고

## [x] G-246 휘두르기 간격 — 도구마다 다르게
- 차선: 3d-start · 시작 2026-10-19 11:19
- spec: spec/08_combat/tools-as-weapons.md
- **사람 결정 2026-10-19**: 도구마다 휘두르기 간격을 다르게 한다 — 이 세션이 임시 수를 잡고, 사람은 「일단 그렇게 해놓고 이상하거나 너무 밸런스 안맞으면 말 할게」 (decisions.md 「평타 · 캐기에 휘두르기 간격이 없다」의 답)
- 이 세션이 2026-10-19 봤다: 지금은 모든 휘두르기가 한 값 — combat/tools_as_weapons/attack.gd:69 `SWING_EVERY := 0.6`(G-916.4 임시) · 주민 HarvestWork.HIT_EVERY · 야생 벌집 WildHives.SWING_EVERY 가 이것을 읽는다 · 도구 목록은 tools.json(필드를 더하면 코드를 안 고쳐도 된다)
- [x] 1. 도구마다 간격
  - 기준: tools.json 도구에 **swing**(초) 필드 — 임시 수: 맨손 0.4 · 칼(knife) · 단검 0.4 · 도끼 · 곡괭이 · 검 0.6 · 망치 · 톱 0.7 · 창 0.8 · 전투 도끼 1.0 · 안 적힌 도구 0.6. 평타 · 캐기 · 야생 벌집 치기가 **든 도구의 swing** 을 따른다 · 주민도 든 도구로 같은 간격 (테스트: 도끼와 전투 도끼로 1초 동안 누른 대 수가 다르다)
  - 기준: spec tools-as-weapons.md 에 간격 표 · 「수는 임시 — 사람이 해 보고 말한다(2026-10-19)」
- [x] 2. (앞 단계 QA) 주민 간격 기록에 낡은 이름이 남았다
  - 기준: 주민 간격 기록에 낡은 이름이 남았다 — spec/07_automation/npc-villagers-history.md:108 이 없어진 HIT_EVERY 와 「좌클릭 Harvest.swing 에는 쿨다운이 없어」를 적고 있다. 기록 줄이면 그대로 두되 끝에 「G-246.1 부터 캐기도 든 도구 간격에 막힌다 · HIT_EVERY 는 HarvestWork.hit_every() 로 바뀌었다」 한 줄을 덧붙여라
  - 기준: (앞 단계 QA) 주민 간격 테스트가 약하다 — test_a_villager_hits_at_the_pace_of_the_tool_in_hand 는 HarvestWork.TOOL 이 상수 "" 라 hit_every 가 0.4 고정값을 돌려줘도 통과한다. 주민에게 도구를 건네는 길이 생기면(TOOL 을 var 로) 도끼 · 전투 도끼를 쥐여 hit_every 가 0.6 · 1.0 으로 바뀌는지 단언을 더하라

## [x] G-918 루프가 찾은 것 — 스스로 갚는다
- 차선: 3d-start · 시작 2026-10-19 12:14
- QA 가 통과시키면서 찾은 것이다. **물어볼 것이 아니면 여기로 온다** (사람 결정 2026-09-22).
  루프가 제 손으로 갚는다 — 사람은 순서를 바꾸고 싶을 때만 손댄다
- 기능을 더하지 않는다. 지키는 것이 없던 자리에 지키는 것을 넣는 일이다
- [x] 1. 주민 tool 이 저장 안 담는다 목록에 없다 (G-246 단계 2)
  - 기준: 주민 tool 이 저장 안 담는다 목록에 없다 — harvest_work.gd 의 var tool 은 게임 안에서 아무도 바꾸지 않아 늘 "" 이라 저장하지 않는다고 주석에만 적혔다(지금은 실제 상태가 아니라 테스트 이음새라 막는 것은 아니다). spec/01_settings/save.md 원칙대로 game/tests/settings/save/test_round_trip.gd 의 DONT_SAVE 와 save.md 38~40줄 「안 담는다」 목록에 「주민 HarvestWork.tool — 건네는 길이 없어 늘 맨손, 길이 생기면 그 길이 담는다」를 더하라

## [x] G-247 나무 약점이 보이게
- 차선: 3d-start · 시작 2026-10-19 14:26
- spec: spec/04_life/logging.md
- **사람이 2026-10-19 봤다**: 「나무 크리티컬 위치를 모르겠음. 표시가 안되있는듯함」 — 이 세션이 「플레이어 쪽에 · 크고 밝게」를 내놓았고 사람이 「그대로」
- 이 세션이 2026-10-19 봤다: 표시는 있다 — life/harvest/harvest.gd `_mark` 의 주황 고리(지름 ≈0.5m). 도끼로 처음 친 뒤에만 뜨고 줄기 둘레 **아무 방향**(`angle` 무작위)이라 뒤쪽에 뜨면 안 보인다
- [x] 1. 약점은 친 사람 쪽 반원에
  - 기준: 약점이 뜰 때 · 크리티컬로 옮길 때 **친 사람이 선 쪽 반원**(줄기에서 그 사람을 향한 방향 ±90°) 안에 뜬다. 조준 없는 주민이 친 나무는 지금처럼 아무 방향 (테스트: 나무 둘레 네 방향에서 쳐 약점이 늘 친 쪽 반원에 있다)
  - 기준: 고리를 지금보다 크고 밝게 — 수는 **임시** (예: 지름 0.5m → 0.7m, 은은하게 깜박이는 빛). 약점 둘레 맞히는 반지름(WEAK_RADIUS)은 고리에 맞춰 같이 고친다
  - 기준: ui_shots 또는 shots 에 약점이 뜬 나무를 플레이어 자리에서 본 한 장 — 고리가 화면에 보인다
- [x] 2. (앞 단계 QA) 반원 가장자리에선 고리가 옆모습이다
  - 기준: 반원 가장자리에선 고리가 옆모습이다 — 약점 방향이 친 사람 쪽 ±90° 안에서 고르게 뽑혀, 끝 가까이(±70~90°)면 고리 축이 시선과 거의 직각이라 얇은 세로 막대로 보인다. QA 가 찍은 tree_weak_spot.png 에서도 고리가 줄기 왼쪽에 납작한 타원으로 섰다. 보이긴 해서 기준은 통과지만, harvest.gd _move_weak 의 from 갈래 범위를 ±60° 쯤으로 좁히거나(spec/04_life/logging.md 의 「±90°」 줄도 같이) _mark 에서 고리 축을 줄기 바깥과 친 사람 방향 사이로 기울이면 더 잘 보인다. 고치면 test_the_weak_spot_rises_and_moves_on_the_strikers_side 의 _faces 를 각도 단언으로 바꿔라

## [x] G-248 야생동물 소환 (개발용)
- 차선: 3d-start · 시작 2026-10-19 15:14
- spec: spec/01_settings/world-settings.md
- **사람 결정 2026-10-19**: 「동물 테스트 해볼 수 있게. 야생동물 소환 하는 것도 만들어줘」 — 이 세션이 「F5 · 목록 창 · 개발용 월드만」을 내놓았고 사람이 「그대로」
- 이 세션이 2026-10-19 봤다: 디버그 키는 core/debug/debug_tools.gd 의 F1 ~ F4 (F4 = 주민 세우기, 개발용 월드만) · 무리 짓기는 IslandAnimals.spawn_family
- [x] 1. F5 동물 목록
  - 기준: F5 → 야생동물 전부(AnimalKinds 의 종, 곰 · 사냥새 포함)가 든 목록 창. 하나 고르면 캐릭터 앞에 선다 — 무리로 다니는 종(오소리 가족 · 담비 셋 · 토끼 다섯 등)은 무리째. **개발용 월드에서만** (F4 와 같은 막음) · 출시 빌드에 붙지 않는다
  - 기준: 소환한 동물은 **디스폰되지 않는다**(G-245 의 하루 · 60m 규칙에서 빠진다) · 저장본에는 다른 야생동물과 같이 담긴다 (테스트: 소환 → 하루 지나고 멀리 가도 남는다)
  - 기준: 디버그 키 목록 주석 · spec 에 F5 한 줄 · ui_shots 에 목록 창 한 장
- [x] 2. (앞 단계 QA) 안 담는다 목록 까닭이 소환 짐승을 못 덮는다
  - 기준: 안 담는다 목록 까닭이 소환 짐승을 못 덮는다 — game/tests/settings/save/test_round_trip.gd:74 의 「Island/Animals」 까닭은 「섬이 설 때마다 집 자리에서 새로 나는 짐승의 몸」인데, F5 로 불러낸 짐승(Animal.summoned)은 집이 없어 다시 나지 않고 껐다 켜면 그냥 사라진다. 까닭 문자열에 「개발용 F5 로 불러낸 짐승의 몸도 담지 않는다 — 다시 들어오면 없다 (G-248.1)」를 덧붙이고, spec/01_settings/save.md 의 「안 담는다」 쪽 들판 짐승 줄에도 같은 한 줄을 적어라

## [x] G-249 주민 소지품 · 일시중지 · 부르기
- 차선: 3d-start · 시작 2026-10-19 16:24
- spec: spec/07_automation/npc-villagers.md
- **사람 결정 2026-10-19**: 「주민 ui에서 장비에 주민들이 가지고 있는 소지품도 있지 않나 ? … 넣고 빼는건 플레이어와 가까이 있을 때만 가능. 왜냐하면 원격으로 되버리면 원거리 인벤토리가 되어버림 … Npc작업 일시중지랑 재개 하는것도 만들고 플레이어 한테 오게 하는것도 만들어야 할듯」 — 이 세션의 안(보기는 어디서든 · 넣고 빼기는 4m 임시)에 사람이 「그대로」
- 이 세션이 2026-10-19 봤다: 소지품은 있다 — automation/villagers/hands.gd `VillagerHands`(6칸, 방 상자에서 꺼낸 것 · 벤 나무 · 캔 돌). 주민 창(ui/hud/villager_window.gd)에는 안 보인다 · 일시중지 · 부르기는 없다
- [x] 1. 장비 탭에 소지품
  - 기준: 주민 창 장비 탭에 「소지품」 6칸(손의 칸 그대로). **보기는 어디서든**. 넣고 빼기(끌어 놓기 · 클릭)는 그 주민이 플레이어 곁 **4m(임시)** 안일 때만 — 멀면 칸이 회색 · 「가까이 가야 한다」. 장비 칸도 같은 거리 규칙인지는 지금 규칙을 보고 맞춘다(다르면 spec 미정에 한 줄)
  - 기준: 옮길 때 한쪽에서 빠진 만큼만 다른 쪽에 든다(Hands 의 규칙) · 멀티: 서버가 거리를 본다 (테스트: 3m 에서 빼짐 · 10m 에서 막힘)
- [x] 2. 일시중지 · 재개 · 부르기
  - 기준: 주민 창에 「일시중지 / 재개」 단추 — 멈추면 하던 일을 그 자리에서 멈추고 선다(손은 그대로). 배고픔 · 목마름 · 잠 · 스트레스는 계속 흐른다. 재개하면 하던 일로 돌아간다. 멈춤은 저장본에 담긴다
  - 기준: 「부르기」 단추 — 하던 일을 멈추고 플레이어에게 걸어와 곁에 서서 기다린다(= 일시중지 상태). 플레이어가 움직이면 따라오지는 않는다(임시). 재개하면 일하러 간다 (테스트: 부르기 → 플레이어 곁 2m 안에 닿는다)
  - 기준: ui_shots 에 소지품 칸 · 단추가 보이는 장비 탭 한 장 (villagers_gear 갱신)

## [x] G-250 스트레스 알림 — 한 번씩 · 가장 높은 주민
- 차선: 3d-start · 시작 2026-10-19 17:46
- spec: spec/07_automation/npc-villagers.md
- **사람이 2026-10-19 봤다**: 「하람 스트레스 50 가온 스트레스 50 하람 스트레스 50 가온 스트레스 50 총 4줄로 2개씩 나왔다」
- **사람 결정 2026-10-19**: 「스트레스는 주민마다 넣으면 ui상 안좋거든 ? 플레이어가 계속 보는 메인 화면에는 간단한 정보들만 보이게 해야돼. 그냥 스트레스 아이콘 옆에 가장 스트레스가 높은 애를 보여주면 될 것 같아」
- 이 세션이 2026-10-19 봤다: 사람 저장본(1790014507_e428c509) 주민은 하람 · 가온 둘, 둘 다 스트레스 100 · 배고픔 0 · 목마름 0. 경고 줄 이름이 `stress:<인스턴스 id>:<문턱>`(villager.gd stress_alert_id) — 주민이 새로 세워지면(불러오기 등) id 가 바뀌어 옛 줄이 남은 채 새 줄이 붙을 수 있다(짐작 — 단계 1 이 원인을 찾는다)
- [x] 1. 같은 주민 같은 문턱은 한 줄
  - 기준: 같은 줄이 둘 붙는 원인을 찾아 고친다 — 줄 이름이 인스턴스가 아니라 주민(name_key)에 묶인다. 사람이 본 차례(새 판 · 불러오기 · 하루 지남 중 무엇)를 테스트로 다시 만들어 줄이 주민마다 하나다
- [x] 2. 메인 화면엔 가장 높은 주민 하나
  - 기준: HUD 에 스트레스 아이콘 + 「가온 100」처럼 **가장 스트레스가 높은 주민 하나**의 이름 · 값. 주민이 없으면 숨는다. 오른쪽 위 알림 목록에 주민마다 · 문턱마다 쌓이던 스트레스 경고 줄은 이 표시로 대신한다(넘을 때 한 번 알림은 남겨도 된다 — 줄이 쌓이지 않게)
  - 기준: 자리는 배고픔 · 목마름 고리 곁 등 ui 규칙(UiTheme)에 맞춘다 · ui_shots hud 에 보인다 · spec 의 스트레스 경고 줄을 고친다
  - 기준: (앞 단계 QA) 이름이 다 쓰이면 주민 둘이 경고 줄 하나를 나눠 쓴다 — Villagers._free_name 은 NAMES 가 다 쓰이면 이름을 겹쳐 고른다(villagers.gd:257 주석 「다 쓰였을 때만 겹친다」). 줄 이름이 stress:<name_key> 라 같은 이름 두 주민은 한 줄을 서로 덮어쓴다. 막는 것은 아니다(주민 수가 이름 수보다 많을 때만). 고치려면 저장되는 주민 고유 표지(없으면 하나 만들어 to_dict/from_dict 에 담기)로 묶거나, 겹친 이름일 때 접미사를 붙이고, 같은 name_key 두 주민이 스트레스 50 을 넘으면 줄이 둘인지 보는 테스트를 더하라
- [x] 3. (앞 단계 QA) persona spec 의 옛 경고 결정 줄에 바뀜 표시가 없다
  - 기준: persona spec 의 옛 경고 결정 줄에 바뀜 표시가 없다 — spec/07_automation/villager-persona.md:10 「경고는 사라지지 않고 오른쪽 위에 로그로 계속 뜬다」(사람 결정 2026-09-28)가 그대로라 같은 절 21~26줄(사람 결정 2026-10-19 — 알림 줄을 없앴다)과 어긋나 읽힌다. 사람 줄은 지우지 말고 그 줄 끝에 「→ 2026-10-19 사람 결정으로 HUD 스트레스 표시(가장 높은 하나 · 고리 색)가 대신한다 — 아래 G-250.2」처럼 덧붙여라
  - 기준: (앞 단계 QA) notice.gd 주석이 없어진 경고를 예로 든다 — game/ui/hud/notice.gd:43 「주민 스트레스 경고의 이름이 올릴 때의 언어로 굳었다」는 이제 없는 스트레스 경고 줄을 들어 설명한다. 「(G-233.3 앞 단계 QA 때의 예 — 그 줄은 G-250.2 에서 없앴다)」처럼 낡았음을 적거나 지금 쓰는 다른 번역 키 이름 예로 바꿔라

## [x] G-251 새장 다섯 마리 — 상자처럼 넣는다
- 차선: 3d-lane2 · 시작 2026-10-19 14:27
- spec: spec/04_life/hunting.md, spec/04_life/ranching-breeding.md
- **사람 결정 2026-10-19**: 「새장안에 5마리까지 … 그 새들이 팀」 · 「새장에 넣는 방법은 새장에서 인벤토리가 보이고 상자처럼 넣을 수 있게」 · 「새가 지금 새장에 있는지 밖에 있는지 확인하는거랑 새를 호출하면 새가 다시 돌아오는거까지」 · 「새가 배회하거나 사냥하거나 하는 것도 날아다니고 새장에 돌아오는것도 날아서 들어옴」
- 이 세션이 2026-10-19 봤다: life/ranching/bird_cage.gd 는 **한 마리**(먹이 · 물 칸 한도 20). 「부르기」는 ui/hud/cage_panel.gd 에 있다(새를 넣은 뒤 보인다) · 새는 이미 난다(HEIGHT 5 + 3, RANGE 15) · 창에 하는 일(CAGE_STATE_*)이 뜬다
- [x] 1. 다섯 칸
  - 기준: 새장 하나에 새 **다섯**까지. 새마다 피로도 · 배고픔 · 하는 일이 따로 돌고, 먹이 · 물 칸은 새장 하나를 같이 쓴다(한도는 임시로 늘린다). 캐면 다섯이 든 채 가방에 · 옛 저장본(한 마리)은 첫 칸으로
  - 기준: 날기 · 돌아오기 · 사냥 · 새장에 내려앉기가 다 **날아서** — 여럿이 같은 자리에 겹치지 않는다(횃대 자리 다섯)
- [x] 2. 창 — 가방이 보이고 상자처럼
  - 기준: 새장 창에 내 가방이 보이고 상자처럼 끌어 놓기 · 클릭으로 새를 넣고 뺀다(새 아이템 = 잡은 매 · 올빼미). 새 칸마다 이름 · 지금 하는 일(새장에 있다 / 난다 / 돌아온다 / 사냥 / 잔다) · 「부르기」. 「다 부르기」 하나
  - 기준: 테스트로 부르면 날아서 돌아오고 1분 뒤 다시 나간다(피로도 50 이상이면 쉬었다 나감)가 다섯 마리 각각에 맞다 · ui_shots bird_cage 갱신
  - 기준: (앞 단계 QA) ranching-breeding.md 가 150줄 턱밑이다 — 147줄이다. 다음 단계(상자처럼 넣는 창 · 안팎 확인 · 부르기)에서 새장 줄을 더 적으면 넘는다. 새장 대목을 spec/04_life/ 아래 따로 떼거나(예: bird-cage.md) 줄을 줄여 두어라
  - 기준: (앞 단계 QA) CagedBird 머리 주석의 「넷째 · 다섯째의 짐승」이 헷갈린다 — caged_bird.gd · bird_cage.gd 머리 주석에서 「넷째 · 다섯째」는 하는 일(HUNT · GUARD)을 가리키는데 칸 다섯과 섞여 넷째 · 다섯째 칸으로 읽힌다. 「사냥 · 지키기의 짐승은 Stations 가 새마다 쥔다」로 고쳐라
- [x] 3. (앞 단계 QA) 새장 창 아래 빈 출력 칸 다섯이 남았다
  - 기준: 새장 창 아래 빈 출력 칸 다섯이 남았다 — ui_shots bird_cage 사진(.loop/out/shots_g251/bird_cage_ko.png) 맨 아래에 시설 창 공통의 출력 칸 다섯이 빈 채 선다. 새장은 출력이 없으니 station_window.gd 에서 새장(cage_panel 이 보일 때)에는 출력 줄을 숨기고, test_bird_cage_window.gd 에 「새장 창에는 출력 칸이 안 보인다」 단언을 더하라
  - 기준: (앞 단계 QA) 가방의 잡은 올빼미 칸 아래 글자가 「0상」으로 읽힌다 — 같은 사진의 가방 7번 칸(잡은 올빼미) 오른쪽 아래 숫자가 「0상」처럼 보인다. 무엇을 가리키는 글자인지(나이 「0살」이 잘렸나 · 상처 표시인가) 확인하고 잘리면 칸 글씨를 줄여라

## [x] G-252 망원경 — 쓰기 켜고 1 · 2 · 3 팀
- 차선: 3d-lane2 · 시작 2026-10-19 16:38
- 순서: G-251 뒤
- spec: spec/04_life/hunting.md, spec/04_life/ranching-breeding.md
- **사람 결정 2026-10-19**: 「망원경 반경안의 새라고 하니까 … 그렇게 하지 말고 … 망원경을 들으면 최대 3개의 새장이 보임. 4개 이후로 설치해도 차례대로 설치한 3개까지만 보이게 하고. 망원경을 들고 1번을 선택하고 야생동물을 좌클릭 하면 1번 새장에 있는 새들이 공격함 … 같은 야생 동물 공격하게 해서 집중공격해도 되고 각각 다른 야생동물 공격할 수 있도록 플레이어에게 자유」 · 「망원경 아이템을 들고 우클릭을 누르면 사용 비사용으로 하고 사용을 눌러야만 망원경이 사용되게 하면 되고 줌은 휠로 하면 되고 좌클릭으로 지정 123번키로 팀 변경」
- 이 세션이 2026-10-19 봤다: life/hunting/telescope.gd — 반경(REACH) 안 새가 다 날아가고 · 우클릭이 불러들이기 · HUD 「망원경 %d배 · 반경 안 새 %d」
- [x] 1. 쓰기 켜기 · 끄기
  - 기준: 망원경을 든 채 **우클릭 = 쓰기 / 안 쓰기**. 쓰는 동안만 줌(휠 1 ~ 8배) · 겨누기 · 1 · 2 · 3 팀 바꾸기. 안 쓸 때는 여느 아이템을 든 것과 같다(휠 · 숫자키가 단축칸 그대로). 옛 「반경 안 새」는 단계 2 의 「플레이어 곁 새장」으로 바뀐다
  - 기준: 옛 「우클릭 = 불러들이기」는 쓰는 동안의 다른 입력으로 옮기거나(예: R · 임시) 새장 창의 부르기로 둔다 — 어느 쪽인지 spec 에 적는다
- [x] 2. 팀 셋
  - 기준: 새장 하나 = 한 팀. **놓은 차례대로 앞의 셋**만 1 · 2 · 3 번(넷째부터는 팀이 없다 — 앞의 것을 캐면 다음 것이 올라온다). 쓰는 동안 1 · 2 · 3 으로 팀을 고르고 좌클릭한 야생동물을 **그 팀의 새들**이 공격한다(잡을 수 있는 새만 — 크기 · 낮밤 규칙 그대로). 팀마다 다른 짐승도 · 같은 짐승도 된다
  - 기준: **사람 결정 2026-10-19** 「플레이어 주변에 새장이 있어야돼. 잘못하면 원격 새장으로 될것 같아」 — 팀이 명령을 받는 것은 **그 새장이 플레이어 곁 반경(임시 30m — 새가 나는 반경 15m 의 두 배) 안**일 때만. 팀 번호는 놓은 차례대로 고정이고, 멀리 있는 새장의 팀은 HUD 에 회색 「1팀 — 새장이 멀다」로 남아 고를 수는 있어도 좌클릭이 먹지 않는다(까닭 한 줄). 겨눈 짐승까지의 거리 제한은 따로 두지 않는다 — 새가 새장에서 날아가 사냥한다. 반경은 서버가 잰다 (테스트: 새장에서 50m 떨어지면 공격 명령이 안 간다 · 다가가면 간다)
  - 기준: spec 에 「망원경은 원격 조종이 아니다 — 곁의 새장만」 한 줄 · 반경 수는 임시
  - 기준: HUD 에 「망원경 4배 · 2팀(매 3)」처럼 고른 팀 · 팀의 새 수, 팀마다 겨눈 짐승에 팀 번호 표시 (테스트: 두 새장 · 두 짐승 — 1팀 → A, 2팀 → B 가 따로 간다)
  - 기준: ui_shots 에 쓰는 중 망원경 HUD 한 장 · ko/en 문구
- [x] 3. (앞 단계 QA) 올빼미가 줄기에 막혀 10초 넘게 선다
  - 기준: 올빼미가 줄기에 막혀 10초 넘게 선다 — 구현 세션이 test_owl_trees.gd 의 밤 배회 흔들림을 씨앗(set_seed 7)과 손으로 맞춘 자리(-0.267, -0.964 방향)로 박아 잡았지만, 게임에서는 올빼미가 고른 곳 맞은편 줄기 둘레에 나오면 _aim_left 가 다할 때까지 서 있다. 박은 자리는 Animal 의 난수 순서가 바뀌면 다시 흔들린다. life/hunting 의 올빼미 나오는 자리(전역 randf)를 고른 곳 쪽으로 잡거나 줄기에 막히면 곧 다시 고르게 고치고, 테스트는 씨앗 · 손 좌표 없이 여러 씨앗(예: 0~9)에서 10초 안에 4m 를 벗어나는지 단언하라
  - 기준: (앞 단계 QA) teams() 의 is_queued_for_deletion 거름은 쓰이지 않는다 — Stations 가 캐면 CraftingStation.vanish() 가 그룹에서 먼저 빼므로 is_in_group 이 이미 거른다(그 거름만 빼도 test_taking_down_a_front_cage_brings_up_the_fourth 가 통과한다). 남겨도 되지만 life/hunting/telescope.gd teams() 주석에 「vanish 가 그룹에서 빼고 · queue 거름은 그 밖의 free 대비」라고 적어 두라

## [x] G-253 잡은 동물 창 — 스탯 · 이름 · 사는 곳
- 차선: 3d-lane2 · 시작 2026-10-19 18:42
- 순서: G-251 뒤
- spec: spec/04_life/ranching-breeding.md
- **사람 결정 2026-10-19**: 「동물마다 스텟을 다르게 할거고 그 동물을 키울 수 있게도 확장성을 가지게 할거야. 그래서 내가 포획한 동물들의 스텟을 보는 ui도 필요해. 동물의 이름도 변경 할 수 있게 해야돼. 일단 지금 동물 스텟은 공격력 방어력 체력 스피드 이정도 ? 만 두자. 나중엔 많아질거라 카테고리별로 분류가 될 수 있게 해줘」 · 이 세션의 「사는 곳으로 나누기(새장 · 우리 · 경비 · 벌통)」에 「좋다 그대로」
- [x] 1. 스탯 표
  - 기준: 종마다 스탯 표 한 곳(json) — 묶음(category) 「전투」 아래 공격력 · 방어력 · 체력 · 스피드. 값은 전부 **임시**. 묶음 · 스탯을 json 에 더하면 코드를 안 고쳐도 창에 나온다. 기르는 동물 하나하나는 제 이름 · 제 스탯(지금은 종 값 그대로)을 지니고 저장본에 담긴다 — 나중에 개체마다 달라질 자리
  - 기준: 사냥새의 싸움(덮치기 피해 · 다침 · 빠르기)이 이 표의 공격력 · 체력 · 스피드를 읽는다 — 지금 값과 같게 잡아 행동은 그대로
- [x] 2. 창
  - 기준: 새 창(단축키는 기존 창과 안 겹치게, 임시) — 탭이 **사는 곳**: 새장(매 · 올빼미) · 우리(사슴 · 앙고라토끼 등 가축) · 경비(거위) · 벌통(개체수 % 라 스탯 없이 벌통마다 한 줄). 동물을 고르면 스탯이 묶음별로 · 이름 바꾸기(빈 이름 막기 · 멀티는 서버가 정한다)
  - 기준: 바꾼 이름이 새장 창 · 망원경 HUD 등에도 보인다 · ui_shots 에 창 한 장 · ko/en 문구 · spec 에 사는 곳 탭 · 스탯 묶음 규칙
- [x] 3. (앞 단계 QA) ui_shots kept 자리를 지키는 단언이 없다
  - 기준: ui_shots kept 자리를 지키는 단언이 없다 — test_villager_gear_window.gd 329줄처럼 test_kept_window.gd 에 assert_true(UiShotsKit.PLACES.has("kept")) 한 줄을 더해라 (지금은 PLACES 에서 kept 를 빼도 아무 테스트도 떨어지지 않는다)

## [ ] G-254 벌통은 농사장 안에만 · 약은 넷만
- 차선: 3d-lane2 · 시작 2026-10-19 20:18
- spec: spec/05_craft/facilities.md (약 줄)
- **사람 결정 2026-10-19**: 「그냥 지금 그대로 하는게 맞는거 같아. 그리고 그냥 설치 자체를 농사장 안에 설치하게 만들자. 이렇게 하면 밀랍도 꿀도 다른 연산 필요 없이 설치만 하면 되는거니까 둘다 나오게 하면 될거고」
- **사람 결정 2026-10-19**: 「의약쪽 재료들이 심각한데 일단 내가 말하는거 빼고는 다 빼. 연고: 밀랍+식물성기름 화상연고: 밀랍+오소리기름 붕대: 동물가죽 의료키트:붕대+연고+술」 — 개수는 말하지 않았다(전부 1 · **임시**). 효과 값도 **임시**
- 이 세션이 2026-10-19 봤다: 약은 life/medicine/medicines.json 13 · 레시피 craft/recipes/apothecary_bench.json · 벌통 밀랍은 stations.json yields room=farm(시간마다 방을 본다) · 놓는 자리 규칙은 Stations.plan_spot(염전의 바닷가와 같은 길) · **식물성 기름은 아이템이 없다** — 지금 「기름(oil)」은 훈연기가 고기 · 생선에서 내는 덤이다 · 「동물 가죽」은 가죽(hide) · 술은 갈래 @liquor(양조통 술 여덟)
- [x] 1. 벌통은 농사장 안에만 놓인다
  - 기준: 벌통은 **농사장(farm) 방 안에만** 놓인다 — 밖이면 미리보기가 빨갛고 우클릭하면 까닭 한 줄(염전의 STATION_PLACE_NEEDS_SEA 와 같은 길). 놓인 벌통은 밀랍 · 꿀이 **방을 다시 보지 않고** 나온다(밀랍 60초 1 · 꿀 칸 규칙 그대로) — 놓은 뒤 방이 바뀌어도 계속 나온다(사람 「다른 연산 필요 없이」)
  - 기준: 옛 저장본에서 농사장 밖에 선 벌통은 그대로 서서 나온다 · 창의 「농사장이 아니라 밀랍이 안 나온다」 류 문구 · 방 캐시(test_beehive_rooms_cache)처럼 방을 보던 길은 걷는다 (테스트: 밭 방 밖에서는 안 놓인다 · 안에서 놓은 뒤 밭을 치워도 밀랍이 난다)
- [>] 2. 약은 넷만 → G-254-2 로 떼었다
  - 기준: 약 · 레시피를 **넷만** 남긴다 — 연고 = 밀랍 1 + 식물성 기름 1 · 화상 연고 = 밀랍 1 + 오소리 기름 1 · 붕대 = 가죽(hide) 1 · **의료키트**(새 아이템) = 붕대 1 + 연고 1 + 술(@liquor) 1. 효과 임시: 연고 HP +15 · 화상 연고 HP +20 · 붕대 HP +20 · 의료키트 HP +60
  - 기준: 나머지 약(진통제 · 마취제 · 독약 · 강한 독 · 해독제 · 기력 회복제 · 꿀약 · 해열제 · 소화제 · 약 붕대)은 목록 · 레시피 · 무게 · 번역 · ui_shots 에서 뺀다. 마취탄 셋 · 비료는 약이 아니라 남긴다. 뺀 약을 쓰던 곳(주민이 먹는 약 · 버프 독 · 상자 · 개발용 상자)을 다 찾아 맞추고, 옛 저장본에 든 뺀 약은 기존의 「없는 아이템」 규칙대로 다룬다(규칙이 없으면 불러올 때 지우고 spec 에 적는다)
  - 기준: **식물성 기름** 아이템을 새로 만든다(무게 · 번역 · 개발용 상자) — 나오는 곳은 단계 3 의 착유기. 훈연기의 「기름」은 그대로 두되 연고에는 안 들어간다
  - 기준: spec 약 표를 넷으로 고친다 · 「밀랍은 연고 · 화상 연고의 재료(사람 2026-10-19)」 · 갈래 oil 의 오소리 기름 주석 갱신
  - 기준: (앞 단계 QA) 벌통 옛 규칙 주석이 셋 남았다 — 놓인 벌통은 이제 방을 안 보는데 아직 「농사장 방 안에서만 시간이 흐른다」고 적힌 곳: game/life/ranching/bee_colony.gd 10줄(「밀랍은 … 농사장 방 안에서만 시간이 흘러 버퍼에 나온다」), spec/04_life/ranching-breeding.md 105줄(「밀랍만 농사장 규칙」), game/craft/stations/stations.json 의 _fields(「room 이 있으면 그 방 종류 안에 섰을 때만 … 시간이 흐른다」). 셋 다 「room 이 있으면 그 방 안에만 놓이고, 놓인 뒤엔 방을 보지 않는다 (G-254.1)」로 고쳐라
- [ ] 3. 착유기
  - 기준: **사람 결정 2026-10-19** 「착유기 시설을 만들어줘서 거기에서 나는걸로 하자」 — 새 시설 착유기(oil_press). 작업대에서 만든다 · 재료 목재 10 + 돌 5 (**임시**, 이 세션이 잡았다). 레시피 옥수수 3 → 식물성 기름 1 · 5초 (**임시** — 옥수수기름처럼 곡물에서 짠다, 사람이 해 보고 바꾼다)
  - 기준: 다른 제작 시설과 같은 길(stations.json · 레시피 파일 oil_press.json · 무게 · 번역 · ui_shots 에 착유기 창 한 장) · spec facilities.md 시설 표에 한 줄 (테스트: 옥수수 3 을 넣으면 식물성 기름 1 → 그것으로 연고가 된다)

## [>] G-254-2 약은 넷만 (G-254 단계 2 에서 떼었다 — 결정을 기다린다)
- 차선: 3d-lane2
- 떼어 온 곳: G-254 단계 2 — 나머지 단계는 G-254 가 이어 한다. 하다 만 것은 브랜치 g/G-254-2
- spec: spec/05_craft/facilities.md (약 줄)
- **사람 결정 2026-10-19**: 「그냥 지금 그대로 하는게 맞는거 같아. 그리고 그냥 설치 자체를 농사장 안에 설치하게 만들자. 이렇게 하면 밀랍도 꿀도 다른 연산 필요 없이 설치만 하면 되는거니까 둘다 나오게 하면 될거고」
- **사람 결정 2026-10-19**: 「의약쪽 재료들이 심각한데 일단 내가 말하는거 빼고는 다 빼. 연고: 밀랍+식물성기름 화상연고: 밀랍+오소리기름 붕대: 동물가죽 의료키트:붕대+연고+술」 — 개수는 말하지 않았다(전부 1 · **임시**). 효과 값도 **임시**
- 이 세션이 2026-10-19 봤다: 약은 life/medicine/medicines.json 13 · 레시피 craft/recipes/apothecary_bench.json · 벌통 밀랍은 stations.json yields room=farm(시간마다 방을 본다) · 놓는 자리 규칙은 Stations.plan_spot(염전의 바닷가와 같은 길) · **식물성 기름은 아이템이 없다** — 지금 「기름(oil)」은 훈연기가 고기 · 생선에서 내는 덤이다 · 「동물 가죽」은 가죽(hide) · 술은 갈래 @liquor(양조통 술 여덟)
- [ ] 1. 약은 넷만
  - 기준: 약 · 레시피를 **넷만** 남긴다 — 연고 = 밀랍 1 + 식물성 기름 1 · 화상 연고 = 밀랍 1 + 오소리 기름 1 · 붕대 = 가죽(hide) 1 · **의료키트**(새 아이템) = 붕대 1 + 연고 1 + 술(@liquor) 1. 효과 임시: 연고 HP +15 · 화상 연고 HP +20 · 붕대 HP +20 · 의료키트 HP +60
  - 기준: 나머지 약(진통제 · 마취제 · 독약 · 강한 독 · 해독제 · 기력 회복제 · 꿀약 · 해열제 · 소화제 · 약 붕대)은 목록 · 레시피 · 무게 · 번역 · ui_shots 에서 뺀다. 마취탄 셋 · 비료는 약이 아니라 남긴다. 뺀 약을 쓰던 곳(주민이 먹는 약 · 버프 독 · 상자 · 개발용 상자)을 다 찾아 맞추고, 옛 저장본에 든 뺀 약은 기존의 「없는 아이템」 규칙대로 다룬다(규칙이 없으면 불러올 때 지우고 spec 에 적는다)
  - 기준: **식물성 기름** 아이템을 새로 만든다(무게 · 번역 · 개발용 상자) — 나오는 곳은 단계 3 의 착유기. 훈연기의 「기름」은 그대로 두되 연고에는 안 들어간다
  - 기준: spec 약 표를 넷으로 고친다 · 「밀랍은 연고 · 화상 연고의 재료(사람 2026-10-19)」 · 갈래 oil 의 오소리 기름 주석 갱신
  - 기준: (앞 단계 QA) 벌통 옛 규칙 주석이 셋 남았다 — 놓인 벌통은 이제 방을 안 보는데 아직 「농사장 방 안에서만 시간이 흐른다」고 적힌 곳: game/life/ranching/bee_colony.gd 10줄(「밀랍은 … 농사장 방 안에서만 시간이 흘러 버퍼에 나온다」), spec/04_life/ranching-breeding.md 105줄(「밀랍만 농사장 규칙」), game/craft/stations/stations.json 의 _fields(「room 이 있으면 그 방 종류 안에 섰을 때만 … 시간이 흐른다」). 셋 다 「room 이 있으면 그 방 안에만 놓이고, 놓인 뒤엔 방을 보지 않는다 (G-254.1)」로 고쳐라

## 다음 — 위가 끝난 뒤 대화로 적는다
- **사람이 직접 플레이해 보는 자리다** — 루프가 할 일이 아니다 (2026-09-21).
  로드맵 한 바퀴: 생활 → 보급 → 배 → 새 자원 → 더 나은 생활
- 사람이 봐 둔 것 (2026-09-21): **치는 티가 안 난다** — 맨손 20대 · 도끼 4대를 치는 동안
  흔들림 · 소리 · 내구도(균열) 표시가 없다 (`struck` 신호를 듣는 곳이 없다). 디자인 단계에서 같이 본다
- 선착장: **내 섬 것은 플레이어가 짓고, 개척 섬 것은 맵에 박혀 있다** (spec/03_world/frontier-islands.md)
- **나아가는 순서** (사람 결정 2026-09-24, spec/00_core/roadmap.md) — **메인섬 → 옆섬 무역 개통 → 원정섬(맨 마지막)**
  - 1단계에 남은 것: **낚시** · **동물 세분화**(동물 목록은 사람이 정리해 준다 — G-148 뒤)
  - **NPC 가 기계(자동화)보다 먼저다** — 이벤트로 하나씩 들어오는 일손 (spec/07_automation/npc-villagers.md)
  - 2단계 「무역」은 spec 에 아직 없다 — 사람이 정해 준 뒤에 묶음으로 올린다
  - 섬 모양은 사람과 같이 다시 빚는다
- **멀티플레이를 할 때 같이**: 데스 상자에 주인이 없다 — `spec/08_combat/damage-death.md` 의
  「죽은 본인만 열고 꺼낼 수 있다」가 코드에 없다. 지금은 혼자뿐이라 드러나지 않는다 (루프가 찾음 2026-09-21)
- 그 밖: 광산 · 목장 · 펫 · 도감 · 날씨 · 멀티 · 경제 (roadmap.md 「빼는 것」)
