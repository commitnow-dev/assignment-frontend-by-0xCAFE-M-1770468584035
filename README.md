
# 검색 자동완성 (Search Autocomplete)

## ✏️ 과제의 개요

| 카테고리 | 난이도 | 제한시간 |
|----------|--------|----------|
| frontend | normal | 7d |

### 💻 과제에서 요구하는 개발언어

- react
- javascript
- typescript


## 📜 과제의 내용

> 과제 설명과 요구사항을 참고하여, 구현해 주세요.

### 👀 과제의 설명

## 검색 자동완성 (Search Autocomplete)

### 개요

* 검색 입력 시 실시간으로 자동완성 결과를 표시하는 컴포넌트를 구현합니다.
* 디바운스로 불필요한 요청을 줄이고, 캐싱으로 중복 요청을 방지하며, 키보드 네비게이션으로 접근성을 확보합니다.

***

### 이 과제로 배우는 것

| 핵심 역량            | 포인트                                   |
| ---------------- | ------------------------------------- |
| **Debounce 패턴**  | 타이핑 중 불필요한 API 호출 방지                  |
| **캐싱 전략**        | TTL 기반 메모리 캐시로 중복 요청 제거               |
| **키보드 접근성**      | ↑↓ Enter Esc 네비게이션, ARIA 속성           |
| **Custom Hooks** | 재사용 가능한 로직 분리 (useDebounce, useCache) |

***

### 사용자 시나리오

1. 사용자가 검색창에 텍스트를 입력한다.
2. 입력이 멈추면(300ms) 자동완성 결과가 드롭다운으로 표시된다.
3. 검색어와 일치하는 부분이 하이라이트된다.
4. 키보드 ↑↓로 항목을 탐색하고, Enter로 선택한다.
5. Esc를 누르거나 외부를 클릭하면 드롭다운이 닫힌다.
6. 선택한 항목이 결과 영역에 표시된다.
7. 동일한 검색어 재입력 시 캐시된 결과가 즉시 표시된다.

***

### 필요한 화면

| 영역        | 핵심 요소                                |
| --------- | ------------------------------------ |
| **검색 입력** | 텍스트 입력, 로딩 스피너, 지우기(X) 버튼            |
| **드롭다운**  | 결과 목록, 검색어 하이라이트, 활성 항목 표시, 빈 결과 메시지 |
| **선택 결과** | 선택된 항목 정보 카드                         |

**접근성**: `role="combobox"`, `role="listbox"`, `role="option"`, `aria-selected`, `aria-expanded`

***

### 데이터 인터페이스 및 예시

```typescript
// types.ts
export interface SearchItem {
  id: string;
  name: string;
  code?: string;
  flag?: string;
}

export interface CacheEntry<T> {
  data: T;
  timestamp: number;
}

export interface SearchState {
  query: string;
  results: SearchItem[];
  isLoading: boolean;
  isOpen: boolean;
  activeIndex: number;
  selectedItem: SearchItem | null;
}
```

```json
// countries.json (검색용 예시)
[
  { "id": "kr", "name": "대한민국", "code": "KR", "flag": "🇰🇷" },
  { "id": "us", "name": "미국", "code": "US", "flag": "🇺🇸" },
  { "id": "jp", "name": "일본", "code": "JP", "flag": "🇯🇵" },
  { "id": "cn", "name": "중국", "code": "CN", "flag": "🇨🇳" },
  { "id": "gb", "name": "영국", "code": "GB", "flag": "🇬🇧" }
]
```

```json
// search.response.example.json (API 응답 형태)
{
  "query": "한",
  "results": [
    { "id": "kr", "name": "대한민국", "code": "KR", "flag": "🇰🇷" }
  ],
  "total": 1
}
```

​


### 🎯 과제의 요구사항

- [입력] 텍스트 입력 시 디바운스 적용 (300ms)
- [입력] 로딩 중 스피너 표시
- [입력]  입력값이 있을 때 지우기(X) 버튼 표시
- [입력]  지우기 버튼 클릭 시 입력값 초기화 및 드롭다운 닫기
- [드롭다운] 검색 결과를 드롭다운 목록으로 표시
- [드롭다운] 검색어와 일치하는 부분 하이라이트 처리
- [드롭다운] 결과 없을 때 "검색 결과가 없습니다" 메시지 표시
- [키보드] ↓ 키로 다음 항목 이동 (마지막→첫번째 순환)
- [키보드] ↑ 키로 이전 항목 이동 (첫번째→마지막 순환)
- [키보드] Enter 키로 현재 활성 항목 선택
- [키보드] Esc 키로 드롭다운 닫기
- [선택] 마우스 클릭으로 항목 선택
- [선택] 마우스 호버 시 해당 항목 활성화
- [선택] 선택 시 드롭다운 닫기 및 입력값 업데이트
- [캐싱] 검색 결과를 메모리에 캐시
- [캐싱] 동일 검색어 재요청 시 캐시에서 반환
- [UX] 외부 클릭 시 드롭다운 닫기
- [접근성] 입력 필드에 role="combobox" 적용
- [접근성] 드롭다운에 role="listbox" 적용
- [접근성] 각 항목에 role="option", aria-selected 적용
