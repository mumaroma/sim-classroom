# 수학 시뮬레이션 클래스룸 — 설정 가이드

## 필요한 것
- 인터넷 브라우저만 있으면 됩니다 (설치 불필요)
- 소요시간: 약 10분

---

## Step 1 — Supabase 계정 만들기 (실시간 데이터 저장소)

1. https://supabase.com 접속 → **Start your project** 클릭
2. GitHub 또는 이메일로 가입 (무료)
3. **New Project** 클릭
   - 이름: `sim-classroom` (아무 이름)
   - 비밀번호: 기억하기 쉬운 것으로 설정
   - Region: **Northeast Asia (Seoul)** 선택
4. 프로젝트 생성 대기 (약 2분)

---

## Step 2 — 데이터베이스 테이블 만들기

1. 왼쪽 메뉴에서 **SQL Editor** 클릭
2. **New query** 클릭 후 아래 SQL을 붙여넣고 **Run** 버튼 클릭:

```sql
CREATE TABLE sim_states (
  session_id text NOT NULL,
  student_name text NOT NULL,
  state jsonb DEFAULT '{}',
  updated_at timestamptz DEFAULT now(),
  PRIMARY KEY (session_id, student_name)
);

ALTER TABLE sim_states ENABLE ROW LEVEL SECURITY;

CREATE POLICY "allow_anon_all" ON sim_states
  FOR ALL TO anon
  USING (true)
  WITH CHECK (true);
```

---

## Step 3 — API 키 복사하기

1. 왼쪽 메뉴 맨 아래 **Project Settings** (톱니바퀴) 클릭
2. **API** 탭 클릭
3. 아래 두 값을 복사해 둡니다:
   - **Project URL** (예: `https://abcdefg.supabase.co`)
   - **anon public** 키 (긴 문자열)

---

## Step 4 — 파일 배포 (Netlify Drop)

1. https://app.netlify.com/drop 접속 (계정 불필요)
2. `classroom` 폴더 전체를 드래그 앤 드롭
3. 배포 완료 → 자동 생성된 URL 확인
   (예: `https://funny-name-12345.netlify.app`)

---

## Step 5 — 시작하기

1. 배포된 URL에서 **teacher.html** 열기
   예: `https://funny-name-12345.netlify.app/teacher.html`
2. 첫 화면에서 Step 3에서 복사한 URL과 키 입력 후 저장
3. **새 세션 만들기** 클릭
4. 학생 공유 링크 복사 → 학생들에게 전송

---

## 학생 접속 방법

학생들은 링크를 클릭하면 이름 입력 후 바로 시뮬레이션 사용 가능.
별도 앱 설치나 로그인 불필요.

---

## 무료 한도 (Supabase 무료 플랜)

| 항목 | 무료 한도 |
|------|-----------|
| 저장 용량 | 500MB |
| 동시 접속 | 200명 |
| 요청 수 | 무제한 (월 50만 row 변경) |

수업용으로 충분합니다.
