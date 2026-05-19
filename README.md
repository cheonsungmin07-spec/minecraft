# 마인크래프트 1.21.4 인생약탈 PVP 서버

Paper 1.21.4 기반 인생약탈 PVP 서버 구성입니다. 저장소 구조는 활성 플러그인 jar와 설정 폴더를 분리해 관리하도록 정리했습니다.

## 폴더 구조

- `plugins/`: 서버에 로드할 활성 `.jar` 플러그인 파일
- `configs/`: 플러그인별 설정 폴더
- `backups/`: 기존 `_backup_` 폴더, `.bak` 파일, 비활성화한 중복 jar 백업
- `config/`: Paper 전역 설정
- `flatworld`, `flatworld_nether`, `flatworld_the_end`, `test`, `www`, `ys`: 서버 월드 데이터

`start.bat`은 실행 전 `configs/*`를 `plugins/*`로 동기화한 뒤 Paper를 실행합니다. 수동 실행 또는 호스팅 패널 사용 시에도 같은 방식으로 설정 폴더를 `plugins` 아래에 복사해야 합니다.

## 플러그인 목록

| 플러그인 | 버전 | jar |
| --- | --- | --- |
| Citizens | 2.0.37-SNAPSHOT build 3762 | `Citizens-2.0.37-b3762.jar` |
| ClearLag | 3.2.2 | `Clearlag2.jar` |
| CommandPrivacy | 1.0.0 | `CommandPrivacy-1.0.0.jar` |
| DeadChest | 4.30.0 | `dead-chest-4.30.0-open-access.jar` |
| DeathTax | 1.0.0 | `DeathTax.jar` |
| EasyGuiShop | 1.0.0 | `EasyGuiShop-1.0.0.jar` |
| EssentialsX | 2.21.0 | `EssentialsX-2.21.0.jar` |
| EssentialsXChat | 2.21.0 | `EssentialsXChat-2.21.0.jar` |
| FancyHolograms | 2.10.0-java21 | `FancyHolograms-2.10.0-java21.jar` |
| LeaderBoard | 1.3.0 | `LeaderBoard-1.3.0.jar` |
| LuckPerms | 5.5.42 | `LuckPerms-Bukkit-5.5.42.jar` |
| Multiverse-Core | 5.6.1 | `multiverse-core-5.6.1.jar` |
| PixelPrinter | 1.0.48.1 | `PixelPrinter-1.21.4.jar` |
| PlaceholderAPI | 2.12.2 | `PlaceholderAPI-2.12.2.jar` |
| SkBee | 3.16.1 | `SkBee-3.16.1.jar` |
| skRayFall | 1.9.30 | `skRayFall v1.9.30.jar` |
| Skript | 2.15.2 | `Skript-2.15.2.jar` |
| skript-placeholders | 1.7.1 | `skript-placeholders-1.7.1.jar` |
| TAB | 6.0.2 | `TAB v6.0.2 - Vanilla.jar` |
| TeamManager | 1.0.0 | `TeamManager.jar` |
| Vault | 1.7.3-b131 | `Vault.jar` |
| WorldEdit | 7.4.2+7450-eb8e82c | `worldedit-bukkit-7.4.2.jar` |
| WorldGuard | 7.0.13+82fdc65 | `worldguard-bukkit-7.0.13-dist.jar` |

## 의존성 관계

- `WorldGuard`는 `WorldEdit`에 의존합니다.
- `EssentialsXChat`은 `EssentialsX`에 의존합니다.
- `EasyGuiShop`, `LeaderBoard`, `DeathTax`는 `Vault` 경제 연동을 사용합니다.
- `LuckPerms`는 `Vault`보다 먼저 로드되도록 설정되어 권한 연동의 기준 플러그인 역할을 합니다.
- `skRayFall`은 `Skript`에 의존합니다.
- `skript-placeholders`는 `Skript`, `PlaceholderAPI`와 연동합니다.
- `Citizens`, `TAB`, `Multiverse-Core`, `DeadChest`, `Skript`, `WorldEdit`, `WorldGuard`는 `Vault`, `PlaceholderAPI`, `Citizens` 등과 선택적으로 연동합니다.

## 점검 및 수정 내역

- `FancyHolograms` jar가 `2.10.0`과 `2.10.0-java21` 두 개로 중복되어 있어 Paper 1.21.4 Java 21 환경에 맞는 `2.10.0-java21`만 활성화했습니다.
- 비활성화한 `FancyHolograms-2.10.0.jar`는 삭제하지 않고 `backups/FancyHolograms-2.10.0.jar.bak`로 보존했습니다.
- `LeaderBoard/config.yml`, `RankSystem/config.yml`의 깨진 한글과 닫히지 않은 따옴표를 수정해 정상 YAML 구조로 정리했습니다.
- `CommandPrivacy/config.yml`의 깨진 허용 명령 목록을 정상 목록으로 재작성하고, 플러그인 목록/버전 확인 명령 차단은 유지했습니다.
- `DeathTax/config.yml`의 깨진 메시지를 한국어 안내문으로 복구했습니다.
- `DeadChest/config.yml`의 깨진 주석을 한국어 운영 설명으로 복구하고, 공개 회수형 사망 상자 설정을 유지했습니다.
- `ClearLag`, `PixelPrinter`의 자동 업데이트를 꺼서 운영 중 예기치 않은 플러그인 변경 가능성을 낮췄습니다.

## 충돌 가능성 및 주의사항

- `DeathTax`와 `DeadChest`는 모두 사망 이벤트를 다룹니다. 현재 설정은 돈 차감/처치 보상은 `DeathTax`, 아이템 보관은 `DeadChest`가 담당하도록 분리되어 있습니다.
- `WorldGuard`와 `DeadChest` 연동은 현재 `DeadChest` 쪽에서 비활성화되어 있습니다. 보호구역 안 사망 상자 생성을 지역별로 통제하려면 `configs/DeadChest/config.yml`의 `integrations.worldguard.enabled`를 켜고 지역 플래그를 별도 설계해야 합니다.
- `configs` 폴더는 저장소 관리용 구조입니다. 실제 서버 실행 전에는 `start.bat`처럼 `configs` 내용을 `plugins`로 복사해야 플러그인이 설정을 읽습니다.
- `logs/`, `cache/`, `libraries/`, `versions/`, 플러그인 로그/캐시는 `.gitignore`로 제외했습니다.
