# 프로필 이미지 업로드 방법

이 디렉토리에 프로필 이미지를 업로드하세요.

## 사용 방법

1. 프로필 이미지를 이 디렉토리에 업로드합니다 (예: `profile.jpg`)
2. `hugo.toml` 파일에서 다음 라인의 주석을 해제하고 경로를 수정합니다:
   ```toml
   profileImage = "/images/profile.jpg"
   ```

## 권장 이미지 사양

- **크기**: 400x400px 이상 (정사각형 권장)
- **형식**: JPG, PNG, WebP
- **용량**: 500KB 이하 (웹 최적화 권장)

## 예시

```toml
[params]
  profileImage = "/images/profile.jpg"
```

이미지를 업로드하면 홈페이지의 프로필 섹션에 표시됩니다.
