# CLAUDE.md - dodota 프로젝트

## 프로젝트 개요
게임 레벨/티켓 계산기. 레벨 선택 시 달성일차, 티켓 현황을 표시하고 7종 취미 레벨 배분을 시뮬레이션하는 단일 HTML 페이지.

## 배포
- **GitHub 저장소**: https://github.com/chickmen89/dodota
- **도메인**: https://dodota.soluni.kr
- **호스팅**: GitHub Pages (main 브랜치, / 루트)
- **DNS**: Cloudflare 프록시 경유 (CNAME → chickmen89.github.io)
- **배포 방법**: `git push` → GitHub Pages 자동 배포 (1~2분 소요)

## 파일 구조
```
dodota/
├── CLAUDE.md
├── CNAME          # GitHub Pages 커스텀 도메인 설정 (자동 생성)
└── index.html     # 메인 페이지 (CSS/JS 내장 단일 파일)
```

## 게임 데이터 모델
- **레벨 범위**: 2 ~ 37
- **일일 획득 XP**: 50
- **달성일차 공식**: `Math.floor((누적XP - 1) / 50) + 1`
- **도감티켓**: 35 (고정)
- **보정치**: 기본값 +1 (게임 데이터와의 오차 보정용)
- **총 티켓**: 누적티켓 + 도감티켓 + 보정치

### 취미 시스템
- **7종**: 원예, 요리, 낚시, 곤충채집, 새관찰, 고양이, 강아지
- **레벨 범위**: 1 ~ 10
- **레벨업 비용**: N→N+1 레벨업에 N 티켓 소모
- **총 소모 공식**: `N * (N - 1) / 2`

## 데이터 저장
- `localStorage` 사용 (키: `gameLevelCalcState`)
- 저장 항목: 레벨, 보정치, 기준일, 취미 7종 레벨

## 코드 수정 시 주의사항
- 단일 파일 구조 유지 (외부 의존성 없음)
- `render()` 함수가 모든 UI 업데이트를 담당
- 취미 옵션 비활성화 로직: 잔여 티켓 부족 시 해당 레벨 선택 불가
- 데이터 배열은 index 0 = 레벨 2에 매핑 (`state.level - 2`로 접근)
