# 이미지 업로드 가이드

## 이미지 요구사항

```json
{
  "image_requirements": {
    "format": ["JPG", "PNG", "HEIC"],
    "max_size": "10MB",
    "min_resolution": "1280x720",
    "max_resolution": "4096x2160",
    "max_count": 15
  }
}
```

***

## 이미지 처리 프로세스

### 1. 전처리

* EXIF 데이터 제거
* 이미지 압축
* 해상도 최적화

### 2. 이미지 변형

* 썸네일 생성: 3가지 크기
* 워터마크 적용
* 최적화된 포맷 변환

### 3. 저장 구조

```
/images
  /{property_id}
    /original
    /thumbnail
    /watermarked
```

***

## 이미지 업로드 예시

```javascript
// 클라이언트 측 코드
const uploadImage = async (file) => {
  const formData = new FormData();
  formData.append('image', file);
  
  try {
    const response = await api.post('/properties/images', formData, {
      headers: {
        'Content-Type': 'multipart/form-data'
      }
    });
    return response.data.imageUrl;
  } catch (error) {
    handleError(error);
  }
};
```
